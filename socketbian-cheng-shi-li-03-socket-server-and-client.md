### Socket编程示例03--Socket Server and Client

* ##### Socket Server

```
# -*- coding:utf-8 -*-

"""
@Author:        hogan.chen@ymail.com
@Create Date:   2018-02-18
@Update Date:   2018-02-19
@Version:       V0.9.20180219
"""

import os
import re
import time
import json
import socket
import serial
import logging
import datetime
import threading
import socketserver

FPGA_SERIAL_PORT = 'COM11'
FPGA_SERIAL_BAUD_RATE = '115200'
FPGA_SERIAL_TIMEOUT = 5  # seconds
FPGA_SERIAL_GET_DATA_FLAG = False

FPGA_LOG_PREFIX_STR = 'FPGA'
PHONE_LOG_PREFIX_STR = 'Phone'

BUILD_INIT_FLAG = False
BUILD_INIT_PER_TEST_FLAG = True

SOCKET_SERVER_ADDRESS = '127.0.0.1'
SOCKET_SERVER_PORT = 5000
SOCKET_RECEIVE_BUFFER = 1024

TEST_LOG_DIR = '.\\logs'
TEST_LOG_SUFFIX = '.log'

TEST_ENV = {
    'version': '',
}

TEST_CASE_CONF = {}

PRINT_THREADING_LOCK = threading.Lock()

LOGGING_LEVEL = logging.INFO


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


class SerialPortHandleClass:
    def __init__(self, port, baud_rate='115200', read_timeout=5, write_time=5):
        self.port = port
        self.baud_rate = baud_rate
        self.read_timeout = read_timeout
        self.write_timeout = write_time

        self.__serial_handle = None
        self.serial_rw_status_flag = False

        self.open_serial_port()

    def open_serial_port(self):
        try:
            self.__serial_handle = serial.Serial(self.port, self.baud_rate, timeout=self.read_timeout,
                                                 write_timeout=self.write_timeout)
        except (serial.SerialTimeoutException, serial.SerialException) as err:
            self.serial_rw_status_flag = False
            logging.critical('Can not open serial port {} due to {}'.format(self.port, err))
            raise IOError(err)
        else:
            self.serial_rw_status_flag = True

    def close_serial_port(self):
        if self.serial_rw_status_flag:
            self.__serial_handle.close()
            self.serial_rw_status_flag = False

    def send_cmd_to_serial(self, cmd_str):
        if self.serial_rw_status_flag:
            cmd_str = re.sub(r'\s+', '', cmd_str)
            cmd_byte = cmd_str.encode('utf-8')

            try:
                self.__serial_handle.write(cmd_byte)
            except (serial.SerialTimeoutException, serial.SerialException):
                self.serial_rw_status_flag = False

    def reset_output_buffer(self):
        if self.serial_rw_status_flag:
            self.__serial_handle.reset_output_buffer()

    def get_data_from_serial(self, read_flag=1):
        serial_data = None

        if self.serial_rw_status_flag:
            try:
                if 0 == read_flag:
                    serial_data = self.__serial_handle.readline()
                elif 1 == read_flag:
                    serial_data = self.__serial_handle.readlines()
                else:
                    logging.error('Unsupported read flag value {}'.format(read_flag))
            except serial.SerialException:
                self.serial_rw_status_flag = False

        return serial_data


class TestLogHandleClass:
    def __init__(self, file_name):
        self.__file_name = os.path.join(TEST_LOG_DIR, file_name)

        self.__fd = None
        self.__file_open_flag = False
        self.__file_end_flag = False

        self.open_log_file()

    @staticmethod
    def create_log_folder():
        log_path = TEST_LOG_DIR

        if os.path.exists(log_path):
            if not os.path.isdir(log_path):
                try:
                    os.remove(log_path)
                    os.makedirs(log_path)
                finally:
                    pass
        else:
            try:
                os.makedirs(log_path)
            finally:
                pass

    def open_log_file(self):
        self. create_log_folder()

        try:
            self.__fd = open(self.__file_name, 'a', encoding='utf-8')
            self.__fd.seek(0, 0)
            self.__file_open_flag = True
        except Exception as err:
            self.__file_open_flag = False
            logging.critical('Can not open {} file, error info is: {}'.format(self.__file_name, err))
            raise IOError(err)

    def close_log_file(self):
        if self.__file_open_flag:
            self.__fd.close()
            self.__file_open_flag = False
            self.__file_end_flag = False

    def write_log_to_file(self, source, msg):
        datetime_str = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S.%f")
        write_msg = '{} -> [{}]{}'.format(datetime_str, source, msg)
        debug_print(write_msg)

        if self.__file_open_flag:
            try:
                self.__fd.write('{}\n'.format(write_msg))
                self.__fd.flush()
            except Exception as err:
                logging.error('Write log file {} error, error info is: {}'.format(self.__file_name, err))
                self.close_log_file()


class SocketRequestHandler(threading.Thread):
    phone_status_list = []
    phone_run_status = {'ip': None, 'flag': False}

    phone_status_list_lock = threading.Lock()

    def __init__(self, app_project_name, request, client_address, test_config):
        super().__init__()
        self.app_project_name = app_project_name
        self.request = request
        self.client_address = client_address
        self.test_config = test_config

        self.phone_info = None

        self.log_handle_inst = None
        self.serial_handle_inst = None

        self.case_flag = False
        self.finished_flag = False
        self.abnormal_flag = False

    def send_message_to_phone(self, response_msg):
        debug_print('Send Message: "{}" to {} ip address'.format(response_msg, self.client_address))

        if response_msg:
            json_str = json.dumps(response_msg)
            try:
                self.request.sendall(json_str.encode(encoding='utf-8'))
            except Exception as err:
                debug_print('Error Message: "{}" with {} client ip address'.format(err, self.client_address))
                self.abnormal_flag = True

    def send_command_to_phone(self, cmd_str):
        cmd_data = {'type': 'cmd', 'category': 'phone', 'message': cmd_str}

        self.send_message_to_phone(cmd_data)

    def send_command_to_fpga(self, cmd_str):
        if self.serial_handle_inst.serial_rw_status_flag:
            debug_print('Send Message: send command "{}" to FPGA'.format(cmd_str))

            log_str = 'send command "{}" to FPGA'.format(cmd_str)
            self.log_handle_inst.write_log_to_file(FPGA_LOG_PREFIX_STR, log_str)

            self.serial_handle_inst.send_cmd_to_serial(cmd_str)

            if FPGA_SERIAL_GET_DATA_FLAG:
                serial_data_list = self.serial_handle_inst.get_data_from_serial()

                for serial_data in serial_data_list:
                    if serial_data and serial_data.strip():
                        self.log_handle_inst.write_log_to_file(FPGA_LOG_PREFIX_STR,
                                                               serial_data.decode(encoding='utf-8'))
        else:
            self.abnormal_flag = True

    def run_test(self):
        self.serial_handle_inst = SerialPortHandleClass(FPGA_SERIAL_PORT, FPGA_SERIAL_BAUD_RATE)
        self.serial_handle_inst.reset_output_buffer()

        debug_print('start to running test cases for {} project...'.format(self.app_project_name))

        debug_print('self.phone_info: {}'.format(self.phone_info))

        # self.log_handle_inst = TestLogHandleClass('{}_{}_{}{}'.format(
        #     re.sub(r'\s', '', self.phone_info['model']), self.phone_info['imei'],
        #     case_name, TEST_LOG_SUFFIX))
        self.log_handle_inst = TestLogHandleClass('{}_{}_{}{}'.format(
            re.sub(r'\s', '', self.phone_info['model']), self.phone_info['imei'], self.app_project_name,
            TEST_LOG_SUFFIX))

        self.send_command_to_phone(self.test_config)
        self.finished_flag = False

        while True:
            if self.abnormal_flag or self.finished_flag:
                break

            try:
                req_str = self.request.recv(SOCKET_RECEIVE_BUFFER).decode(encoding='utf-8')
            except Exception as err:
                debug_print('Error Message: "{}" with {} client ip address'.format(err, self.client_address))
                self.abnormal_flag = True
                break
            else:
                if req_str:
                    debug_print('Received Message: "{}", from {} ip address'.format(req_str, self.client_address))

                    try:
                        json_str = json.loads(req_str)
                    except BaseException as err:
                        debug_print('Error Message: json load error with "{}" error'.format(err))
                    else:
                        msg_type = json_str['type']

                        if 'cmd' == msg_type:
                            self.command_msg_handle(json_str)
                        elif 'log' == msg_type:
                            self.log_msg_handle(json_str)
                        else:
                            debug_print('Incorrect Message: {}'.format(json_str))
                else:
                    self.abnormal_flag = True

            if FPGA_SERIAL_GET_DATA_FLAG:
                self.get_data_from_serial_port()

        self.finished_flag = True

    def get_data_from_serial_port(self):
        if self.serial_handle_inst.serial_rw_status_flag:
            serial_data_list = self.serial_handle_inst.get_data_from_serial()
            for serial_data in serial_data_list:
                if serial_data and serial_data.strip():
                    self.log_handle_inst.write_log_to_file(FPGA_LOG_PREFIX_STR,
                                                           serial_data.decode(encoding='utf-8'))
        else:
            self.abnormal_flag = True

    def end_handle(self):
        if self.log_handle_inst is not None:
            self.log_handle_inst.close_log_file()

        if self.serial_handle_inst is not None:
            self.serial_handle_inst.close_serial_port()

        SocketRequestHandler.phone_run_status['ip'] = None
        SocketRequestHandler.phone_run_status['flag'] = False

        for phone_status_dict in SocketRequestHandler.phone_status_list:
            if self.client_address[0] == phone_status_dict['ip']:
                phone_status_dict['tested'] = True

    def command_msg_handle(self, json_msg):
        if 'fpga' == json_msg['category']:
            self.send_command_to_fpga(json_msg['message'])
        elif 'pc' == json_msg['category']:
            pass
        else:
            pass

    def log_msg_handle(self, json_msg):
        if self.log_handle_inst is not None:
            self.log_handle_inst.write_log_to_file(PHONE_LOG_PREFIX_STR, json_msg['message'])

        if 'result' == json_msg['category']:
            self.log_handle_inst.close_log_file()
            self.finished_flag = True
        else:
            ack_data = {'type': 'info', 'category': 'ack', 'message': 'ack message'}
            self.send_message_to_phone(ack_data)

    def info_msg_handle(self, info_msg):
        for phone_status_dict in SocketRequestHandler.phone_status_list:
            if self.client_address[0] == phone_status_dict['ip']:
                break
        else:
            SocketRequestHandler.phone_status_list_lock.acquire()
            SocketRequestHandler.phone_status_list.append({'ip': self.client_address[0],
                                                           'port': self.client_address[1],
                                                           'tested': False,
                                                           })
            # SocketRequestHandler.phone_status_list.append({'ip': '{}:{}'.
            #                                               format(self.client_address[0], self.client_address[1]),
            #                                                'tested': False,
            #                                                })
            SocketRequestHandler.phone_status_list_lock.release()

        if 'heart' == info_msg['category']:
            debug_print("Heartbeat Message: Received heartbeat packet from {}, the message content is: {}".
                        format(self.client_address, info_msg))

            # self.send_message_to_phone(info_msg)
        elif 'phone' == info_msg['category']:
            self.phone_info = info_msg['message']
            debug_print("Phone Info Message: Received info message from {}, the message content is: {}".
                        format(self.client_address, info_msg))
        else:
            pass

    def request_handle(self):
        while True:
            try:
                req_str = self.request.recv(SOCKET_RECEIVE_BUFFER).decode(encoding='utf-8')
            except Exception as err:
                debug_print('Error Message: {} with client ip address {}'.format(err, self.client_address))
                break
            else:
                if req_str:
                    debug_print('Received Message: "{}", which from {} ip address'.
                                format(req_str, self.client_address))

                    try:
                        json_str = json.loads(req_str)
                    except BaseException as err:
                        debug_print('Error Message: json load error with {}'.format(err))
                    else:
                        msg_type = json_str['type']

                        if 'info' == msg_type:
                            self.info_msg_handle(json_str)
                        else:
                            debug_print('Incorrect Message: {}'.format(json_str))
                else:
                    debug_print('Error Message: The connection shutdown with client ip address {}'.
                                format(self.client_address))

                    for entry_index in range(len(SocketRequestHandler.phone_status_list)):
                        if self.client_address[0] == SocketRequestHandler.phone_status_list[entry_index].get('ip'):
                            SocketRequestHandler.phone_status_list_lock.acquire()
                            SocketRequestHandler.phone_status_list.pop(entry_index)
                            SocketRequestHandler.phone_status_list_lock.release()
                            break

                    break

                debug_print('SocketRequestHandler.phone_status_list: {}'.
                            format(SocketRequestHandler.phone_status_list))
                debug_print('SocketRequestHandler.phone_run_status: {}'.
                            format(SocketRequestHandler.phone_run_status))

                if not SocketRequestHandler.phone_run_status.get('flag'):
                    for phone_status_dict in SocketRequestHandler.phone_status_list:
                        if not phone_status_dict.get('tested'):
                            if self.client_address[0] == phone_status_dict.get('ip'):

                                SocketRequestHandler.phone_run_status['flag'] = True
                                SocketRequestHandler.phone_run_status['ip'] = self.client_address

                                self.run_test()

                            break

                # abnormal occur, enter the end handle
                if self.abnormal_flag or self.finished_flag:
                    self.end_handle()
                    debug_print('Test finished for {} project..., abnormal_flag: {}, finished_flag: {}'.
                                format(self.app_project_name, self.abnormal_flag, self.finished_flag))
                    break

            '''
            send_data = 'Response from socket server with threading {}...'.format(self.name)

            # https://stackoverflow.com/questions/180095/how-to-handle-a-broken-pipe-sigpipe-in-python/180922#180922
            # https://stackoverflow.com/questions/11866792/how-to-prevent-errno-32-broken-pipe
            # https://stackoverflow.com/questions/41014252/socket-error-errno-32-broken-pipe/41015359
            # https://stackoverflow.com/questions/409783/socket-shutdown-vs-socket-close
            # https://www.programcreek.com/python/example/3678/socket.SHUT_RDWR
            # https://pymotw.com/2/socket/tcp.html
            # https://pythontips.com/2013/08/06/python-socket-network-programming/
            # https://docs.python.org/3/howto/sockets.html
            # https://docs.python.org/3/library/socket.html
            try:
                self.request.sendall(send_data.encode())
            except ConnectionError as err:
                # self.socket.shutdown(socket.SHUT_RDWR)
                # self.socket.close()
                # 客户端调用上面两条语句断开连接，这时候服务器端的发送会产生如下错误
                # Error message: [Errno 32] Broken pipe
                print('Error message: {} with client ip address {}'.format(err, self.client_address))

                # socket.shutdown(how)
                # Shut down one or both halves of the connection. If how is SHUT_RD, further receives are disallowed.
                # If how is SHUT_WR, further sends are disallowed. If how is SHUT_RDWR,
                # further sends and receives are disallowed.

                # 如客户端已断开连接，这时调用socket.shutdown(socket.SHUT_RDWR)会报错，
                # 但是没有试验SHUT_RD或者SHUT_WR参数有没有问题，待验证
                # self.request.shutdown(socket.SHUT_RDWR)

                # Mark the socket closed. The underlying system resource (e.g. a file descriptor) is also closed when
                # all file objects from makefile() are closed. Once that happens, all future operations on the socket
                # object will fail. The remote end will receive no more data (after queued data is flushed).
                # Sockets are automatically closed when they are garbage-collected, but it is recommended to close()
                # them explicitly, or to use a with statement around them.

                # Note: close() releases the resource associated with a connection but does not necessarily close
                # the connection immediately. If you want to close the connection in a timely fashion, call shutdown()
                # before close().
                self.request.close()
                break
            '''

    def run(self):
        self.request_handle()


def debug_print(msg):
    datetime_str = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S.%f")
    print_msg = 'Debug Information: {} -> {}'.format(datetime_str, msg)

    PRINT_THREADING_LOCK.acquire()
    print(print_msg)
    PRINT_THREADING_LOCK.release()


def app_init(app_project):
    pass


def run_test(app_project_name, app_project_dict):
    client_thread_lists = []
    server_address = (SOCKET_SERVER_ADDRESS, SOCKET_SERVER_PORT)

    test_config = app_project_dict.get('test_config')
    app_project = app_project_dict.get('app_project')

    # download bit file, compile project and download app to flash, run debug
    if BUILD_INIT_FLAG and BUILD_INIT_PER_TEST_FLAG:
        app_init(app_project)

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as socket_server:
        # Running an example several times with too small delay between executions, could lead to this error:
        # OSError: [Errno 98] Address already in use
        # This is because the previous execution has left the socket in a TIME_WAIT state,
        # and can’t be immediately reused.
        # There is a socket flag to set, in order to prevent this, socket.SO_REUSEADDR:
        '''
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        s.bind((HOST, PORT))
        '''
        # the SO_REUSEADDR flag tells the kernel to reuse a local socket in TIME_WAIT state,
        # without waiting for its natural timeout to expire.
        socket_server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

        # Bind the socket to address. The socket must not already be bound.
        socket_server.bind(server_address)

        # Set a timeout on blocking socket operations
        socket_server.settimeout(60)

        # Enable a server to accept connections. If backlog is specified,
        # it must be at least 0 (if it is lower, it is set to 0);
        # it specifies the number of unaccepted connections that the system will allow before refusing new connections.
        # If not specified, a default reasonable value is chosen.
        socket_server.listen()

        while True:
            try:
                request, client_address = socket_server.accept()
            except Exception as err:
                debug_print('error happened while accept connection, error message: {}'.format(err))
            else:
                debug_print('####################New connect from {} ...####################'.format(client_address))

                client_thread = SocketRequestHandler(app_project_name, request, client_address, test_config)
                client_thread_lists.append(client_thread)
                client_thread.daemon = True
                client_thread.start()
                time.sleep(5)
            finally:
                for phone_status_dict in SocketRequestHandler.phone_status_list:
                    if not phone_status_dict['tested']:
                        break
                else:
                    break


def run_all_tests(app_project_list):
    test_env = TEST_ENV
    test_info_dict = TEST_CASE_CONF[test_env['version']]
    app_project_group_dict = test_info_dict.get('app_project_dict')

    for app_project_name in app_project_list:
        app_project_dict = app_project_group_dict.get(app_project_name)
        app_project = app_project_dict.get('app_project')

        if app_project is not None:
            run_test(app_project_name, app_project_dict)


class ThreadedTCPRequestHandler(socketserver.BaseRequestHandler):
    running_flag = False
    phone_list = []
    tested_phone_list = []

    def start_running(self):
        pass

    def log_handle(self, msg_dict):
        pass

    def cmd_handle(self, msg_dict):
        pass

    def heartbeat_handle(self, msg_dict):
        # cur_thread = threading.current_thread()
        logging.info("Received heartbeat packet from %s, the message content is: %s", self.client_address, msg_dict)

        if self.client_address not in ThreadedTCPRequestHandler.phone_list:
            ThreadedTCPRequestHandler.phone_list.append(self.client_address)

        if self.client_address not in ThreadedTCPRequestHandler.tested_phone_list:
            if not ThreadedTCPRequestHandler.running_flag:
                self.start_running()

    def handle(self):
        """
        data = str(self.request.recv(1024), 'ascii')
        cur_thread = threading.current_thread()
        response = bytes("{}: {}".format(cur_thread.name, data), 'ascii')
        self.request.sendall(response)
        :return:
        """
        req_str = self.request.recv(SOCKET_RECEIVE_BUFFER).decode(encoding='utf-8')

        try:
            json_str = json.loads(req_str)
        except BaseException as Err:
            logging.error(Err)
        else:
            msg_type = json_str['type']
            # msg_content = json_str['message']

            if 'cmd' == msg_type:
                self.cmd_handle(json_str)
            elif 'log' == msg_type:
                self.log_handle(json_str)
            elif 'heart' == msg_type:
                self.heartbeat_handle(json_str)
            else:
                pass

    def finish(self):
        logging.debug("end socket connect with client ip: %s" % str(self.client_address))
        pass

    def setup(self):
        logging.debug("start socket connect with client ip: %s" % str(self.client_address))
        pass


class ThreadedTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    """
    This is because the previous execution has left the socket in a TIME_WAIT state, and can’t be immediately reused.
    There is a socket flag to set, in order to prevent this, socket.SO_REUSEADDR:
    """
    socketserver.TCPServer.allow_reuse_address = True
    pass


class OpenSocketServer:
    def __init__(self, server_ip, server_port=20000):
        self.server_ip = server_ip
        self.server_port = server_port
        self.server_handle = None

    def open_socket_server(self, server_ip, server_port=20000):
        self.server_ip = server_ip
        self.server_port = server_port
        self.server_handle = ThreadedTCPServer((server_ip, server_port), ThreadedTCPRequestHandler)

        with self.server_handle:
            # Start a thread with the server -- that thread will then start one
            # more thread for each request
            server_thread = threading.Thread(target=self.server_handle.serve_forever)
            # Exit the server thread when the main thread terminates
            server_thread.daemon = True
            server_thread.start()
            logging.debug("Server loop running in thread:", server_thread.name)

    def close_socket_server(self):
        self.server_handle.server_close()
        self.server_handle.shutdown()


def client(ip, port, message):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock_client:
        # sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        sock_client.connect((ip, port))
        sock_client.sendall(bytes(message, 'ascii'))
        response = str(sock_client.recv(1024), 'ascii')
        logging.debug("Received: {}".format(response))
        logging.debug(socket.gethostbyname(socket.gethostname()),
                      socket.gethostbyaddr(socket.gethostname()), socket.gethostname())
        # sock.close()


def socket_connect_test():
    # Port 0 means to select an arbitrary unused port
    # host, port = "localhost", 0
    host, port = "172.16.49.142", 20000

    server = ThreadedTCPServer((host, port), ThreadedTCPRequestHandler)
    with server:
        ip, port = server.server_address

        logging.debug(ip, port)

        # Start a thread with the server -- that thread will then start one
        # more thread for each request
        server_thread = threading.Thread(target=server.serve_forever)
        # Exit the server thread when the main thread terminates
        server_thread.daemon = True
        server_thread.start()
        logging.debug("Server loop running in thread:", server_thread.name)

        client(ip, port, "Hello World 1")
        time.sleep(10)
        client(ip, port, "Hello World 2")
        time.sleep(10)
        client(ip, port, "Hello World 3")
        time.sleep(10)

        server.server_close()
        server.shutdown()


def main():
    logging_config(LOGGING_LEVEL)
    # socket_connect_test()

    run_all_tests(['ble_app_hrs'])


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal Elapsed Time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))
```

