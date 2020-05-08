---
layout: post
author: kele
date: "2019-10-16"
title: "python 按行读取 socket "
math: false
tags:
  - python

---

# Python 按行读取 socket 套接字

## socket api makefile 
[文档位置](https://docs.python.org/3.7/library/socket.html?highlight=makefile#socket.socket.makefile_)

很好用，但是速度很慢，所以如果程序需要实时性，很难保证   

[bugs位置](https://bugs.python.org/issue18329)

我使用测试脚本，发现 makefile 比 fopen 慢一半   

```python
'''
sock ==> socket 套接字类型
'''
rfile = sock.makefile()
print(rfile.readline())
rfile.close()
```


## 手动处理
```python
'''
sock  ==> socket 套接字
'''

last = b''
def readlines():
    datas = sock.recv(4096)
    data_list = (last+data).split("\r\n")
    last = data_list[-1]
    return data_list[:-1]
```


## fopen 处理

```python
rfile = os.fopen(os.dup(sock.fileno()),"rb",4096*2) # "rb" 一定要二进制读，因为 socket 是字节流
data = rfile.readline()
print(data)
sock.close()
rfile.close()
```



** 注意 **:最好还是用 os.dup 复制一个文件描述符出来 

## 奇怪的问题
```python
#server
import socket
import os
import time
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(("0.0.0.0",5000))
sock.listen(1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEPORT,1)

while True:
    conn , client_address = sock.accept()
    print("conn's fd is {}".format(conn.fileno()))
    rfile = os.fdopen(conn.fileno(),"rb",1024)
    while True:
        data = rfile.readline()
        print(data)
        conn.close()
        time.sleep(1)
        break
```

```python
#cilent
#!/usr/bin/env python
import socket

address = ("192.168.3.20",5000)


sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

sock.connect(address)

sock.send(b"test data\r\n")
```


当连接到达时，系统会自动分配一个最小的文件描述符给 socket，比如我电脑上是 4 。当我们读取一行数据之后，关闭 socket，close api 会把 socket 底层的文件描述符直接关闭掉，但是由于 tcp 四次挥手中 TIME_WAIT 状态的存在，所以socket依然存活 2MSL。 `TIME_WAIT` 状态的套接字主要是用来接收无效的数据包的，不允许读，只允许写，而这段时间内 4 这个文件描述符只允许写，不允许读。     

所以我们当我们重新 accept 接受到一个连接之后，系统自动分配一个最小的文件描述符仍然是 4(因为我们调用 close ，关闭了 4)，这时我们尝试读取文件描述符 4 ，但是文件描述符目前不可读，所以接下来我们调用 rfile.readline(),读取失败，抛异常 OSERROR


简单地说就是: 系统设置了关闭的套接字所对应的的文件描述符 2MSL 内无法读取

解决方法：
不手动操作 socket 对应的文件描述符   
1. 不处理，让系统自动处理失效的 socket。这样断开的 socket 系统会自动回收文件描述符，并且销毁socket。新来的连接，创建 socekt之后，会自动选择一个最小的文件描述符来绑定。这样我们就不会用文件描述符访问到上次失效的 socket 了
2. 使用 shutdown ，shutdown 不会关闭套接字的文件描述符，它只作用于套接字
3. fopen(os.dup(conn.fileno),"rb",1024)，dup出来的文件描述符没有读权限的问题，可以正常读取

```python
# 第一种方法
import socket
import os
import time
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(("0.0.0.0",5000))
sock.listen(1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEPORT,1)

while True:
    conn , client_address = sock.accept()
    print("conn's fd is {}".format(conn.fileno()))
    rfile = os.fdopen(conn.fileno(),"rb",1024)
    while True:
        data = rfile.readline()
        print(data)
        time.sleep(1)
        break
    # 我们多次用client 发送数据，会发现不一样的文件描述符，或者说是几个文件描述符轮换用
```

```python
#第二种方法
import socket
import os
import time
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(("0.0.0.0",5000))
sock.listen(1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEPORT,1)

while True:
    conn , client_address = sock.accept()
    print("conn's fd is {}".format(conn.fileno()))
    rfile = os.fdopen(conn.fileno(),"rb",1024)
    while True:
        data = rfile.readline()
        print(data)
        shutdown(conn.fileno(),socket.SHUT_RDWR)
        time.sleep(1)
        break
```

但本质是一样的，都是不可以直接操作 socket 对应
```python
# 使用 dup复制文件描述符
import socket
import os
import time
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEPORT,1)    
sock.bind(("0.0.0.0",5000))
sock.listen(1)


while True:
    conn , client_address = sock.accept()
    print("conn's fd is {}".format(conn.fileno()))
    copyfd = os.dup(conn.fileno())
    print("dup's fd is {}".format(copyfd))
    rfile = os.fdopen(copyfd,"rb",1024)
    while True:
        data = rfile.readline()
        conn.close()
        rfile.close()
        print(data)
        time.sleep(1)
        break
```



