### subprocess的使用

* ##### subprocess介绍

https://www.rddoc.com/doc/Python/3.6.0/zh/library/subprocess/\#module-subprocess

subprocess.call\(args, \*, stdin=None, stdout=None, stderr=None, shell=False, timeout=None\)



    运行 args 描述的命令。等待命令完成，然后返回 returncode 属性。



    这相当于:

    复制



    run\(...\).returncode



    （除了不支持 input 和 check 参数）



    上面显示的参数只是最常见的参数。完整的函数签名在很大程度上与 Popen 构造函数相同 - 该函数将除 timeout 之外的所有提供的参数直接传递到该接口。



    注解



    不要在此功能中使用 stdout=PIPE 或 stderr=PIPE。子进程将阻塞它是否生成足够的输出到管道以填充OS管道缓冲区，因为未从中读取管道。



    在 3.3 版更改: 加入 timeout。



subprocess.check\_call\(args, \*, stdin=None, stdout=None, stderr=None, shell=False, timeout=None\)



    运行带参数的命令。等待命令完成。如果返回码为零，那么返回，否则提出 CalledProcessError。 CalledProcessError 对象将具有 returncode 属性中的返回代码。



    这相当于:

    复制



    run\(..., check=True\)



    （除了不支持 input 参数）



    上面显示的参数只是最常见的参数。完整的函数签名在很大程度上与 Popen 构造函数相同 - 该函数将除 timeout 之外的所有提供的参数直接传递到该接口。



    注解



    不要在此功能中使用 stdout=PIPE 或 stderr=PIPE。子进程将阻塞它是否生成足够的输出到管道以填充OS管道缓冲区，因为未从中读取管道。



    在 3.3 版更改: 加入 timeout。



subprocess.check\_output\(args, \*, stdin=None, stderr=None, shell=False, encoding=None, errors=None, universal\_newlines=False, timeout=None\)



    使用参数运行命令并返回其输出。



    如果返回码非零，它会产生一个 CalledProcessError。 CalledProcessError 对象将具有 returncode 属性中的返回码和 output 属性中的任何输出。



    这相当于:

    复制



    run\(..., check=True, stdout=PIPE\).stdout



    上面显示的参数只是最常见的参数。完整的函数签名在很大程度上与 run\(\) 相同 - 大多数参数直接传递到该接口。但是，不支持明确传递 input=None 以继承父标准输入文件句柄。



    默认情况下，此函数将以编码字节返回数据。输出数据的实际编码可以取决于被调用的命令，因此对文本的解码通常需要在应用级处理。



    可以通过将 universal\_newlines 设置为如上所述的 常用参数 中的 True 来覆盖该行为。



    要捕获结果中的标准误差，请使用 stderr=subprocess.STDOUT:

    复制



    &gt;&gt;&gt; subprocess.check\_output\(

    ...     "ls non\_existent\_file; exit 0",

    ...     stderr=subprocess.STDOUT,

    ...     shell=True\)

    'ls: non\_existent\_file: No such file or directory\n'