* ##### Socket Client

```
# -*- coding:utf-8 -*-

"""
@Author:        hogan.chen@ymail.com
@Create Date:   2018-12-18
@Update Date:   2018-12-19
@Version:       V0.1.20181219
"""

import time
import json
import socket
import random
import inspect
import logging
import datetime
import threading

MAX_THREAD_NUM = 1
SERVER_IP_ADDR = '127.0.0.1'
SERVER_IP_PORT = 5000
LUCK_NUM = 1000
PRINT_THREADING_LOCK = threading.Lock()

PHONE_SYSTEM_INFO_LIST = [
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 1', 'version': '1.1.1', 'imei': '123456789000001'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 2', 'version': '2.1.1', 'imei': '123456789000002'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 3', 'version': '3.1.1', 'imei': '123456789000003'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 4', 'version': '4.1.1', 'imei': '123456789000004'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 5', 'version': '5.1.1', 'imei': '123456789000005'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 6', 'version': '6.1.1', 'imei': '123456789000006'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 7', 'version': '7.1.1', 'imei': '123456789000007'},
    {'manufacturer': 'XiaoMi', 'model': 'XiaoMi 8', 'version': '8.1.1', 'imei': '123456789000008'},

    {'manufacturer': 'Honor', 'model': 'Honor-1', 'version': '1.1.1', 'imei': '123456789000011'},
    {'manufacturer': 'Honor', 'model': 'Honor-2', 'version': '2.1.1', 'imei': '123456789000012'},
    {'manufacturer': 'Honor', 'model': 'Honor-3', 'version': '3.1.1', 'imei': '123456789000013'},
    {'manufacturer': 'Honor', 'model': 'Honor-4', 'version': '4.1.1', 'imei': '123456789000014'},
    {'manufacturer': 'Honor', 'model': 'Honor-5', 'version': '5.1.1', 'imei': '123456789000015'},
    {'manufacturer': 'Honor', 'model': 'Honor-6', 'version': '6.1.1', 'imei': '123456789000016'},
    {'manufacturer': 'Honor', 'model': 'Honor-7', 'version': '7.1.1', 'imei': '123456789000017'},
    {'manufacturer': 'Honor', 'model': 'Honor-8', 'version': '8.1.1', 'imei': '123456789000018'},

    {'manufacturer': 'Vivo', 'model': 'Vivo X11', 'version': '1.1.1', 'imei': '123456789000021'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X12', 'version': '2.1.1', 'imei': '123456789000022'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X13', 'version': '3.1.1', 'imei': '123456789000023'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X14', 'version': '4.1.1', 'imei': '123456789000024'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X15', 'version': '5.1.1', 'imei': '123456789000025'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X16', 'version': '6.1.1', 'imei': '123456789000026'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X17', 'version': '7.1.1', 'imei': '123456789000027'},
    {'manufacturer': 'Vivo', 'model': 'Vivo X18', 'version': '8.1.1', 'imei': '123456789000028'},

    {'manufacturer': 'Oppo', 'model': 'Oppo F11', 'version': '1.1.1', 'imei': '123456789000031'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F12', 'version': '2.1.1', 'imei': '123456789000032'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F13', 'version': '3.1.1', 'imei': '123456789000033'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F14', 'version': '4.1.1', 'imei': '123456789000034'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F15', 'version': '5.1.1', 'imei': '123456789000035'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F16', 'version': '6.1.1', 'imei': '123456789000036'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F17', 'version': '7.1.1', 'imei': '123456789000037'},
    {'manufacturer': 'Oppo', 'model': 'Oppo F18', 'version': '8.1.1', 'imei': '123456789000038'},
]

LOG_MESSAGE = '{}: Log message {}'
RESULT_MESSAGE = [
    '{}: TEST RESULT: PASSED',
    '{}: TEST RESULT: FAILED',
    '{}: TEST RESULT: ERROR',
    '{}: TEST RESULT: SKIPPED',
]

frame = inspect.currentframe()

# log level
LOGGING_LEVEL = logging.DEBUG


def logging_config(logging_level):
    # log_format = '%(asctime)s - %(levelname)s - %(message)s'
    # log_format = '%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s'
    log_format = '[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s'
    logging.basicConfig(level=logging_level, format=log_format)


def connect(ip, port):
    while True:
        try:
            socket_handle = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            socket_handle.connect((ip, port))
        except Exception as err:
            print('Error message: {}'.format(err))
            time.sleep(5)
        else:
            break

    return socket_handle


def client_send(socket_handle, message):
    ret_value = True
    json_str = json.dumps(message)

    if socket_handle is not None:
        print('Send: {}'.format(json_str))
        try:
            socket_handle.sendall(json_str.encode(encoding='utf-8'))
            print('address: {}, hostname tripe: {}, hostname: {}'.format(socket.gethostbyname(socket.gethostname()),
                                                                         socket.gethostbyaddr(socket.gethostname()),
                                                                         socket.gethostname()))
        except Exception as err:
            print('Error message: {}'.format(err))
            ret_value = False

        # sock.close()

    return ret_value


def client_send_received(socket_handle, message):
    ret_value = True
    json_str = json.dumps(message)

    if socket_handle is not None:
        print('Send: {}'.format(json_str))
        try:
            socket_handle.sendall(json_str.encode(encoding='utf-8'))
            response = socket_handle.recv(1024).decode(encoding='utf-8')
            print('Received: {}'.format(response))
            print('address: {}, hostname tripe: {}, hostname: {}'.format(socket.gethostbyname(socket.gethostname()),
                                                                         socket.gethostbyaddr(socket.gethostname()),
                                                                         socket.gethostname()))
        except Exception as err:
            print('Error message: {}'.format(err))
            ret_value = False

        # sock.close()

    return ret_value


def client_received(socket_handle):
    response = None

    if socket_handle is not None:
        response = socket_handle.recv(1024).decode(encoding='utf-8')
        print('Received: {}'.format(response))
        print('address: {}, hostname tripe: {}, hostname: {}'.format(socket.gethostbyname(socket.gethostname()),
                                                                     socket.gethostbyaddr(socket.gethostname()),
                                                                     socket.gethostname()))

    return response


def run_socket_client():
    ip, port = '172.16.53.55', 5000
    sys_message = {'type': 'info', 'category': 'phone',
                   'message': {'manufacturer': 'XiaoMi', 'model': 'MI 6', 'version': '8.1.1',
                               'imei': '201808020957129'}}
    fpga_cmd_message = {'type': 'cmd', 'category': 'fpga', 'message': 'AA 00 00'}
    hb_message = {'type': 'info', 'category': 'heart', 'message': 'heart message'}
    log_message = {'type': 'log', 'category': 'log', 'message': ''}
    result_message = {'type': 'log', 'category': 'result', 'message': 'Test Result: PASSED'}

    socket_handle = connect(ip, port)
    test_count = 3

    client_send(socket_handle, sys_message)
    time.sleep(5)

    count = 5
    received_msg = None

    while True:
        send_ret = client_send(socket_handle, hb_message)
        if not send_ret:
            break

        received_json_msg = client_received(socket_handle)
        if received_json_msg is None or not received_json_msg:
            break
        else:
            received_msg = json.loads(received_json_msg)

        if 'cmd' == received_msg['type']:
            break
        else:
            time.sleep(5)

    while True:
        if 'cmd' == received_msg['type'] and 'EE 00 00 00' == received_msg['message']['command']:
            client_send(socket_handle, fpga_cmd_message)
            time.sleep(2)
            while count > 0:
                message_content = 'log message {}'.format(count)
                log_message['message'] = message_content
                client_send(socket_handle, log_message)
                time.sleep(5)
                count -= 1

            client_send(socket_handle, fpga_cmd_message)
            time.sleep(2)
            client_send(socket_handle, result_message)
            test_count -= 1

        if test_count <= 0:
            break
        else:
            count = 5

        time.sleep(5)
        # count -= 1

        received_json_msg = client_received(socket_handle)
        if received_json_msg is None or not received_json_msg:
            break
        else:
            received_msg = json.loads(received_json_msg)

    try:
        socket_handle.shutdown(socket.SHUT_RDWR)
        socket_handle.close()
    except Exception as err:
        print('Error message: {}'.format(err))


def debug_print(msg):
    datetime_str = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S.%f')
    print_msg = 'Debug Information: {} -> {}'.format(datetime_str, msg)

    PRINT_THREADING_LOCK.acquire()
    print(print_msg)
    PRINT_THREADING_LOCK.release()


class SocketClientHandler(threading.Thread):
    def __init__(self, thread_index, server_ip, server_port):
        super().__init__()
        self.thread_index = thread_index
        self.server_ip = server_ip
        self.server_port = server_port

        self.connect_flag = False
        self.socket_handle = None

    def socket_connect(self):
        time.sleep(random.randint(2, 20))

        while True:
            try:
                self.socket_handle = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                self.socket_handle.connect((self.server_ip, self.server_port))
                # self.socket_handle.setblocking(False)
                self.socket_handle.settimeout(5)
            except Exception as err:
                logging.debug('Error Happened[{}, {}, {}]: {}'.
                              format(self.name, self.thread_index, frame.f_lineno, err))
                time.sleep(random.randint(1, 5))
            else:
                self.connect_flag = True
                break

    def send_message_to_server(self, message):
        json_str = json.dumps(message)

        if self.socket_handle is not None:
            logging.debug('Send Message[{}, {}, {}]: {}'.
                          format(self.name, self.thread_index, frame.f_lineno, json_str))

            try:
                self.socket_handle.sendall(json_str.encode(encoding='utf-8'))
                logging.debug('address: {}, hostname tripe: {}, hostname: {}'.
                              format(socket.gethostbyname(socket.gethostname()),
                                     socket.gethostbyaddr(socket.gethostname()),
                                     socket.gethostname()))
            except Exception as err:
                logging.debug('Error Happened[{}, {}, {}]: {}'.
                              format(self.name, self.thread_index, frame.f_lineno, err))
                self.connect_flag = False

    def send_message(self, msg_type, msg_category, msg_str):
        cmd_message = {'type': msg_type, 'category': msg_category, 'message': msg_str}
        self.send_message_to_server(cmd_message)

    def received_message_from_server(self):
        response = None

        if self.socket_handle is not None:
            try:
                response = self.socket_handle.recv(1024).decode(encoding='utf-8')
                logging.debug('Received Message[{}, {}, {}]: {}'.
                              format(self.name, self.thread_index, frame.f_lineno, response))
                logging.debug('address: {}, hostname tripe: {}, hostname: {}'.
                              format(socket.gethostbyname(socket.gethostname()),
                                     socket.gethostbyaddr(socket.gethostname()),
                                     socket.gethostname()))
            except socket.timeout as err:
                logging.debug('Error Happened[{}, {}, {}]: {}'.
                              format(self.name, self.thread_index, frame.f_lineno, err))
            except socket.error as err:
                logging.debug('Error Happened[{}, {}, {}]: {}'.
                              format(self.name, self.thread_index, frame.f_lineno, err))
                self.connect_flag = False

        return response

    def received_handle(self):
        log_message_count = random.randint(50, 100)
        log_message_index = 0
        start_test_flag = False

        while True:
            if not self.connect_flag:
                break

            # send heart message
            self.send_message('info', 'heart', 'heart message')
            time.sleep(5)

            response = self.received_message_from_server()

            if response is not None:
                try:
                    json_str = json.loads(response)
                except BaseException as err:
                    logging.debug('Error Message[{}, {}, {}]: json load error with "{}" error'.
                                  format(self.name, self.thread_index, frame.f_lineno, err))
                    break
                else:
                    msg_type = json_str['type']

                    if 'cmd' == msg_type:
                        start_test_flag = True
                    elif 'info' == msg_type:
                        logging.debug('Info Message[{}, {}, {}]: {}'.
                                      format(self.name, self.thread_index, frame.f_lineno, json_str))
                    else:
                        logging.debug('Incorrect Message[{}, {}, {}]: {}'.
                                      format(self.name, self.thread_index, frame.f_lineno, json_str))

                # random to end the connection
                if LUCK_NUM == random.randint(0, 10):
                    logging.debug('disconnect with the server[{}, {}]'.format(self.name, self.thread_index))
                    self.socket_handle.close()
                    self.socket_handle = None
                    break

                # start to send log message
                if start_test_flag:
                    if log_message_index < log_message_count:
                        self.send_message('log', 'log', LOG_MESSAGE.
                                          format('[{}, {}]'.format(self.name, self.thread_index),
                                                 log_message_index))
                        log_message_index += 1
                        time.sleep(1)
                    else:
                        self.send_message('log', 'result', random.choice(RESULT_MESSAGE))
                        break
            else:
                pass
                # break

    def run(self):
        while True:
            if not self.connect_flag:
                self.socket_connect()

                # send device information to server
                # self.send_message('info', 'phone', PHONE_SYSTEM_INFO_LIST[self.thread_index])
                device_info = random.choice(PHONE_SYSTEM_INFO_LIST)
                device_info['imei'] = random.randint(100000000000000, 999999999999999)
                self.send_message('info', 'phone', device_info)

                # random to end the connection
                if LUCK_NUM == random.randint(0, 10):
                    logging.debug('disconnect with the server[{}, {}]'.format(self.name, self.thread_index))
                    self.socket_handle.close()
                    self.socket_handle = None
                    # break

            # # send heart message
            # self.send_message('info', 'heart', 'heart message')
            # time.sleep(5)

            # # random to end the connection
            # if LUCK_NUM == random.randint(0, 10):
            #     logging.debug('disconnect with the server[{}, {}]'.format(self.name, self.thread_index))
            #     self.socket_handle.close()
            #     self.socket_handle = None
            #     # break

            self.received_handle()


def run_threading_socket_client():
    socket_thread_list = []

    for thread_index in range(MAX_THREAD_NUM):
        thread_handler = SocketClientHandler(thread_index, SERVER_IP_ADDR, SERVER_IP_PORT)
        socket_thread_list.append(thread_handler)

    for thread_index in range(MAX_THREAD_NUM):
        socket_thread_list[thread_index].start()

    for thread_index in range(MAX_THREAD_NUM):
        socket_thread_list[thread_index].join()


def run_socket_client_multi_times(times=10):
    run_time = times
    while run_time > 0:
        run_socket_client()
        run_time -= 1


def main():
    # run_socket_client_multi_times()

    run_threading_socket_client()


if __name__ == '__main__':
    print('Script start execution at {}'.format(str(datetime.datetime.now())))

    logging_config(LOGGING_LEVEL)

    time_start = time.time()
    main()

    print('\n\nTotal Elapsed Time: {} seconds'.format(time.time() - time_start))
    print('\nScript end execution at {}'.format(datetime.datetime.now()))
```



