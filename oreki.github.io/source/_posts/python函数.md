---
title: python函数
toc: true
mathjx: true
cover: /2018/02/10/python函数/head.png
date: 2018-02-10 23:03:49
update:
tags: [Python]
categories:
  - Python
  - Python基础
---
## 函数的定义格式
~~~Python
def  函数名():
    函数功能代码...
    函数功能代码...
~~~
* 特征:函数定义之后不会自动执行,必须在调用函数后函数才会执行.
* 函数名的命名规则:和变量基本一样
  * 推荐使用英文或者拼音,禁止使用中文
  * 可以使用数字，但是不能用数字开头
  * 不可以使用特殊字符，除了_
  * 函数名严格区分大小写
  * 函数名必须要有意义。
  * 不能和系统已经存在的保留关键字冲突!
  * 禁止使用和系统提供函数相同的函数名

~~~Python
>>> def hello() :
   print("Hello World!")


>>> hello()
Hello World!
~~~

## 带有参数的函数格式
~~~Python
def 函数名(参数,参数...):
    函数功能代码...
    函数功能代码...
    ...
~~~

形参:形式上的参数,声明函数时()中的参数是形参
实参:实际上的参数,调用函数时()中的参数是实参
~~~Python
def area(width, height):
    return width * height

def print_welcome(name):
    print("Welcome", name)

print_welcome("Python")
w = 4
h = 5
print("width =", w, " height =", h, " area =", area(w, h))
~~~
输出结果
>Welcome Python
width = 4  height = 5  area = 20

注意:
* 实参将值传递给形参的过程本质上就是简单的变量赋值仅此而已
* 参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样

## 带有默认值的参数
~~~Python
def printinfo( name, age = 35 ):
   "打印任何传入的字符串"
   print ("名字: ", name);
   print ("年龄: ", age);
   return;

printinfo( age=50, name="runoob" );#调用printinfo函数
print ("==============")
printinfo( name="runoob" );
~~~
输出结果
>名字:  runoob
>年龄:  50
==============
>名字:  runoob
>年龄:  35

* 注意：在此情况下使用实参值覆盖原有形参的默认值，本质上就是变量的重新赋值操作
* 调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 age 参数

## 收集参数
### 非关键字收集参数
* 非关键字收集参数，在形参前添加一个*即可
* 非关键字收集参数收集实参组成一个元组
* 非关键字收集参数，仅收集没有任何形参接受的非关键字实参
* 非关键字收集参数和普通的形参可以共存
~~~python
def 函数名(*参数名):
    函数功能代码...
    函数功能代码...
    ...

调用函数：函数名(实参，实参...)   没有数量限制
~~~
~~~python
def printinfo( arg1, *vartuple ):
   '''打印任何传入的参数'''
   print ("输出: ")
   print (arg1)
   for var in vartuple:
      print (var)
   return;

printinfo( 10 );
printinfo( 70, 60, 50 );
~~~
输出结果
>输出:
10
输出:
70
60
50

### 关键字收集参数
* 关键字收集参数，在形参前添加两个**即可
* 关键字收集参数，收集的结果组成一个字典，关键字成为字典的键，实参成为值
* 关键字收集参数，仅收集没有任何形参接受的关键字参数
* 关键字参数可以和普通的形参共存
~~~python
def func(country,province,**kwargs):
    print(country,province,kwargs)

'''使用'''
func("China","Sichuan",city = "Chengdu", section = "JingJiang")

'''结果'''
China Sichuan {'city': 'Chengdu', 'section': 'JingJiang'}
~~~

## 匿名函数（lambda）
* 所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。
* lambda 只是一个表达式，函数体比 def 简单很多

~~~python
'''可写函数说明'''
sum = lambda arg1, arg2: arg1 + arg2;

'''调用sum函数'''
print ("相加后的值为 : ", sum( 10, 20 ))
print ("相加后的值为 : ", sum( 20, 20 ))
~~~
输出结果
>相加后的值为 :  30
相加后的值为 :  40
