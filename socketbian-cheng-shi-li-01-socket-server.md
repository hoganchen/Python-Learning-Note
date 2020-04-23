### Socket编程示例01--Socket Server

```
# -*- coding:utf-8 -*-

"""
@Author:        clhon@qq.com
@Create Date:   2018-06-13
@Update Date:   2018-06-14
@Version:       V0.9.20180614
"""

import re
import time
import socket
import logging
import datetime
import threading
import socketserver

MAX_THREAD_NUM = 20

MIN_SLEEP_INTERVAL = 5
MAX_SLEEP_INTERVAL = 10

MIN_SEND_TIME = 5
MAX_SEND_TIME = 10

SOCKET_SERVER_ADDRESS = ('127.0.0.1', 5000)

THREAD_LOCK_FLAG = True
G_THREAD_LOCK = threading.Lock()

# log level
LOGGING_LEVEL = logging.INFO


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


def debug_print(msg):
    # global G_THREAD_LOCK

    if THREAD_LOCK_FLAG:
        G_THREAD_LOCK.acquire()
        print(msg)
        G_THREAD_LOCK.release()
    else:
        print(msg)


class ResponseRequestHandler(threading.Thread):
    def __init__(self, request, client_address):
        super().__init__()
        self.request = request
        self.client_address = client_address

    def response_request(self):
        while True:
            recv_data = self.request.recv(1024)
            if recv_data:
                debug_print('Received Msg: {}'.format(recv_data.decode()))
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
                debug_print('Error message: {} with ip address {}'.format(err, self.client_address))

                # socket.shutdown(how)
                # Shut down one or both halves of the connection. If how is SHUT_RD, further receives are disallowed.
                # If how is SHUT_WR, further sends are disallowed. If how is SHUT_RDWR,
                # further sends and receives are disallowed.

                # 如客户端已断开连接，这时调用socket.shutdown(socket.SHUT_RDWR)会报错，
                # 但是没有试验SHUT_RD或者SHUT_WR参数有没有问题，待验证
                # self.request.shutdown(socket.SHUT_RDWR)
                self.request.close()
                break

    def run(self):
        self.response_request()


def open_socket_server(server_address):
    client_thread_lists = []

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

        # Enable a server to accept connections. If backlog is specified,
        # it must be at least 0 (if it is lower, it is set to 0);
        # it specifies the number of unaccepted connections that the system will allow before refusing new connections.
        # If not specified, a default reasonable value is chosen.
        socket_server.listen(20)

        while True:
            request, client_address = socket_server.accept()
            debug_print('####################Connect from {}####################'.format(client_address))
            client_thread_lists.append(ResponseRequestHandler(request, client_address))
            client_thread_lists[-1].start()
            # client_thread_lists[-1].join()


def main():
    logging_config(LOGGING_LEVEL)
    open_socket_server(SOCKET_SERVER_ADDRESS)


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal Elapsed Time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))
```



