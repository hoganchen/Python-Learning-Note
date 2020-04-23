### Python多线程示例02

```
# -*- coding:utf-8 -*-
"""
@Author:    clhon@qq.com
@Date:      2018-06-12
@FileName:  threading_example_02.py
"""

import time
import random
import logging
import datetime
import threading
import multiprocessing


THREAD_LOCK_FLAG = True
ThREAD_NUM = 20
MIN_RANDOM_MINUTES = 1
MAX_RANDOM_MINUTES = 10

THREAD_LOCK = threading.Lock()


# log level
LOGGING_LEVEL = logging.INFO


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


def debug_print(msg):
    global THREAD_LOCK

    if THREAD_LOCK_FLAG:
        THREAD_LOCK.acquire()
        print(msg)
        THREAD_LOCK.release()
    else:
        print(msg)


class MultiThreadHandle(threading.Thread):
    def __init__(self, sleep_time):
        threading.Thread.__init__(self)
        self.sleep_time = sleep_time
        self.count = 0

    def work_handle(self):
        while True:
            # threading.current_thread().getName(), threading.Thread.getName(self)和self.name都能获取当前线程的名字
            # threading.Thread.getName(self)和self.name是因为这个类是继承于threading.Thread，
            # 而threading.current_thread().getName()是获取当前线程的名字，可以在函数中使用

            # debug_print('Thread Name: {}, Message: {} with {}'.
            #             format(threading.current_thread().getName(), 'MultiThreadHandle is working', self.count))
            # debug_print('Thread Name: {}, Message: {} with {}'.
            #             format(threading.Thread.getName(self), 'MultiThreadHandle is working', self.count))
            debug_print('Thread Name: {}, Message: {} with {}'.
                        format(self.name, 'MultiThreadHandle is working', self.count))
            time.sleep(1)
            self.count += 1

            if self.count > self.sleep_time:
                break

    def run(self):
        self.work_handle()


def run_multi_thread_class():
    print('Start to run run_multi_thread_class function...')
    multi_thread_list = []

    for thread_index in range(ThREAD_NUM):
        threads = MultiThreadHandle(random.randint(MIN_RANDOM_MINUTES, MAX_RANDOM_MINUTES))
        multi_thread_list.append(threads)

    for thread_index in range(len(multi_thread_list)):
        multi_thread_list[thread_index].start()

    for thread_index in range(len(multi_thread_list)):
        multi_thread_list[thread_index].join()


def work_handle(sleep_time):
    count = 0

    while True:
        # debug_print('Thread Name: {}, Message: {} with {}'.
        #             format(threading.current_thread().getName(), 'work_handle is working', count))
        debug_print('Thread Name: {}, Message: {} with {}'.
                    format(threading.current_thread().name, 'work_handle is working', count))
        time.sleep(1)
        count += 1

        if count > sleep_time:
            break


def run_multi_thread_func():
    print('Start to run run_multi_thread_func function...')
    multi_thread_list = []

    for thread_index in range(ThREAD_NUM):
        threads = threading.Thread(target=work_handle, args=(random.randint(MIN_RANDOM_MINUTES, MAX_RANDOM_MINUTES), ))
        multi_thread_list.append(threads)

    for thread_index in range(len(multi_thread_list)):
        multi_thread_list[thread_index].start()

    for thread_index in range(len(multi_thread_list)):
        multi_thread_list[thread_index].join()


def main():
    print('Main function is working on {}'.format(threading.current_thread().name))
    print('Main function is working on {}'.format(multiprocessing.current_process().name))

    logging_config(LOGGING_LEVEL)

    run_multi_thread_func()
    print('\n\n--------------------------------------------------------------------------------')
    run_multi_thread_class()


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal elapsed time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))



```



