---
title: Python基础语法
toc: true
mathjx: true
cover: /2018/02/10/Python基础语法/head.png
date: 2018-02-10 15:34:56
update:
tags: [Python]
categories:
  - Python
  - Python基础
---
## input输入
* input()的小括号中放入的是，提示信息，用来在获取数据之前给用户的一个简单提示
* input()在从键盘获取了数据以后，会存放到等号右边的变量中
* input()会把用户输入的任何值都作为字符串来对待
* 注意：在python2中还有一个raw_input()输入，但到python3中没有了

~~~Python
str = input("请输入：");
print ("你输入的内容是: ", str)
~~~
* 这会产生如下的对应着输入的结果
> 请输入：Hello Python!
> 你输入的内容是:  Hello Python!

## Print()输出
* print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 end=""：

~~~Python
x="a"
y="b"
# 换行输出
print( x )
print( y )

print('---------')
# 不换行输出
print( x, end=" " )
print( y, end=" " )
print()

# 同时输出多个变量
print(x,y)
~~~

## 行与缩进
* python最具特色的就是使用缩进来表示代码块，不需要使用大括号({})。
* 缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。实例如下：
~~~python
if True:
    print ("True")
else:
    print ("False")
~~~
* 以下代码最后一行语句缩进数的空格数不一致，会导致运行错误：
~~~python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误
~~~
* 以上程序由于缩进不一致，执行后会出现类似以下错误：
~~~Python
File "test.py", line 6
    print ("False")    # 缩进不一致，会导致运行错误
IndentationError: unindent does not match any outer indentation level
~~~

### 多行语句
* Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠()来实现多行语句，例如：
~~~Python
total = item_one + \
        item_two + \
        item_three
~~~
* 在 [], {}, 或 () 中的多行语句，不需要使用反斜杠()，例如：
~~~Python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
~~~

### 空行
* 函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。
* 空行与代码缩进不同，空行并不是Python语法的一部分。书写时不插入空行，Python解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。
* 记住：空行也是程序代码的一部分。

## format的格式化函数（了解）
* 格式化字符串的函数 str.format()，它增强了字符串格式化的功能。
* 基本语法是通过 {} 和 : 来代替以前的 % 。

~~~Python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'

>>> "{0} {1}".format("hello", "world")  # 设置指定位置
'hello world'

>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'

>>> print("网站名：{name}, 地址 {url}".format(name="百度", url="www.baidu.com")) #指定参数名
'网站名：百度, 地址 www.baidu.com'


>>>site = {"name": "百度", "url": "www.baidu.com"}
>>>print("网站名：{name}, 地址 {url}".format(**site)) # 通过字典设置参数
'网站名：百度, 地址 www.baidu.com'

>>>my_list = ['百度', 'www.baidu.com']
>>>print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的 通过列表索引设置参数
'网站名：百度, 地址 www.baidu.com'

>>> print("{:.2f}".format(3.1415926)); #数字格式化
3.14
~~~

## Python中的注释有单行注释和多行注释
* python中单行注释采用 # 开头。
~~~Python
name = "Madisetti" # 这是一个注释
~~~
* python 中多行注释使用三个单引号(''')或三个双引号(""")。
~~~Python
'''
这是多行注释，使用单引号。
这是多行注释，使用单引号。
这是多行注释，使用单引号。
'''

"""
这是多行注释，使用双引号。
这是多行注释，使用双引号。
这是多行注释，使用双引号。
"""
~~~

## 标识符
* 在Python里，标识符: 由字母、数字、下划线组成,但不能以数字开头。
* Python 中的标识符是区分大小写的。
* 特殊标识符：
* 以下划线开头的标识符是有特殊意义的。以单下划线开头 _foo 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 from xxx import * 而导入；
* 以双下划线开头的 __foo 代表类的私有成员；以双下划线开头和结尾的 __foo__ 代表 Python 里特殊方法专用的标识，如 __init__() 代表类的构造函数。
* python保留字： 保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字：
~~~Python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue',
'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global',
'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'if',
'return','try', 'while', 'with', 'yield']
~~~

## 变量
* Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
* 在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。
* 等号（=）用来给变量赋值。
* 等号（=）运算符左边是一个变量名,等号（=）运算符右边是存储在变量中的值。例如：
* 实例(Python 3.0+)
~~~python
counter = 100          # 整型变量
miles   = 1000.0       # 浮点型变量
name    = "demo"     # 字符串

print (counter)
print (miles)
print (name)
~~~

执行以上程序会输出如下结果:
~~~Python
100
1000.0
demo
~~~

### 多个变量赋值
* Python允许你同时为多个变量赋值。例如：
~~~Python
a = b = c = 1
~~~
* 以上实例，创建一个整型对象，值为1，三个变量被分配到相同的内存空间上。
* 您也可以为多个对象指定多个变量。例如：
~~~Python
a, b, c = 1, 2, "demo"
~~~
* 以上实例，两个整型对象 1 和 2 的分配给变量 a 和 b，字符串对象 "demo" 分配给变量 c。
