---
title: python文件操作
toc: true
mathjx: true
cover: /2018/02/14/python文件操作/head.png
tags:
  - Python
categories:
  - Python
  - Python基础
abbrlink: 9583
date: 2018-02-14 23:32:58
update:
---
## 基本操作
### open（） 打开或者创建一个文件 and close() 关闭文件
open
>格式：open('文件路径','打开模式')
    返回值：文件io对象
>    打开模式一共N种：
>        w模式 写模式write  文件不存在时会创建文件，如果文件已存在则会清空文件
        r模式  读模式read  文件不存在就报错，存在则准备读取文件
        a模式 追加模式 append 文件不存在则新建，文件存在则在文件末尾追加内容
        x模式 抑或模式 xor 文件存在则报错，文件 不存在则新建文件
        b模式 二进制模式 binary 辅助模式不能单独使用
        +模式 增强模式plus  也是辅助模式不能单独使用

close
>格式：文件io对象.close()
返回值：None


~~~Python
f = open('test.txt', 'w')
f.close()
~~~

### read() 读取文件
~~~Python
f = open('test.txt', 'r')

content = f.read(5)

print(content)

print("-"*30)

content = f.read()

print(content)

f.close()
~~~

### readline() 读取一行文件
~~~Python
f = open('test.txt', 'r')

content = f.readline()
print("1:%s"%content)

content = f.readline()
print("2:%s"%content)

f.close()
~~~

### write() 写入文件
~~~Python
fo = open("foo.txt", "w")
fo.write( "www.runoob.com!\nVery good site!\n")

fo.close()
~~~

## OS模块
* OS -- 操作系统的简称
* os模块就是对操作系统进行操作
使用该模块必须先导入模块：
~~~python
import os
~~~
os模块函数太多了，具体自己百度。
