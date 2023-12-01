# network programming



## 网络编程



### 理论

* IPC，InterProcess Communation，进程间通信
* socket是最流行的一种跨平台IPC机制，在同一个系统下可能会有更高效的方式
* socket作为BSC Unix操作系统的一部分在伯克利诞生，称为因特网的基础。
* 创建套接字的方法：
  * 客户端：
    1. 创建一个类型的socket，`s = socket(AF_INET, SOCKET_STREAM)`。
    2. 连接服务端地址，自动分配地址，`s.connect(('www.python.com', 80))`。
  * 服务端：
    1. 创建一个类型的socket，`s = socket(AF_INET, SOCKET_STREAM)`。
    2. 绑定一个地址端口，`s.bind((socket.gethostname(), 80))`。
    3. 创建监听队列，默认最大声容量5个`s.listen()`。
  * 绑定地址：
    1. `socket.gethostname()`。获取本机名称，极端情况下才可能是网址。
    2. `s.bind(('localhost', 80))`。本机。
    3. `s.bind(('127.0.0.1', 80))`。本机。
    4. `s.bind(('0.0.0.0', 80)`。任意地址。
    5. `s.bind(('', 80)`。任意地址。
  * 服务端多连接处理方式:
    1. 使用线程处理新连接。
    2. 使用非阻塞方式轮询处理新连接。
    3. 使用select多路复用处理新连接。
  * 易错点、重点、要点：
    1. socket并未规定连接如何开始对话，需要自定设计。
    2. send只将内容写入缓冲区。如果想立即发送，需要调用flush。
    3. recv如果返回长度0（空数据串），则表示对端已经关闭。返回负数为阻塞，返回正数为数据，否则阻塞。
    4. send和recv可能只完成部分传输就返回，尤其在高负载的情况下。
    5. 断开连接之前应该调用`socket.shutdown()`，一般都会忽略。
    6. python垃圾回收时会自动调用`socket.close()`，但是如果不手动关闭可能导致对端被阻塞。
  * 四种消息结构：
    1. 消息具有固定长度。
    2. 消息有分界点，如换行、'\0'。
    3. 消息头规定了长度。
    4. 以关闭连接为消息结束。
  * 二进制流问题：
    * 要注意传输到网络的数序使用网络字节序。
      * n：network
      * h：host
      * s：16位short
      * l：32位long
      * ntohl, htonl, ntohs, htons
    * ascii可以使用1个字节存储，也有可能使用4字节存储，如果使用'\0'作为分界符，可能导致长度变化。
  * 阻塞与非阻塞：
    * 设置为非阻塞模式：socket.setblocking(False)
    * 影响函数：send、recv、connect、accept



### 底层接口 socket



#### 套接字编程指南

<https://docs.python.org/zh-cn/3/howto/sockets.html>



#### TCP

* 简单TCPClient


```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # AF_INET, IPv4，面向流的TCP协议, AF_INET6, IPv6
s.connect(('www.baidu.com', 80)) # 阻塞连接
s.send(b'GET / HTTP/1.1\r\nHost: www.baidu.com\r\nConnection: close\r\n\r\n') # 阻塞发送

buffer = []
while True:
    # 读数据长度
    d = s.recv(1024) # 阻塞读
    if d:
        buffer.append(d)
    else:
        break
data = b''.join(buffer)

# 关闭连接
s.close()
```

* 简单TCPServer


```python
import socket

def tcplink(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Success!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        sock.send(('Hello, %s' % data.decode('utf-8')).decode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 绑定IP、端口，1024以下端口需要管理员权限
s.bind(('0.0.0.0', 10086)
s.listen(5) # 监听
print('Waiting for connection...')
while True:
    # 接收连接，阻塞
    sock, addr = s.accept()
    t = threading.Thread(target=tcplink, args=(sock, addr))
    t.start()
```

* 带数据结构的TCPServer


```python
import socket
from threading import Thread
import time

class T(Thread):
    def __init__(self, client_socket, addr):
        self.client_socket = client_socket
        self.addr = addr
        self.data = b''
        Thread.__init__(self)

    def try_read(self, data):
        self.data = self.data + data

        l = self.data.split(sep=b',')
        if len(l) > 1:
            for i in range(len(l) - 1):
                self.client_socket.send(l[i]+b',')
                print('get data {}'.format(l[i]))
            self.data = l[-1]

    def run(self):
        while True:
            try:
                data = self.client_socket.recv(128)
                self.try_read(data)
                time.sleep(1)
            except Exception:
                break

IP = '0.0.0.0'
PORT = 888
ss = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
ss.bind((IP, PORT))
ss.listen(5)

while True:
    print('Listening on {}:{}'.format(IP, PORT))
    client_socket, addr = ss.accept()
    print('Accept {}'.format(addr))

    client_socket.send(b'Hello,World,Bye,Bye,Yingxuan')
    
    t = T(client_socket, addr)
    t.start()
```

