---
title: golang中什么时候使用指针
toc: true
mathjx: true
cover: /2019/09/17/golang中什么时候使用指针/head.png
date: 2019-09-17 14:22:35
update:
tags: ['Golang']
categories:
  - Golang
  - Golang基础
---
什么是指针：即一个指针变量指向一个值的内存地址。

### 使用值类型和指针类型的区别
首先，我们来看一个计算面积的代码，如下所示。

```Go
package main

import (
	"fmt"
)

type Rect struct {                               //定义一个结构体
    width  float64
    length float64
}

func (rect Rect) area() float64 {               //定义一个方法，按值传递
	return rect.width * rect.length
}

func (rect *Rect) area1() float64 {            //定义一个方法，按指针传递
    rect.width *= 2
    rect.length *= 2
    return rect.width * rect.length
}

func main() {
    var rect = new(Rect)     //使用new函数创建一个结构体指针rect，也就是说rect的类型是*Rect
    rect.width = 100
    rect.length = 200
    fmt.Println("Width:", rect.width, "Length:", rect.length,"Area:", rect.area())  //通过结构体指针类型的变量调用area()方法
    fmt.Println("Width:", rect.width, "Length:", rect.length,"Area:", rect.area1())
}
```
在Go语言中，默认是按值传递。当一个变量当作参数传递的时候，会创建一个变量的副本，然后传递给函数或者方法，你可以看到这个副本的地址和变量的地址是不一样的。当变量当做指针被传递的时候，一个新的指针被创建，它指向变量同样的内存地址，所以你可以将这个指针看成原始变量指针的副本。

### 故此
1.是否使用结构体指针，取决于是否要在函数内部改变传递进来的参数的值。如果你的struct足够大，使用指针可以加快效率。如果不使用指针，在函数内部则无法修改struct中的值。
2.结构体赋值默认是按值传递，你要改变原来的那个值，要使用指针（即如果你要修改对象本身，那就要传指针，否则修改的是副本）。

### go什么情况下使用指针
1.推荐在方法上使用指针（前提是这个类型不是 map、slice 等引用类型）
2.当结构体较大的时候使用指针会更高效，可以避免内存拷贝，“结构较大” 到底多大才算大可能需要自己或团队衡量，如超过5个字段或者根据结构体内存占用来计算
3.如果要修改结构体内部的数据或状态必须使用指针
4.如果方法的receiver是map、slice 、channel等引用类型不要使用指针
5.小数据类型如 bool、int 等没必要使用指针传递
6.如果该函数会修改receiver或变量等，使用指针
