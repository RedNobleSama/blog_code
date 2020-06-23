---
title: django继承默认User表
toc: true
mathjx: true
cover: /2018/06/01/django继承默认User表/head.png
date: 2018-06-01 11:52:18
update:
tags: ['Python']
categories:
  - Python
  - Django
---
### 继承默认User表
models.py中继承AbstractUser类
~~~Python
from django.contrib.auth.models import AbstractUser

class UserInfo(AbstractUser):
    """
    用户信息
    """
    nickname = models.CharField(verbose_name='昵称', max_length=32, null=True)
    telephone = models.CharField(max_length=11, blank=True, null=True, unique=True, verbose_name='手机号码')
    avatar = models.ImageField(verbose_name='头像', upload_to='avatar/', default="avatar/default.png")  # 保存图片
    create_time = models.DateTimeField(verbose_name='创建时间', auto_now_add=True)
~~~

并在settings.py中指明AUTH_USER_MODEL
~~~Python
AUTH_USER_MODEL = 'user.UserInfo'
~~~
