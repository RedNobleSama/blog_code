---
title: DRF解决vue请求跨域问题
toc: true
mathjx: true
cover: /2019/10/16/DRF解决vue请求跨域问题/head.png
tags:
  - Python
categories:
  - Python
  - Django
  - restframework
abbrlink: 14466
date: 2019-10-16 03:51:04
update:
---
### 什么是跨域
在浏览器上当前访问的网站向另一个网站发送请求获取数据的过程就是跨域请求。
跨域是浏览器同源策略造成；协议，域名，端口，三者有一不同就会产出跨域。

### 怎么解决跨域
前端JSONP和CORS

#### CORS(跨域资源共享)
它允许浏览器向跨源服务器发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

整个CORS通信过程都是浏览器自动完成的，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多一次附加的请求，但用户不会有感觉。因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。


#### django后端解决

##### 安装第三方包
pip install django-cors-headers


##### 配置settings.py文件
###### 在INSTALLED_APPS里添加 "corsheaders"
~~~Python
INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]
~~~
###### 在MIDDLEWARE_CLASSES添加中间件
~~~Python
MIDDLEWARE_CLASSES = (
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
  )
~~~
最好添加到第一行，要放在csrfview之前。

###### 配置白名单(配置settings.py)
~~~Python
# 单个配置
CORS_ORIGIN_WHITELIST = (
    '域名'
)

# 正则配置
CORS_ORIGIN_REGEX_WHITELIST = (r'^(https?://)?(\w+\.)?jim\.com $')
~~~

###### 允许所有主机跨域
~~~Python
CORS_ORIGIN_ALLOW_ALL = True
~~~

###### 允许携带cookie
~~~Python
CORS_ALLOW_CREDENTALS = True
~~~
