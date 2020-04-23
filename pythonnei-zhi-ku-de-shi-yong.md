### Python内置库的使用示例

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os
import time
import copy
import random
import logging
import threading
import subprocess
from multiprocessing import Pool, Process, Queue

# log level
LOGGING_LEVEL = logging.DEBUG


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[File: %(filename)s line: %(lineno)d] - %(levelname)s - %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)

logging_config(LOGGING_LEVEL)


title = '''
###########################################################
# 关于操作 Python 列表最常见的10个问答
# https://foofish.net/python-list-top10.html
###########################################################
'''
print('\n\n{}'.format(title))

# 迭代列表时如何访问列表下标索引
items = [8, 23, 45]

# 普通版
for index in range(len(items)):
    logging.debug('{} {} {}'.format(index, "-->", items[index]))

# 优雅版
for index, item in enumerate(items):
    logging.debug('{} {} {}'.format(index, "-->", item))

for index, item in enumerate(items, start=1):
    logging.debug('{} {} {}'.format(index, "-->", item))

# append 与 extend 方法有什么区别
x = [1, 2, 3]
y = [4, 5]

x.append(y)
logging.debug('x append y is: {}'.format(x))

x = [1, 2, 3]
y = [4, 5]

x.extend(y)
logging.debug('x extend y is: {}'.format(x))

# 检查列表是否为空
# 普通版
if len(items) == 0:
    print("空列表")

if items == []:
    print("空列表")

# 优雅版
if not items:
    print("空列表")

# 如何理解切片
'''
切片用于获取列表中指定范的子集，语法非常简单

items[start:end:step]

从 start 到 end-1 位置之间的元素。step 表示步长，默认为1，表示连续获取，如果 step 为 2 就表示每隔一个元素获取。
'''
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

logging.debug('{}'.format(a[3:8])) # 第3到第8位置之间的元素
# [4, 5, 6, 7, 8]

logging.debug('{}'.format(a[3:8:2])) # 第3到第8位置之间的元素，每隔一个元素获取
# [4, 6, 8]

logging.debug('{}'.format(a[:5])) # 省略start表示从第0个元素开始
# [1, 2, 3, 4, 5]

logging.debug('{}'.format(a[3:])) # 省略end表示到最后一个元素
# [4, 5, 6, 7, 8, 9, 10]

logging.debug('{}'.format(a[::])) # 都省略相当于拷贝一个列表，这种拷贝属于浅拷贝
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

