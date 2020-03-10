---
title: python分支结构
toc: true
mathjx: true
cover: /2018/02/12/python分支结构/head.png
date: 2018-02-12 18:52:37
update:
tags: [Python]
categories: Python
---
流程控制: 对计算机代码执行顺序的管理就是流程控制

## 单项分支
~~~Python
if age < 0:
    print("你是在逗我吧!")
~~~



## 多项分支
~~~Python
age = int(input("请输入你家狗狗的年龄: "))
print("")
if age < 0:
    print("你是在逗我吧!")
elif age == 1:
    print("相当于 14 岁的人。")
elif age == 2:
    print("相当于 22 岁的人。")
elif age > 2:
    human = 22 + (age -2)*5
    print("对应人类年龄: ", human)

input("点击 enter 键退出")
~~~


## 巢状分支
巢状分支是其他分支结构的嵌套结构，无论哪个分支都可以嵌套
~~~Python
num=int(input("输入一个数字："))
if num%2==0:
    if num%3==0:
        print ("你输入的数字可以整除 2 和 3")
    else:
        print ("你输入的数字可以整除 2，但不能整除 3")
else:
    if num%3==0:
        print ("你输入的数字可以整除 3，但不能整除 2")
    else:
        print  ("你输入的数字不能整除 2 和 3")
~~~
