---
title: 分布式消息队列celery
toc: true
mathjx: true
cover: /2020/04/21/分布式消息队列celery/head.png
abbrlink: 12455
date: 2020-04-21 21:23:38
update:
tags: ['Python']
categories:
  - Python
---

### Celery设计
celery由五大模块实现。

1. Task
就是任务，有异步任务和定时任务。

2. Broker
中间人，接收生产者发来的消息即Task，将任务存入队列。任务的消费者是Worker。Celery本身不提供队列服务，推荐用Redis或RabbitMQ实现队列服务。

3. Worker
执行任务的单元，它实时监控消息队列，如果有任务就获取任务并执行它。

4. Beat
定时任务调度器，根据配置定时将任务发送给Broler。

5. Backend
用于存储任务的执行结果。

![](组成关系.png)

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
首先编写一个文件 命名为task1.py
~~~Python
from celery import Celery
app = Celery('tasks',broker='redis://192.168.1.102:6379/0')
# redis://192.168.1.102:6379/0 是redis数据库地址，无需账号密码验证，也是ssrf在获取内网系统权限的方式之一
@app.task
def add(x,y):
    print('传递 {} + {} = {}'.format(x,y,x+y))
    return x+y
~~~
然后启动redis数据库
接下来再task1文件夹执行命令
~~~
celery -A task1 worker --loglevel=info
~~~
就会看到消息队列都启动
![](1.png)
到现在所有的队列都启动，可以向这个队列添加任务等待处理

方法是再task1目录下打开cmd窗口，进入python交互界面
~~~Python
from task1 import add
add.delay(6,12)
add.delay(6,6)
~~~

![](2.png)

#### 保存结果
上面只是一个发送任务的调用，结果是拿不到的。上面也没有接收返回值，这次把返回值保存到起来
修改task1内容
~~~Python
app = Celery('tasks',broker='redis://192.168.1.102:6379/0',backend='redis://192.168.1.102:6379/0')
~~~
然后要重启Worker，IDLE也要重启
然后这样就能获取结果了
~~~Python
t = add.delay(1, 1)
t.get()
# 还可以设置超时时间 t.get(timeout=5)

# 如果出错，获取错误结果，不触发异常
# 使用命令t.get(propagate=False)    
# t.traceback  （打印异常详细结果）

# 还可以获取任务状态
# t.ready() 返回True 或者False
~~~

### 在项目中使用celery

可以把celery配置成一个应用，假设应用名字是CeleryPro，目录格式如下：
CeleryPro
├─\__init.py
├─celery.py
├─tasks.py

这里的连接文件命名必须为celery.py，其他名字随意

#### celery文件
这个文件名必须是celery.py：
~~~Python
from __future__ import absolute_import, unicode_literals
from celery import Celery

app = Celery('CeleryPro',
             broker='redis://192.168.1.102:6379',
             backend='redis://192.168.1.102:6379',
             include=['CeleryPro.tasks'])
app.conf.update(
    result_expires=3600,
)

if __name__ == '__main__':
    app.start()
~~~

#### tasks文件
这个文件开始两行就多了一个点，这里要导入上面的celery.py文件。后面只要写各种任务加上装饰器就可以了：
~~~Python
from __future__ import absolute_import, unicode_literals
from .celery import app
import time

@app.task
def add(x, y):
    print("计算2个值的和: %s %s" % (x, y))
    return x+y

@app.task
def upper(v):
    for i in range(10):
        time.sleep(1)
        print(i)
    return v.upper()
~~~

#### 启动worker
这里注意用的都是CeleryPro：
~~~
celery -A CeleryPro worker -loglevel=info  # 前台启动不推荐
celery -A CeleryPro worker -l info  # 前台启动简写
celery multi start w1 -A  CeleryPro -l info  # 推荐用后台启动
~~~

#### 定时任务
主要修改 celery.py文件
~~~Python
from __future__ import absolute_import, unicode_literals
from celery import Celery
from celery.schedules import crontab
from datetime import timedelta

app = Celery('CeleryPro',
             broker='redis://192.168.1.102',
             backend='redis://192.168.1.102',
             include=['CeleryPro.tasks'])

app.conf.CELERYBEAT_SCHEDULE = {
    'add every 10 seconds': {
        'task': 'CeleryPro.tasks.add',
        'schedule': timedelta(seconds=10),  
        # 可以用timedelta对象
        # 'schedule': 10,  # 也支持直接用数字表示秒数
        'args': (1, 2)
    },
    'upper every 2 minutes': {
        'task': 'CeleryPro.tasks.upper',
        'schedule': crontab(minute='*/2'),
        'args': ('abc', ),
    },
}

# app.conf.CELERY_TIMEZONE = 'UTC'
app.conf.CELERY_TIMEZONE = 'Asia/Shanghai'

# Optional configuration, see the application user guide.
app.conf.update(
    CELERY_TASK_RESULT_EXPIRES=3600,
)

if __name__ == '__main__':
    app.start()
~~~

启动使用命令
~~~
celery -A CeleryPro beat -l info
celery -A CeleryPro worker -l info
~~~

### 新例子
~~~Python
# tasks.py
# coding:utf-8
from celery import Celery,platforms

app = Celery('tasks')
app.config_from_object('config')
platforms.C_FORCE_ROOT = True

@app.task
def add(x,y):
    return x + y
~~~
和另一个文件
~~~Python
# config.py
# coding:utf-8
from __future__ import absolute_import
from celery.schedules import crontab
from datetime import timedelta

BROKER_URL = 'redis://127.0.0.1:6379/0'

CELERYBEAT_SCHEDULE = {
    'add-every-2-seconds': {
        'task': 'tasks.add',
        'schedule': timedelta(seconds=2),
        'args': (16, 10),
    },
}

CELERY_TIMEZONE = 'Asia/Shanghai'
~~~
然后打开三个cmd窗口，依次输入：
~~~
celery -A tasks beat -l info
celery -A tasks worker -l info
celery -A tasks flower
~~~
然后访问本地5555端口即可~

#### 查看异步任务情况
Celery提供了一个工具flower，将各个任务的执行情况、各个worker的健康状态进行监控并以可视化的方式展现

安装flower:
~~~
pip install flower
~~~
启动flower（默认会启动一个webserver，端口为5555）

在另一个Terminal中：
~~~
celery -A task1 flower
~~~

这里的task1是上面创建的py文件

进入
~~~
http://localhost:5555
~~~
查看
