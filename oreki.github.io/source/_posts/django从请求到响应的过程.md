---
title: django从请求到响应的过程
toc: true
mathjx: true
cover: /2020/01/23/django从请求到响应的过程/head.png
tags:
  - Python
categories:
  - Python
  - Django
abbrlink: 26339
date: 2020-01-23 00:35:36
update:
---
#### Django请求生命周期的概念
从用户输入URL到用户看到网页的整个过程
##### 请求过程描述
1. 用户输入网址，浏览器发起请求。
2. WSGI（服务器网关接口）创建socket服务端，接受请求。
3. 中间件处理请求。
4. url路由，根据当前请求的url找到相应的视图函数。
5. 进入view，进行业务处理，执行类或者函数，返回字符串。
6. 再次通过中间件处理响应。
7. WSGI返回响应。
8. 浏览器渲染。

#### django启动
我们在启动一个django项目的时候，无论你是在命令行执行还是在pycharm直接点击运行，其实都是执行’runserver’的操作，而ruserver是使用django自带的的web server，主要用于开发和调试中，而在正式的环境中，一般会使用nginx+uwsgi模式。
无论是哪种方式，当启动一个项目，都会做2件事：
1. 创建一个WSGIServer类的实例，接受用户的请求。
2. 当一个用户的http请求到达的时，为用户指定一个WSGIHandler，用于处理用户请求与响应，这个Handler是处理整个request的核心。


#### WSGI
WSGI：全称是Web Server Gateway interface, WSGI不是服务器，也不用于与程序交互的API，更不是代码，而只是定义了一个接口，用于描述web server如何与web application通信的规范。

客户端发送一次请求后，最先处理请求的实际上是 web 服务器就是我们经常说的 nginx、Apache 这类的 web 服务器，然后web服务器再把请求交给web应用程序(如django)处理，这中间的中介就是WSGI，它把 web 服务器和 web 框架 (Django) 连接起来。

Django自带的wsgi服务是单线程的，适用于开发阶段。部署到生产环境时一般使用nginx代理服务器实现异步多线程(Apache是同步的，不适用于高并发处理),能处理理论高达五万并发请求。在Django2.0开始提出异步，在ORM做了初步使用，但不完全异步，3.0以后实现异步处理函数，基于Python的协程实现（不是greenlet这些第三方库，是asyncio库以及ansync/await关键字实现的）,使用的是Django特有的asgi服务器，这个服务器可以处理同步和异步请求。

Django异步多线程场配合celery框架来实现，celery是一个用python开发的分布式任务队列框架，支持线程/进程处理调度。


#### 中间件基本概念
顾名思义，中间件是位于Web服务器端和Web应用之间的，它可以添加额外的功能。当我们创建一个django项目(通过pycharm)，它会自动帮我们设置一些必要的中间件。
~~~python
MIDDLEWARE_CLASSES = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
~~~
中间件要么对来自用户的数据进行预处理，然后发送给应用；要么在应用将响应负载返回给用户之前，对结果数据进行一些最终的调整。

通俗一点，在django中，中间能够帮我们准备好request这个对象，然后应用可以直接使用request对象获取到各类数据，也帮我们将response添加头部，状态码等。
例如：
1. Django项目中默认启用了csrf保护,每次请求时通过CSRF中间件检查请求中是否有正确#token值

2. 当用户在页面上发送请求时，通过自定义的认证中间件，判断用户是否已经登陆，未登陆就去登陆。

3. 当有用户请求过来时，判断用户是否在白名单或者在黑名单里

其内置的五个方法:

1. process_request : 请求进来时,权限认证

2. process_view : 路由匹配之后,能够得到视图函数

3. process_exception : 异常时执行

4. process_template_responseprocess : 模板渲染时执行

5. process_response : 请求有响应时执行
