### Socket编程示例01--Socket Client

```
# -*- coding:utf-8 -*-

"""
@Author:        clhon@qq.com
@Create Date:   2018-06-13
@Update Date:   2018-06-14
@Version:       V0.9.20180614
"""
import time
import socket
import random
import logging
import datetime
import threading


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


class RunSocketClient(threading.Thread):
    def __init__(self, server_address):
        super().__init__()
        self.server_address = server_address
        self.socket = None

    def run_socket_client(self):
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as self.socket:
            self.socket.connect(self.server_address)
            count = 0
            send_count = random.randint(MIN_SEND_TIME, MAX_SEND_TIME)

            while True:
                time.sleep(random.randint(MIN_SLEEP_INTERVAL, MAX_SLEEP_INTERVAL))
                send_data = 'Hello from socket client with threading {} ...'.format(self.name)
                try:
                    self.socket.sendall(send_data.encode())
                except ConnectionError as err:
                    debug_print('Error message: {}'.format(err))
                    self.socket.shutdown(socket.SHUT_RDWR)
                    self.socket.close()
                    break

                recv_data = self.socket.recv(1024)
                if recv_data:
                    debug_print('Received Msg: {}'.format(recv_data.decode()))
                count += 1

                if count > send_count:
                    self.socket.shutdown(socket.SHUT_RDWR)
                    self.socket.close()
                    break

    def run(self):
        self.run_socket_client()


def run_multi_threading_sock_client():
    socket_thread_lists = []

    for thread_index in range(MAX_THREAD_NUM):
        threads = RunSocketClient(SOCKET_SERVER_ADDRESS)
        socket_thread_lists.append(threads)

    for thread_index in range(len(socket_thread_lists)):
        socket_thread_lists[thread_index].start()

    for thread_index in range(len(socket_thread_lists)):
        socket_thread_lists[thread_index].join()


def main():
    logging_config(LOGGING_LEVEL)
    run_multi_threading_sock_client()


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal Elapsed Time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))
```



