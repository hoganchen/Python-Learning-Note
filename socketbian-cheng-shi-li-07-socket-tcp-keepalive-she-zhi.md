### Socket编程示例07--Socket TCP keepalive设置

```py
import socket

# http://www.jyguagua.com/?p=3066
# https://note.artchiu.org/2014/07/10/how-to-change-tcp-keepalive-timer-using-python-script/

# https://www.programcreek.com/python/example/4925/socket.SO_KEEPALIVE
# https://www.programcreek.com/python/example/6725/socket.SOL_TCP
# https://www.programcreek.com/python/example/57579/socket.SIO_KEEPALIVE_VALS

# https://stackoverflow.com/questions/12248132/how-to-change-tcp-keepalive-timer-using-python-script
# https://stackoverflow.com/questions/40779973/how-to-set-keep-alive-timer-in-python-2-5-running-in-windows-7

# https://blog.csdn.net/hengyunabc/article/details/44310193
'''
TCP_KEEPCNT 覆盖  tcp_keepalive_probes，默认9（次）
TCP_KEEPIDLE 覆盖 tcp_keepalive_time，默认7200（秒）
TCP_KEEPINTVL 覆盖 tcp_keepalive_intvl，默认75（秒）
'''

# https://www.cnblogs.com/lakeone/p/7204639.html
'''
最后一个参数1表示启用 KeepAlive。程序会使用系统默认的参数进行连接状态检测。

在 Debian 操作系统中，默认在连接 idle 7200 秒（/proc/sys/net/ipv4/tcp_keepalive_time）后才发送第一个 KeepAlive 状态检测包，整整两个小时，着实有点长，对有些应用场景不太适合。因此，我们通常需要调整触发 KeepAlive 的 idle 时间间隔：
1

s.setsockopt(socket.SOL_TCP, socket.TCP_KEEPIDLE, 10)

最后一个参数 10 表示在连接不活跃 10s 后开始 KeepAlive 检测。

开始 KeepAlive 检测之后，程序会每隔一定时间发送一次 KeepAlive 状态检测包，Debian 操作系统下默认是 75 秒（/proc/sys/net/ipv4/tcp_keepalive_intvl）发送一次，我们也可以在程序中定义这个发送间隔：
1

s.setsockopt(socket.SOL_TCP, socket.TCP_KEEPINTVL, 6)

最后一个参数表示每隔 6s 发送一次。

连接的另一方收到 KeepAlive 状态检测包后会发送一个响应包，表示还活着。如果对方未及时发送响应包，程序会对失败次数进行记录，Debian 操作系统中如果连续 9 次（/proc/sys/net/ipv4/tcp_keepalive_probe）失败，会认为对方已经失效，会触发连接异常操作，中断所有正在进行的操作，比如 recv 会返回 -1，并设置 error code 为 Broken Pipe。当然，我们也可以在程序中定义允许失败的次数：
1

s.setsockopt(socket.SOL_TCP, socket.TCP_KEEPCNT, 3)

在这个例子中，我们把它设置成了 3 次。
'''

# http://webkit.cc/post/tcp-http-keep-alive.html
'''
TCP中keep-alive的概念，是为了防止建立连接的双方因为长时间没有数据发送，以至于可能会意外断开连接，而定时发送的一个数据包，从而保证连接仍alive的一种手段。

keep-alive数据包是不携带实际数据(payload)的包，所以不会影响到上层应用。

所有平台的tcp实现，都支持keep-alive特性，但是默认是关闭了。可以修改内核参数来默认开启，也可以通过应用程序来开启。

以 Python 为例，只需给socket设置这几个参数即可：

import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)

client.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1) #开启keep-alive
client.setsockopt(socket.SOL_TCP, socket.TCP_KEEPIDLE, 3) #空闲多久发送keep-alive包
client.setsockopt(socket.SOL_TCP, socket.TCP_KEEPINTVL, 1) #多久循环一次
client.setsockopt(socket.SOL_TCP, socket.TCP_KEEPCNT, 10) # 最大重试次数，这里所示：如果发了10次keep-alive包，都没有收到响应，就可以认为连接已经断开
'''

def set_keepalive_linux(sock, after_idle_sec=1, interval_sec=3, max_fails=5):
    """Set TCP keepalive on an open socket.

    It activates after 1 second (after_idle_sec) of idleness,
    then sends a keepalive ping once every 3 seconds (interval_sec),
    and closes the connection after 5 failed ping (max_fails), or 15 seconds
    """
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPIDLE, after_idle_sec)
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPINTVL, interval_sec)
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPCNT, max_fails)

def set_keepalive_osx(sock, after_idle_sec=1, interval_sec=3, max_fails=5):
    """Set TCP keepalive on an open socket.

    sends a keepalive ping once every 3 seconds (interval_sec)
    """
    # scraped from /usr/include, not exported by python's socket module
    TCP_KEEPALIVE = 0x10
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
    sock.setsockopt(socket.IPPROTO_TCP, TCP_KEEPALIVE, interval_sec)

def set_keepalive_win(sock, after_idle_sec=1, interval_sec=3, max_fails=5):
    # 心跳检测,1表示启动，后面两个分别表示检测开始时间和间隔，单位为毫秒
    sock.ioctl(socket.SIO_KEEPALIVE_VALS, (1, 10000, 3000))
```

