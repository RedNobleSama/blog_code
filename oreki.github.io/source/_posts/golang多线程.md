---
title: golang多线程
toc: true
mathjx: true
cover: /2020/09/25/golang多线程/head.png
tags:
  - Golang
categories:
  - Golang
  - Golang基础
abbrlink: 47486
date: 2020-09-25 15:28:56
update:
---

### golang多线程

#### 注意
go是默认一个核只在一个时间点只可以运行一个线程 如果要实现并发 需要告诉go我们允许同时使用多个核 这样才可以实现真正意义上的并发

#### goroutine
