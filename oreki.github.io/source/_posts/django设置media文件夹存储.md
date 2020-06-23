---
title: django设置media文件夹存储
toc: true
mathjx: true
cover: /2018/06/07/django设置media文件夹存储/head.png
date: 2018-06-07 11:51:29
update:
tags: ['Python']
categories:
  - Python
  - Django
---

### Media文件夹设置和使用

在settings.py文件中写入
~~~Python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
~~~

urls.py中写入路径
~~~Python
from BookShop.settings import MEDIA_ROOT # 从配置中导入MEDIA_ROOT
url(r'^media/(?P<path>.*)$', serve, {"document_root":MEDIA_ROOT})  # media文件夹路径
~~~

models.py中使用
~~~Python
avatar = models.ImageField(verbose_name='头像', upload_to='avatar/', default="avatar/default.png")  # 保存图片
~~~
