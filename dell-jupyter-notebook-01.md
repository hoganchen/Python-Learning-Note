### Dell Jupyter Notebook 01 -- ipynb

```
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Before sleep, time: 1529998954.9086807\n",
      "After sleep, time: 1529998956.9124296\n",
      "gctime: time.struct_time(tm_year=2018, tm_mon=6, tm_mday=26, tm_hour=7, tm_min=42, tm_sec=36, tm_wday=1, tm_yday=177, tm_isdst=0)\n",
      "localtime: time.struct_time(tm_year=2018, tm_mon=6, tm_mday=26, tm_hour=15, tm_min=42, tm_sec=36, tm_wday=1, tm_yday=177, tm_isdst=0)\n",
      "time.gmtime: Tue, 26 Jun 2018 07:42:36 +0000\n",
      "time.localtime: Tue, 26 Jun 2018 15:42:36 +0000\n"
     ]
    }
   ],
   "source": [
    "import time\n",
    "print('Before sleep, time: {}'.format(time.time()))\n",
    "time.sleep(2)\n",
    "print('After sleep, time: {}'.format(time.time()))\n",
    "print('gctime: {}'.format(time.gmtime()))\n",
    "print('localtime: {}'.format(time.localtime()))\n",
    "print('time.gmtime: {}'.format(time.strftime(\"%a, %d %b %Y %H:%M:%S +0000\", time.gmtime())))\n",
    "print('time.localtime: {}'.format(time.strftime(\"%a, %d %b %Y %H:%M:%S +0000\", time.localtime())))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "10 9 8 7 6 5 4 3 2 1 "
     ]
    }
   ],
   "source": [
    "for i in range(10, 0, -1):\n",
    "    print(i, end=' ')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "{'a': 10, 'b': 20, 'c': 30}\n",
      "{'a': 10, 'b': 20, 'c': 30}\n"
     ]
    }
   ],
   "source": [
    "a_dict = {'a':10, 'b':20, 'c':30}\n",
    "print(a_dict)\n",
    "\n",
    "def func(**kw):\n",
    "    print(kw)\n",
    "    \n",
    "func(**a_dict)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "00020509\n"
     ]
    }
   ],
   "source": [
    "import re\n",
    "\n",
    "cmd_str = '00 02   05 09'\n",
    "print(re.sub(r'\\s+', '', cmd_str))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "entry_index: 0\n",
      "entry_index: 1\n",
      "entry_index: 1\n",
      "[{'address': '192.168.0.1', 'tested': False}, {'address': '192.168.0.3', 'tested': False}]\n"
     ]
    }
   ],
   "source": [
    "x_list = [{'address':'192.168.0.1', 'tested': False}, {'address':'192.168.0.2', 'tested': True}, {'address':'192.168.0.3', 'tested': False}]\n",
    "for entry_index in range(len(x_list)):\n",
    "    print('entry_index: {}'.format(entry_index))\n",
    "    if '192.168.0.2' == x_list[entry_index].get('address'):\n",
    "        print('entry_index: {}'.format(entry_index))\n",
    "        x_list.pop(entry_index)\n",
    "        break\n",
    "        \n",
    "print(x_list)\n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "TestClass.g_list: [1, 2, 3, 4]\n",
      "t.g_list: [1, 2, 3, 4]\n",
      "TestClass.g_list: [1, 2, 3, 4]\n",
      "t.list: [5, 6, 7]\n",
      "t2.g_list: [1, 2, 3, 4]\n",
      "TestClass.g_list: [1, 2, 3, 4]\n",
      "t2.list: [5, 6, 7, 8]\n",
      "t.g_list: [1, 2, 3]\n",
      "TestClass.g_list: [1, 2, 3]\n",
      "t.list: [5, 6, 7]\n",
      "t2.g_list: [1, 2, 3]\n",
      "TestClass.g_list: [1, 2, 3]\n",
      "t2.list: [5, 6, 7, 8]\n"
     ]
    }
   ],
   "source": [
    "class TestClass:\n",
    "    g_list = [1, 2, 3, 4]\n",
    "    \n",
    "    def __init__(self):\n",
    "        self.list = [5, 6, 7, 8]\n",
    "        \n",
    "    def pop_list(self):\n",
    "        self.list.pop()\n",
    "        \n",
    "    def pop_g_list(self):\n",
    "        self.g_list.pop()\n",
    "        \n",
    "t = TestClass()\n",
    "t2 = TestClass()\n",
    "print('TestClass.g_list: {}'.format(TestClass.g_list))\n",
    "t.pop_list()\n",
    "print('t.g_list: {}'.format(t.g_list))\n",
    "print('TestClass.g_list: {}'.format(TestClass.g_list))\n",
    "print('t.list: {}'.format(t.list))\n",
    "\n",
    "print('t2.g_list: {}'.format(t2.g_list))\n",
    "print('TestClass.g_list: {}'.format(TestClass.g_list))\n",
    "print('t2.list: {}'.format(t2.list))\n",
    "\n",
    "t.pop_g_list()\n",
    "print('t.g_list: {}'.format(t.g_list))\n",
    "print('TestClass.g_list: {}'.format(TestClass.g_list))\n",
    "print('t.list: {}'.format(t.list))\n",
    "\n",
    "print('t2.g_list: {}'.format(t2.g_list))\n",
    "print('TestClass.g_list: {}'.format(TestClass.g_list))\n",
    "print('t2.list: {}'.format(t2.list))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "2018-06-26 15:43:09.002326\n",
      "2018-06-26 15:43:09.004051\n"
     ]
    }
   ],
   "source": [
    "import datetime\n",
    "print(datetime.datetime.now().strftime(\"%Y-%m-%d %H:%M:%S.%f\"))\n",
    "print(datetime.datetime.now().strftime(\"%F %H:%M:%S.%f\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "new_fun\n",
      "got args: (5,), {'start_number': 5}\n",
      "this is a fun's start\n",
      "0\n",
      "1\n",
      "2\n",
      "3\n",
      "4\n",
      "the final number is 15\n",
      "this is the fun's end\n"
     ]
    }
   ],
   "source": [
    "from time import sleep\n",
    "\n",
    "\n",
    "def decorate_fun(fun_name):\n",
    "    def new_fun(*args, **kwargs):\n",
    "        print('got args: {}, {}'.format(args, kwargs))\n",
    "        print('this is a fun\\'s start')\n",
    "        fun_name(*args, **kwargs)\n",
    "        print('this is the fun\\'s end')\n",
    "    return new_fun\n",
    "\n",
    "@decorate_fun\n",
    "def fun(number, start_number=0):\n",
    "    for n in range(number):\n",
    "        start_number += n\n",
    "        print(n)\n",
    "        sleep(1)\n",
    "    print('the final number is {}'.format(start_number))\n",
    "\n",
    "print(fun.__name__) # new_fun\n",
    "fun(5, start_number=5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "15\n",
      "No route defined for this message\n"
     ]
    }
   ],
   "source": [
    "from clockwork import clockwork\n",
    "\n",
    "api = clockwork.API('62c3a7ad4beecfe8cc5ee6a55e6237b582f437b0')\n",
    "\n",
    "message = clockwork.SMS(\n",
    "    to = '8613518169033',\n",
    "    message = 'This is a test message.')\n",
    "\n",
    "response = api.send(message)\n",
    "\n",
    "if response.success:\n",
    "    print (response.id)\n",
    "else:\n",
    "    print (response.error_code)\n",
    "    print (response.error_message)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Waiting:  2\n",
      "TIME:  0.0010402202606201172\n"
     ]
    }
   ],
   "source": [
    "import time\n",
    "import asyncio\n",
    "\n",
    "now = lambda : time.time()\n",
    "\n",
    "async def do_some_work(x):\n",
    "    print('Waiting: ', x)\n",
    "\n",
    "start = now()\n",
    "\n",
    "coroutine = do_some_work(2)\n",
    "\n",
    "loop = asyncio.get_event_loop()\n",
    "loop.run_until_complete(coroutine)\n",
    "\n",
    "print('TIME: ', now() - start)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "con1: <generator object consumer at 0x7f5fa0042830>\n",
      "con2: <generator object consumer at 0x7f5f91973570>\n",
      "c1 is eating xoxo None\n",
      "r: None\n",
      "c2 is eating xoxo None\n",
      "r: None\n",
      "c1 is eating xoxo 1\n",
      "c2 is eating xoxo 1\n",
      "\u001b[32;1mproducer\u001b[0m is making xoxo 1\n",
      "c1 is eating xoxo 2\n",
      "c2 is eating xoxo 2\n",
      "\u001b[32;1mproducer\u001b[0m is making xoxo 2\n",
      "c1 is eating xoxo 3\n",
      "c2 is eating xoxo 3\n",
      "\u001b[32;1mproducer\u001b[0m is making xoxo 3\n",
      "c1 is eating xoxo 4\n",
      "c2 is eating xoxo 4\n",
      "\u001b[32;1mproducer\u001b[0m is making xoxo 4\n",
      "c1 is eating xoxo 5\n",
      "c2 is eating xoxo 5\n",
      "\u001b[32;1mproducer\u001b[0m is making xoxo 5\n"
     ]
    }
   ],
   "source": [
    "import time,queue\n",
    " \n",
    "def consumer(name):\n",
    "    print(\"-->starting eating xoxo\")\n",
    "    while True:\n",
    "        new_xo = yield\n",
    "        print(\"%s is eating xoxo %s\" % (name, new_xo))\n",
    " \n",
    "def producer():\n",
    "    print('con1: {}'.format(con1))\n",
    "    print('con2: {}'.format(con2))\n",
    "    r = con1.__next__()\n",
    "    print('r: {}'.format(r))\n",
    "    r = con2.__next__()\n",
    "    print('r: {}'.format(r))\n",
    "    n = 0\n",
    "    while n < 5:\n",
    "        n += 1\n",
    "        con1.send(n)\n",
    "        con2.send(n)\n",
    "        # assert(n < 5)\n",
    "        print(\"\\033[32;1mproducer\\033[0m is making xoxo %s\" % n)\n",
    " \n",
    "if __name__ == \"__main__\":\n",
    "    conn1 = consumer(\"c1\")\n",
    "    conn2 = consumer(\"c2\")\n",
    "    p = producer() "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1\n"
     ]
    },
    {
     "ename": "NameError",
     "evalue": "name 'b' is not defined",
     "output_type": "error",
     "traceback": [
      "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[0;31mNameError\u001b[0m                                 Traceback (most recent call last)",
      "\u001b[0;32m<ipython-input-11-d5bb139eee38>\u001b[0m in \u001b[0;36m<module>\u001b[0;34m()\u001b[0m\n\u001b[1;32m      4\u001b[0m     \u001b[0mprint\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mb\u001b[0m\u001b[0;34m)\u001b[0m  \u001b[0;31m# 当函数执行到这一步时会报错。\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      5\u001b[0m               \u001b[0;31m# NameError: global name 'b' is not defined\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m----> 6\u001b[0;31m \u001b[0mtest1\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;36m1\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m\u001b[1;32m      7\u001b[0m \u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      8\u001b[0m \u001b[0;31m# 示例二\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
      "\u001b[0;32m<ipython-input-11-d5bb139eee38>\u001b[0m in \u001b[0;36mtest1\u001b[0;34m(a)\u001b[0m\n\u001b[1;32m      2\u001b[0m \u001b[0;32mdef\u001b[0m \u001b[0mtest1\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0ma\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m:\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      3\u001b[0m     \u001b[0mprint\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0ma\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m----> 4\u001b[0;31m     \u001b[0mprint\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mb\u001b[0m\u001b[0;34m)\u001b[0m  \u001b[0;31m# 当函数执行到这一步时会报错。\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m\u001b[1;32m      5\u001b[0m               \u001b[0;31m# NameError: global name 'b' is not defined\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      6\u001b[0m \u001b[0mtest1\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;36m1\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
      "\u001b[0;31mNameError\u001b[0m: name 'b' is not defined"
     ]
    }
   ],
   "source": [
    "# 示例一\n",
    "def test1(a):\n",
    "    print(a)\n",
    "    print(b)  # 当函数执行到这一步时会报错。\n",
    "              # NameError: global name 'b' is not defined\n",
    "test1(1) \n",
    "\n",
    "# 示例二\n",
    "b = 6\n",
    "def test2(a):\n",
    "    print(a)\n",
    "    print(b) \n",
    "test2(1)  # 1 6\n",
    "\n",
    "# 示例三\n",
    "b = 6\n",
    "def test3(a):\n",
    "    print(a)\n",
    "    print(b)  # 当函数执行到这一步仍然会报错\n",
    "              # UnboundLocalError: local variable 'b' referenced before assignment\n",
    "    b = 9     # 比示例二多了一行赋值\n",
    "test3(1)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[]"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test1 = [1, 2, 3, 4, 5]\n",
    "test2 = [1, 2, 3, 4, 5]\n",
    "zip1 = zip(test1, test2)\n",
    "list(zip1)\n",
    "list(zip1)\n",
    "zip1 = zip(test1, test2)\n",
    "mapobj = map(lambda args: args[0] * args[1], zip1)\n",
    "list(mapobj)\n",
    "list(mapobj)\n",
    "list(zip1)\n",
    "zip1 = zip(test1, test2)\n",
    "mapobj = map(lambda args: args[0] * args[1], zip1)\n",
    "list(zip1)\n",
    "list(mapobj)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "param1 = 1, param2 = 1, param3 = (1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15)\n",
      "param1 = 3, param2 = 2, param3 = (1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "param1 = 4, param2 = 1, param3 = (1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15)\n",
      "param1 = 5, param2 = 2, param3 = (1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "param1 = 6, param2 = 2, param3 = (1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "param1 = 7, param2 = 3, param3 = (1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n",
      "(1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15, 1, 3, 5, 7, 9, 11, 13, 15)\n"
     ]
    }
   ],
   "source": [
    "iphy_sel = 1\n",
    "\n",
    "def func(param1, param2, *param3):\n",
    "    print(\"param1 = {}, param2 = {}, param3 = {}\".format(param1, param2, param3))\n",
    "\n",
    "con_param = (1, 3, 5, 7, 9, 11, 13, 15)\n",
    "\n",
    "for iphy_sel in (1, 3, 4, 5, 6, 7):\n",
    "    phy_num = 0\n",
    "    for index in range(8):\n",
    "        if 0x1 == (iphy_sel >> index) & 0x1:\n",
    "            phy_num += 1\n",
    "    phys_con_param = con_param * phy_num\n",
    "    func(iphy_sel, phy_num, *phys_con_param)\n",
    "    print(\"{}\".format(phys_con_param))\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1846601193046305420024\n"
     ]
    }
   ],
   "source": [
    "x = '0x12345678'\n",
    "y = int(x, 16) + 128\n",
    "z = ''\n",
    "for i in range(4):\n",
    "    z += str(y >> ((4 - i - 1) * 8))\n",
    "print(z)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['GATT_CL_GAR_BV-03-C', 'GATT_CL_GPA_BV-11-C', 'GATT_CL_GPA_BV-12-C', 'GATT_SR_GAW_BI-06-C', 'GATT_SR_GAW_BV-02-C', 'SM_MAS_JW_BI-01-C']\n"
     ]
    }
   ],
   "source": [
    "special_case_list = ('GATT_SR_GAW_BI-06-C', 'GATT_CL_GPA_BV-12-C', 'SM_MAS_JW_BI-01-C', 'GATT_SR_GAW_BV-02-C', 'GATT_CL_GPA_BV-11-C', 'GATT_CL_GAR_BV-03-C')\n",
    "new_list = sorted(special_case_list)\n",
    "print(new_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "call \"__new__\" function\n",
      "call \"__init__\" function\n",
      "<class '__main__.MyClass'> <class 'type'>\n",
      "call \"__str__\" function <class '__main__.MyClass'>\n",
      "Hello, world.\n",
      "call \"__call__\" function\n",
      "<class '__main__.MyClass'> <class 'type'>\n",
      "<__main__.MyClass object at 0x7fd4d4023b00> <class '__main__.MyClass'>\n",
      "Hello, world.\n",
      "Hello, world.\n"
     ]
    }
   ],
   "source": [
    "'''\n",
    "https://docs.python.org/3.7/library/functions.html#super\n",
    "For both use cases, a typical superclass call looks like this:\n",
    "\n",
    "class C(B):\n",
    "    def method(self, arg):\n",
    "        super().method(arg)    # This does the same thing as:\n",
    "                               # super(C, self).method(arg)\n",
    "\n",
    "\n",
    "https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/19/README.html\n",
    "\n",
    "super()的好处就是可以避免直接使用父类的名字.但是它主要用于多重继承,这里面有很多好玩的东西.如果还不了解的话可以看看官方文档\n",
    "\n",
    "注意在Python3.0里语法有所改变:你可以用super().__init__()替换super(ChildB, self).__init__().(在我看来非常nice)\n",
    "\n",
    "\n",
    "https://foofish.net/magic-method.html\n",
    "从输出结果来看，__new__ 方法的返回值就是类的实例对象，这个实例对象会传递给 __init__ 方法中定义的 self 参数，以便实例对象可以被正确地初始化。\n",
    "\n",
    "如果 __new__ 方法不返回值（或者说返回 None）那么 __init__ 将不会得到调用，这个也说得通，因为实例对象都没创建出来，调用 init 也没什么意义，此外，Python 还规定，__init__ 只能返回 None 值，否则报错，这个留给大家去试。\n",
    "'''\n",
    "\n",
    "# The python language reference -- APPENDIX D\n",
    "class MyClass(object):\n",
    "    def __init__(self):\n",
    "        print('call \"__init__\" function')\n",
    "        super(MyClass, self).__init__()\n",
    "        \n",
    "    def __new__(cls):\n",
    "        print('call \"__new__\" function')\n",
    "        return super(MyClass, cls).__new__(cls)\n",
    "        \n",
    "    def func(self,name='world'):\n",
    "        print('Hello, %s.' % name)\n",
    "        \n",
    "    # http://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html\n",
    "    def __str__(self):\n",
    "        return 'call \"__str__\" function'\n",
    "    \n",
    "    def __call__(self):\n",
    "        print('call \"__call__\" function')\n",
    "\n",
    "myc = MyClass()\n",
    "\n",
    "print(MyClass, type(MyClass))\n",
    "print(myc, type(myc))\n",
    "myc.func()\n",
    "myc()\n",
    "\n",
    "def fn(self, name='world'): # 先定义函数\n",
    "    print('Hello, %s.' % name)\n",
    "    \n",
    "def print_str(self):\n",
    "    return('print_str')\n",
    "\n",
    "def class_call(self):\n",
    "    fn(self)\n",
    "\n",
    "MyClass = type('MyClass', (object,), {'func':fn, '_str_': print_str, '__call__':class_call}) # 创建MyClass类,得到一个type的类对象\n",
    "# MyClass = type('MyClass', (object,), {'func':lambda self,name:name}) # 创建MyClass类\n",
    "\n",
    "myc=MyClass()\n",
    "\n",
    "print(MyClass, type(MyClass))\n",
    "print(myc, type(myc))\n",
    "myc.func()\n",
    "myc()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]\n",
      "self is <__main__.D object at 0x7fd4d40353c8> @D.add, self.n = 5\n",
      "self is <__main__.D object at 0x7fd4d40353c8> @B.add, self.n = 5\n",
      "self is <__main__.D object at 0x7fd4d40353c8> @C.add, self.n = 5\n",
      "self is <__main__.D object at 0x7fd4d40353c8> @A.add, self.n = 5\n",
      "19\n"
     ]
    }
   ],
   "source": [
    "class A:\n",
    "    def __init__(self):\n",
    "        self.n = 2\n",
    "\n",
    "    def add(self, m):\n",
    "        # 第四步\n",
    "        # 来自 D.add 中的 super\n",
    "        # self == d, self.n == d.n == 5\n",
    "        print('self is {0} @A.add, self.n = {1}'.format(self, self.n))\n",
    "        self.n += m\n",
    "        # d.n == 7\n",
    "\n",
    "\n",
    "class B(A):\n",
    "    def __init__(self):\n",
    "        self.n = 3\n",
    "\n",
    "    def add(self, m):\n",
    "        # 第二步\n",
    "        # 来自 D.add 中的 super\n",
    "        # self == d, self.n == d.n == 5\n",
    "        print('self is {0} @B.add, self.n = {1}'.format(self, self.n))\n",
    "        # 等价于 suepr(B, self).add(m)\n",
    "        # self 的 MRO 是 [D, B, C, A, object]\n",
    "        # 从 B 之后的 [C, A, object] 中查找 add 方法\n",
    "        super().add(m)\n",
    "\n",
    "        # 第六步\n",
    "        # d.n = 11\n",
    "        self.n += 3\n",
    "        # d.n = 14\n",
    "\n",
    "class C(A):\n",
    "    def __init__(self):\n",
    "        self.n = 4\n",
    "\n",
    "    def add(self, m):\n",
    "        # 第三步\n",
    "        # 来自 B.add 中的 super\n",
    "        # self == d, self.n == d.n == 5\n",
    "        print('self is {0} @C.add, self.n = {1}'.format(self, self.n))\n",
    "        # 等价于 suepr(C, self).add(m)\n",
    "        # self 的 MRO 是 [D, B, C, A, object]\n",
    "        # 从 C 之后的 [A, object] 中查找 add 方法\n",
    "        super().add(m)\n",
    "\n",
    "        # 第五步\n",
    "        # d.n = 7\n",
    "        self.n += 4\n",
    "        # d.n = 11\n",
    "\n",
    "\n",
    "class D(B, C):\n",
    "    def __init__(self):\n",
    "        self.n = 5\n",
    "\n",
    "    def add(self, m):\n",
    "        # 第一步\n",
    "        print('self is {0} @D.add, self.n = {1}'.format(self, self.n))\n",
    "        # 等价于 super(D, self).add(m)\n",
    "        # self 的 MRO 是 [D, B, C, A, object]\n",
    "        # 从 D 之后的 [B, C, A, object] 中查找 add 方法\n",
    "        super().add(m)\n",
    "\n",
    "        # 第七步\n",
    "        # d.n = 14\n",
    "        self.n += 5\n",
    "        # self.n = 19\n",
    "\n",
    "d = D()\n",
    "print(D.mro())\n",
    "d.add(2)\n",
    "print(d.n)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "ename": "KeyboardInterrupt",
     "evalue": "",
     "output_type": "error",
     "traceback": [
      "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[0;31mKeyboardInterrupt\u001b[0m                         Traceback (most recent call last)",
      "\u001b[0;32m<ipython-input-1-da4255f2299d>\u001b[0m in \u001b[0;36m<module>\u001b[0;34m()\u001b[0m\n\u001b[1;32m     33\u001b[0m \u001b[0;32mwhile\u001b[0m \u001b[0;32mTrue\u001b[0m\u001b[0;34m:\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     34\u001b[0m     \u001b[0mclient\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0msocket_con\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mmessage\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 35\u001b[0;31m     \u001b[0mtime\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0msleep\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;36m0.1\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m\u001b[1;32m     36\u001b[0m     \u001b[0mcount\u001b[0m \u001b[0;34m-=\u001b[0m \u001b[0;36m1\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
      "\u001b[0;31mKeyboardInterrupt\u001b[0m: "
     ]
    }
   ],
   "source": [
    "import time\n",
    "import json\n",
    "import socket\n",
    "\n",
    "def connect(ip, port):\n",
    "    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)\n",
    "    try:\n",
    "        sock.connect((ip, port))\n",
    "    except Exception as err:\n",
    "        return None\n",
    "    \n",
    "    return sock\n",
    "\n",
    "def client(sock, message):\n",
    "    json_str = json.dumps(message)\n",
    "    if sock is not None:\n",
    "        sock.sendall(json_str.encode(encoding='utf-8'))\n",
    "        response = sock.recv(1024).decode(encoding='utf-8')\n",
    "        print(\"Received: {}\".format(response))\n",
    "        print(socket.gethostbyname(socket.gethostname()),\n",
    "              socket.gethostbyaddr(socket.gethostname()), socket.gethostname())\n",
    "        # sock.close()\n",
    "\n",
    "\n",
    "\n",
    "ip, port = '172.16.50.20', 5000\n",
    "message = {\"type\": \"info\", \"category\": \"heart\", \"message\": \"heart message\"}\n",
    "\n",
    "socket_con = connect(ip, port)\n",
    "\n",
    "count = 100\n",
    "\n",
    "while True:\n",
    "    client(socket_con, message)\n",
    "    time.sleep(0.1)\n",
    "    count -= 1\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "usage: ipykernel_launcher.py [-h] [-o OUTPUT] [-v]\n",
      "ipykernel_launcher.py: error: unrecognized arguments: -f /run/user/1000/jupyter/kernel-b0a1f5c8-2c74-4fb6-bce3-d2b432f28dbe.json\n"
     ]
    },
    {
     "ename": "SystemExit",
     "evalue": "2",
     "output_type": "error",
     "traceback": [
      "An exception has occurred, use %tb to see the full traceback.\n",
      "\u001b[0;31mSystemExit\u001b[0m\u001b[0;31m:\u001b[0m 2\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/chenlianghong/anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2889: UserWarning: To exit: use 'exit', 'quit', or Ctrl-D.\n",
      "  warn(\"To exit: use 'exit', 'quit', or Ctrl-D.\", stacklevel=1)\n"
     ]
    }
   ],
   "source": [
    "import argparse\n",
    "\n",
    "def main():\n",
    "    parser = argparse.ArgumentParser()\n",
    "    parser.add_argument('-o', '--output')\n",
    "    parser.add_argument('-v', dest='verbose', action='store_true')\n",
    "    args = parser.parse_args()\n",
    "    print('output: {}'.format(args.output))\n",
    "    print('verbose: {}'.format(args.verbose))\n",
    "\n",
    "\n",
    "if __name__ == '__main__':\n",
    "    main()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "{'Host': 'item.jd.com', 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0', 'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8', 'Accept-Language': 'en-US,en;q=0.5', 'Accept-Encoding': 'gzip, deflate', 'DNT': '1', 'Connection': 'keep-alive', 'Upgrade-Insecure-Requests': '1'}\n"
     ]
    },
    {
     "ename": "UnicodeDecodeError",
     "evalue": "'utf-8' codec can't decode byte 0xa1 in position 146: invalid start byte",
     "output_type": "error",
     "traceback": [
      "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[0;31mUnicodeDecodeError\u001b[0m                        Traceback (most recent call last)",
      "\u001b[0;32m<ipython-input-5-54bf56fbfc32>\u001b[0m in \u001b[0;36m<module>\u001b[0;34m()\u001b[0m\n\u001b[1;32m     39\u001b[0m     \u001b[0mhtml_doc\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0murl_open\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mread\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mdecode\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;34m'utf-8'\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     40\u001b[0m \u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 41\u001b[0;31m \u001b[0mprint\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mhtml_doc\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mdecode\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m",
      "\u001b[0;31mUnicodeDecodeError\u001b[0m: 'utf-8' codec can't decode byte 0xa1 in position 146: invalid start byte"
     ]
    }
   ],
   "source": [
    "import re\n",
    "import time\n",
    "import gzip\n",
    "import random\n",
    "import urllib.request\n",
    "from bs4 import BeautifulSoup\n",
    "\n",
    "url_link = 'https://item.jd.com/2316993.html'\n",
    "\n",
    "match_obj = re.match(r'^http[s]*://([^/]*)', url_link)\n",
    "if match_obj:\n",
    "    host = match_obj.group(1)\n",
    "\n",
    "header = {'Host': host,\n",
    "          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0',\n",
    "          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',\n",
    "          'Accept-Language': 'en-US,en;q=0.5',\n",
    "          # 'Accept-Encoding': 'gzip, deflate, sdch',\n",
    "          'Accept-Encoding': 'gzip, deflate',\n",
    "          'DNT': '1',\n",
    "          'Connection': 'keep-alive',\n",
    "          'Upgrade-Insecure-Requests': '1',\n",
    "          }\n",
    "print(header)\n",
    "\n",
    "enable_gzip = False\n",
    "if header.get('Accept-Encoding') is not None and re.search(r'gzip', str(header.get('Accept-Encoding'))):\n",
    "    enable_gzip = True\n",
    "\n",
    "url_request = urllib.request.Request(url_link, headers=header)\n",
    "url_open = urllib.request.urlopen(url_request, timeout=5)\n",
    "if enable_gzip:\n",
    "    html_doc = url_open.read()\n",
    "    # print(html_doc)\n",
    "    # html_doc = gzip.decompress(html_doc).decode('utf-8')\n",
    "    html_doc = gzip.decompress(html_doc)\n",
    "\n",
    "else:\n",
    "    html_doc = url_open.read().decode('utf-8')\n",
    "\n",
    "print(html_doc)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "200\n",
      "[{symbol:\"sh600000\",code:\"600000\",name:\"XD浦发银\",trade:\"9.500\",pricechange:\"0.030\",changepercent:\"0.317\",buy:\"9.490\",sell:\"9.500\",settlement:\"9.470\",open:\"9.570\",high:\"9.580\",low:\"9.460\",volume:13337862,amount:126681136,ticktime:\"14:38:39\",per:5.163,pb:0.7,mktcap:27884476.37715,nmc:26698244.62905,turnoverratio:0.04746},{symbol:\"sh600004\",code:\"600004\",name:\"XD白云机\",trade:\"14.430\",pricechange:\"0.200\",changepercent:\"1.405\",buy:\"14.420\",sell:\"14.430\",settlement:\"14.230\",open:\"14.330\",high:\"14.580\",low:\"14.100\",volume:12121293,amount:173395663,ticktime:\"14:38:42\",per:17.598,pb:1.934,mktcap:2986029.501702,nmc:2986029.501702,turnoverratio:0.58576},{symbol:\"sh600006\",code:\"600006\",name:\"东风汽车\",trade:\"3.960\",pricechange:\"0.020\",changepercent:\"0.508\",buy:\"3.950\",sell:\"3.960\",settlement:\"3.940\",open:\"3.950\",high:\"3.970\",low:\"3.920\",volume:3194396,amount:12611431,ticktime:\"14:38:39\",per:39.442,pb:1.159,mktcap:792000,nmc:792000,turnoverratio:0.15972},{symbol:\"sh600007\",code:\"600007\",name:\"中国国贸\",trade:\"14.420\",pricechange:\"-0.030\",changepercent:\"-0.208\",buy:\"14.420\",sell:\"14.460\",settlement:\"14.450\",open:\"14.440\",high:\"14.930\",low:\"14.400\",volume:521755,amount:7599189,ticktime:\"14:38:36\",per:22.889,pb:2.178,mktcap:1452501.414028,nmc:1452501.414028,turnoverratio:0.0518},{symbol:\"sh600008\",code:\"600008\",name:\"首创股份\",trade:\"4.180\",pricechange:\"-0.040\",changepercent:\"-0.948\",buy:\"4.180\",sell:\"4.190\",settlement:\"4.220\",open:\"4.200\",high:\"4.220\",low:\"4.170\",volume:8525928,amount:35679136,ticktime:\"14:38:39\",per:32.913,pb:2.459,mktcap:2015016.703832,nmc:2015016.703832,turnoverratio:0.17686},{symbol:\"sh600009\",code:\"600009\",name:\"上海机场\",trade:\"61.610\",pricechange:\"1.560\",changepercent:\"2.598\",buy:\"61.610\",sell:\"61.620\",settlement:\"60.050\",open:\"59.770\",high:\"62.800\",low:\"59.770\",volume:6266826,amount:387961615,ticktime:\"14:38:39\",per:32.257,pb:4.54,mktcap:11871990.998128,nmc:6736908.081917,turnoverratio:0.57311},{symbol:\"sh600010\",code:\"600010\",name:\"包钢股份\",trade:\"1.550\",pricechange:\"0.020\",changepercent:\"1.307\",buy:\"1.540\",sell:\"1.550\",settlement:\"1.530\",open:\"1.530\",high:\"1.550\",low:\"1.520\",volume:114498087,amount:176229349,ticktime:\"14:38:42\",per:34.292,pb:1.414,mktcap:7065680.06044,nmc:4909967.795985,turnoverratio:0.36145},{symbol:\"sh600011\",code:\"600011\",name:\"华能国际\",trade:\"6.970\",pricechange:\"-0.070\",changepercent:\"-0.994\",buy:\"6.960\",sell:\"6.970\",settlement:\"7.040\",open:\"7.050\",high:\"7.100\",low:\"6.920\",volume:7672674,amount:53701011,ticktime:\"14:38:42\",per:63.364,pb:1.48,mktcap:10594667.25768,nmc:7318500,turnoverratio:0.07307},{symbol:\"sh600012\",code:\"600012\",name:\"皖通高速\",trade:\"6.100\",pricechange:\"0.120\",changepercent:\"2.007\",buy:\"6.090\",sell:\"6.100\",settlement:\"5.980\",open:\"5.980\",high:\"6.150\",low:\"5.930\",volume:4231540,amount:25678835,ticktime:\"14:38:42\",per:9.271,pb:1.049,mktcap:1011752.1,nmc:711016,turnoverratio:0.36304},{symbol:\"sh600015\",code:\"600015\",name:\"华夏银行\",trade:\"7.360\",pricechange:\"-0.020\",changepercent:\"-0.271\",buy:\"7.350\",sell:\"7.360\",settlement:\"7.380\",open:\"7.380\",high:\"7.390\",low:\"7.330\",volume:9816229,amount:72153306,ticktime:\"14:38:42\",per:4.973,pb:0.62,mktcap:9437497.376608,nmc:9437497.376608,turnoverratio:0.07655},{symbol:\"sh600016\",code:\"600016\",name:\"民生银行\",trade:\"5.830\",pricechange:\"0.010\",changepercent:\"0.172\",buy:\"5.820\",sell:\"5.830\",settlement:\"5.820\",open:\"5.830\",high:\"5.850\",low:\"5.810\",volume:27789426,amount:161901071,ticktime:\"14:38:42\",per:4.319,pb:0.567,mktcap:25525149.986666,nmc:20674417.833179,turnoverratio:0.07836},{symbol:\"sh600017\",code:\"600017\",name:\"日照港\",trade:\"3.220\",pricechange:\"-0.020\",changepercent:\"-0.617\",buy:\"3.220\",sell:\"3.230\",settlement:\"3.240\",open:\"3.240\",high:\"3.250\",low:\"3.210\",volume:4023718,amount:12988575,ticktime:\"14:38:39\",per:26.833,pb:0.916,mktcap:990360.551936,nmc:990360.551936,turnoverratio:0.13082},{symbol:\"sh600018\",code:\"600018\",name:\"上港集团\",trade:\"6.040\",pricechange:\"-0.040\",changepercent:\"-0.658\",buy:\"6.040\",sell:\"6.050\",settlement:\"6.080\",open:\"6.120\",high:\"6.120\",low:\"6.040\",volume:3730168,amount:22621896,ticktime:\"14:38:42\",per:12.133,pb:1.958,mktcap:13996899.4886,nmc:13996899.4886,turnoverratio:0.0161},{symbol:\"sh600019\",code:\"600019\",name:\"宝钢股份\",trade:\"7.660\",pricechange:\"0.050\",changepercent:\"0.657\",buy:\"7.660\",sell:\"7.670\",settlement:\"7.610\",open:\"7.680\",high:\"7.720\",low:\"7.560\",volume:42462806,amount:324571762,ticktime:\"14:38:42\",per:8.907,pb:1.008,mktcap:17057222.98575,nmc:16929432.58455,turnoverratio:0.19213},{symbol:\"sh600020\",code:\"600020\",name:\"XD中原高\",trade:\"3.660\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"3.650\",sell:\"3.660\",settlement:\"3.660\",open:\"3.680\",high:\"3.680\",low:\"3.650\",volume:5131144,amount:18806118,ticktime:\"14:38:36\",per:9.114,pb:0.808,mktcap:822538.090512,nmc:822538.090512,turnoverratio:0.22832},{symbol:\"sh600021\",code:\"600021\",name:\"上海电力\",trade:\"6.930\",pricechange:\"-0.020\",changepercent:\"-0.288\",buy:\"6.920\",sell:\"6.930\",settlement:\"6.950\",open:\"6.940\",high:\"6.980\",low:\"6.850\",volume:2282990,amount:15787512,ticktime:\"14:38:39\",per:17.995,pb:1.309,mktcap:1669892.404257,nmc:593101.808145,turnoverratio:0.26675},{symbol:\"sh600022\",code:\"600022\",name:\"山东钢铁\",trade:\"2.020\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"0.000\",sell:\"0.000\",settlement:\"2.020\",open:\"0.000\",high:\"0.000\",low:\"0.000\",volume:0,amount:0,ticktime:\"14:38:21\",per:11.49,pb:1.182,mktcap:2211203.022432,nmc:2159099.847614,turnoverratio:0},{symbol:\"sh600023\",code:\"600023\",name:\"浙能电力\",trade:\"4.750\",pricechange:\"-0.020\",changepercent:\"-0.419\",buy:\"4.740\",sell:\"4.750\",settlement:\"4.770\",open:\"4.750\",high:\"4.800\",low:\"4.730\",volume:4439590,amount:21141770,ticktime:\"14:38:39\",per:14.844,pb:1.057,mktcap:6460327.7443,nmc:6460327.7443,turnoverratio:0.03264},{symbol:\"sh600025\",code:\"600025\",name:\"华能水电\",trade:\"2.900\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"2.890\",sell:\"2.900\",settlement:\"2.900\",open:\"2.900\",high:\"2.920\",low:\"2.880\",volume:19670119,amount:57002344,ticktime:\"14:38:39\",per:20.714,pb:1.314,mktcap:5220000,nmc:522000,turnoverratio:1.09278},{symbol:\"sh600026\",code:\"600026\",name:\"XD中远海\",trade:\"4.070\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"4.070\",sell:\"4.080\",settlement:\"4.070\",open:\"4.070\",high:\"4.090\",low:\"4.050\",volume:2593300,amount:10553127,ticktime:\"14:38:39\",per:9.29,pb:0.596,mktcap:1641037.374427,nmc:1113565.374427,turnoverratio:0.09478},{symbol:\"sh600027\",code:\"600027\",name:\"华电国际\",trade:\"4.140\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"4.130\",sell:\"4.150\",settlement:\"4.140\",open:\"4.140\",high:\"4.190\",low:\"4.110\",volume:21920800,amount:90830631,ticktime:\"14:38:42\",per:94.091,pb:0.962,mktcap:4083272.334342,nmc:2808826.752348,turnoverratio:0.3231},{symbol:\"sh600028\",code:\"600028\",name:\"中国石化\",trade:\"6.350\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"6.340\",sell:\"6.350\",settlement:\"6.350\",open:\"6.330\",high:\"6.370\",low:\"6.300\",volume:41007886,amount:260021814,ticktime:\"14:38:42\",per:15.047,pb:1.031,mktcap:76880218.12521,nmc:60679184.61421,turnoverratio:0.04291},{symbol:\"sh600029\",code:\"600029\",name:\"南方航空\",trade:\"7.540\",pricechange:\"-0.140\",changepercent:\"-1.823\",buy:\"7.530\",sell:\"7.540\",settlement:\"7.680\",open:\"7.720\",high:\"7.750\",low:\"7.480\",volume:100446098,amount:759095546,ticktime:\"14:38:42\",per:12.567,pb:1.439,mktcap:7606482.647088,nmc:5295078.1,turnoverratio:1.43032},{symbol:\"sh600030\",code:\"600030\",name:\"中信证券\",trade:\"16.570\",pricechange:\"-0.010\",changepercent:\"-0.060\",buy:\"16.560\",sell:\"16.570\",settlement:\"16.580\",open:\"16.700\",high:\"16.750\",low:\"16.510\",volume:44311359,amount:735827747,ticktime:\"14:38:42\",per:17.628,pb:1.323,mktcap:20077717.2188,nmc:16262894.4369,turnoverratio:0.45148},{symbol:\"sh600031\",code:\"600031\",name:\"三一重工\",trade:\"8.460\",pricechange:\"0.050\",changepercent:\"0.595\",buy:\"8.450\",sell:\"8.460\",settlement:\"8.410\",open:\"8.410\",high:\"8.470\",low:\"8.330\",volume:36646650,amount:308137866,ticktime:\"14:38:42\",per:30.955,pb:2.417,mktcap:6546873.888828,nmc:6514299.431226,turnoverratio:0.47592},{symbol:\"sh600033\",code:\"600033\",name:\"福建高速\",trade:\"2.970\",pricechange:\"-0.010\",changepercent:\"-0.336\",buy:\"2.970\",sell:\"2.980\",settlement:\"2.980\",open:\"2.980\",high:\"2.980\",low:\"2.960\",volume:2427565,amount:7203488,ticktime:\"14:38:39\",per:12.422,pb:0.918,mktcap:815086.8,nmc:815086.8,turnoverratio:0.08846},{symbol:\"sh600035\",code:\"600035\",name:\"楚天高速\",trade:\"3.070\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"3.070\",sell:\"3.080\",settlement:\"3.070\",open:\"3.070\",high:\"3.110\",low:\"3.060\",volume:4545160,amount:14030444,ticktime:\"14:38:42\",per:9.029,pb:0.86,mktcap:531354.348361,nmc:457858.287718,turnoverratio:0.30476},{symbol:\"sh600036\",code:\"600036\",name:\"招商银行\",trade:\"26.200\",pricechange:\"0.160\",changepercent:\"0.614\",buy:\"26.200\",sell:\"26.210\",settlement:\"26.040\",open:\"26.280\",high:\"26.300\",low:\"25.970\",volume:21634897,amount:565778444,ticktime:\"14:38:39\",per:9.424,pb:1.43,mktcap:66075995.47462,nmc:54047834.40398,turnoverratio:0.10488},{symbol:\"sh600037\",code:\"600037\",name:\"歌华有线\",trade:\"10.020\",pricechange:\"0.020\",changepercent:\"0.200\",buy:\"10.020\",sell:\"10.030\",settlement:\"10.000\",open:\"10.020\",high:\"10.040\",low:\"9.940\",volume:2142208,amount:21403803,ticktime:\"14:38:39\",per:18.318,pb:1.084,mktcap:1394561.439768,nmc:1170688.730052,turnoverratio:0.18335},{symbol:\"sh600038\",code:\"600038\",name:\"中直股份\",trade:\"41.000\",pricechange:\"1.020\",changepercent:\"2.551\",buy:\"40.990\",sell:\"41.000\",settlement:\"39.980\",open:\"40.070\",high:\"41.240\",low:\"39.440\",volume:6210062,amount:251354156,ticktime:\"14:38:39\",per:53.074,pb:3.299,mktcap:2416854.5356,nmc:2416854.5356,turnoverratio:1.05349},{symbol:\"sh600039\",code:\"600039\",name:\"四川路桥\",trade:\"3.210\",pricechange:\"0.010\",changepercent:\"0.312\",buy:\"3.200\",sell:\"3.210\",settlement:\"3.200\",open:\"3.200\",high:\"3.220\",low:\"3.160\",volume:6930328,amount:22045146,ticktime:\"14:38:42\",per:9.704,pb:0.883,mktcap:1158978.68871,nmc:969334.187712,turnoverratio:0.2295},{symbol:\"sh600048\",code:\"600048\",name:\"保利地产\",trade:\"11.160\",pricechange:\"-0.050\",changepercent:\"-0.446\",buy:\"11.160\",sell:\"11.170\",settlement:\"11.210\",open:\"11.550\",high:\"11.630\",low:\"11.130\",volume:90735179,amount:1027811083,ticktime:\"14:38:42\",per:8.455,pb:1.267,mktcap:13234020.224076,nmc:13097756.487924,turnoverratio:0.77311},{symbol:\"sh600050\",code:\"600050\",name:\"中国联通\",trade:\"4.930\",pricechange:\"-0.020\",changepercent:\"-0.404\",buy:\"4.920\",sell:\"4.930\",settlement:\"4.950\",open:\"4.950\",high:\"4.960\",low:\"4.880\",volume:86620583,amount:425614811,ticktime:\"14:38:42\",per:262.234,pb:1.116,mktcap:15296711.161691,nmc:10449922.022735,turnoverratio:0.40865},{symbol:\"sh600051\",code:\"600051\",name:\"宁波联合\",trade:\"6.090\",pricechange:\"0.040\",changepercent:\"0.661\",buy:\"6.080\",sell:\"6.090\",settlement:\"6.050\",open:\"6.050\",high:\"6.090\",low:\"6.030\",volume:1598801,amount:9696693,ticktime:\"14:38:24\",per:3.904,pb:0.796,mktcap:189325.92,nmc:189325.92,turnoverratio:0.51428},{symbol:\"sh600052\",code:\"600052\",name:\"浙江广厦\",trade:\"3.730\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"0.000\",sell:\"0.000\",settlement:\"3.730\",open:\"0.000\",high:\"0.000\",low:\"0.000\",volume:0,amount:0,ticktime:\"14:38:33\",per:16.955,pb:1.367,mktcap:325177.331316,nmc:325177.331316,turnoverratio:0},{symbol:\"sh600053\",code:\"600053\",name:\"九鼎投资\",trade:\"16.570\",pricechange:\"0.220\",changepercent:\"1.346\",buy:\"16.550\",sell:\"16.570\",settlement:\"16.350\",open:\"16.320\",high:\"16.830\",low:\"16.070\",volume:1262366,amount:20948321,ticktime:\"14:38:30\",per:22.182,pb:3.556,mktcap:718377.1056,nmc:718377.1056,turnoverratio:0.29118},{symbol:\"sh600054\",code:\"600054\",name:\"黄山旅游\",trade:\"11.380\",pricechange:\"-0.020\",changepercent:\"-0.175\",buy:\"11.380\",sell:\"11.390\",settlement:\"11.400\",open:\"11.430\",high:\"11.450\",low:\"11.300\",volume:894303,amount:10174681,ticktime:\"14:38:33\",per:20.691,pb:2.044,mktcap:850427.4,nmc:584135.4,turnoverratio:0.17423},{symbol:\"sh600055\",code:\"600055\",name:\"万东医疗\",trade:\"11.440\",pricechange:\"0.550\",changepercent:\"5.051\",buy:\"11.430\",sell:\"11.440\",settlement:\"10.890\",open:\"10.850\",high:\"11.450\",low:\"10.810\",volume:4229273,amount:47431438,ticktime:\"14:38:42\",per:56.634,pb:3.348,mktcap:618693.731656,nmc:554666.112,turnoverratio:0.87229},{symbol:\"sh600056\",code:\"600056\",name:\"中国医药\",trade:\"18.910\",pricechange:\"0.190\",changepercent:\"1.015\",buy:\"18.900\",sell:\"18.910\",settlement:\"18.720\",open:\"18.700\",high:\"19.170\",low:\"18.650\",volume:9580487,amount:181122886,ticktime:\"14:38:39\",per:15.56,pb:2.468,mktcap:2020506.144794,nmc:1913949.297024,turnoverratio:0.94656},{symbol:\"sh600057\",code:\"600057\",name:\"厦门象屿\",trade:\"4.900\",pricechange:\"0.070\",changepercent:\"1.449\",buy:\"4.890\",sell:\"4.900\",settlement:\"4.830\",open:\"4.810\",high:\"4.940\",low:\"4.810\",volume:2665910,amount:13035481,ticktime:\"14:38:30\",per:9.608,pb:0.783,mktcap:1057152.50165,nmc:1047396.44779,turnoverratio:0.12472},{symbol:\"sh600058\",code:\"600058\",name:\"五矿发展\",trade:\"8.310\",pricechange:\"0.040\",changepercent:\"0.484\",buy:\"8.310\",sell:\"8.320\",settlement:\"8.270\",open:\"8.330\",high:\"8.330\",low:\"8.230\",volume:1191160,amount:9871754,ticktime:\"14:38:42\",per:258.075,pb:1.761,mktcap:890757.800841,nmc:890757.800841,turnoverratio:0.11112},{symbol:\"sh600059\",code:\"600059\",name:\"古越龙山\",trade:\"8.040\",pricechange:\"0.060\",changepercent:\"0.752\",buy:\"8.020\",sell:\"8.030\",settlement:\"7.980\",open:\"8.000\",high:\"8.060\",low:\"7.950\",volume:2725607,amount:21849444,ticktime:\"14:38:42\",per:40.2,pb:1.602,mktcap:650053.42866,nmc:650053.42866,turnoverratio:0.33711},{symbol:\"sh600060\",code:\"600060\",name:\"海信电器\",trade:\"12.150\",pricechange:\"-0.310\",changepercent:\"-2.488\",buy:\"12.140\",sell:\"12.150\",settlement:\"12.460\",open:\"12.480\",high:\"12.490\",low:\"11.980\",volume:31710867,amount:384317625,ticktime:\"14:38:39\",per:16.875,pb:1.125,mktcap:1589804.68473,nmc:1589804.68473,turnoverratio:2.42349},{symbol:\"sh600061\",code:\"600061\",name:\"国投资本\",trade:\"9.040\",pricechange:\"-0.060\",changepercent:\"-0.659\",buy:\"9.040\",sell:\"9.050\",settlement:\"9.100\",open:\"9.080\",high:\"9.130\",low:\"8.980\",volume:3949755,amount:35669827,ticktime:\"14:38:42\",per:13.294,pb:1.041,mktcap:3821325.273208,nmc:3339513.148552,turnoverratio:0.10692},{symbol:\"sh600062\",code:\"600062\",name:\"华润双鹤\",trade:\"24.030\",pricechange:\"0.220\",changepercent:\"0.924\",buy:\"24.030\",sell:\"24.080\",settlement:\"23.810\",open:\"23.730\",high:\"24.450\",low:\"23.730\",volume:3450982,amount:83221446,ticktime:\"14:38:42\",per:24.794,pb:2.693,mktcap:2089083.513474,nmc:1648542.436614,turnoverratio:0.50303},{symbol:\"sh600063\",code:\"600063\",name:\"皖维高新\",trade:\"2.620\",pricechange:\"-0.010\",changepercent:\"-0.380\",buy:\"2.610\",sell:\"2.620\",settlement:\"2.630\",open:\"2.630\",high:\"2.630\",low:\"2.590\",volume:4575598,amount:11954286,ticktime:\"14:38:33\",per:52.4,pb:1.073,mktcap:504584.409304,nmc:504584.409304,turnoverratio:0.23758},{symbol:\"sh600064\",code:\"600064\",name:\"南京高科\",trade:\"7.230\",pricechange:\"-0.030\",changepercent:\"-0.413\",buy:\"7.230\",sell:\"7.240\",settlement:\"7.260\",open:\"7.270\",high:\"7.300\",low:\"7.190\",volume:2419495,amount:17501577,ticktime:\"14:38:33\",per:5.902,pb:0.53,mktcap:893596.830024,nmc:893596.830024,turnoverratio:0.19576},{symbol:\"sh600066\",code:\"600066\",name:\"宇通客车\",trade:\"17.940\",pricechange:\"0.270\",changepercent:\"1.528\",buy:\"17.930\",sell:\"17.940\",settlement:\"17.670\",open:\"18.000\",high:\"18.350\",low:\"17.850\",volume:10173896,amount:183955391,ticktime:\"14:38:39\",per:12.723,pb:2.685,mktcap:3971806.966062,nmc:3971806.966062,turnoverratio:0.45954},{symbol:\"sh600067\",code:\"600067\",name:\"冠城大通\",trade:\"4.000\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"4.000\",sell:\"4.010\",settlement:\"4.000\",open:\"4.010\",high:\"4.050\",low:\"3.960\",volume:2698240,amount:10813880,ticktime:\"14:38:30\",per:10,pb:0.826,mktcap:596844.29,nmc:596844.29,turnoverratio:0.18083},{symbol:\"sh600068\",code:\"600068\",name:\"葛洲坝\",trade:\"6.890\",pricechange:\"0.040\",changepercent:\"0.584\",buy:\"6.890\",sell:\"6.900\",settlement:\"6.850\",open:\"6.850\",high:\"6.970\",low:\"6.800\",volume:19284708,amount:132401077,ticktime:\"14:38:39\",per:7.724,pb:1.25,mktcap:3172691.636868,nmc:3172691.636868,turnoverratio:0.4188},{symbol:\"sh600069\",code:\"600069\",name:\"银鸽投资\",trade:\"3.960\",pricechange:\"-0.030\",changepercent:\"-0.752\",buy:\"3.960\",sell:\"3.970\",settlement:\"3.990\",open:\"3.980\",high:\"4.050\",low:\"3.930\",volume:4822965,amount:19143291,ticktime:\"14:38:42\",per:99,pb:2.339,mktcap:643038.202224,nmc:643038.202224,turnoverratio:0.29701},{symbol:\"sh600070\",code:\"600070\",name:\"浙江富润\",trade:\"6.930\",pricechange:\"-0.040\",changepercent:\"-0.574\",buy:\"6.920\",sell:\"6.930\",settlement:\"6.970\",open:\"7.020\",high:\"7.030\",low:\"6.900\",volume:2008900,amount:13946039,ticktime:\"14:38:39\",per:21.656,pb:1.497,mktcap:361708.659774,nmc:278252.048844,turnoverratio:0.50033},{symbol:\"sh600071\",code:\"600071\",name:\"凤凰光学\",trade:\"11.320\",pricechange:\"-0.150\",changepercent:\"-1.308\",buy:\"11.300\",sell:\"11.320\",settlement:\"11.470\",open:\"11.370\",high:\"11.460\",low:\"11.230\",volume:717963,amount:8122925,ticktime:\"14:38:42\",per:80.857,pb:6.38,mktcap:268818.820192,nmc:268818.820192,turnoverratio:0.30234},{symbol:\"sh600072\",code:\"600072\",name:\"中船科技\",trade:\"9.090\",pricechange:\"-0.100\",changepercent:\"-1.088\",buy:\"9.090\",sell:\"9.100\",settlement:\"9.190\",open:\"9.220\",high:\"9.220\",low:\"9.070\",volume:4224680,amount:38541303,ticktime:\"14:38:42\",per:221.707,pb:1.827,mktcap:669251.143647,nmc:546107.90193,turnoverratio:0.7032},{symbol:\"sh600073\",code:\"600073\",name:\"上海梅林\",trade:\"6.880\",pricechange:\"0.170\",changepercent:\"2.534\",buy:\"6.880\",sell:\"6.890\",settlement:\"6.710\",open:\"6.700\",high:\"6.910\",low:\"6.640\",volume:10769758,amount:73165425,ticktime:\"14:38:42\",per:22.933,pb:1.745,mktcap:645157.876736,nmc:645157.876736,turnoverratio:1.14849},{symbol:\"sh600074\",code:\"600074\",name:\"*ST保千\",trade:\"1.430\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"1.420\",sell:\"1.430\",settlement:\"1.430\",open:\"1.410\",high:\"1.450\",low:\"1.410\",volume:8414387,amount:12021622,ticktime:\"14:38:36\",per:-0.451,pb:-1.003,mktcap:348617.705007,nmc:146725.597876,turnoverratio:0.82007},{symbol:\"sh600075\",code:\"600075\",name:\"新疆天业\",trade:\"6.230\",pricechange:\"-0.010\",changepercent:\"-0.160\",buy:\"6.220\",sell:\"6.230\",settlement:\"6.240\",open:\"6.250\",high:\"6.250\",low:\"6.180\",volume:3289460,amount:20436883,ticktime:\"14:38:30\",per:11.327,pb:1.347,mktcap:605881.425296,nmc:518584.999394,turnoverratio:0.39518},{symbol:\"sh600076\",code:\"600076\",name:\"康欣新材\",trade:\"4.640\",pricechange:\"-0.070\",changepercent:\"-1.486\",buy:\"4.630\",sell:\"4.640\",settlement:\"4.710\",open:\"4.730\",high:\"4.780\",low:\"4.620\",volume:13272560,amount:61987839,ticktime:\"14:38:42\",per:10.288,pb:1.36,mktcap:479898.555856,nmc:349101.831568,turnoverratio:1.76409},{symbol:\"sh600077\",code:\"600077\",name:\"宋都股份\",trade:\"3.350\",pricechange:\"-0.010\",changepercent:\"-0.298\",buy:\"3.350\",sell:\"3.360\",settlement:\"3.360\",open:\"3.360\",high:\"3.390\",low:\"3.330\",volume:2841100,amount:9537367,ticktime:\"14:38:33\",per:27.917,pb:1.198,mktcap:448940.97921,nmc:448940.97921,turnoverratio:0.212},{symbol:\"sh600078\",code:\"600078\",name:\"澄星股份\",trade:\"3.400\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"3.390\",sell:\"3.400\",settlement:\"3.400\",open:\"3.370\",high:\"3.420\",low:\"3.360\",volume:2043501,amount:6929937,ticktime:\"14:38:33\",per:37.778,pb:1.198,mktcap:225274.77274,nmc:225274.77274,turnoverratio:0.30842},{symbol:\"sh600079\",code:\"600079\",name:\"人福医药\",trade:\"12.330\",pricechange:\"0.210\",changepercent:\"1.733\",buy:\"12.330\",sell:\"12.340\",settlement:\"12.120\",open:\"12.120\",high:\"12.500\",low:\"12.110\",volume:10132411,amount:124850606,ticktime:\"14:38:39\",per:8.059,pb:1.429,mktcap:1669117.404366,nmc:1585698.493446,turnoverratio:0.78787},{symbol:\"sh600080\",code:\"600080\",name:\"金花股份\",trade:\"10.280\",pricechange:\"-0.070\",changepercent:\"-0.676\",buy:\"10.280\",sell:\"10.290\",settlement:\"10.350\",open:\"10.270\",high:\"10.340\",low:\"10.230\",volume:1294500,amount:13291439,ticktime:\"14:38:36\",per:58.709,pb:2.19,mktcap:383721.85298,nmc:313844.156416,turnoverratio:0.42401},{symbol:\"sh600081\",code:\"600081\",name:\"东风科技\",trade:\"10.080\",pricechange:\"-0.160\",changepercent:\"-1.563\",buy:\"10.080\",sell:\"10.090\",settlement:\"10.240\",open:\"10.300\",high:\"10.300\",low:\"9.950\",volume:1438608,amount:14578600,ticktime:\"14:38:36\",per:22.698,pb:2.43,mktcap:316068.48,nmc:316068.48,turnoverratio:0.4588},{symbol:\"sh600082\",code:\"600082\",name:\"XD海泰发\",trade:\"4.330\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"4.320\",sell:\"4.330\",settlement:\"4.330\",open:\"4.330\",high:\"4.380\",low:\"4.310\",volume:901600,amount:3899857,ticktime:\"14:38:33\",per:195.928,pb:1.649,mktcap:279768.152658,nmc:273890.600266,turnoverratio:0.14254},{symbol:\"sh600083\",code:\"600083\",name:\"博信股份\",trade:\"19.110\",pricechange:\"-0.040\",changepercent:\"-0.209\",buy:\"19.120\",sell:\"19.140\",settlement:\"19.150\",open:\"19.130\",high:\"19.230\",low:\"19.080\",volume:293600,amount:5620074,ticktime:\"14:38:39\",per:522.131,pb:62.594,mktcap:439530,nmc:435695.245986,turnoverratio:0.12878},{symbol:\"sh600084\",code:\"600084\",name:\"中葡股份\",trade:\"4.030\",pricechange:\"0.010\",changepercent:\"0.249\",buy:\"4.030\",sell:\"4.040\",settlement:\"4.020\",open:\"4.000\",high:\"4.120\",low:\"3.970\",volume:19595602,amount:79457572,ticktime:\"14:38:39\",per:-50.375,pb:1.957,mktcap:452861.91249,nmc:452861.91249,turnoverratio:1.7438},{symbol:\"sh600085\",code:\"600085\",name:\"同仁堂\",trade:\"36.290\",pricechange:\"0.720\",changepercent:\"2.024\",buy:\"36.280\",sell:\"36.290\",settlement:\"35.570\",open:\"35.580\",high:\"36.480\",low:\"35.400\",volume:9204541,amount:332429560,ticktime:\"14:38:42\",per:48.908,pb:5.727,mktcap:4977065.580798,nmc:4977065.580798,turnoverratio:0.67114},{symbol:\"sh600086\",code:\"600086\",name:\"东方金钰\",trade:\"10.040\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"0.000\",sell:\"0.000\",settlement:\"10.040\",open:\"0.000\",high:\"0.000\",low:\"0.000\",volume:0,amount:0,ticktime:\"14:38:21\",per:58.645,pb:4.054,mktcap:1355400,nmc:1061072.396064,turnoverratio:0},{symbol:\"sh600088\",code:\"600088\",name:\"中视传媒\",trade:\"9.490\",pricechange:\"-0.060\",changepercent:\"-0.628\",buy:\"9.490\",sell:\"9.520\",settlement:\"9.550\",open:\"9.530\",high:\"9.590\",low:\"9.450\",volume:847082,amount:8059425,ticktime:\"14:38:39\",per:38.266,pb:2.82,mktcap:377423.3736,nmc:377423.3736,turnoverratio:0.21299},{symbol:\"sh600089\",code:\"600089\",name:\"特变电工\",trade:\"6.680\",pricechange:\"-0.020\",changepercent:\"-0.299\",buy:\"6.680\",sell:\"6.690\",settlement:\"6.700\",open:\"6.780\",high:\"6.780\",low:\"6.660\",volume:8837484,amount:59276113,ticktime:\"14:38:39\",per:10.922,pb:0.876,mktcap:2481287.863052,nmc:2481160.943052,turnoverratio:0.23793},{symbol:\"sh600090\",code:\"600090\",name:\"同济堂\",trade:\"5.680\",pricechange:\"0.010\",changepercent:\"0.176\",buy:\"5.680\",sell:\"5.690\",settlement:\"5.670\",open:\"5.750\",high:\"5.750\",low:\"5.630\",volume:5047500,amount:28698151,ticktime:\"14:38:39\",per:15.778,pb:1.373,mktcap:817728.55276,nmc:399131.884072,turnoverratio:0.7183},{symbol:\"sh600091\",code:\"600091\",name:\"ST明科\",trade:\"3.670\",pricechange:\"0.040\",changepercent:\"1.102\",buy:\"3.670\",sell:\"3.680\",settlement:\"3.630\",open:\"3.640\",high:\"3.700\",low:\"3.610\",volume:759848,amount:2776508,ticktime:\"14:38:39\",per:367,pb:1.801,mktcap:160530.396308,nmc:123505.042,turnoverratio:0.22579},{symbol:\"sh600093\",code:\"600093\",name:\"易见股份\",trade:\"10.470\",pricechange:\"0.070\",changepercent:\"0.673\",buy:\"10.470\",sell:\"10.490\",settlement:\"10.400\",open:\"10.460\",high:\"10.660\",low:\"10.360\",volume:6965393,amount:73305556,ticktime:\"14:38:39\",per:14.342,pb:1.706,mktcap:1175202.5325,nmc:1175202.5325,turnoverratio:0.62055},{symbol:\"sh600094\",code:\"600094\",name:\"大名城\",trade:\"5.370\",pricechange:\"-0.010\",changepercent:\"-0.186\",buy:\"5.360\",sell:\"5.370\",settlement:\"5.380\",open:\"5.390\",high:\"5.430\",low:\"5.340\",volume:2688800,amount:14455611,ticktime:\"14:38:39\",per:9.414,pb:1.117,mktcap:1329249.555609,nmc:1222536.864594,turnoverratio:0.11811},{symbol:\"sh600095\",code:\"600095\",name:\"哈高科\",trade:\"4.100\",pricechange:\"0.030\",changepercent:\"0.737\",buy:\"4.090\",sell:\"4.100\",settlement:\"4.070\",open:\"4.080\",high:\"4.130\",low:\"4.060\",volume:2309000,amount:9476935,ticktime:\"14:38:30\",per:70.69,pb:2.028,mktcap:148118.06165,nmc:148118.06165,turnoverratio:0.63915},{symbol:\"sh600096\",code:\"600096\",name:\"云天化\",trade:\"5.080\",pricechange:\"0.040\",changepercent:\"0.794\",buy:\"5.070\",sell:\"5.080\",settlement:\"5.040\",open:\"5.070\",high:\"5.090\",low:\"5.040\",volume:2642653,amount:13384684,ticktime:\"14:38:42\",per:33.246,pb:1.809,mktcap:671260.602104,nmc:570042.0654,turnoverratio:0.2355},{symbol:\"sh600097\",code:\"600097\",name:\"开创国际\",trade:\"10.370\",pricechange:\"0.090\",changepercent:\"0.875\",buy:\"10.360\",sell:\"10.370\",settlement:\"10.280\",open:\"10.280\",high:\"10.390\",low:\"10.250\",volume:355500,amount:3668550,ticktime:\"14:38:21\",per:20.333,pb:1.686,mktcap:249851.211683,nmc:210094.023337,turnoverratio:0.17547},{symbol:\"sh600098\",code:\"600098\",name:\"广州发展\",trade:\"6.250\",pricechange:\"0.010\",changepercent:\"0.160\",buy:\"6.250\",sell:\"6.270\",settlement:\"6.240\",open:\"6.240\",high:\"6.270\",low:\"6.190\",volume:1171450,amount:7294985,ticktime:\"14:38:33\",per:25.11,pb:1.058,mktcap:1703872.84875,nmc:1703872.84875,turnoverratio:0.04297},{symbol:\"sh600099\",code:\"600099\",name:\"林海股份\",trade:\"6.340\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"6.320\",sell:\"6.340\",settlement:\"6.340\",open:\"6.420\",high:\"6.440\",low:\"6.310\",volume:1945645,amount:12363065,ticktime:\"14:38:39\",per:720.455,pb:2.952,mktcap:138922.08,nmc:138922.08,turnoverratio:0.88794},{symbol:\"sh600100\",code:\"600100\",name:\"同方股份\",trade:\"9.350\",pricechange:\"0.340\",changepercent:\"3.774\",buy:\"9.340\",sell:\"9.350\",settlement:\"9.010\",open:\"8.990\",high:\"9.380\",low:\"8.980\",volume:10691082,amount:98584526,ticktime:\"14:38:39\",per:267.143,pb:1.309,mktcap:2771245.519185,nmc:2771245.519185,turnoverratio:0.36071},{symbol:\"sh600101\",code:\"600101\",name:\"明星电力\",trade:\"7.050\",pricechange:\"0.060\",changepercent:\"0.858\",buy:\"7.040\",sell:\"7.050\",settlement:\"6.990\",open:\"7.000\",high:\"7.090\",low:\"6.960\",volume:1053002,amount:7400316,ticktime:\"14:38:21\",per:23.5,pb:1.085,mktcap:228546.178785,nmc:228546.178785,turnoverratio:0.32482},{symbol:\"sh600103\",code:\"600103\",name:\"青山纸业\",trade:\"2.970\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"2.960\",sell:\"2.970\",settlement:\"2.970\",open:\"2.970\",high:\"2.990\",low:\"2.950\",volume:2931610,amount:8687115,ticktime:\"14:38:42\",per:50,pb:1.573,mktcap:526790.683485,nmc:315366.9552,turnoverratio:0.27609},{symbol:\"sh600104\",code:\"600104\",name:\"上汽集团\",trade:\"34.940\",pricechange:\"0.550\",changepercent:\"1.599\",buy:\"34.930\",sell:\"34.940\",settlement:\"34.390\",open:\"34.570\",high:\"35.050\",low:\"34.530\",volume:11644250,amount:405507974,ticktime:\"14:38:42\",per:11.808,pb:1.742,mktcap:40822014.00931,nmc:40192994.402358,turnoverratio:0.10122},{symbol:\"sh600105\",code:\"600105\",name:\"永鼎股份\",trade:\"3.550\",pricechange:\"-0.010\",changepercent:\"-0.281\",buy:\"3.540\",sell:\"3.550\",settlement:\"3.560\",open:\"3.580\",high:\"3.580\",low:\"3.530\",volume:2386090,amount:8471241,ticktime:\"14:38:42\",per:11.452,pb:1.269,mktcap:444836.6621,nmc:351621.1384,turnoverratio:0.2409},{symbol:\"sh600106\",code:\"600106\",name:\"重庆路桥\",trade:\"2.970\",pricechange:\"0.010\",changepercent:\"0.338\",buy:\"2.970\",sell:\"2.980\",settlement:\"2.960\",open:\"2.970\",high:\"2.980\",low:\"2.960\",volume:2296087,amount:6817967,ticktime:\"14:38:39\",per:10.607,pb:0.827,mktcap:326215.24254,nmc:326215.24254,turnoverratio:0.20905},{symbol:\"sh600107\",code:\"600107\",name:\"美尔雅\",trade:\"6.050\",pricechange:\"-0.120\",changepercent:\"-1.945\",buy:\"6.050\",sell:\"6.060\",settlement:\"6.170\",open:\"6.170\",high:\"6.220\",low:\"6.000\",volume:2816000,amount:17066153,ticktime:\"14:38:42\",per:472.656,pb:3.958,mktcap:217800,nmc:217800,turnoverratio:0.78222},{symbol:\"sh600108\",code:\"600108\",name:\"亚盛集团\",trade:\"2.970\",pricechange:\"0.000\",changepercent:\"0.000\",buy:\"2.960\",sell:\"2.970\",settlement:\"2.970\",open:\"2.960\",high:\"2.990\",low:\"2.940\",volume:6732174,amount:19929446,ticktime:\"14:38:39\",per:59.046,pb:1.222,mktcap:578233.790937,nmc:578233.790937,turnoverratio:0.34579},{symbol:\"sh600109\",code:\"600109\",name:\"国金证券\",trade:\"7.060\",pricechange:\"-0.030\",changepercent:\"-0.423\",buy:\"7.060\",sell:\"7.070\",settlement:\"7.090\",open:\"7.090\",high:\"7.110\",low:\"7.020\",volume:8276995,amount:58476370,ticktime:\"14:38:42\",per:17.783,pb:1.119,mktcap:2135197.67286,nmc:2135197.67286,turnoverratio:0.27368},{symbol:\"sh600110\",code:\"600110\",name:\"诺德股份\",trade:\"5.310\",pricechange:\"-0.010\",changepercent:\"-0.188\",buy:\"5.300\",sell:\"5.310\",settlement:\"5.320\",open:\"5.290\",high:\"5.480\",low:\"5.270\",volume:20629098,amount:111289102,ticktime:\"14:38:36\",per:32.143,pb:2.921,mktcap:610815.723507,nmc:610815.723507,turnoverratio:1.79335},{symbol:\"sh600111\",code:\"600111\",name:\"北方稀土\",trade:\"10.970\",pricechange:\"0.070\",changepercent:\"0.642\",buy:\"10.970\",sell:\"10.980\",settlement:\"10.900\",open:\"10.900\",high:\"11.040\",low:\"10.820\",volume:26477880,amount:289385283,ticktime:\"14:38:42\",per:99.276,pb:4.462,mktcap:3985473.402,nmc:3985473.402,turnoverratio:0.7288},{symbol:\"sh600112\",code:\"600112\",name:\"天成控股\",trade:\"3.080\",pricechange:\"0.010\",changepercent:\"0.326\",buy:\"3.080\",sell:\"3.090\",settlement:\"3.070\",open:\"3.050\",high:\"3.100\",low:\"3.040\",volume:4970910,amount:15209697,ticktime:\"14:38:42\",per:79.381,pb:1.284,mktcap:156835.092568,nmc:156835.092568,turnoverratio:0.97621},{symbol:\"sh600113\",code:\"600113\",name:\"浙江东日\",trade:\"8.240\",pricechange:\"0.010\",changepercent:\"0.122\",buy:\"8.240\",sell:\"8.250\",settlement:\"8.230\",open:\"8.240\",high:\"8.350\",low:\"8.190\",volume:4486885,amount:37049856,ticktime:\"14:38:42\",per:27.467,pb:3.861,mktcap:262526.4,nmc:262526.4,turnoverratio:1.40831},{symbol:\"sh600114\",code:\"600114\",name:\"东睦股份\",trade:\"9.560\",pricechange:\"0.060\",changepercent:\"0.632\",buy:\"9.510\",sell:\"9.550\",settlement:\"9.500\",open:\"9.480\",high:\"9.580\",low:\"9.380\",volume:1415471,amount:13428823,ticktime:\"14:38:39\",per:13.278,pb:1.575,mktcap:617379.562076,nmc:600733.498876,turnoverratio:0.22526},{symbol:\"sh600115\",code:\"600115\",name:\"东方航空\",trade:\"6.200\",pricechange:\"-0.100\",changepercent:\"-1.587\",buy:\"6.200\",sell:\"6.210\",settlement:\"6.300\",open:\"6.300\",high:\"6.350\",low:\"6.150\",volume:45169742,amount:279870361,ticktime:\"14:38:42\",per:14.12,pb:1.62,mktcap:8969903.12284,nmc:6081261.12284,turnoverratio:0.46052},{symbol:\"sh600116\",code:\"600116\",name:\"三峡水利\",trade:\"6.230\",pricechange:\"-0.050\",changepercent:\"-0.796\",buy:\"6.220\",sell:\"6.230\",settlement:\"6.280\",open:\"6.280\",high:\"6.280\",low:\"6.210\",volume:2764654,amount:17241258,ticktime:\"14:38:39\",per:17.8,pb:2.231,mktcap:618642.427746,nmc:618642.427746,turnoverratio:0.27841},{symbol:\"sh600117\",code:\"600117\",name:\"西宁特钢\",trade:\"4.760\",pricechange:\"0.140\",changepercent:\"3.030\",buy:\"4.760\",sell:\"4.770\",settlement:\"4.620\",open:\"4.620\",high:\"4.870\",low:\"4.550\",volume:33974977,amount:160128041,ticktime:\"14:38:39\",per:79.333,pb:1.581,mktcap:497476.287952,nmc:352820.363952,turnoverratio:4.58366},{symbol:\"sh600118\",code:\"600118\",name:\"中国卫星\",trade:\"18.980\",pricechange:\"0.070\",changepercent:\"0.370\",buy:\"18.980\",sell:\"18.990\",settlement:\"18.910\",open:\"18.920\",high:\"19.150\",low:\"18.820\",volume:3559982,amount:67518667,ticktime:\"14:38:39\",per:54.229,pb:4.276,mktcap:2244364.37823,nmc:2244364.37823,turnoverratio:0.30106},{symbol:\"sh600119\",code:\"600119\",name:\"长江投资\",trade:\"8.000\",pricechange:\"-0.050\",changepercent:\"-0.621\",buy:\"8.010\",sell:\"8.020\",settlement:\"8.050\",open:\"8.010\",high:\"8.090\",low:\"7.990\",volume:2162635,amount:17393304,ticktime:\"14:38:36\",per:-26.667,pb:3.887,mktcap:245920,nmc:245920,turnoverratio:0.70352},{symbol:\"sh600120\",code:\"600120\",name:\"浙江东方\",trade:\"13.020\",pricechange:\"-0.150\",changepercent:\"-1.139\",buy:\"13.010\",sell:\"13.020\",settlement:\"13.170\",open:\"13.160\",high:\"13.270\",low:\"13.000\",volume:2880950,amount:37728745,ticktime:\"14:38:39\",per:11.836,pb:0.858,mktcap:1138453.297086,nmc:865991.996142,turnoverratio:0.43314},{symbol:\"sh600121\",code:\"600121\",name:\"郑州煤电\",trade:\"3.450\",pricechange:\"0.010\",changepercent:\"0.291\",buy:\"3.440\",sell:\"3.450\",settlement:\"3.440\",open:\"3.430\",high:\"3.460\",low:\"3.400\",volume:6445400,amount:22074247,ticktime:\"14:38:39\",per:5.583,pb:1.021,mktcap:350293.460925,nmc:350293.460925,turnoverratio:0.6348}]\n"
     ]
    }
   ],
   "source": [
    "import requests\n",
    "import json\n",
    "\n",
    "r = requests.get('http://vip.stock.finance.sina.com.cn/quotes_service/api/json_v2.php/Market_Center.getHQNodeData?page=1&num=10000&sort=symbol&asc=1&node=hs_a&symbol=&_s_r_a=sort')\n",
    "print(r.status_code)\n",
    "print(r.text)\n",
    "r_json = json.\n",
    "# print(r.content)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<!DOCTYPE html>\n",
      "<html>\n",
      "<head>\n",
      "    <title>PTS Full Test Summary Report</title>\n",
      "</head>\n",
      "<body>\n",
      "<center>\n",
      "    <font color=\"red\" size=\"5\">PTS Full Test Summary Report</font>\n",
      "</center>\n",
      "<br><style>p.indent{ padding-left: 19.6em; line-height: 0.0}</style>\n",
      "<p class=\"indent\"><b>SVN Revision: 4173</b></p>\n",
      "<p class=\"indent\"><b>FPGA Version: R07_15a_B0718a_b2_k7</b></p>\n",
      "<table border=\"1\" bgcolor=\"Lavender\" color=\"\" align=\"center\">\n",
      "<tr><th>Module Name</th><th>Case Number</th><th>Passed Number</th><th>Failed Number</th><th>Timeout Number</th><th>Skipped Number</th></tr>\n",
      "\n"
     ]
    }
   ],
   "source": [
    "import re\n",
    "\n",
    "class ArgsParameter(object):\n",
    "    def __init__(self):\n",
    "        self.module_name = 'all'\n",
    "        self.target_name = 'full'\n",
    "        self.flash_mode = 'xip'\n",
    "        self.svn_revision = '4173'\n",
    "        # self.svn_revision = None\n",
    "\n",
    "\n",
    "html_header_template = \"\"\"<!DOCTYPE html>\n",
    "<html>\n",
    "<head>\n",
    "    <title>PTS {0} Test Summary Report</title>\n",
    "</head>\n",
    "<body>\n",
    "<center>\n",
    "    <font color=\"red\" size=\"5\">PTS {0} Test Summary Report</font>\n",
    "</center>\n",
    "<br>\n",
    "<br>\n",
    "<table border=\"1\" bgcolor=\"Lavender\" color=\"\" align=\"center\">\n",
    "<tr><th>Module Name</th><th>Case Number</th><th>Passed Number</th><th>Failed Number</th><th>Timeout Number</th><th>Skipped Number</th></tr>\n",
    "\"\"\"\n",
    "\n",
    "args_param = ArgsParameter()\n",
    "\n",
    "if args_param is not None:\n",
    "    target_name = args_param.target_name\n",
    "    svn_revision = args_param.svn_revision\n",
    "\n",
    "    if svn_revision is None:\n",
    "        html_header = html_header_template.format(target_name.capitalize())\n",
    "    else:\n",
    "        fpga_version = 'R07_15a_B0718a_b2_k7'\n",
    "        html_header_template = re.sub(r'</center>\\n<br>\\n<br>',\n",
    "                                      r'</center>\\n<br><style>p.indent{{ padding-left: 19.6em; line-height: 0.0}}</style>\\n<p class=\"indent\"><b>SVN Revision: {1}</b></p>\\n<p class=\"indent\"><b>FPGA Version: {2}</b></p>',\n",
    "                                      html_header_template)\n",
    "        html_header = html_header_template.format(target_name.capitalize(), svn_revision, fpga_version)\n",
    "else:\n",
    "    html_header = html_header_template.format('Automation')\n",
    "\n",
    "print(html_header)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.1"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}

```



