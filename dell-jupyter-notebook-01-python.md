### Dell Jupyter Notebook 01 -- Python

```

# coding: utf-8

# In[1]:

import time
print('Before sleep, time: {}'.format(time.time()))
time.sleep(2)
print('After sleep, time: {}'.format(time.time()))
print('gctime: {}'.format(time.gmtime()))
print('localtime: {}'.format(time.localtime()))
print('time.gmtime: {}'.format(time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.gmtime())))
print('time.localtime: {}'.format(time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.localtime())))


# In[2]:

for i in range(10, 0, -1):
    print(i, end=' ')


# In[3]:

a_dict = {'a':10, 'b':20, 'c':30}
print(a_dict)

def func(**kw):
    print(kw)
    
func(**a_dict)


# In[4]:

import re

cmd_str = '00 02   05 09'
print(re.sub(r'\s+', '', cmd_str))


# In[5]:

x_list = [{'address':'192.168.0.1', 'tested': False}, {'address':'192.168.0.2', 'tested': True}, {'address':'192.168.0.3', 'tested': False}]
for entry_index in range(len(x_list)):
    print('entry_index: {}'.format(entry_index))
    if '192.168.0.2' == x_list[entry_index].get('address'):
        print('entry_index: {}'.format(entry_index))
        x_list.pop(entry_index)
        break
        
print(x_list)
    


# In[6]:

class TestClass:
    g_list = [1, 2, 3, 4]
    
    def __init__(self):
        self.list = [5, 6, 7, 8]
        
    def pop_list(self):
        self.list.pop()
        
    def pop_g_list(self):
        self.g_list.pop()
        
t = TestClass()
t2 = TestClass()
print('TestClass.g_list: {}'.format(TestClass.g_list))
t.pop_list()
print('t.g_list: {}'.format(t.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t.list: {}'.format(t.list))

print('t2.g_list: {}'.format(t2.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t2.list: {}'.format(t2.list))

t.pop_g_list()
print('t.g_list: {}'.format(t.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t.list: {}'.format(t.list))

print('t2.g_list: {}'.format(t2.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t2.list: {}'.format(t2.list))


# In[7]:

import datetime
print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S.%f"))
print(datetime.datetime.now().strftime("%F %H:%M:%S.%f"))


# In[8]:

from time import sleep


def decorate_fun(fun_name):
    def new_fun(*args, **kwargs):
        print('got args: {}, {}'.format(args, kwargs))
        print('this is a fun\'s start')
        fun_name(*args, **kwargs)
        print('this is the fun\'s end')
    return new_fun

@decorate_fun
def fun(number, start_number=0):
    for n in range(number):
        start_number += n
        print(n)
        sleep(1)
    print('the final number is {}'.format(start_number))

print(fun.__name__) # new_fun
fun(5, start_number=5)


# In[9]:

from clockwork import clockwork

api = clockwork.API('62c3a7ad4beecfe8cc5ee6a55e6237b582f437b0')

message = clockwork.SMS(
    to = '8613518169033',
    message = 'This is a test message.')

response = api.send(message)

if response.success:
    print (response.id)
else:
    print (response.error_code)
    print (response.error_message)


# In[10]:

import time
import asyncio

now = lambda : time.time()

async def do_some_work(x):
    print('Waiting: ', x)

start = now()

coroutine = do_some_work(2)

loop = asyncio.get_event_loop()
loop.run_until_complete(coroutine)

print('TIME: ', now() - start)


# In[17]:

import time,queue
 
def consumer(name):
    print("-->starting eating xoxo")
    while True:
        new_xo = yield
        print("%s is eating xoxo %s" % (name, new_xo))
 
def producer():
    print('con1: {}'.format(con1))
    print('con2: {}'.format(con2))
    r = con1.__next__()
    print('r: {}'.format(r))
    r = con2.__next__()
    print('r: {}'.format(r))
    n = 0
    while n < 5:
        n += 1
        con1.send(n)
        con2.send(n)
        # assert(n < 5)
        print("\033[32;1mproducer\033[0m is making xoxo %s" % n)
 
if __name__ == "__main__":
    conn1 = consumer("c1")
    conn2 = consumer("c2")
    p = producer() 


# In[11]:

# 示例一
def test1(a):
    print(a)
    print(b)  # 当函数执行到这一步时会报错。
              # NameError: global name 'b' is not defined
test1(1) 

# 示例二
b = 6
def test2(a):
    print(a)
    print(b) 
test2(1)  # 1 6

# 示例三
b = 6
def test3(a):
    print(a)
    print(b)  # 当函数执行到这一步仍然会报错
              # UnboundLocalError: local variable 'b' referenced before assignment
    b = 9     # 比示例二多了一行赋值
test3(1)


# In[12]:

test1 = [1, 2, 3, 4, 5]
test2 = [1, 2, 3, 4, 5]
zip1 = zip(test1, test2)
list(zip1)
list(zip1)
zip1 = zip(test1, test2)
mapobj = map(lambda args: args[0] * args[1], zip1)
list(mapobj)
list(mapobj)
list(zip1)
zip1 = zip(test1, test2)
mapobj = map(lambda args: args[0] * args[1], zip1)
list(zip1)
list(mapobj)


# In[13]:

iphy_sel = 1

def func(param1, param2, *param3):
    print("param1 = {}, param2 = {}, param3 = {}".format(param1, param2, param3))

con_param = (1, 3, 5, 7, 9, 11, 13, 15)

for iphy_sel in (1, 3, 4, 5, 6, 7):
    phy_num = 0
    for index in range(8):
        if 0x1 == (iphy_sel >> index) & 0x1:
            phy_num += 1
    phys_con_param = con_param * phy_num
    func(iphy_sel, phy_num, *phys_con_param)
    print("{}".format(phys_con_param))


# In[14]:

x = '0x12345678'
y = int(x, 16) + 128
z = ''
for i in range(4):
    z += str(y >> ((4 - i - 1) * 8))
print(z)


# In[1]:

special_case_list = ('GATT_SR_GAW_BI-06-C', 'GATT_CL_GPA_BV-12-C', 'SM_MAS_JW_BI-01-C', 'GATT_SR_GAW_BV-02-C', 'GATT_CL_GPA_BV-11-C', 'GATT_CL_GAR_BV-03-C')
new_list = sorted(special_case_list)
print(new_list)


# In[25]:

'''
https://docs.python.org/3.7/library/functions.html#super
For both use cases, a typical superclass call looks like this:

class C(B):
    def method(self, arg):
        super().method(arg)    # This does the same thing as:
                               # super(C, self).method(arg)


https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/19/README.html

super()的好处就是可以避免直接使用父类的名字.但是它主要用于多重继承,这里面有很多好玩的东西.如果还不了解的话可以看看官方文档

注意在Python3.0里语法有所改变:你可以用super().__init__()替换super(ChildB, self).__init__().(在我看来非常nice)


https://foofish.net/magic-method.html
从输出结果来看，__new__ 方法的返回值就是类的实例对象，这个实例对象会传递给 __init__ 方法中定义的 self 参数，以便实例对象可以被正确地初始化。

如果 __new__ 方法不返回值（或者说返回 None）那么 __init__ 将不会得到调用，这个也说得通，因为实例对象都没创建出来，调用 init 也没什么意义，此外，Python 还规定，__init__ 只能返回 None 值，否则报错，这个留给大家去试。
'''

# The python language reference -- APPENDIX D
class MyClass(object):
    def __init__(self):
        print('call "__init__" function')
        super(MyClass, self).__init__()
        
    def __new__(cls):
        print('call "__new__" function')
        return super(MyClass, cls).__new__(cls)
        
    def func(self,name='world'):
        print('Hello, %s.' % name)
        
    # http://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html
    def __str__(self):
        return 'call "__str__" function'
    
    def __call__(self):
        print('call "__call__" function')

myc = MyClass()

print(MyClass, type(MyClass))
print(myc, type(myc))
myc.func()
myc()

def fn(self, name='world'): # 先定义函数
    print('Hello, %s.' % name)
    
def print_str(self):
    return('print_str')

def class_call(self):
    fn(self)

MyClass = type('MyClass', (object,), {'func':fn, '_str_': print_str, '__call__':class_call}) # 创建MyClass类,得到一个type的类对象
# MyClass = type('MyClass', (object,), {'func':lambda self,name:name}) # 创建MyClass类

myc=MyClass()

print(MyClass, type(MyClass))
print(myc, type(myc))
myc.func()
myc()


# In[29]:

class A:
    def __init__(self):
        self.n = 2

    def add(self, m):
        # 第四步
        # 来自 D.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @A.add, self.n = {1}'.format(self, self.n))
        self.n += m
        # d.n == 7


class B(A):
    def __init__(self):
        self.n = 3

    def add(self, m):
        # 第二步
        # 来自 D.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @B.add, self.n = {1}'.format(self, self.n))
        # 等价于 suepr(B, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 B 之后的 [C, A, object] 中查找 add 方法
        super().add(m)

        # 第六步
        # d.n = 11
        self.n += 3
        # d.n = 14

class C(A):
    def __init__(self):
        self.n = 4

    def add(self, m):
        # 第三步
        # 来自 B.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @C.add, self.n = {1}'.format(self, self.n))
        # 等价于 suepr(C, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 C 之后的 [A, object] 中查找 add 方法
        super().add(m)

        # 第五步
        # d.n = 7
        self.n += 4
        # d.n = 11


class D(B, C):
    def __init__(self):
        self.n = 5

    def add(self, m):
        # 第一步
        print('self is {0} @D.add, self.n = {1}'.format(self, self.n))
        # 等价于 super(D, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 D 之后的 [B, C, A, object] 中查找 add 方法
        super().add(m)

        # 第七步
        # d.n = 14
        self.n += 5
        # self.n = 19

d = D()
print(D.mro())
d.add(2)
print(d.n)


# In[1]:

import time
import json
import socket

def connect(ip, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        sock.connect((ip, port))
    except Exception as err:
        return None
    
    return sock

def client(sock, message):
    json_str = json.dumps(message)
    if sock is not None:
        sock.sendall(json_str.encode(encoding='utf-8'))
        response = sock.recv(1024).decode(encoding='utf-8')
        print("Received: {}".format(response))
        print(socket.gethostbyname(socket.gethostname()),
              socket.gethostbyaddr(socket.gethostname()), socket.gethostname())
        # sock.close()



ip, port = '172.16.50.20', 5000
message = {"type": "info", "category": "heart", "message": "heart message"}

socket_con = connect(ip, port)

count = 100

while True:
    client(socket_con, message)
    time.sleep(0.1)
    count -= 1


# In[2]:

import argparse

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('-o', '--output')
    parser.add_argument('-v', dest='verbose', action='store_true')
    args = parser.parse_args()
    print('output: {}'.format(args.output))
    print('verbose: {}'.format(args.verbose))


if __name__ == '__main__':
    main()


# In[5]:

import re
import time
import gzip
import random
import urllib.request
from bs4 import BeautifulSoup

url_link = 'https://item.jd.com/2316993.html'

match_obj = re.match(r'^http[s]*://([^/]*)', url_link)
if match_obj:
    host = match_obj.group(1)

header = {'Host': host,
          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0',
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
          'Accept-Language': 'en-US,en;q=0.5',
          # 'Accept-Encoding': 'gzip, deflate, sdch',
          'Accept-Encoding': 'gzip, deflate',
          'DNT': '1',
          'Connection': 'keep-alive',
          'Upgrade-Insecure-Requests': '1',
          }
print(header)

enable_gzip = False
if header.get('Accept-Encoding') is not None and re.search(r'gzip', str(header.get('Accept-Encoding'))):
    enable_gzip = True

url_request = urllib.request.Request(url_link, headers=header)
url_open = urllib.request.urlopen(url_request, timeout=5)
if enable_gzip:
    html_doc = url_open.read()
    # print(html_doc)
    # html_doc = gzip.decompress(html_doc).decode('utf-8')
    html_doc = gzip.decompress(html_doc)

else:
    html_doc = url_open.read().decode('utf-8')

print(html_doc)


# In[16]:

import requests
import json

r = requests.get('http://vip.stock.finance.sina.com.cn/quotes_service/api/json_v2.php/Market_Center.getHQNodeData?page=1&num=10000&sort=symbol&asc=1&node=hs_a&symbol=&_s_r_a=sort')
print(r.status_code)
print(r.text)
r_json = json.
# print(r.content)


# In[12]:

import re

class ArgsParameter(object):
    def __init__(self):
        self.module_name = 'all'
        self.target_name = 'full'
        self.flash_mode = 'xip'
        self.svn_revision = '4173'
        # self.svn_revision = None


html_header_template = """<!DOCTYPE html>
<html>
<head>
    <title>PTS {0} Test Summary Report</title>
</head>
<body>
<center>
    <font color="red" size="5">PTS {0} Test Summary Report</font>
</center>
<br>
<br>
<table border="1" bgcolor="Lavender" color="" align="center">
<tr><th>Module Name</th><th>Case Number</th><th>Passed Number</th><th>Failed Number</th><th>Timeout Number</th><th>Skipped Number</th></tr>
"""

args_param = ArgsParameter()

if args_param is not None:
    target_name = args_param.target_name
    svn_revision = args_param.svn_revision

    if svn_revision is None:
        html_header = html_header_template.format(target_name.capitalize())
    else:
        fpga_version = 'R07_15a_B0718a_b2_k7'
        html_header_template = re.sub(r'</center>\n<br>\n<br>',
                                      r'</center>\n<br><style>p.indent{{ padding-left: 19.6em; line-height: 0.0}}</style>\n<p class="indent"><b>SVN Revision: {1}</b></p>\n<p class="indent"><b>FPGA Version: {2}</b></p>',
                                      html_header_template)
        html_header = html_header_template.format(target_name.capitalize(), svn_revision, fpga_version)
else:
    html_header = html_header_template.format('Automation')

print(html_header)


# In[ ]:

```



