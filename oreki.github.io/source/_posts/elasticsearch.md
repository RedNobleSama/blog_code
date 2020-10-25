---
title: elasticsearch
toc: true
mathjx: true
cover: /2020/01/09/elasticsearch/head.png
tags:
  - Python
categories:
  - Python
abbrlink: 22534
date: 2020-01-09 11:02:27
update:
---

### elasticsearch

elasticsearch 原理
![](1.png)
1. 首先来看图中, 用户将全文搜索的请求发送至django, 即输入搜索内容
2. 全文搜索需要分词和模糊查询, 这些操作在mysql中也可以使用, 但如果遇到数据量大的项目, 效率会很低, 因此, 就需要借助搜索引擎elasticsearch
3. 要实现查询, 那么我们的django需要连接mysql和elasticsearch:
   1. 连接mysql使用的是mysqlclient
   2. 连接elasticsearch使用的是django-haystack, 以及python的es驱动
4. elasticsearch会去到mysql中获取数据, 然后进行索引, 并储存到它自己那里
5. 然后django就会利用haystack到elasticsearch中查询想要的数据, 即执行搜索
6. es查询到后返回给haystack(haystack是属于django项目中的一个从外部引入的app)
7. haystack会返回给django框架, 然后django再展示给用户看, 即展示搜索结果
