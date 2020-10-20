---
title: python批量重命名
toc: true
mathjx: true
cover: /2018/05/20/python批量重命名/head.png
tags:
  - Python
categories:
  - Python
  - Python基础
abbrlink: 51704
date: 2018-05-20 21:59:39
update:
---

看代码就很明了啦~
~~~python
import os
def rename(j):
    i=0
    path="E:/finalData/"+str(j)+"/phone"
    filelist=os.listdir(path)#该文件夹下所有的文件（包括文件夹）
    for files in filelist:#遍历所有文件
        i=i+1
        Olddir=os.path.join(path,files);#原来的文件路径                
        if os.path.isdir(Olddir):#如果是文件夹则跳过
                continue
        filename=os.path.splitext(files)[0];#文件名
        filetype=os.path.splitext(files)[1];#文件扩展名
        Newdir=os.path.join(path,str(i)+filetype);#新的文件路径
        os.rename(Olddir,Newdir)#重命名
for j in range(7):
    rename(j)
~~~
![](1.png)

解释下用到的os库中的函数

#### os.listdir()
os.listdir() 方法用于返回指定的文件夹包含的文件或文件夹的名字的列表。这个列表以字母顺序。 它不包括 ‘.’ 和’..’ 即使它在文件夹中。
只支持在 Unix, Windows 下使用。

#### os.path.join()
是在拼接路径的时候用的,可以根据系统自动选择正确的路径分隔符”/“或”\”
举个例子，
os.path.join(“home”, “me”, “mywork”)
在Linux系统上会返回
“home/me/mywork”
在Windows系统上会返回
“home\me\mywork”

#### os.path.splitext()
作用 ：分离文件名与扩展名；默认返回(fname,fextension)元组，可做分片操作 。如：

>os.path.splitext(‘d:\library\book.txt’)(‘d:\library\book’, ‘.txt’)

>os.path.splitext(‘book.txt’)(‘book’, ‘.txt’)

#### os.rename()
用于命名文件或目录，从 src 到 dst,如果dst是一个存在的目录, 将抛出OSError。
