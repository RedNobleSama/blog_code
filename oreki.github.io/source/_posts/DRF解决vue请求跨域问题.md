---
title: DRF解决vue请求跨域问题
toc: true
mathjx: true
cover: /2019/10/16/DRF解决vue请求跨域问题/head.png
date: 2019-10-16 03:51:04
update:
tags: ['Python']
categories:
  - Python
  - Django
  - restframework
---
### 什么是跨域
在浏览器上当前访问的网站向另一个网站发送请求获取数据的过程就是跨域请求。

### django后端解决

#### 安装第三方包
pip install django-cors-headers


#### 配置settings.py文件
##### 在INSTALLED_APPS里添加 "corsheaders"
~~~Python
INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]
~~~
##### 在MIDDLEWARE_CLASSES添加中间件
~~~Python
MIDDLEWARE_CLASSES = (
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
  )
~~~
最好添加到第一行，要放在csrfview之前。

##### 配置白名单(配置settings.py)
~~~Python
# 单个配置
CORS_ORIGIN_WHITELIST = (
    '域名'
)

# 正则配置
CORS_ORIGIN_REGEX_WHITELIST = (r'^(https?://)?(\w+\.)?jim\.com $')
~~~

##### 允许所有主机跨域
~~~Python
CORS_ORIGIN_ALLOW_ALL = True
~~~

##### 允许携带cookie
~~~Python
CORS_ALLOW_CREDENTALS = True
~~~
