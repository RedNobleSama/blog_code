---
title: Django使用命令makemigrations提示NoChanges
toc: true
mathjx: true
cover: /2020/01/16/Django使用命令makemigrations提示NoChanges/head.png
date: 2020-01-16 01:12:49
update:
tags: ['排错记录']
categories:
  - Python
  - Django
---

在项目中，遇到models模型变动，变动后合并发生问题，故当时做了删除应用文件夹下migrations文件，由于数据库里无较多新数据，故删除后重建，但重建后执行模型合并操作结果为No Changes，不会对应用模型进行变动。

解决方法：

>执行python manage.py makemigrations --empty 应用名;
>执行python manage.py makemigrations;
>执行python manage.py migrate;
