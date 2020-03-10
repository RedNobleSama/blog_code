---
title: python数据类型
toc: true
mathjx: true
cover: /2018/02/12/python数据类型/head.png
date: 2018-02-12 17:55:16
update:
tags: [Python]
categories: Python
---
标准数据类型
* Python3 中有六个标准的数据类型：
  * Number（数字）
    * int
    * bool
    * float
  * complex（复数）
  * String（字符串）
  * List（列表）
  * Tuple（元组）
  * Sets（集合）
  * Dictionary（字典）

## Number（数字）
* Python3 支持 int、float、bool、complex（复数）。
* 在Python 3里，只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
* 像大多数语言一样，数值类型的赋值和计算都是很直观的。
* 内置的 type() 函数可以用来查询变量所指的对象类型。

~~~Python
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
~~~

此外还可以用 isinstance 来判断：

~~~Python
>>>a = 111
>>> isinstance(a, int)
True
>>>
~~~

isinstance 和 type 的区别在于:
  * type()不会认为子类是一种父类类型。
  * isinstance()会认为子类是一种父类类型。

~~~Python
class A:
    pass

class B(A):
    pass

isinstance(A(), A)  # returns True
type(A()) == A      # returns True
isinstance(B(), A)    # returns True
type(B()) == A        # returns False
~~~

>注意：在 Python2 中是没有布尔型的，它用数字 0 表示 False，用 1 表示 True。
到 Python3 中，把 True 和 False 定义成关键字了，但它们的值还是 1 和 0，它们可以和数字相加。

## String（字符串）
* Python中的字符串用单引号(')或双引号(")括起来，同时使用反斜杠()转义特殊字符。
* 字符串的截取的语法格式如下：
  * 变量[头下标:尾下标]
* 索引值以 0 为开始值，-1 为从末尾的开始位置。
* 加号 (+) 是字符串的连接符， 星号 (*) 表示复制当前字符串，紧跟的数字为复制的次数。实例如下：
~~~Python
str = 'zhangsan'

print (str)          # 输出字符串
print (str[0:-1])    # 输出第一个到倒数第二个的所有字符
print (str[0])       # 输出字符串第一个字符
print (str[2:5])     # 输出从第三个开始到第五个的字符
print (str[2:])      # 输出从第三个开始的后的所有字符
print (str * 2)      # 输出字符串两次
print (str + "TEST") # 连接字符串
~~~
输出结果:
>zhangsan
zhangsa
z
ang
angsan
zhangsanzhangsan
zhangsanTEST

* Python 使用反斜杠()转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个r，表示原始字符串：
~~~Python
>>> print('Ru\noob')
Ru
oob
>>> print(r'Ru\noob')
Ru\noob
>>>
~~~

## List（列表）
* List（列表） 是 Python 中使用最频繁的数据类型。
* 列表可以完成大多数集合类的数据结构实现。列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）。
* 列表是写在方括号[]之间、用逗号分隔开的元素列表。
* 和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。
* 列表截取的语法格式如下：
  * 变量[头下标:尾下标]
* 索引值以 0 为开始值，-1 为从末尾的开始位置。
* 加号（+）是列表连接运算符，星号（*）是重复操作。如下实例：
~~~Python
list = [ 'abcd', 786 , 2.23, 'demo', 70.2 ]
tinylist = [123, 'demo']

print (list)            # 输出完整列表
print (list[0])         # 输出列表第一个元素
print (list[1:3])       # 从第二个开始输出到第三个元素
print (list[2:])        # 输出从第三个元素开始的所有元素
print (tinylist * 2)    # 输出两次列表
print (list + tinylist) # 连接列表
~~~
输出结果
>['abcd', 786, 2.23, 'demo', 70.2]
abcd
[786, 2.23]
[2.23, 'demo', 70.2]
[123, 'demo', 123, 'demo']
['abcd', 786, 2.23, 'demo', 70.2, 123, 'demo']

* 与Python字符串不一样的是，列表中的元素是可以改变的
~~~Python
>>>a = [1, 2, 3, 4, 5, 6]
>>> a[0] = 9
>>> a[2:5] = [13, 14, 15]
>>> a
[9, 2, 13, 14, 15, 6]
>>> a[2:5] = []   # 将对应的元素值设置为 []
>>> a
[9, 2, 6]
~~~

## Tuple（元组）
* 元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号(())里，元素之间用逗号隔开。
* 元组中的元素类型也可以不相同：
~~~Python
tuple = ( 'abcd', 786 , 2.23, 'demo', 70.2  )
tinytuple = (123, 'demo')

print (tuple)             # 输出完整元组
print (tuple[0])          # 输出元组的第一个元素
print (tuple[1:3])        # 输出从第二个元素开始到第三个元素
print (tuple[2:])         # 输出从第三个元素开始的所有元素
print (tinytuple * 2)     # 输出两次元组
print (tuple + tinytuple) # 连接元组
~~~

输出结果
>('abcd', 786, 2.23, 'demo', 70.2)
abcd
(786, 2.23)
(2.23, 'demo', 70.2)
(123, 'demo', 123, 'demo')
('abcd', 786, 2.23, 'demo', 70.2, 123, 'demo')

* 元组与字符串类似，可以被索引且下标索引从0开始，-1 为从末尾开始的位置。
* 也可以进行截取（看上面，这里不再赘述）。
* 虽然tuple的元素不可改变，但它可以包含可变的对象，比如list列表。

## Set（集合）
* 集合（set）是一个无序不重复元素的序列。
* 基本功能是进行成员关系测试和删除重复元素。
* 可以使用大括号 { } 或者set()函数创建集合，注意：创建一个空集合必须用set()而不是 { }，因为 { } 是用来创建一个空字典。
* 创建格式：

~~~Python
parame = {value1,value2}
~~~

实例:
~~~Python
student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}