* ##### ioctl用法

```py
def set_tcp_timeout(stream):
    """
    Set kernel-level TCP timeout on the stream.
    """
    if stream.closed():
        return

    timeout = config.get('tcp-timeout', 30)
    timeout = int(parse_timedelta(timeout, default='seconds'))

    sock = stream.socket

    # Default (unsettable) value on Windows
    # https://msdn.microsoft.com/en-us/library/windows/desktop/dd877220(v=vs.85).aspx
    nprobes = 10
    assert timeout >= nprobes + 1, "Timeout too low"

    idle = max(2, timeout // 4)
    interval = max(1, (timeout - idle) // nprobes)
    idle = timeout - interval * nprobes
    assert idle > 0

    try:
        if sys.platform.startswith("win"):
            logger.debug("Setting TCP keepalive: idle=%d, interval=%d",
                         idle, interval)
            sock.ioctl(socket.SIO_KEEPALIVE_VALS, (1, idle * 1000, interval * 1000))
        else:
            sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
            try:
                TCP_KEEPIDLE = socket.TCP_KEEPIDLE
                TCP_KEEPINTVL = socket.TCP_KEEPINTVL
                TCP_KEEPCNT = socket.TCP_KEEPCNT
            except AttributeError:
                if sys.platform == "darwin":
                    TCP_KEEPIDLE = 0x10  # (named "TCP_KEEPALIVE" in C)
                    TCP_KEEPINTVL = 0x101
                    TCP_KEEPCNT = 0x102
                else:
                    TCP_KEEPIDLE = None

            if TCP_KEEPIDLE is not None:
                logger.debug("Setting TCP keepalive: nprobes=%d, idle=%d, interval=%d",
                             nprobes, idle, interval)
                sock.setsockopt(socket.SOL_TCP, TCP_KEEPCNT, nprobes)
                sock.setsockopt(socket.SOL_TCP, TCP_KEEPIDLE, idle)
                sock.setsockopt(socket.SOL_TCP, TCP_KEEPINTVL, interval)

        if sys.platform.startswith("linux"):
            logger.debug("Setting TCP user timeout: %d ms",
                         timeout * 1000)
            TCP_USER_TIMEOUT = 18  # since Linux 2.6.37
            sock.setsockopt(socket.SOL_TCP, TCP_USER_TIMEOUT, timeout * 1000)
    except EnvironmentError as e:
        logger.warning("Could not set timeout on TCP stream: %s", e)
```



