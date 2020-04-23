### Socket编程示例05--非阻塞超时和连接断开

http://python.jobbole.com/81860/

https://docs.python.org/zh-cn/3/howto/sockets.html

https://codeday.me/bug/20171014/84020.html

https://stackoverrun.com/cn/q/4554555

https://stackoverflow.com/questions/16745409/what-does-pythons-socket-recv-return-for-non-blocking-sockets-if-no-data-is-r

https://stackoverflow.com/questions/31141124/what-is-the-timeout-value-for-a-non-blocking-python-socket-when-reading

https://stackoverflow.com/questions/3432102/python-socket-connection-timeout

https://stackoverflow.com/questions/37870127/set-correctly-timeout-when-connection-fails-on-python-socket

https://stackoverflow.com/questions/45039993/python-socket-wont-timeout

https://stackoverflow.com/questions/11865685/handling-a-timeout-error-in-python-sockets

https://stackoverflow.com/questions/23642478/will-a-python-socket-timeout

https://stackoverflow.com/questions/34371096/how-to-use-python-socket-settimeout-properly

https://stackoverflow.com/questions/2719017/how-to-set-timeout-on-pythons-socket-recv-method



https://mozillazg.com/2013/11/python-socket-connect-set-timeout.html

https://www.programcreek.com/python/example/97/socket.error

https://www.networkcomputing.com/applications/python-network-programming-handling-socket-errors/1384050408

https://code.i-harness.com/zh-CN/q/ff83c1

https://blog.gfkui.cn/2018/06/15/Python%E5%AE%9E%E7%8E%B0socket%E7%9A%84%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8F%E7%BC%96%E7%A8%8B/index.html

https://oomake.com/question/105624

https://blog.csdn.net/xhw88398569/article/details/47102187

https://stackoverflow.com/questions/15705948/python-socketserver-timeout

* ##### 非阻塞超时

```
def receive_msg_from_client(self):
    json_str = None
    logging.info('Receive Client Message[{}, {}]...'.format(self.name, self.client_address))

    if not self.abnormal_flag:
        try:
            req_msg = self.request.recv(st_config.SOCKET_RECEIVE_BUFFER).decode(encoding='utf-8')
        except socket.timeout as err:
            logging.warning('Receive Client Message Error[{}, {}]: "{}"'.format(self.name, self.client_address, err))
        except socket.error as err:
            logging.warning('Receive Client Message Error[{}, {}]: "{}"'.format(self.name, self.client_address, err))
            self.abnormal_flag = True
        except Exception as err:
            logging.warning('Receive Client Message Error[{}, {}]: "{}"'.format(self.name, self.client_address, err))
            self.abnormal_flag = True
        else:
            if req_msg:
                logging.info('Receive Client Message[{}, {}]: {}'.format(self.name, self.client_address, req_msg))

                try:
                    json_str = json.loads(req_msg)
                except BaseException as err:
                    logging.warning('JSON Load Error[{}, {}]: "{}"'.format(self.name, self.client_address, err))
            else:
                # 如果req_msg为空，则表示连接断开
                logging.warning('Network Abnormal[{}, {}]: Disconnect'.
                                format(self.name, self.client_address, req_msg))
                self.abnormal_flag = True

    return json_str

```

* ##### 阻塞与非阻塞超时

https://stackoverflow.com/questions/16745409/what-does-pythons-socket-recv-return-for-non-blocking-sockets-if-no-data-is-r

In the case of a non blocking socket that has no data available, recv will throw the socket.error exception and the value of the exception will have the errno of either EAGAIN or EWOULDBLOCK. Example:

```
import sys
import socket
import fcntl, os
import errno
from time import sleep

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('127.0.0.1',9999))
fcntl.fcntl(s, fcntl.F_SETFL, os.O_NONBLOCK)

while True:
    try:
        msg = s.recv(4096)
    except socket.error, e:
        err = e.args[0]
        if err == errno.EAGAIN or err == errno.EWOULDBLOCK:
            sleep(1)
            print 'No data available'
            continue
        else:
            # a "real" error occurred
            print e
            sys.exit(1)
    else:
        # got a message, do something :)
```

The situation is a little different in the case where you've enabled non-blocking behavior via a time out with [`socket.settimeout(n)`](https://docs.python.org/2.7/library/socket.html#socket.socket.settimeout)or [`socket.setblocking(False)`](https://docs.python.org/2.7/library/socket.html#socket.socket.setblocking). In this case a socket.error is stil raised, but in the case of a time out, the accompanying value of the exception is always a string set to 'timed out'. So, to handle this case you can do:

```
import sys
import socket
from time import sleep

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('127.0.0.1',9999))
s.settimeout(2)

while True:
    try:
        msg = s.recv(4096)
    except socket.timeout, e:
        err = e.args[0]
        # this next if/else is a bit redundant, but illustrates how the
        # timeout exception is setup
        if err == 'timed out':
            sleep(1)
            print 'recv timed out, retry later'
            continue
        else:
            print e
            sys.exit(1)
    except socket.error, e:
        # Something else happened, handle error, exit, etc.
        print e
        sys.exit(1)
    else:
        if len(msg) == 0:
            print 'orderly shutdown on server end'
            sys.exit(0)
        else:
            # got a message do something :)
```

As indicated in the comments, this is also a more portable solution since it doesn't depend on OS specific functionality to put the socket into non-blockng mode.

See [recv\(2\)](http://linux.die.net/man/2/recv) and [python socket](https://docs.python.org/2.7/library/socket.html) for more details.

