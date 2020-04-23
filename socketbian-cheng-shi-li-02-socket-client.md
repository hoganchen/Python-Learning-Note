### Socket编程示例02--Socket client

```
import time
import json
import socket

def client(ip, port, message):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        # sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        sock.connect((ip, port))
        json_str = json.dumps(message)
        sock.sendall(json_str.encode(encoding='utf-8'))
        response = sock.recv(1024).decode(encoding='utf-8')
        print("Received: {}".format(response))
        print(socket.gethostbyname(socket.gethostname()),
              socket.gethostbyaddr(socket.gethostname()), socket.gethostname())
        sock.close()

ip, port = '10.88.89.104', 5000
message = {"type": "info", "category": "heart", "message": "heart message"}

count = 10

while count > 0:
    client(ip, port, message)
    time.sleep(10)
    count -= 1





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

count = 10

while count > 0:
    client(socket_con, message)
    time.sleep(10)
    count -= 1
```



