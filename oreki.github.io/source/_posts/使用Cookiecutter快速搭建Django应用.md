---
title: 使用Cookiecutter快速搭建Django应用
toc: true
mathjx: true
cover: /2018/03/16/使用Cookiecutter快速搭建Django应用/head.png
date: 2019-03-16 05:25:20
update:
tags: [Python, Django, Cookiecutter]
categories: Python, Django, Cookiecutter
---

## 安装Cookiecutter
在Python3环境中使用pip3安装：
~~~
$ yum install git  # 先在系统上安装git
$ pip3 install cookiecutter
$ ln -s /usr/local/python3/bin/cookiecutter /usr/bin/cookiecutter  # 创建软链接，/usr/local/python3/是Python3的安装路径
~~~
然后到你想要创建项目的目录，执行如下命令：
~~~
$ cookiecutter https://github.com/pydanny/cookiecutter-django.git
~~~

配置过程如下：
~~~
project_name [My Awesome Project]: myproject  # 项目名称
project_slug [myproject]: app01  # slug
description [Behold My Awesome Project!]: This is the first application!  # 项目描述
author_name [Daniel Roy Greenfeld]: __oreki__  # 作者
domain_name [example.com]:   # 部署的域名
email [__oreki__@example.com]:   # 邮箱
version [0.1.0]:  # 版本号，默认为0.1.0
Select open_source_license:  # 选择项目License
1 - MIT
2 - BSD
3 - GPLv3
4 - Apache Software License 2.0
5 - Not open source
Choose from 1, 2, 3, 4, 5 (1, 2, 3, 4, 5) [1]: 5
timezone [UTC]: Asia/Shanghai  # Django settings中的TIME_ZONE
windows [n]: n # 是否是Windows环境
use_pycharm [n]: y  # 是否使用Pycharm开发
use_docker [n]: y  # 是否使用Docker容器
Select postgresql_version:  # 选择Postgres数据库版本，cookiecutter-django默认只支持Postgres
1 - 10.5
2 - 10.4
3 - 10.3
4 - 10.2
5 - 10.1
6 - 9.6
7 - 9.5
8 - 9.4
9 - 9.3
Choose from 1, 2, 3, 4, 5, 6, 7, 8, 9 (1, 2, 3, 4, 5, 6, 7, 8, 9) [1]: 1
Select js_task_runner:  # js运行方式
1 - None
2 - Gulp
Choose from 1, 2 (1, 2) [1]: 1
custom_bootstrap_compilation [n]: n  # 是否自定义bootstrap压缩
use_compressor [n]: n  # 是否使用压缩
use_celery [n]: n  # 是否使用celery，一个异步任务队列
use_mailhog [n]: n  # 是否使用mailhog，Django项目中发送邮件的，也可以使用Mailgun代替
use_sentry [n]: n  # 是否使用错误日志日志监控，sentry也是不错的开源python项目
use_whitenoise [n]: y  # 是否使用whitenoise
use_heroku [n]: n  # 是否使用heroku，heroku是国外著名的云服务厂商之一，提供PaaS
use_travisci [n]: n  # 是否使用travisci，类似于jekins，用于DevOps中的持续集成与发布
keep_local_envs_in_vcs [y]: y  # 对于本地环境变量使用版本控制
debug [n]: y  # 是否开启debug模式，settings中配置
 [SUCCESS]: Project initialized, keep up the good work!
[root@shiyanlou ~]#
~~~

