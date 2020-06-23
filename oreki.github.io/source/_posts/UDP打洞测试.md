---
title: UDP打洞测试
toc: true
mathjx: true
cover: /2018/09/13/UDP打洞测试/head.png
date: 2018-09-13 18:58:44
update:
tags: ['Python']
categories:
  - Python
  - 网络编程
---

UDP打洞实现了私有网络中的Internet主机之间建立双向UDP连接的方法。不过由于NAT的行为是非标准化的，这方法不能应用于所有类型的NAT。

基本上就是让位于NAT后的两台主机都连上某个拥有Public IP 的伺服器，当两个主机与服务器建立好UDP连线后，转成直接连线来传送资料，其原理简单来说是让NAT以为他连线的是原本的那台服务器主机。

下面是我用了python来实验UDP打洞的原始码。

### Server.py
~~~Python
import socket

# 建立 UDP Scoket
server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 监听所有IP的 3386 端口
listen_addr = ("", 3386)
server.bind(listen_addr)

# 储存IP用的 Array
ips = []

while True:
    # 接收数据
    data, addr = server.recvfrom(1024)
    print(data.decode('gbk'), addr, 'is connected.')
    # 將Client IP:Port 儲存到Array內
    ips.append(str(addr[0]) + ':' + str(addr[1]))

    # 当第二个client连上時，進行IP交換動作
    if len(ips) == 2:
        dest = ''
        for ip in ips:
            for i in ips:
                if ip != i:
                    dest = i  # 对方的IP
                # 將A的IP传給B，B的IP传給A
                    server.sendto(dest.encode('gbk'),(ip.split(':')[0],int(ip.split(':')[1])))
~~~

### ClientA.py
~~~Python
import socket
from threading import Thread
from time import sleep


client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

data = 'hello world A'
# 先连线到公开的服务器
addr = ("120.78.222.167", 3386)

def threaded_function(arg):
    for i in range(100):
        print ('Send ' , i ,'to ', arg)
        client.sendto(b'i', (arg.split(':')[0], int(arg.split(':')[1])))
        sleep(1)


client.sendto(data.encode('gbk'), addr)

# 接收返回的主机IP
dest,adr = client.recvfrom(1024)
print(dest.decode('gbk'))

#发送数据开始打洞
client.sendto(b'ping', (dest.decode('gbk').split(':')[0], int(dest.decode('gbk').split(':')[1])))
thread = Thread(target = threaded_function, args = (dest.decode('gbk'), ))
thread.start()

while True:
    data,adr = client.recvfrom(1024)
    print('Recv：', data, 'from：',adr)

~~~

### ClientB.py
~~~Python
import socket
from threading import Thread
from time import sleep


client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

data = 'hello world B'
# 先连线到公开的服务器
addr = ("120.78.222.167", 3386)

def threaded_function(arg):
    for i in range(100):
        print ('Send ' , i ,'to ', arg)
        client.sendto(b'i', (arg.split(':')[0], int(arg.split(':')[1])))
        sleep(1)


client.sendto(data.encode('gbk'), addr)

# 接收返回的主机IP
dest,adr = client.recvfrom(1024)
print(dest.decode('gbk'))

#发送数据开始打洞
client.sendto(b'ping', (dest.decode('gbk').split(':')[0], int(dest.decode('gbk').split(':')[1])))
thread = Thread(target = threaded_function, args = (dest.decode('gbk'), ))
thread.start()

while True:
    data,adr = client.recvfrom(1024)
    print('Recv：', data, 'from：',adr)

~~~

### 测试结果
![](result.png)
![](result2.png)
