---
title: golang接口理解与使用
toc: true
mathjx: true
cover: /2020/01/17/golang接口理解与使用/head.png
date: 2020-01-17 21:50:23
update:
tags: ['Golang']
categories:
  - Golang
  - Golang基础
---

### interface是一种类型
首先 interface 是一种类型，从它的定义可以看出来用了 type 关键字，更准确的说 interface 是一种具有一组方法的类型，这些方法定义了 interface 的行为

go 允许不带任何方法的 interface ，这种类型的 interface 叫 empty interface


### interface 变量存储的是实现者的值
~~~Golang
//1
type I interface {    
    Get() int
    Set(int)
}

//2
type S struct {
    Age int
}

func(s S) Get()int {
    return s.Age
}

func(s *S) Set(age int) {
    s.Age = age
}

//3
func f(i I){
    i.Set(10)
    fmt.Println(i.Get())
}

func main() {
    s := S{}
    f(&s)  //4
}
~~~

这段代码在 #1 定义了 interface I，在 #2 用 struct S 实现了 I 定义的两个方法，接着在 #3 定义了一个函数 f 参数类型是 I，S 实现了 I 的两个方法就说 S 是 I 的实现者，执行 f(&s) 就完了一次 interface 类型的使用。
