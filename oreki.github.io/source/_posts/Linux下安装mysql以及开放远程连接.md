---
title: Linux下安装mysql以及开放远程连接
toc: true
mathjx: true
cover: /2018/06/09/Linux下安装mysql以及开放远程连接/head.png
date: 2018-06-09 20:46:46
update:
tags: ['Linux']
categories: Linux
---

### 安装MYSQL
~~~Bash
sudo apt-get update
sudo apt-get install mysql-server
~~~

### 初始化环境
~~~Bash
sudo mysql_secure_installation
~~~

~~~Bash
#1
VALIDATE PASSWORD PLUGIN can be used to test passwords...
Press y|Y for Yes, any other key for No: N (我的选项)

#2
Please set the password for root here...
New password: (输入密码)
Re-enter new password: (重复输入)

#3
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them...
Remove anonymous users? (Press y|Y for Yes, any other key for No) : N (我的选项)

#4
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network...
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : Y (我的选项)

#5
By default, MySQL comes with a database named 'test' that
anyone can access...
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : N (我的选项)

#6
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y (我的选项)
~~~

### 远程访问
编辑文件
~~~Bash
$ vim /etc/mysql/mysql.conf.d/mysqld.cnf
~~~
把下面内容注释掉
~~~Bash
bind-address           = 127.0.0.1
~~~

进入MYSQL
查看用户权限
~~~sql
select user,host from mysql.user;
~~~

旧版
~~~sql
grant all on *.* to root@'%' identified by 'root' with grant option;
flush privileges;
~~~

新版
8.0版本更换了权限设置和密码加密协议，因此会报出42000错误：
~~~sql
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'IDENTIFIED BY 'password' WITH GRANT OPTION' at line 1
~~~

解决方法：
~~~sql
mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL ON db1.* TO 'root'@'%';
mysql> FLUSH PRIVILEGES;
~~~


重启Mysql
~~~Bash
sudo /etc/init.d/mysql restart
~~~
