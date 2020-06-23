---
title: Linux下安装redis以及开放远程连接
toc: true
mathjx: true
cover: /2018/07/03/Linux下安装redis以及开放远程连接/head.png
date: 2018-07-03 20:46:58
update:
tags: ['Linux']
categories: Linux
---

### 安装Redis
~~~bash
$ sudo apt update
$ sudo apt install redis-server
~~~

### 重启Redis
~~~bash
$ sudo service redis restart
~~~


### 查看运行状态
~~~bash
$ sudo systemctl status redis
~~~

### 设置远程连接
~~~bash
vim /etc/redis/redis.conf
~~~
将第70行bind注释掉，第90行的protected-mode改为no

然后重启Redis即可远程连接
