---
title: nginx+uwsgi部署django项目步骤
toc: true
mathjx: true
cover: /2019/12/21/nginx+uwsgi部署django项目步骤/head.png
date: 2019-12-21 01:13:10
update:
tags: ['运维']
categories:
  - 运维
  - 项目部署
---

### 项目部署
#### 项目环境准备
安装启动nginx
~~~bash
sudo apt-get install nginx
sudo /etc/init.d/nginx restart
~~~
验证
打开浏览器输入: 127.0.0.1:80 -> Welcome to Nginx

安装uwsgi
~~~bash
sudo pip3 install uwsgi
~~~

#### 部署配置
##### 1. 在项目目录中新建文件SourcelistUwsgi.ini文件
  在配置文件中写入如下内容：
  ~~~ini
  [uwsgi]
  # 指定和nginx通信的端口
  socket=127.0.0.1:8001
  # 项目路径
  chdir=/home/sourcelist/project/Sourcelist
  # wsgi.py路径
  wsgi-file=Sourcelist/wsgi.py
  # 进程数
  processes=4
  # 线程数
  thread=2
  # uwsgi自身占用端口
  stats=127.0.0.1:8080
  ~~~
##### 2. 配置nginx
~~~bash
cd /etc/nginx/sites-enabled/
vi projectNginx.conf
~~~
内容：
~~~conf
server{
        # 指定本项目监听端口,浏览器输入端口
        listen 80;
        # 域名
        server_name 127.0.0.1:80;
        # 指定字符集
        charset utf-8;
        # 指定收集静态文件路径
        location /static{
            alias /home/sourcelist/project/Sourcelist/static;
          }
        # 指定收集媒体文件路径
        location /media{
          alias /home/sourcelist/project/Sourcelist/media;
          }
        # 和uwsgi通信端口和通信文件
        location /{
          include uwsgi_params;
          uwsgi_pass 127.0.0.1:8001;
      }            
    }
~~~

###### 3. 拷贝uwsgi_params到项目根目录
~~~bash
cd /etc/nginx
cp uwsgi_params /home/sourcelist/project/Sourcelist
~~~

###### 4. 改掉nginx默认的server(80)
~~~bash
cd /etc/nginx/sites-enabled
vi deault #把listen的端口由80改为800
~~~
>server {
        listen 800 default_server;
        listen [::]:80 default_server;

###### 5. 重启nginx服务
~~~bash
sudo /etc/init.d/nginx restart
~~~

###### 6. 收集静态文件
1. 在settings.py文件中添加路径(STATIC_ROOT)
STATIC_ROOT = '/home/sourcelist/project/Sourcelist/static'
2. 收集静态文件
~~~bash
cd /home/sourcelist/project/Sourcelist
python3 manage.py collectstatic
~~~

###### 7. uwsgi启动项目
在项目根目录启动项目
~~~bash
uwsgi --ini fruitdayUwsgi.ini
~~~
