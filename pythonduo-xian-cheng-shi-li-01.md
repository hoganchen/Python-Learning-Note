### Python多线程示例01

```
# -*- coding:utf-8 -*-
"""
@Author:    clhon@qq.com
@Date:      2018-06-12
@FileName:  threading_example_01.py
"""

import time
import datetime
import threading
import multiprocessing


Daemon_Thread_Flag = True
Main_Thread_Flag = True
Thread_Lock_Flag = True

thread_lock = threading.Lock()

# 未加锁的打印输出
'''
You are in ui_thread!
You are in working thread!
You are in working thread!
Exit the ui_thread!
You are in main loop!
You are in working thread!
 You are in main loop!
You are in main loop!
 You are in working thread!
You are in working thread!
 You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
event is set, exit work_thread!
exit main loop!
'''

# 加锁的打印输出
'''
You are in ui_thread!
You are in working thread!
Exit the ui_thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in main loop!
You are in main loop!
You are in working thread!
You are in main loop!
You are in working thread!
You are in working thread!
You are in main loop!
event is set, exit work_thread!
exit main loop!
'''


def debug_print(msg):
    global thread_lock

    if Thread_Lock_Flag:
        thread_lock.acquire()
        print(msg)
        thread_lock.release()
    else:
        print(msg)


def work_thread(event):
    while True:
        # print('You are in working thread!')
        debug_print('You are in working thread, thread name is: {}'.format(threading.current_thread().name))
        time.sleep(2)

        if event.is_set():
            debug_print('event is set, exit work_thread!')
            break


def ui_thread(event):
    debug_print('You are in ui_thread, thread name is: {}'.format(threading.current_thread().getName()))

    t = threading.Thread(target=work_thread, args=(event, ))

    '''
    如果主线程准备退出时,不需要等待某些子线程完成,就可以为这些子线程设置守护
    线程标记。该标记值为真时,表示该线程是不重要的,或者说该线程只是用来等待客户端
    请求而不做任何其他事情。
    如果是子线程再去创建线程，创建的线程跟子线程也是平级关系，没有父子关系，然后子线程退出，创建的线程也不会结束，
    当设置守护线程，主线程退出，子线程就会结束，如果没有设置守护线程，主线程退出，子线程会继续执行直到自身结束
    '''
    if Daemon_Thread_Flag:
        '''
        要将一个线程设置为守护线程,需要在启动线程之前执行如下赋值语句:
        thread.daemon = True(调用 thread.setDaemon(True)的旧方法已经弃用了)
        '''
        # t.setDaemon(True)
        t.daemon = True

    '''
    https://blog.csdn.net/chenpkai/article/details/70943609
    https://blog.csdn.net/tornado886/article/details/4524346
    http://www.cnblogs.com/i-honey/p/8043648.html

    start() 方法是启动一个子线程，线程名就是我们定义的name
    run() 方法并不启动一个新线程，就是在主线程中调用了一个普通函数而已。

    因此，如果你想启动多线程，就必须使用start()方法。
    '''
    t.start()
    # t.run()

    time.sleep(20)
    debug_print('Exit the ui_thread!')


def main():
    print('Main function is working on {}'.format(threading.current_thread().name))
    print('Main function is working on {}'.format(multiprocessing.current_process().name))

    counter = 0
    ui_threads = None
    event = threading.Event()
    '''
    由于ui_thread函数的作用只是另外启动了一个work_thread线程，所以具体以函数的方式调用和
    以线程的方式调用没有太大关系，ui_thread函数会很快执行结束
    '''
    if True:
        '''
        https://my.oschina.net/D7Space/blog/1526970
        http://ayuliao.com/2017/07/17/python%E5%A4%9A%E7%BA%BF%E7%A8%8B%E3%80%81%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%92%8CGIL/
        
        学过Python的人应该都知道，Python是支持多线程的，并且是native的线程。本文主要是通过threading这个模块来实现多线程的，
        threading模块是对thread模块的二次封装，提供了更方便的API来操作线程 。目前Python对线程的支持还不够完善，不能利用多CPU，
        所以本文概述的都是进程下的多线程。注意：线程中没有父子关系，一个进程中可以包含N个线程，每个线程之间都是平等关系。
        '''
        ui_threads = threading.Thread(target=ui_thread, args=(event, ))
        # 由于ui_thread的作用只是另外启动一个work_thread线程后就结束了，并没有循环执行或者等待某个事件，所以是否设置守护进程不会影响结果
        # 但是如果ui_thread的执行时间过长，比主线程的执行时间长，最好还是设置守护进程
        ui_threads.daemon = True
        ui_threads.start()
    else:
        ui_thread(event)

    if Main_Thread_Flag:
        while True:
            if ui_threads is not None:
                debug_print('ui_thread name: {}, is_live: {}'.format(ui_threads.getName(), ui_threads.is_alive()))

            debug_print('')
            debug_print('You are in main loop, thread name: {}, active thread count: {}, enumerate: {}'.
                        format(threading.current_thread().getName(), threading.active_count(), threading.enumerate()))
            time.sleep(2)
            counter += 1

            if counter >= 10:
                event.set()
                break

    debug_print('exit main loop!')

    if ui_threads.is_alive():
        ui_threads.join()

    time.sleep(2)
    debug_print('exit main func!')


if __name__ == "__main__":
    print('Script start execution at %s\n\n' % str(datetime.datetime.now()))
    time_start = time.time()

    main()

    print('\n\nScript end execution at %s' % str(datetime.datetime.now()))
    print('Total Elapsed Time: %s seconds\n' % (time.time() - time_start))

```



