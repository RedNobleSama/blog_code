---
title: celery异步任务框架的使用
toc: true
mathjx: true
cover: /2020/04/21/celery异步任务框架的使用/head.png
abbrlink: 12455
date: 2020-04-21 21:23:38
update:
tags: ['Python']
categories:
  - Python
  - Django
---

### Celery设计
celery由三部分组成，消息中间件(message broker)、任务执行单元(worker)和任务执行结果(task result store)存储。

1. 消息中间件
celery本身不提供消息服务，但是可以和第三方提供的消息中间件集成。包括RabbitMQ, Redis。

2. 任务执行单元
worker是celery提供的任务执行单元，worker并发的运行在分布式的系统节点中。

3. 任务结果存储
Task result store用来存储Worker执行的任务的结果，Celery支持以不同方式存储任务的结果。

### Celery安装配置
Celery 4.0+及以后版本不支持在windows系统上运行。如果你希望在windows系统上使用celery, 有两种方法。
1.安装3.1.25版本
~~~
pip install celery==3.1.25
~~~
2.安装gevent
~~~
pip install gevent
# 启动worker
celery -A <module> worker -l info -P gevent
~~~

### 简单案例