* 带数据结构TCPClient


```python
#!/usr/bin/python3

import socket
import sys
import time

IP='192.168.4.11'
PORT=888

buffer = b''

def try_read(socket, data):
        global buffer
        buffer = buffer + data
        l = buffer.split(sep=b',')
        if len(l) > 1:
                for i in range(len(l)-1):
                        socket.send(l[i] + b',')
                        print('get data {}'.format(l[i]))
                buffer = l[-1]

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((IP, PORT))

while True:
        receive_data = s.recv(128)
        try_read(s, receive_data)
        time.sleep(1)
```



#### UDP

* UDP无绑定，不会新建一个client_socket
* UDP数据不保证顺序，不保证完整


* UDPServer示例


```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('127.0.0.1', 10086))
print('Bind UDP on 10086...')
while True:
    data, addr = s.recvfrom(1024)
    print('Received from %s:%s.' % addr)
    s.sendto(b'Hello, %s' % data, addr)
```

* UDPClient示例


```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
for data in [b'a', b'b', b'c']:
    s.sendto(data, ('127.0.0.1', 9999)
    print(s.recv(1024).decode('utf-8'))
s.close()
```



#### API

* python的socket是Unix系统调用和套接字接口的直译
* python读写操作封装了IO缓冲，较c接口更加高级
* 套接字地址与协议族相关
  * AF_UNIX为一个路径字符串
  * AF_INET为一个host, port的tuple，host是域名或者IPv4字符串，port是一个整数
    * ''代表INADDR_ANY
    * '<broadcast>'代表INADDR_BROADCAST
  * AF_INET6地址为(host, port, flowinfo, scopid)
* 其他支持的协议族还有：
  * AF_NETLINK
  * AF_TIPC，TIPC协议
  * AF_CAN
  * PF_SYSTEM
  * AF_BLUETOOTH，蓝牙通讯
  * AF_ALG，Linux连接内核加密算法
  * AF_VSOCK，虚拟机通讯
  * AF_PACKET，网卡底层接口
  * AF_QIPCRTR，高通平台协处理器通信
* 异常：
  * socket.error，OSError别名，已弃用
  * socket.herror，地址错误
  * socket.gaierror
  * socket.timeout，超时异常
* socket辅助API


```python
# socket构造函数定义
socket(family=AF_INET, type=SOCK_STREAM, proto=0, fileno=None)

# 使用SOCK_NONBLOCK标志位
socket(AF_INET, SOCK_STREAM | SOCK_NONBLOCK)

socketpair([family[, type[, proto]]])
# 返回一对已经连接的socket
# 默认类型为AF_UNIX，如果不支持则返回AF_INET
# 示例：
import socket
s1, s2 = socket.socketpair()
s1.send(b'xxx\n')
print(s2.recv(10))

create_connection(address[, timeout[, source_address]])
# 高级connect方法，TCP连接便捷方法
# 会自动解析dns
# 同时支持IPv4、IPv6

create_server(address, *, family=AF_INET, blocklog=None, reuse_port=False, dualstack_ipv6=False)
# 高级server方法，TCP监听便捷方法
# family只能设置为AF_INET或AF_INET6
# backlog是listen方法的参数


socket.gethostbyname(hostname)    # 主机名称转IP
socket.gethostbyname_ex(hostname) # 支持别名列表和IP列表
socket.gethostname()              # 获取当前主机名称
socket.sethostname(hostname)      # 设置当前主机名称，无权限会抛出异常
socket.getprotobyname('icmp')     # 根据协议名称获取常量定义
socket.getservbyport(port)

socket.inet_aton(ip_string)       # '1.1.1.1' 转 int
socket.inet_ntoa(packed_ip)       # int 转 '1.1.1.1'，不支持ipv6
socket.inet_pton                  # 支持ipv6
socket.inet_ntop                  # 支持ipv6
```

* socket对象API


