---
title: python循环结构
toc: true
mathjx: true
cover: /2018/02/12/python循环结构/head.png
categories:
  - Python
  - Python基础
abbrlink: 3834
date: 2018-02-12 19:00:09
update:
tags:
---
## while型循环
~~~Python
n = 100
sum = 0
counter = 1
while counter <= n:
    sum = sum + counter
    counter += 1

print("1 到 %d 之和为: %d" % (n,sum))
~~~
执行结果
>1 到 100 之和为: 5050

### 死循环
死循环就是循环不会终止的循环类型，通过将用于判断的条件表达式设置为永远为True来实现。
~~~Python
var = 1
while var == 1 :  # 表达式永远为 true
   num = int(input("输入一个数字  :"))
   print ("你输入的数字是: ", num)

print ("Good bye!")
~~~
* 你可以使用 CTRL+C 来退出当前的无限循环
* 执行以上脚本，输出结果如下：
>输入一个数字  :5
你输入的数字是:  5
输入一个数字  :

## for ... in 循环
for...in 循环用于遍历容器类的数据（字符串，列表，元组，字典，集合）
~~~Python
for i in ['c','py','java']
    print(i)
else:
    break
~~~

## range()函数
* 如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列
~~~Python
>>>for i in range(5,9) :
    print(i)


5
6
7
8
~~~

## break和continue语句及循环中的else子句
### break
* break作用:在循环中break的作用是终止当前循环结构的后续操作，一旦程序运行了break，循环也就终止了！
* break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。

~~~Python
for letter in 'Runoob':     # 第一个实例
   if letter == 'b':
      break
   print ('当前字母为 :', letter)

var = 10                    # 第二个实例
while var > 0:              
   print ('当期变量值为 :', var)
   var = var -1
   if var == 5:
      break

print ("Good bye!")
~~~
输出结果为
>当前字母为 : R
当前字母为 : u
当前字母为 : n
当前字母为 : o
当前字母为 : o
当期变量值为 : 10
当期变量值为 : 9
当期变量值为 : 8
当期变量值为 : 7
当期变量值为 : 6
Good bye!

### continue语句
~~~Python
for letter in 'Runoob':     # 第一个实例
   if letter == 'o':        # 字母为 o 时跳过输出
      continue
   print ('当前字母 :', letter)

var = 10                    # 第二个实例
while var > 0:              
   var = var -1
   if var == 5:             # 变量为 5 时跳过输出
      continue
   print ('当前变量值 :', var)
print ("Good bye!")
~~~
>输出结果为
当前字母 : R
当前字母 : u
当前字母 : n
当前字母 : b
当前变量值 : 9
当前变量值 : 8
当前变量值 : 7
当前变量值 : 6
当前变量值 : 4
当前变量值 : 3
当前变量值 : 2
当前变量值 : 1
当前变量值 : 0
Good bye!

### pass 语句
* pass是空语句，是为了保持程序结构的完整性。
* pass 不做任何事情，一般用做占位语句
