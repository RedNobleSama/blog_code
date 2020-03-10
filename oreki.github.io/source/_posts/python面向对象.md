---
title: python面向对象
toc: true
mathjx: true
cover: /2018/02/17/python面向对象/head.png
date: 2018-02-17 09:43:33
update:
tags: [Python]
categories: Python
---
* 类(Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
* 类变量：类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
* 数据成员：类变量或者实例变量用于处理类及其实例对象的相关的数据。
* 方法重写：如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
* 实例变量：定义在方法中的变量，只作用于当前实例的类。
* 继承：即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
* 实例化：创建一个类的实例，类的具体对象。
* 方法：类中定义的函数。
* 对象：通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。

## 类的定义
~~~Python
'''类的定义'''
class MyClass:
    i = 12345
    def f(self):
        return 'hello world'

'''实例化类'''
x = MyClass()

'''访问类的属性和方法'''
print("MyClass 类的属性 i 为：", x.i)
print("MyClass 类的方法 f 输出为：", x.f())

结果：
MyClass 类的属性 i 为： 12345
MyClass 类的方法 f 输出为： hello world
~~~

## 构造函数
很多类都倾向于将对象创建为有初始状态的。因此类可能会定义一个名为 init() 的特殊方法（构造方法）
~~~Python
def __init__(self):
    self.data = []
~~~

类定义了 init() 方法的话，类的实例化操作会自动调用 init() 方法。所以在下例中，可以这样创建一个新的实例:

~~~Python
x = MyClass()
~~~

当然， init() 方法可以有参数，参数通过 init() 传递到类的实例化操作上。例如
~~~Python
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
x = Complex(3.0, -4.5)
print(x.r, x.i)   # 输出结果：3.0 -4.5
~~~

### self
self代表类的实例，而非类
类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 self.
~~~Python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)

t = Test()
t.prt()
~~~
结果
~~~Python
<__main__.Test instance at 0x100771878>
__main__.Test
~~~

## 属性和方法
### 类的方法
在类地内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例

~~~Python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))

# 实例化类
p = people('runoob',10,30)
p.speak()
~~~
输出结果
>runoob 说: 我 10 岁。