```python
socket.accept()
socket.bind(address)
socket.close()                    # close()与with语句等效
socket.connect(address)
socket.connect_ex()
socket.detach()
socket.fileno()
socket.get_inheritable()          # 判断进程是否可以继承socket
socket.getpeername()              # 对端地址
socket.getsockname()              # 本端地址
socket.getblocking()              # 判断是否阻塞
socket.gettimeout()               # 0为非阻塞
socket.listen([backlog])          # 参数为等待队列，0为默认值
socket.makefile(mode='r', buffering=None, *, encoding=None, errors=None, newline=None)
socket.recv(bufsize[, flags])     # 返回数据
socket.recvfrom(bufsize[, flags]) # 返回数据、地址tuple
socket.send(bytes[, flags])       # 发送
socket.sendall(bytes[, flags)     # 全部发送，可能多次send，成功返回None
socket.sendto(bytes, address)     # 发送到地址，udp使用
socket.sendto(bytes, flags, address)
socket.sendfile(file, offset=0, count=None) # 发送文件，可指定长度
socket.setblocking(flag)          # flag = True/False
socket.settimeout(value)          # value = 0.0 非阻塞， None 阻塞
socket.shutdown(how)              # how=SHUT_RD不接收/SHUT_WR不发送/SHUT_RDWR
socket.share(process_id)          # 为进程复制socket
socket.family                     # 协议族，AddressFamily.AF_UNIX
socket.type                       # 类型，SocketKind.SOCK_STREAM
socket.proto                      # 协议，0
```



### 网络通信框架 socketserver

* <https://docs.python.org/zh-cn/3/library/socketserver.html>

* 该模块提供了以下方便使用的高级类：

  * 任务类：BaseRequestHandler

    * StreamRequestHandler(BaseRequestHandler)
    * DatagramRequestHandler(BaseRequestHandler)

  * 服务端类：BaseServer

    * TCPServer(BaseServer)
    * UDPServer(TCPServer)
    * UnixStreamServer(TCPServer)
    * UnixDatagramServer(UDPServer)
    * 需要注意这里继承关系为了实现方便，比较混乱

  * 并发MixIn类：

    * ForkingMixIn
    * ThreadingMixIn
    * 必须放在第一继承位置，不然方法会被覆盖。

  * 快捷集成类：

    * ForkingTCPServer
    * ForkingUDPServer
    * ThreadingTCPServer
    * ThreadingUDPServer

  * 该模块将创建一个Server类简化为：

    1. 创建服务端对象server
    2. 调用server.serve_forever()
    3. 如果不使用with语句，则应该手动调用server.server_close()

    * 原服务端循环阻塞在accept，被封装在server.serve_forever()。
    * 使用MinIn类继承可快速实现并发处理连接请求

  * 该模块将处理client消息简化为实现一个BaseRequestHandler.handle方法



#### API

```python
# 同步Server类默认会绑定和监听，除非设置bind_and_activate=False
TCPServer(server_address, RequestHandlerClass, bind_and_activate=True)
UDPServer(server_address, RequestHandlerClass, bind_and_activate=True)
UnixStreamServer(server_address, RequestHandlerClass, bind_and_activate=True)
UnixDatagramServer(server_address, RequestHandlerClass, bind_and_activate=True)

# 异步MixIn类默认会在server_close()时阻塞等待线程、进程退出
ForkingMixIn.server_close()
ThreadingMixIn.server_close()

# 通过设置 block_on_close=False 停止阻塞
ForkingMixIn.block_on_close = False # 3.7版本之后支持
ThreadingMixIn.block_on_close = False

# 通过设置 daemon_threads=True 启动监控线程，等待线程退出再退出Server。
ThreadingMixIn.daemon_threads = True 

# 基类定义
BaseServer(server_address, RequestHandlerClass):

BaseServer.fineno()           # 返回Linux文件句柄

BaseServer.handle_request() 
# handle_request() 依次调用 get_request(), verify_request(), process_request()
# 如果用户实现handle()抛出异常，则会调用 handle_error()
# 如果 timeout 超时，则会调用 handle_timeout()，然后返回

BaseServer.serve_forever(poll_interval=0.5)
# poll_interval是阻塞间隔时长，设置后会忽略timeout设置
# serve_forever()会阻塞直到显式调用shutdown()方法
# service_actions()会在server_forever()每次循环中调用，如ForkingMixIn.service_actions()清除僵尸进程。

BaseServer.service_actions()
BaseServer.shutdown()         # 必须在另一个线程调用，不然会出现死锁
BaseServer.server_close()     # 清理server
BaseServer.address_family     # AF_INET or AF_UNIX
BaseServer.socket_type        # SOCK_STREAM or SOCK_DGRAM
BaseServer.socket             # server socket
BaseServer.allow_reuse_address# 默认False
BaseServer.request_queue_size # listen 队列长度
BaseServer.timeout            # 超时

BaseServer.finish_request(request, client_address)
BaseServer.get_request()
BaseServer.handle_error(request, client_address)
BaseServer.handle_timeout()
BaseServer.process_request(request, client_address)
BaseServer.server_activate()
BaseServer.server_bind()
BaseServer.verify_request(request, client_address)


# Handler类
BaseRequestHandler()
BaseRequestHandler.setup()   # handle()调用前执行初始化
BaseRequestHandler.finish()  # socket清理前执行

BaseRequestHandler.handle()
BaseRequestHandler.request        # 获取request对象
BaseRequestHandler.client_address # 获取客户端地址
BaseRequestHandler.server         # 获取server对象

StreamRequestHandler()
DatagramRequestHandler()
# 实现了setup()和finish()方法。
# 提供了self.rfile，self.wfile可以使用读写方法访问
# rfile，wfile是io.BufferedIOBase对象
```