logging.debug('{}'.format(a[:])) # 都省略相当于拷贝一个列表，这种拷贝属于浅拷贝
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 如何拷贝一个列表对象
# 第一种方法，浅拷贝
a = [1, 2, 3]
b = {'a':1, 'b':2, 'c':3}
old_list = [a, b, 4, 5, 6]
new_list = old_list[:]
logging.debug('id(a): {}, id(b): {}'.format(id(a), id(b)))
logging.debug('id(old_list): {}, id(old_list[0]): {}, id(old_list[1]): {}'.format(id(old_list), id(old_list[0]), id(old_list[1])))
logging.debug('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))

# 第二种方法，浅拷贝
new_list = list(old_list)
logging.debug('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))

# 第三种方法：
# 浅拷贝
new_list = copy.copy(old_list)
logging.debug('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))
# 深拷贝
new_list = copy.deepcopy(old_list)
logging.debug('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))

# 如何获取列表中的最后一个元素
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
logging.debug('The last element: {}'.format(a[len(a)-1]))
# 10
logging.debug('The last element: {}'.format(a[-1]))
# 10

# 如何对列表进行排序
items = [{'name': 'Homer', 'age': 39},
         {'name': 'Bart', 'age': 10},
         {"name": 'cater', 'age': 20}]

# 升序
items.sort(key=lambda item: item.get("age"))
logging.debug(items)

# 降序
items.sort(key=lambda item: item.get("age"), reverse=True)
logging.debug(items)

# 如果不希望改变原列表，而是生成一个新的有序列表对象，那么可以内置函数 sorted ，该函数返回新列表
items = [{'name': 'Homer', 'age': 39},
         {'name': 'Bart', 'age': 10},
         {"name": 'cater', 'age': 20}]

new_items = sorted(items, key=lambda item: item.get("age"))
logging.debug(items)
logging.debug(new_items)

# 如何移除列表中的元素
a = [0, 1, 2, 3, 4, 5, 6, 7, 8]
a.remove(random.choice(a))
logging.debug(a)

del a[0]
logging.debug(a)

x = a.pop(3)
logging.debug(a)

a.pop()
logging.debug(a)

# 如何连接两个列表
listone = [1, 2, 3]
listtwo = [4, 5, 6]

mergedlist = listone + listtwo

logging.debug(mergedlist)

# 列表实现了 + 的运算符重载，使得 + 不仅支持数值相加，还支持两个列表相加，只要你实现了 对象的 __add__操作，任何对象都可以实现 + 操作
class User(object):
    def __init__(self, age):
        self.age = age

    def __repr__(self):
        return 'User(%d)' % self.age

    def __add__(self, other):
        age = self.age + other.age
        return User(age)


user_a = User(10)
user_b = User(20)

c = user_a + user_b

logging.debug(c)

# 如何随机获取列表中的某个元素
# import random
items = [8, 23, 45, 12, 78]
logging.debug(random.choice(items))

# 字典访问
d = {'x':'A', 'y':'B', 'z':'C'}

for k in d:
    print(k, end=' ')

print()

for k in d.keys():
    print(k, end=' ')

print()

for v in d.values():
    print(v, end=' ')

print()

for k, v in d.items():
    print(k, '==>', v, end=' ')

print()

logging.debug('d[f] = {}'.format(d.get('f', None)))
logging.debug('f in d: {}'.format('f' in d))

'''
使用 iteritems() 迭代大数据

迭代大数据字典时，如果是使用 items() 方法，那么在迭代之前，迭代器迭代前需要把数据完整地加载到内存，
这种方式不仅处理非常慢而且浪费内存，下面代码约占1.6G内存（为什么是1.6G?可以参考：http://stackoverflow.com/questions/4279358/pythons-underlying-hash-data-structure-for-dictionaries）

d = {i: i * 2 for i in range(10000000)}
for key, value in d.iteritem():
    print("{0} = {1}".format(key, value))
'''

# 高效合并字典
x = {'a': 1, 'b': 2}
y = {'b': 3, 'c': 4}
z = {**x, **y}

logging.debug('z: {}'.format(z))


title = '''
###########################################################
# Python陷阱：为什么不能用可变对象作为默认参数的值
# https://foofish.net/python-tricks.html
###########################################################
'''
print('\n\n{}'.format(title))

def bad_append(item, item_list=[]):
    item_list.append(item)

    return item_list

def right_append(item, item_list=None):
    if item_list is None:
        item_list = []

    item_list.append(item)

    return item_list

item_list = []
item_list = bad_append('one')
logging.debug('id(item_list): {}, item_list: {}'.format(id(item_list), item_list))
item_list = bad_append('two')
logging.debug('id(item_list): {}, item_list: {}'.format(id(item_list), item_list))

item_list = right_append('one')
logging.debug('id(item_list): {}, item_list: {}'.format(id(item_list), item_list))
item_list = right_append('two')
logging.debug('id(item_list): {}, item_list: {}'.format(id(item_list), item_list))

items_list = list(range(0, 10))
logging.debug('type(items_list): {}'.format(type(items_list)))
bad_append('one', items_list)
logging.debug('id(items_list): {}, items_list: {}'.format(id(items_list), items_list))
bad_append('two', items_list)
logging.debug('id(items_list): {}, items_list: {}'.format(id(items_list), items_list))

right_append('one', items_list)
logging.debug('id(items_list): {}, items_list: {}'.format(id(items_list), items_list))
right_append('two', items_list)
logging.debug('id(items_list): {}, items_list: {}'.format(id(items_list), items_list))

for item in range(10):
    print(item, end= ' ')

print()

for item in list(range(10)):
    print(item, end= ' ')

print()

logging.debug('{}'.format({'name':'daxiang', 'age':36, 'title':'sr engineer'}.keys()))
logging.debug('{}'.format({'name':'daxiang', 'age':36, 'title':'sr engineer'}.values()))
logging.debug('{}'.format({'name':'daxiang', 'age':36, 'title':'sr engineer'}.items()))

for key, value in {'name':'daxiang', 'age':36, 'title':'sr engineer'}.items():
    logging.debug('{} --> {}'.format(key, value))


title = '''
###########################################################
可迭代对象需要实现__iter__方法，并返回一个迭代器，什么是迭代器呢？迭代器只需要实现 __next__方法。现在我们就来验证一下列表为什么支持迭代：
>>> x = [1,2,3]
>>> its = x.__iter__() # x有此方法，说明列表是可迭代对象
>>> its
<list_iterator object at 0x100f32198>

>>> its.__next__()  # its有此方法，说明its是迭代器
1
>>> its.__next__()
2
>>> its.__next__()
3
>>> its.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration

从试验结果来看，列表是一个可迭代对象，因为它实现了 __iter__方法，并且返回了一个迭代器对象（list_iterator），因为它实现了 __next__方法。我们看到它不断地调用__next__方法，其实就是不断地迭代获取容器中的元素，直到容器中没有更多元素抛出 StopIteration 异常为止。（在Python2中，实现迭代器变成了没有下划线的 next 方法）

那么 for 语句又是如何循环的呢？到这里，恐怕你也猜到了，它的步骤是：

    先判断对象是否为可迭代对象，不是的话直接报错，抛出TypeError异常，是的话，调用 __iter__方法，返回一个迭代器
    不断地调用迭代器的__next__方法，每次按序返回迭代器中的一个值
    迭代到最后，没有更多元素了，就抛出异常 StopIteration，这个异常 python 自己会处理，不会暴露给开发者
https://foofish.net/how-for-works-in-python.html
###########################################################
'''
print('\n\n{}'.format(title))

class MyRange:
    def __init__(self, num):
        self.i = 0
        self.num = num

    def __iter__(self):
        return self

    def __next__(self):
        if self.i < self.num:
            i = self.i
            self.i += 1
            return i
        else:
            # 达到某个条件时必须抛出此异常，否则会无止境地迭代下去
            raise StopIteration()

range_list = list(MyRange(10))

logging.debug('range_list: {}'.format(range_list))

range_list = MyRange(10)

logging.debug('range_list: {}'.format(range_list))

for item in range_list:
    print(item, end=' ')

print()


title = '''
###########################################################
# Python3.6引入了类型信息，定义变量的时候可以指定类型
# https://foofish.net/dynamic_type_and_duck_type.html
###########################################################
'''
print('\n\n{}'.format(title))

def greeting(name: str) -> str:
    return 'Hello ' + name

logging.debug('string: {}'.format(greeting('daxiang')))


title = '''
###########################################################
# Python 表达式 i += x 与 i = i + x 等价吗？
# https://foofish.net/iadd_add.html

+= 操作首先会尝试调用对象的 __iadd__方法，如果没有该方法，那么尝试调用__add__方法，先来看看这两个方法有什么区别
_add_ 和 _iadd_ 的区别

    _add_ 方法接收两个参数，返回它们的和，两个参数的值均不会改变。
    _iadd_ 方法同样接收两个参数，但它是属于 in-place 操作，就是说它会改变第一个参数的值，因为这需要对象是可变的，所以对于不可变对象没有__iadd__方法。

显然，整数对象是没有__iadd__的，而列表对象提供了__iadd__方法。
###########################################################
'''
print('\n\n{}'.format(title))

l1 = list(range(3))
l2 = l1
l2 += [3]
logging.debug('id(l1): {}, id(l2): {}, l1: {}, l2: {}'.format(id(l1), id(l2), l1, l2))

l1 = list(range(3))
l2 = l1
l2 = l2 + [3]
logging.debug('id(l1): {}, id(l2): {}, l1: {}, l2: {}'.format(id(l1), id(l2), l1, l2))


title = '''
###########################################################
# 多线程，互斥锁
# http://yangcongchufang.com/%E9%AB%98%E7%BA%A7python%E7%BC%96%E7%A8%8B%E5%9F%BA%E7%A1%80/python-process-thread.html
###########################################################
'''
print('\n\n{}'.format(title))

# 来看看多个线程同时操作一个变量怎么把内容给改乱了
balance = 0

def change_it(n):
    # 先存后取，结果应该是0
    global balance
    balance = balance + n
    balance = balance - n
    # print('balance: {}'.format(balance))

def run_thread(n):
    time.sleep(random.random())
    for i in range(10000000):
        change_it(n)

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t3 = threading.Thread(target=run_thread, args=(12,))
t1.start()
t2.start()
t3.start()
t1.join()
t2.join()
t3.join()
logging.debug('balance: {}'.format(balance))

# 互斥锁
balance = 0

def change_it(n):
    # 先存后取，结果应该是0
    global balance

    # threadLock.acquire()
    balance = balance + n
    balance = balance - n
    # print('balance: {}'.format(balance))
    # threadLock.release()

def run_thread(n):
    time.sleep(random.random())
    threadLock.acquire()

    for i in range(10000000):
        change_it(n)

    threadLock.release()

threadLock = threading.Lock()

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t3 = threading.Thread(target=run_thread, args=(12,))
t1.start()
t2.start()
t3.start()
t1.join()
t2.join()
t3.join()
logging.debug('balance: {}'.format(balance))

# http://blog.csdn.net/sunhuaqiang1/article/details/70168015
# http://www.runoob.com/python/python-multithreading.html
class myThread (threading.Thread):
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter
    def run(self):
        logging.debug("Starting " + self.name)
       # 获得锁，成功获得锁定后返回True
       # 可选的timeout参数不填时将一直阻塞直到获得锁定
       # 否则超时后将返回False
        threadLock.acquire()
        print_time(self.name, self.counter, 3)
        # 释放锁
        threadLock.release()

def print_time(threadName, delay, counter):
    while counter:
        time.sleep(delay)
        logging.debug("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1

threadLock = threading.Lock()
threads = []

# 创建新线程
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)

# 开启新线程
thread1.start()
thread2.start()

# 添加线程到线程列表
threads.append(thread1)
threads.append(thread2)

# 等待所有线程完成
for t in threads:
    t.join()
logging.debug("Exiting Main Thread")


title = '''
###########################################################
# 子进程
# https://docs.python.org/3/library/subprocess.html
# http://everettjf.com/2016/01/29/python27-subprocess-timeout/
# https://stackoverflow.com/questions/1191374/using-module-subprocess-with-timeout
# https://www.blog.pythonlibrary.org/2016/05/17/python-101-how-to-timeout-a-subprocess/
###########################################################
'''
print('\n\n{}'.format(title))

class Command(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout):
        if True:
            '''
            The recommended approach to invoking subprocesses is to use the run() function for all use cases it can handle. For more advanced use cases, the underlying Popen interface can be used directly.
            The run() function was added in Python 3.5; if you need to retain compatibility with older versions, see the Older high-level API section.
            subprocess.run(args, *, stdin=None, input=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None)
            '''
            try:
                self.process = subprocess.run(self.cmd, shell=False, timeout=timeout)
            except Exception as err:
                logging.debug('error message: {}'.format(err))
            finally:
                # self.process is already define in init function, otherwise please use 'if "self.process" in locals().keys()' to instead it
                if self.process is not None:
                    logging.debug('run stdout: {}'.format(self.process.stdout))

            '''
            17.5.5. Older high-level API
            Prior to Python 3.5, these three functions comprised the high level API to subprocess. You can now use run() in many cases, but lots of existing code calls these functions.
            subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None)
            This is equivalent to:
            run(...).returncode

            subprocess.check_call(args, *, stdin=None, stdout=None, stderr=None, shell=False, cwd=None, timeout=None)
            This is equivalent to:
            run(..., check=True)

            subprocess.check_output(args, *, stdin=None, stderr=None, shell=False, cwd=None, encoding=None, errors=None, universal_newlines=False, timeout=None)
            This is equivalent to:
            run(..., check=True, stdout=PIPE).stdout
            '''
            try:
                self.process = subprocess.call(self.cmd, shell=False, timeout=timeout)
            except Exception as err:
                logging.debug('error message: {}'.format(err))

            try:
                self.process = subprocess.check_call(self.cmd, shell=False, timeout=timeout)
            except Exception as err:
                logging.debug('error message: {}'.format(err))

            try:
                # cmd超时退出，而不是正常退出，没有stdout，即self.process变量为None类型
                '''
                >>> ttval = subprocess.check_output((['ping', '127.0.0.1', '-c', '10']), shell=False, timeout=5)
                Traceback (most recent call last):
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 405, in run
                    stdout, stderr = process.communicate(input, timeout=timeout)
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 836, in communicate
                    stdout, stderr = self._communicate(input, endtime, timeout)
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 1497, in _communicate
                    self._check_timeout(endtime, orig_timeout)
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 864, in _check_timeout
                    raise TimeoutExpired(self.args, orig_timeout)
                subprocess.TimeoutExpired: Command '['ping', '127.0.0.1', '-c', '10']' timed out after 5 seconds

                During handling of the above exception, another exception occurred:

                Traceback (most recent call last):
                File "<stdin>", line 1, in <module>
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 336, in check_output
                    **kwargs).stdout
                File "/home/chenlianghong/anaconda3/lib/python3.6/subprocess.py", line 410, in run
                    stderr=stderr)
                subprocess.TimeoutExpired: Command '['ping', '127.0.0.1', '-c', '10']' timed out after 5 seconds
                >>> type(ttval)
                Traceback (most recent call last):
                File "<stdin>", line 1, in <module>
                NameError: name 'ttval' is not defined
                >>>
                '''
                self.process = subprocess.check_output(self.cmd, shell=False, timeout=timeout)
            except Exception as err:
                logging.debug('error message: {}'.format(err))
            finally:
                # self.process is already define in init function, otherwise please use 'if "self.process" in locals().keys()' to instead it
                if self.process is not None:
                    logging.debug('check_output: {}'.format(self.process))
        else:
            def target():
                logging.debug('Thread started')

                # process.terminate() doesn't work when using shell=True. This answer will help you
                # https://stackoverflow.com/questions/4084322/killing-a-process-created-with-pythons-subprocess-popen
                self.process = subprocess.Popen(self.cmd, shell=False)
                # self.process = subprocess.Popen(self.cmd, shell=False, stdout=subprocess.PIPE, stdin=subprocess.PIPE,
                #                                 stderr=subprocess.PIPE)

                # while True:
                #     line = self.process.stdout.readline()
                #     if not line:
                #         break
                #     print(line)

                self.process.communicate(timeout=5)

                logging.debug('Thread finished')

            if True:
                target()
                self.process.terminate()
            else:
                thread = threading.Thread(target=target)
                thread.start()

                thread.join(timeout)

                if thread.is_alive():
                    logging.debug('Terminating process')

                    self.process.terminate()
                    # self.process.kill()

                    thread.join()

                logging.debug('return code: {}'.format(self.process.returncode))

# run_command = Command('ping 127.0.0.1') # No such file or directory error
run_command = Command(['ping', '127.0.0.1', '-c', '100'])
run_command.run(timeout=5)


title = '''
###########################################################
# 闭包
闭包本身是一个晦涩难懂的概念，它可以专门单独用一篇文章来介绍，不过在这里我们可以简单粗暴地理解为闭包就是一个定义在函数内部的函数，闭包使得变量即使脱离了该函数的作用域范围也依然能被访问到。
###########################################################
'''
print('\n\n{}'.format(title))

def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    logging.debug(locals())
    return fs

f1, f2, f3 = count()
logging.debug('f1():{}, f2(): {}, f3():{}'.format(f1(), f2() ,f3()))
logging.debug(f1.__closure__[0].cell_contents) # 打印闭包值 即i的值 =3

def newcount():
    fs = []
    for i in range(1, 4):
        def f(j=i):
             return j*j
        fs.append(f)
    logging.debug(locals())
    return fs

f1, f2, f3 = newcount()
logging.debug('f1():{}, f2(): {}, f3():{}'.format(f1(), f2() ,f3()))
logging.debug(f1.__closure__)


title = '''
###########################################################
# 装饰器
# https://foofish.net/decorator.html
解析过程是这样子的：
1.python 解释器发现@dobi，就去调用与其对应的函数（ dobi 函数）
2.dobi 函数调用前要指定一个参数，传入的就是@dobi下面修饰的函数，也就是 qinfeng()
3.dobi() 函数执行，调用 qinfeng()，qinfeng() 打印“dobi”
###########################################################
'''
print('\n\n{}'.format(title))

def doubi(qf):
    logging.debug('Decorator')
    return qf

@doubi
def qinfeng():
    logging.debug('doubi')

qinfeng()

# 简单装饰器
'''
函数use_logging就是装饰器，它把真正的业务方法func包裹在函数里面，看起来像bar被use_logging装饰了。
在这个例子中，函数进入和退出时 ，被称为一个横切面(Aspect)，这种编程方式被称为面向切面的编程(Aspect-Oriented Programming)。

@符号是装饰器的语法糖，在定义函数的时候使用，避免再一次赋值操作
'''
def use_logging(func):

    def wrapper(*args, **kwargs):
        logging.warn("%s is running" % func.__name__)
        return func(*args, **kwargs)
    return wrapper

def bar():
    print('i am bar')

bar = use_logging(bar)
bar()

@use_logging
def foo():
    print('i am foo')

foo()

'''
装饰器还有更大的灵活性，例如带参数的装饰器：在上面的装饰器调用中，比如@use_logging，该装饰器唯一的参数就是执行业务的函数。
装饰器的语法允许我们在调用时，提供其它参数，比如@decorator(a)。这样，就为装饰器的编写和使用提供了更大的灵活性。

下面的use_logging_with_param是允许带参数的装饰器。它实际上是对原有装饰器的一个函数封装，并返回一个装饰器。
我们可以将它理解为一个含有参数的闭包。当我 们使用@use_logging(level="warn")调用的时候，Python能够发现这一层的封装，并把参数传递到装饰器的环境中。
'''
def use_logging_with_param(level):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if level == "warn":
                logging.warn("%s is running" % func.__name__)
            return func(*args)
        return wrapper

    return decorator

@use_logging_with_param(level="warn")
def foo_param(name='foo_param'):
    print("i am %s" % name)

foo_param()

'''
内置装饰器

@staticmathod、@classmethod、@property
装饰器的顺序

@a
@b
@c
def f ():

等效于

f = a(b(c(f)))
'''


title = '''
###########################################################
# 多进程
# http://yangcongchufang.com/%E9%AB%98%E7%BA%A7python%E7%BC%96%E7%A8%8B%E5%9F%BA%E7%A1%80/python-process-thread.html
###########################################################
'''
print('\n\n{}'.format(title))

'''
创建子进程时，只需要传入一个执行函数和函数的参数，创建一个Process实例，用start()方法启动，这样创建进程比fork()还要简单。

join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步。
'''
def run_proc(name):
    logging.debug('Run child process %s (%s)' % (name, os.getpid()))

logging.debug('Parent process %s.' % os.getpid())
p = Process(target=run_proc, args=('test_code',))
logging.debug('Child process will start.')
p.start()
p.join()
logging.debug('Child process end.')

'''
如果要启动大量的子进程，可以用进程池的方式批量创建子进程

对Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了。

注意输出的结果，task0，1，2，3是立刻执行的，而task4要等待前面某个task完成后才执行，这是因为Pool的默认大小设置成了4(p = Pool(4))，代表着最多同时执行4个进程。这是Pool有意设计的限制，并不是操作系统的限制。
'''
def long_time_task(name):
    logging.debug("Run task %s (%s)..." % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    logging.debug("Task %s run %0.f seconds." % (name, (end - start)))

logging.debug("Parent process %s." % os.getpid())
p = Pool(4)
for i in range(5):
    p.apply_async(long_time_task, args=(i,))
logging.debug("Waiting for all subprocess done...")
p.close()
p.join()
logging.debug("All subprocess done.")

'''
进程间通信

Process之间肯定是需要通信的，操作系统提供了很多机制来实现进程间的通信。Python的multiprocessing模块包装了底层的机制，提供了Queue、Pipes等多种方式来交换数据。

我们以Queue为例，在父进程中创建两个子进程，一个往Queue里写数据，一个从Queue里读数据
'''
# 写数据进程执行的代码
def write(q):
    logging.debug("Process to write: %s" % os.getpid())
    for value in ['A', 'B', 'C']:
        logging.debug("Put %s to queue..." % value)
        q.put(value)
        time.sleep(random.randint(1, 5))

# 读数据进程执行的代码
def read(q):
    logging.debug("Process to read: %s" % os.getpid())
    while True:
        value = q.get(True)
        logging.debug("Get %s from queue." % value)

# 父进程创建Queue,并传给各个子进程
q = Queue()
pw = Process(target=write, args=(q,))
pr = Process(target=read, args=(q,))
# 启动子进程pw，写入：
pw.start()
# 启动子进程pr，读取：
pr.start()
# 等待pw结束
pw.join()
# pr进程里的死循环，无法等待结束，只能强制终止
pr.terminate()


title = '''
###########################################################
# 生成器和推导式
# https://foofish.net/python-tricks-tips.html
推导式比生成器对象能带来更快的速度。相对地，生成器更能节省内存开销，它的值是按需生成，不需要像列表推倒式一样把整个结果保存在内存中，同时它不能重新迭代，列表推导式则不然。
###########################################################
'''
print('\n\n{}'.format(title))

# 生成器
x = (x * y for x in range(100) for y in range(10))
logging.debug(next(x))

for index in x:
    print(index, end=' ')

print()

# https://foofish.net/understanding-yield.html
# Yield是关键字，它类似于return，只是函数会返回一个生成器
def createGenerator():
    mylist = range(3)
    for i in mylist:
        yield i*i

mygenerator = createGenerator() # create a generator
print(mygenerator)

for i in mygenerator:
    print(i, end=' ')

# 列表生成式
x = [x * y for x in range(100) for y in range(10)]
logging.debug('{}'.format(x))

x = [x * y for x in range(100) if x % 3 == 0 for y in range(10) if y % 2 == 0]
logging.debug('{}'.format(x))

x = [m + n for m in 'ABC' for n in 'XYZ']
logging.debug('{}'.format(x))


title = '''
###########################################################
# lambda函数
# https://foofish.net/lambda.html
Python 中定义函数有两种方法，一种是用常规方式 def 定义，函数要指定名字，第二种是用 lambda 定义，不需要指定名字，称为 Lambda 函数。

Lambda 函数又称匿名函数，匿名函数就是没有名字的函数，函数没有名字也行？当然可以啦。有些函数如果只是临时一用，而且它的业务逻辑也很简单时，就没必要非给它取个名字不可。###########################################################
###########################################################
'''
print('\n\n{}'.format(title))

add = lambda x, y: x + y
logging.debug('add: {}, add(1, 2): {}'.format(add, add(1, 2)))


title = '''
###########################################################
# 函数式编程
函数式编程就是一种抽象程度很高的编程范式,纯粹的函数式编程语言编写的函数
没有变量,因此,任意一个函数,只要输入是确定的,输出就是确定的,这种纯函
数我们称之为没有副作用。而允许使用变量的程序设计语言,由于函数内部的变量
状态不确定,同样的输入,可能得到不同的输出,因此,这种函数是有副作用的。
函数式编程的一个特点就是,允许把函数本身作为参数传入另一个函数,还允许返
回一个函数!
Python对函数式编程提供部分支持。由于Python允许使用变量,因此,Python不是
纯函数式编程语言。
把函数作为参数传入,这样的函数称为高阶函数,函数式编程就是指这种高度抽象
的编程范式。
###########################################################
'''
print('\n\n{}'.format(title))


title = '''
###########################################################
# Python 为什么要继承 object 类
# https://www.zhihu.com/question/19754936
历史遗留问题.2.2以前的时候type和object还不统一. 在2.2统一以后到3之间,  要用class Foo(object)来申明新式类, 因为他的type是 < type 'type' > .
不然的话, 生成的类的type就是 < type 'classobj' >

继承 object 类的是新式类，不继承 object 类的是经典类，在 Python 2.7 里面新式类和经典类在多继承方面会有差异

B、C 是 A 的子类，D 多继承了 B、C 两个类，其中 C 重写了 A 中的 foo() 方法。
如果 A 是经典类（如下代码），当调用 D 的实例的 foo() 方法时，Python 会按照深度优先的方法去搜索 foo() ，路径是 B-A-C ，执行的是 A 中的 foo() ；
如果 A 是新式类，当调用 D 的实例的 foo() 方法时，Python 会按照广度优先的方法去搜索 foo() ，路径是 B-C-A ，执行的是 C 中的 foo() 。因为 D 是直接继承 C 的，从逻辑上说，执行 C 中的 foo() 更加合理，因此新式类对多继承的处理更为合乎逻辑。
在 Python 3.x 中的新式类貌似已经兼容了经典类，无论 A 是否继承 object 类， D 实例中的 foo() 都会执行 C 中的 foo() 。但是在 Python 2.7 中这种差异仍然存在，因此还是推荐使用新式类，要继承 object 类。
###########################################################
'''
print('\n\n{}'.format(title))

class A:
    def foo(self):
        logging.debug('called A.foo()')

class B(A):
    def foo(self):
        logging.debug('called B.foo()')

class C(A):
    def foo(self):
        logging.debug('called C.foo()')

class D(B, C):
    pass

d = D()
d.foo()
```