print(student)   # 输出集合，重复的元素被自动去掉

'''
成员测试
'''
if('Rose' in student) :
    print('Rose 在集合中')
else :
    print('Rose 不在集合中')

'''
set可以进行集合运算
'''
a = set('abracadabra')
b = set('alacazam')

print(a)

print(a - b)     # a和b的差集

print(a | b)     # a和b的并集

print(a & b)     # a和b的交集

print(a ^ b)     # a和b中不同时存在的元素
~~~

输出结果
>{'Mary', 'Jim', 'Rose', 'Jack', 'Tom'}
Rose 在集合中
{'b', 'a', 'c', 'r', 'd'}
{'b', 'd', 'r'}
{'l', 'r', 'a', 'c', 'z', 'm', 'b', 'd'}
{'a', 'c'}
{'l', 'r', 'z', 'm', 'b', 'd'}

## Dictionary（字典）
* 字典（dictionary）是Python中另一个非常有用的内置数据类型。
* 列表是有序的对象结合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。
* 字典是一种映射类型，字典用"{ }"标识，它是一个无序的键(key) : 值(value)对集合。
* 键(key)必须使用不可变类型。
* 在同一个字典中，键(key)必须是唯一的。

~~~Python
dict = {}
dict['one'] = "1 - Python教程"
dict[2]     = "2 - Python工具"

tinydict = {'name': 'demo','code':1, 'site': 'www.demo.com'}


print (dict['one'])       # 输出键为 'one' 的值
print (dict[2])           # 输出键为 2 的值
print (tinydict)          # 输出完整的字典
print (tinydict.keys())   # 输出所有键
print (tinydict.values()) # 输出所有值
~~~

输出结果
>1 - Python教程
2 - Python工具
{'name': 'demo', 'site': 'www.demo.com', 'code': 1}
dict_keys(['name', 'site', 'code'])
dict_values(['demo', 'www.demo.com', 1])

## Python数据类型转换
* 有时候，我们需要对数据内置的类型进行转换，数据类型的转换，你只需要将数据类型作为函数名即可。
* 以下几个内置的函数可以执行数据类型之间的转换。这些函数返回一个新的对象，表示转换的值

函数 | 描述
---- | ----
int(x [,base]) |	将x转换为一个整数
float(x) |	将x转换到一个浮点数
complex(real [,imag]) |	创建一个复数
str(x) |	将对象 x 转换为字符串
repr(x) |	将对象 x 转换为表达式字符串
eval(str) |	用来计算在字符串中的有效Python表达式,并返回一个对象
tuple(s) |	将序列 s 转换为一个元组
list(s) |	将序列 s 转换为一个列表
set(s) |	转换为可变集合
dict(d) |	创建一个字典。d 必须是一个序列 (key,value)元组。
frozenset(s) |	转换为不可变集合
chr(x) |	将一个整数转换为一个字符
unichr(x) |	将一个整数转换为Unicode字符
ord(x) |	将一个字符转换为它的整数值
hex(x) |	将一个整数转换为一个十六进制字符串
oct(x) |	将一个整数转换为一个八进制字符串