#### TCP示例

* TCP服务端

```python
import socketserver

class MyHandler(socketserver.BaseRequestHandler):
    def handle(self):
        self.data = self.request.recv(1024).strip()
        print("{} wrote:".format(self.client_address[0]))
        print(self.data)
        self.request.sendall(self.data.upper())

# 阻塞方式实现服务端
from socketserver import TCPServer

# 并发快捷类服务端
from socketserver import ThreadingTCPServer
from socketserver import ForkingTCPServer

# MinIn类实现服务端
from socketserver import ThreadingMixIn, TCPServer, ForkingMixIn
class MyThreadingTCPServer(ThreadingMixIn, TCPServer):
    pass

class MyForkingTCPServer(ForkingMixIn, TCPServer):
    pass

if __name__ == "__main__":
    HOST, PORT = "localhost", 9999

    # with TCPServer((HOST, PORT), MyHandler) as server:
    #     server.serve_forever()

    # with ThreadingTCPServer((HOST, PORT), MyHandler) as server:
    #     server.serve_forever()

    # with MyThreadingTCPServer((HOST, PORT), MyHandler) as server:
    #     server.serve_forever()

    # with ForkingTCPServer((HOST, PORT), MyHandler) as server:
    #     server.serve_forever()

    with MyForkingTCPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()
```

* TCP客户端

```python
import socket
import sys

HOST, PORT = "localhost", 9999
data = " ".join(sys.argv[1:])

# Create a socket (SOCK_STREAM means a TCP socket)
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    # Connect to server and send data
    sock.connect((HOST, PORT))
    sock.sendall(bytes(data + "\n", "utf-8"))

    # Receive data from the server and shut down
    received = str(sock.recv(1024), "utf-8")

print("Sent:     {}".format(data))
print("Received: {}".format(received))
```



#### UDP示例

* UDP服务端


```python
import socketserver

class MyHandler(socketserver.BaseRequestHandler):
    def handle(self):
        data = self.request[0].strip()
        socket = self.request[1]
        print("{} wrote:".format(self.client_address[0]))
        print(data)
        socket.sendto(data.upper(), self.client_address)

# 阻塞方式实现服务端
from socketserver import UDPServer

# 并发快捷类服务端
from socketserver import ThreadingUDPServer
from socketserver import ForkingUDPServer

# MinIn类实现服务端
from socketserver import ThreadingMixIn, UDPServer, ForkingMixIn
class MyThreadingUDPServer(ThreadingMixIn, UDPServer):
    pass

class MyForkingUDPServer(ForkingMixIn, UDPServer):
    pass

if __name__ == "__main__":
    HOST, PORT = "localhost", 9999

    with UDPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()

    with ThreadingUDPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()

    with ForkingUDPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()

    with MyThreadingUDPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()

    with MyForkingUDPServer((HOST, PORT), MyHandler) as server:
        server.serve_forever()
```

* UDP客户端

```python
import socket
import sys

HOST, PORT = "localhost", 9999
data = " ".join(sys.argv[1:])

# SOCK_DGRAM is the socket type to use for UDP sockets
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# As you can see, there is no connect() call; UDP has no connections.
# Instead, data is directly sent to the recipient via sendto().
sock.sendto(bytes(data + "\n", "utf-8"), (HOST, PORT))
received = str(sock.recv(1024), "utf-8")

print("Sent:     {}".format(data))
print("Received: {}".format(received))
```