```
>>> ipconfig_out = subprocess.run('ipconfig', timeout=5, stdout=subprocess.PIPE).stdout
>>> ipconfig_out
b'\r\nWindows IP \xc5\xe4\xd6\xc3\r\n\r\n\r\n\xd2\xd4\xcc\xab\xcd\xf8\xca\xca\xc5\xe4\xc6\xf7 \xb1\xbe\xb5\xd8\xc1\xac\xbd\xd3 2:\r\n\r\n   \xc3\xbd\xcc\xe5\xd7\xb4\xcc\xac  . . .
. . . . . . . . . : \xc3\xbd\xcc\xe5\xd2\xd1\xb6\xcf\xbf\xaa\r\n   \xc1\xac\xbd\xd3\xcc\xd8\xb6\xa8\xb5\xc4 DNS \xba\xf3\xd7\xba . . . . . . . : \r\n\r\n\xd2\xd4\xcc\xab\xcd\xf8\xc
a\xca\xc5\xe4\xc6\xf7 \xb1\xbe\xb5\xd8\xc1\xac\xbd\xd3:\r\n\r\n   \xc1\xac\xbd\xd3\xcc\xd8\xb6\xa8\xb5\xc4 DNS \xba\xf3\xd7\xba . . . . . . . : goodix.com\r\n   IPv4 \xb5\xd8\xd6\x
b7 . . . . . . . . . . . . : 10.0.2.15\r\n   \xd7\xd3\xcd\xf8\xd1\xda\xc2\xeb  . . . . . . . . . . . . : 255.255.255.0\r\n   \xc4\xac\xc8\xcf\xcd\xf8\xb9\xd8. . . . . . . . . . . .
 . : 10.0.2.2\r\n\r\n\xcb\xed\xb5\xc0\xca\xca\xc5\xe4\xc6\xf7 isatap.goodix.com:\r\n\r\n   \xc3\xbd\xcc\xe5\xd7\xb4\xcc\xac  . . . . . . . . . . . . : \xc3\xbd\xcc\xe5\xd2\xd1\xb6\
xcf\xbf\xaa\r\n   \xc1\xac\xbd\xd3\xcc\xd8\xb6\xa8\xb5\xc4 DNS \xba\xf3\xd7\xba . . . . . . . : goodix.com\r\n\r\n\xcb\xed\xb5\xc0\xca\xca\xc5\xe4\xc6\xf7 Teredo Tunneling Pseudo-I
nterface:\r\n\r\n   \xc3\xbd\xcc\xe5\xd7\xb4\xcc\xac  . . . . . . . . . . . . : \xc3\xbd\xcc\xe5\xd2\xd1\xb6\xcf\xbf\xaa\r\n   \xc1\xac\xbd\xd3\xcc\xd8\xb6\xa8\xb5\xc4 DNS \xba\xf3
\xd7\xba . . . . . . . : \r\n\r\n\xcb\xed\xb5\xc0\xca\xca\xc5\xe4\xc6\xf7 isatap.{86481C2F-98A5-400B-A8A4-11509855F240}:\r\n\r\n   \xc3\xbd\xcc\xe5\xd7\xb4\xcc\xac  . . . . . . . .
 . . . . : \xc3\xbd\xcc\xe5\xd2\xd1\xb6\xcf\xbf\xaa\r\n   \xc1\xac\xbd\xd3\xcc\xd8\xb6\xa8\xb5\xc4 DNS \xba\xf3\xd7\xba . . . . . . . : \r\n'
>>> ipconfig_out.encode()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'bytes' object has no attribute 'encode'
>>> ipconfig_out.decode('GB2312')
'\r\nWindows IP 配置\r\n\r\n\r\n以太网适配器 本地连接 2:\r\n\r\n   媒体状态  . . . . . . . . . . . . : 媒体已断开\r\n   连接特定的 DNS 后缀 . . . . . . . : \r\n\r\n以太网适配器 本
地连接:\r\n\r\n   连接特定的 DNS 后缀 . . . . . . . : goodix.com\r\n   IPv4 地址 . . . . . . . . . . . . : 10.0.2.15\r\n   子网掩码  . . . . . . . . . . . . : 255.255.255.0\r\n
默认网关. . . . . . . . . . . . . : 10.0.2.2\r\n\r\n隧道适配器 xx.xx.com:\r\n\r\n   媒体状态  . . . . . . . . . . . . : 媒体已断开\r\n   连接特定的 DNS 后缀 . . . . . . . :
 goodix.com\r\n\r\n隧道适配器 Teredo Tunneling Pseudo-Interface:\r\n\r\n   媒体状态  . . . . . . . . . . . . : 媒体已断开\r\n   连接特定的 DNS 后缀 . . . . . . . : \r\n\r\n隧道适配
器 isatap.{86481C2F-98A5-400B-A8A4-11509855F240}:\r\n\r\n   媒体状态  . . . . . . . . . . . . : 媒体已断开\r\n   连接特定的 DNS 后缀 . . . . . . . : \r\n'
>>> import chardect
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'chardect'
>>> import chardct
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'chardct'
>>> import chardet
>>> dir(chardet(
... )
... )
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'module' object is not callable
>>> dir(chardet)
['PY2', 'PY3', 'UniversalDetector', 'VERSION', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', '__version__',
'big5freq', 'big5prober', 'chardistribution', 'charsetgroupprober', 'charsetprober', 'codingstatemachine', 'compat', 'cp949prober', 'detect', 'enums', 'escprober', 'escsm', 'eucjpp
rober', 'euckrfreq', 'euckrprober', 'euctwfreq', 'euctwprober', 'gb2312freq', 'gb2312prober', 'hebrewprober', 'jisfreq', 'jpcntx', 'langbulgarianmodel', 'langcyrillicmodel', 'langg
reekmodel', 'langhebrewmodel', 'langthaimodel', 'langturkishmodel', 'latin1prober', 'mbcharsetprober', 'mbcsgroupprober', 'mbcssm', 'sbcharsetprober', 'sbcsgroupprober', 'sjisprobe
r', 'universaldetector', 'utf8prober', 'version']
>>> chardet.detect(ipconfig_out)
{'encoding': 'GB2312', 'confidence': 0.99, 'language': 'Chinese'}
```

* ##### 
* ##### 



