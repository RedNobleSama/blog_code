---
title: ubuntu安装mariadb
toc: true
mathjx: true
cover: /2020/05/12/ubuntu安装mariadb/head.png
tags:
  - Linux
categories:
  - Linux
abbrlink: 6886
date: 2020-05-12 17:40:28
update:
---

### 安装
~~~bash
sudo apt update
sudo apt install mariadb-server
~~~

### 验证
~~~bash
sudo systemctl status mariadb
~~~

### 初始化
~~~bash
sudo mysql_secure_installation
~~~

脚本将会提示你输入 root 密码：
> Enter current password for root (enter for none):

你会被问到是否 MySQL root 用户设置密码：
> Set root password? [Y/n]

你将会被要求移除匿名用户，限制 root 用户访问本地机器，移除测试数据库，并且重新加载权限表。
>Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y

### 允许远程连接
#### 修改表
~~~bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root';
flush privileges;
~~~

#### 修改配置文件
将/etc/mysql/mariadb.conf.d/50-server.cnf中bind-address = 127.0.0.1加# 注释掉

#### 重启服务
~~~bash
systemctl restart mariadb.service
~~~