#### Unix示例

* Unix服务端

```python
import socketserver

class MyHandler(socketserver.BaseRequestHandler):
    def handle(self):
        self.data = self.request.recv(1024).strip()
        # print("{} wrote:".format(self.client_address[0]))
        print(self.data)
        self.request.sendall(self.data.upper())

# 阻塞方式实现服务端
from socketserver import UnixStreamServer

# MinIn类实现服务端
from socketserver import ThreadingMixIn
class MyThreadingUnixStreamServer(ThreadingMixIn, UnixStreamServer):
    pass

from socketserver import ForkingMixIn
class MyForkingUnixStreamServer(ForkingMixIn, UnixStreamServer):
    pass

if __name__ == "__main__":
    FILE = './test.sock'

    import os
    if os.path.exists(FILE):
        os.unlink(FILE)

    # with UnixStreamServer(FILE, MyHandler) as server:
    #     server.serve_forever()

    # with MyThreadingUnixStreamServer(FILE, MyHandler) as server:
    #     server.serve_forever()

    with MyForkingUnixStreamServer(FILE, MyHandler) as server:
        server.serve_forever()
```



* Unix客户端

```python
import socket
import sys

FILE = './test.sock'
data = " ".join(sys.argv[1:])

with socket.socket(socket.AF_UNIX, socket.SOCK_STREAM) as sock:
    sock.connect(FILE)
    sock.sendall(bytes(data+'\n', 'utf-8'))
    received = str(sock.recv(1024), 'utf-8')
```



### 多路复用低级API，select

略



### 多路复用高级API，selector

* selectors是操作系统级操作的高级封装
* BaseSelector各种多路复用方法的抽象基类
* DefaultSelector是本平台最高效的实现
* BaseSelector子类
  * SelectSelector(BaseSelector)
  * PollSelector(BaseSelector)
  * EpollSelector(BaseSelector) ， 会使用一个文件描述符，用fileno()获取
  * DevpollSelector(BaseSelector)， 会使用一个文件描述符，用fileno()获取
  * KqueueSelector(BaseSelector)，会使用一个文件描述符，用fileno()获取
* 事件标记
  * selectors.EVENT_READ
  * selectors.EVENT_WRITE
* selectors.SelectorKey(namedtuple)
  * fileobj ， 文件对象
  * fd      ， 文件描述符
  * events  ， 等待的方法
  * data
* BaseSelector方法
  * BaseSelector.register(fileobj, events, data=None) -> SelectorKey
  * BaseSelector.unregister(fileobj) -> SelectorKey
  * BaseSelector.modify(fileobj, events, data=None) -> SelectorKey
  * BaseSelector.select(timeout=None)
    * 返回一个(key, events)的list
    * timeout <= 0 无阻塞
    * timeout > 0  阻塞最大时间，单位秒
    * timeout = None 阻塞直到有数据到达
  * BaseSelector.close()
  * BaseSelector.get_key(fileobj) 
  * BaseSelector.get_map()



```python
import selectors
import socket

sel = selectors.DefaultSelector()

def accept(sock, mask):
    conn, addr = sock.accept()  # Should be ready
    print('accepted', conn, 'from', addr)
    conn.setblocking(False)
    sel.register(conn, selectors.EVENT_READ, read)

def read(conn, mask):
    data = conn.recv(1000)  # Should be ready
    if data:
        print('echoing', repr(data), 'to', conn)
        conn.send(data)  # Hope it won't block
    else:
        print('closing', conn)
        sel.unregister(conn)
        conn.close()

sock = socket.socket()
sock.bind(('localhost', 1234))
sock.listen(100)
sock.setblocking(False)
sel.register(sock, selectors.EVENT_READ, accept)

while True:
    events = sel.select()
    for key, mask in events:
        callback = key.data
        callback(key.fileobj, mask)
```



## Web开发



### WSGI

* 200，成功
* 3xx，重定向
* 4xx，请求错误
* 5xx，服务器错误
* WSGI接口：Web Server Gateway Interface

```python
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])  #返回HEAD
    return [b'<h1>Hello, web!</h1>'] #返回BODY
```



#### WSGI服务器

```python
from wsgiref.simple_server import make_server
httpd = make_server('', 8000, application)
httpd.serve_forever()
```