安装完成后，完整项目结构如下：
~~~
[root@shiyanlou ~]# cd app01/
[root@shiyanlou app01]# tree  # 如果没有，先yum install tree
.
├── app01
│   ├── conftest.py
│   ├── contrib
│   │   ├── __init__.py
│   │   └── sites
│   │       ├── __init__.py
│   │       └── migrations
│   │           ├── 0001_initial.py
│   │           ├── 0002_alter_domain_unique.py
│   │           ├── 0003_set_site_domain_and_name.py
│   │           └── __init__.py
│   ├── __init__.py
│   ├── static  # 静态文件
│   │   ├── css
│   │   │   └── project.css
│   │   ├── fonts
│   │   ├── images
│   │   │   └── favicons
│   │   │       └── favicon.ico
│   │   ├── js
│   │   │   └── project.js
│   │   └── sass
│   │       ├── custom_bootstrap_vars.scss
│   │       └── project.scss
│   ├── templates  # 模板
│   │   ├── 403_csrf.html
│   │   ├── 404.html
│   │   ├── 500.html
│   │   ├── account
│   │   │   ├── account_inactive.html
│   │   │   ├── base.html
│   │   │   ├── email_confirm.html
│   │   │   ├── email.html
│   │   │   ├── login.html
│   │   │   ├── logout.html
│   │   │   ├── password_change.html
│   │   │   ├── password_reset_done.html
│   │   │   ├── password_reset_from_key_done.html
│   │   │   ├── password_reset_from_key.html
│   │   │   ├── password_reset.html
│   │   │   ├── password_set.html
│   │   │   ├── signup_closed.html
│   │   │   ├── signup.html
│   │   │   ├── verification_sent.html
│   │   │   └── verified_email_required.html
│   │   ├── base.html
│   │   ├── pages
│   │   │   ├── about.html
│   │   │   └── home.html
│   │   └── users
│   │       ├── user_detail.html
│   │       ├── user_form.html
│   │       └── user_list.html
│   └── users  # 用户模块
│       ├── adapters.py
│       ├── admin.py
│       ├── apps.py
│       ├── forms.py
│       ├── __init__.py
│       ├── migrations
│       │   ├── 0001_initial.py
│       │   └── __init__.py
│       ├── models.py
│       ├── tests
│       │   ├── factories.py
│       │   ├── __init__.py
│       │   ├── test_forms.py
│       │   ├── test_models.py
│       │   ├── test_urls.py
│       │   └── test_views.py
│       ├── urls.py
│       └── views.py
├── compose  # docker compose
│   ├── local  # 开发环境
│   │   └── django
│   │       ├── Dockerfile
│   │       └── start
│   └── production  # 生产环境
│       ├── caddy  # caddy用于https部署
│       │   ├── Caddyfile
│       │   └── Dockerfile
│       ├── django  # django应用容器
│       │   ├── Dockerfile
│       │   ├── entrypoint
│       │   └── start
│       └── postgres  # 数据库容器
│           ├── Dockerfile
│           └── maintenance
│               ├── backup
│               ├── backups
│               ├── restore
│               └── _sourced
│                   ├── constants.sh
│                   ├── countdown.sh
│                   ├── messages.sh
│                   └── yes_no.sh
├── config  # Django的配置
│   ├── __init__.py
│   ├── settings
│   │   ├── base.py
│   │   ├── __init__.py
│   │   ├── local.py  # 开发环境
│   │   ├── production.py  # 生产环境
│   │   └── test.py  # 测试环境
│   ├── urls.py
│   └── wsgi.py
├── docs # 项目文档
│   ├── conf.py
│   ├── deploy.rst
│   ├── docker_ec2.rst
│   ├── index.rst
│   ├── __init__.py
│   ├── install.rst
│   ├── make.bat
│   ├── Makefile
│   └── pycharm
│       ├── configuration.rst
│       └── images
│           ├── 1.png
│           ├── 2.png
│           ├── 3.png
│           ├── 4.png
│           ├── 7.png
│           ├── 8.png
│           ├── f1.png
│           ├── f2.png
│           ├── f3.png
│           ├── f4.png
│           ├── issue1.png
│           └── issue2.png
├── locale
│   └── README.rst
├── local.yml
├── manage.py
├── merge_production_dotenvs_in_dotenv.py
├── production.yml
├── pytest.ini
├── README.rst
├── requirements  # 包和模块
│   ├── base.txt  # 必备的
│   ├── local.txt  # 开发环境，可能包含一些测试用的包
│   └── production.txt  # 生产环境
└── setup.cfg

34 directories, 109 files
[root@shiyanlou app01]#
~~~

## 包环境管理
### pipenv创建环境
~~~
[root@shiyanlou ~]# cd app01/
[root@shiyanlou app01]# pipenv --python 3.7
Creating a virtualenv for this project…
Pipfile: /root/app01/Pipfile
Using /usr/bin/python3 (3.7.2) to create virtualenv…
⠙ Creating virtual environment...Using base prefix '/usr/local/python3'
New python executable in /root/.local/share/virtualenvs/app01-DYWCxUWF/bin/python3
Also creating executable in /root/.local/share/virtualenvs/app01-DYWCxUWF/bin/python
Installing setuptools, pip, wheel...
done.
Running virtualenv with interpreter /usr/bin/python3
✔ Successfully created virtual environment!
Virtualenv location: /root/.local/share/virtualenvs/app01-DYWCxUWF
Creating a Pipfile for this project…
[root@shiyanlou app01]#
~~~

### 安装包并lock
安装前先将requirements/local.txt中的
~~~
psycopg2==2.7.4 --no-binary psycopg2  # https://github.com/psycopg/psycopg2
~~~
改成
~~~
psycopg2 --no-binary psycopg2  # https://github.com/psycopg/psycopg2
~~~
否则会安装失败。
~~~
[root@shiyanlou app01]# pipenv install -r requirements/local.txt --skip-lock
Requirements file provided! Importing into Pipfile…
Installing dependencies from Pipfile…
  ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 28/28 — 00:00:13
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
[root@shiyanlou app01]#
~~~
如何有提示An error occurred while installing xxx，重新运行一遍就行了，因为pipenv不稳定。
