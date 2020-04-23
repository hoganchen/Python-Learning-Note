### Python Socket多线程编程基本用法

```
# -*- coding:utf-8 -*-

"""
@Author:    chenlianghong@goodix.com
@Date:      2017-09-01
@Version:   V0.1
"""

import time
import threading
import socket
import socketserver

'''
g_client_dict = {}


class ThreadedTCPRequestHandler(socketserver.BaseRequestHandler):
    def handle(self):
        data = str(self.request.recv(1024), 'ascii')
        cur_thread = threading.current_thread()
        response = bytes("{}: {}".format(cur_thread.name, data), 'ascii')
        self.request.sendall(response)

    # def setup(self):
    #     g_client_dict[self.client_address[0]] = self.client_address
    #     print("client ip address is: %s" % str(self.client_address))
    #     pass


class ThreadedTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    pass


def client(ip, port, message):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        sock.connect((ip, port))
        sock.sendall(bytes(message, 'ascii'))
        response = str(sock.recv(1024), 'ascii')
        print("Received: {}".format(response))
        # sock.close()


def main():
    global g_client_dict
    pass    # Port 0 means to select an arbitrary unused port
    host, port = "localhost", 20000

    server = ThreadedTCPServer((host, port), ThreadedTCPRequestHandler)
    with server:
        ip, port = server.server_address

        # Start a thread with the server -- that thread will then start one
        # more thread for each request
        server_thread = threading.Thread(target=server.serve_forever)
        # Exit the server thread when the main thread terminates
        server_thread.daemon = True
        server_thread.start()
        print("Server loop running in thread:", server_thread.name)

        client(ip, port, "Hello World 1")
        client(ip, port, "Hello World 2")
        client(ip, port, "Hello World 3")

        # print(g_client_dict)

        server.shutdown()
'''


class ThreadedTCPRequestHandler(socketserver.BaseRequestHandler):

    def handle(self):
        data = str(self.request.recv(1024), 'ascii')
        cur_thread = threading.current_thread()
        response = bytes("{}: {}".format(cur_thread.name, data), 'ascii')
        self.request.sendall(response)

    def finish(self):
        print("end socket connect with client ip: %s" % str(self.client_address))
        pass

    def setup(self):
        print("start socket connect with client ip: %s" % str(self.client_address))
        pass


class ThreadedTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    """
    This is because the previous execution has left the socket in a TIME_WAIT state, and can’t be immediately reused.
    There is a socket flag to set, in order to prevent this, socket.SO_REUSEADDR:
    """
    socketserver.TCPServer.allow_reuse_address = True
    pass


def client(ip, port, message):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        # sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        sock.connect((ip, port))
        sock.sendall(bytes(message, 'ascii'))
        response = str(sock.recv(1024), 'ascii')
        print("Received: {}".format(response))
        print(socket.gethostbyname(socket.gethostname()), socket.gethostbyaddr(socket.gethostname()), socket.gethostname())
        # sock.close()


def main():
    # Port 0 means to select an arbitrary unused port
    # host, port = "localhost", 0
    host, port = "172.16.49.142", 20000

    server = ThreadedTCPServer((host, port), ThreadedTCPRequestHandler)
    with server:
        ip, port = server.server_address

        print(ip, port)

        # Start a thread with the server -- that thread will then start one
        # more thread for each request
        server_thread = threading.Thread(target=server.serve_forever)
        # Exit the server thread when the main thread terminates
        server_thread.daemon = True
        server_thread.start()
        print("Server loop running in thread:", server_thread.name)

        client(ip, port, "Hello World 1")
        time.sleep(10)
        client(ip, port, "Hello World 2")
        time.sleep(10)
        client(ip, port, "Hello World 3")
        time.sleep(10)

        server.server_close()
        server.shutdown()


if __name__ == "__main__":
    time_start = time.time()
    main()
    print("\n\nTotal Elapsed Time: %s seconds\n" % (time.time() - time_start))

```



