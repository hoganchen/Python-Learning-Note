### Socket编程示例06--Socket Server的连接超时设置和接收发送超时设置

* ##### 连接超时

[https://stackoverflow.com/questions/7354476/python-socket-object-accept-time-out](https://stackoverflow.com/questions/7354476/python-socket-object-accept-time-out)

```
import socket

tcpServer = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
tcpServer.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1) 
tcpServer.bind(('0.0.0.0', 10000)) # IP and PORT

stopped = False
while not stopped:
  try:
    tcpServer.settimeout(0.2) # timeout for listening
    tcpServer.listen(1) 
    (conn, (ip, port)) = tcpServer.accept() 
  except socket.timeout:
    pass
  except:
    raise
  else:
    # work with the connection, create a thread etc.
    ...
```

* ##### 接收发送超时

[https://stackoverflow.com/questions/15705948/python-socketserver-timeout](https://stackoverflow.com/questions/15705948/python-socketserver-timeout)

```
TIMEOUT = 3
HOST = '192.0.0.202'              
PORT = 2000             

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind((HOST, PORT))
s.listen(1)

while 1:
    conn, addr = s.accept()
    conn.settimeout(TIMEOUT)
    while 1:
        try:
            data = conn.recv(1024)
            #Do things

        except socket.timeout:
            #Timeout occurred, do things

        if not data or P=='end':
            print 'Connection lost. Listening for a new controller.' 
            break

conn.close()
```

* ##### IO阻塞操作多线程

https://python3-cookbook.readthedocs.io/zh\_CN/latest/c12/p01\_start\_stop\_thread.html

如果线程执行一些像I/O这样的阻塞操作，那么通过轮询来终止线程将使得线程之间的协调变得非常棘手。比如，如果一个线程一直阻塞在一个I/O操作上，它就永远无法返回，也就无法检查自己是否已经被结束了。要正确处理这些问题，你需要利用超时循环来小心操作线程。 例子如下：

```

class IOTask:
    def terminate(self):
        self._running = False

    def run(self, sock):
        # sock is a socket
        sock.settimeout(5)        # Set timeout period
        while self._running:
            # Perform a blocking I/O operation w/ timeout
            try:
                data = sock.recv(8192)
                break
            except socket.timeout:
                continue
            # Continued processing
            ...
        # Terminated
        return
```



