---
title: golang的继承
toc: true
mathjx: true
cover: /2019/09/18/golang的继承/head.png
date: 2019-09-18 01:34:28
update:
tags: ['Golang']
categories:
  - Golang
  - Golang基础
---

对于 go 语言的继承，之前总是模模糊糊的分不清是什么。不知道如何通过何种方式来继承的。
然后我就开始对照着 java 的方式，用 java 实现继承，然后用 go 语言实现同样的继承。

#### Java的继承
~~~java
// 动物类
class Animal {
    public   String name;
    public String subject;

    void eat(String food) {
        System.out.println(name + "喜欢吃：" + food + ",它属于：" + subject);
    }
}

// 猫类。 猫类继承动物类
class Cat extends Animal{
    // 猫自己的属性和方法
    public int age;
    void sleep() {
        System.out.println(name + "今年" + age + "岁了，特别喜欢睡觉");
    }
}

public class Test{
    public static void main(String[] args){
        // 创建一个动物实例
        Animal a = new Animal();
        a.name = "动物";
        a.subject = "动物科";
        a.eat("肉");

        // 创建一个猫实例
        Cat cat = new Cat();
        cat.name = "咪咪";
        cat.subject = "猫科";
        cat.age = 1;
        cat.eat("鱼");
        cat.sleep();
    }
}
~~~

~~~
输出结果入下：
    动物喜欢吃：肉,它属于：动物科
    咪咪喜欢吃：鱼,它属于：猫科
    咪咪今年1岁了，特别喜欢睡觉
~~~

#### golang的继承

~~~Go
package main

import (
  "fmt"
  "strconv"
)

// 动物类
type Animal struct {
  name string
  subject string
}

// 动物的公共方法
func (a *Animal) eat(food string) {
  fmt.Println(a.name + "喜欢吃：" + food +",它属于:" + a.subject)
}

// 猫类，继承动物类
type Cat struct {
  // 继承动物的属性和方法
  Animal
  // 猫自己的属性
  age int
}

// 猫类独有的方法
func (c Cat) sleep() {
  fmt.Println(c.name + " 今年" + strconv.Itoa(c.age) + "岁了,特别喜欢睡觉")
}

func main() {
  // 创建一个动物类
  animal := Animal{name:"动物", subject:"动物科"}
  animal.eat("肉")

  // 创建一个猫类
  cat := Cat{Animal: Animal{name:"咪咪", subject:"猫科"},age:1}
  cat.eat("鱼")
  cat.sleep()
}
~~~

~~~
输出结果：
    动物喜欢吃：肉,它属于:动物科
    咪咪喜欢吃：鱼,它属于:猫科
    咪咪 今年1岁了,特别喜欢睡觉
~~~

#### 总结
1. 在 go 语言中， type name struct{} 结构体 就相当于其他语言中的 class 类的概念。
2. 在其他语言中，方法是直接写在在 类 里面的，而在 go 语言中，我们对于该结构体，如果存在方法，比如猫咪存在睡觉的方法那么是以 func (结构体名) 方法名{}，即 func(c Cat) sleep{} 的方式来声明方法。
3. 在 java 中， string + int = string，int 类型的值不需要类型转换，而在 go 语言中，string + int，如果想要一个字符串，则需要对 int 类型的值转换为 string 类型，然后才能拼接。

#### 理解
1. 结构体解决的是基本数据类型不能满足我们日常需要的问题。再简单点理解就是一个结构体就是一个 json 类型的 object。
2. 接口是一种类型。是一种特殊的类型，它规定了不同结构体有哪些相同的行为，只是制定标准，而不实现标准。就好比自来水厂只规定水龙头的半径大小，而不去考虑谁生产水龙头，生产水龙头的厂家不管用什么材料，只需要按照自来水厂的标准制定就好。
