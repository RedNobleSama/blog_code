---
title: golang并发
toc: true
mathjx: true
cover: /2020/01/25/golang并发/head.png
tags:
  - Golang
categories:
  - Golang
  - Golang基础
abbrlink: 47486
date: 2020-01-25 15:28:56
update:
---

### goroutine

#### 线程
线程是操作系统进行运算调度的最小单位，它是操作系统内核进行调度的，可以将其理解为“轻量级进程”。但是无论如何，作为一个内核态的调度，控制权限从线程A到线程B需要经历一个完整的内核态上下文切换。而Go语言的调度器不需要切换到内核语境，由它的语言结构完成线程的调度。这在很大程度上节省了无用的时间开销。

#### goroutine用法
使用“go”关键词即可以创建一个goroutine。go Function(param1, …)将会在相同的地址空间运行这个函数，并实现函数的并发操作。
goroutine有些类似于协程，但一般来说，协程是不支持并发的，而goroutine是支持并发操作的，一个简单的用法如下：
~~~go
package main

import (
    "fmt"
    "time"
)

func test_loop(id string, times int) {
    for i := 0 i < times; i++ {
        fmt.Printf("%s: Time %d", id, i)
    }
}

func main() {
    go test_loop("ID 1", 10) // 启动一个goroutine
    time.Sleep(time.Second) // 让主程序休眠一秒
}
~~~
注意，如果没有让主程序休眠一秒，那么有可能在goroutine执行完前主线程已经终止，从而使得goroutine没有执行完成就被迫终止。

### 通道
通道是goroutine直接的连接方式，是可以让一个goroutine发送特定值到另一个goroutine的通信机制，每一个通道都有一个具体的对应类型。创建一个通道的语法如下：
~~~go
ch := make(chan int) // 创建一个chan int类型的无缓冲通道
ch := make(chan int, 0) // 创建一个chan int类型的无缓冲通道
ch := make(chan, int, 10) // 创建一个容量为10的chan int类型的缓冲通道
~~~

通道主要支持三种操作，分别为发送，接收和关闭，其语法如下：
~~~go
ch <- x // 发送

x = <- ch // 接收并赋值
<- ch // 接收并丢弃

close(ch) // 关闭
~~~
利用通道，我们可以实现goroutine间的连接，形成一个管道（pipeline），使得goroutine间能够互相通信。

#### 单向通道
在实际使用中，多数情况下一个通道只需完成发送或者接收中的一个功能。为了安全性问题，GO语言也提供了单向通道类型。例如在下面生产者和消费者的示例中，生产者只负责发送数据给通道，而消费者只负责从通道中取出数据，它们的代码分别如下：
~~~go
// 生产者
func producer(data string, channel chan <- string) {
    // 类型chan <- string是一个只能发送数据的通道
    ...
}

// 消费者
func consumer(channel <- chan string) {
    // 类型<- chan string是一个只能接收数据的通道
    ...
}
~~~

### 事例
我们可以用goroutine和channel能模拟一个生产者和消费者的情景，生产者可以无限循环产生新的数据到channel中，而消费者可以通过无限循环来从channel中获取数据。
生产者函数如下：
~~~go
// 生产者函数
func producer(data string, channel chan <- string) {
     // 生产者开始生产
     for {
            // 将随机数和字符串格式化为字符串发送给通道
            fmt.Printf("Produced: %s\n", data)
            channel <- fmt.Sprintf("%s", data)

            time.Sleep(time.Second)
        }
}
~~~

消费者函数如下：
~~~go
// 消费者函数
func consumer(channel <- chan string) {
     // 消费者开始消费
     for {
            // 从通道中取出数据, 此处会阻塞直到信道中返回数据
            data := <- channel
            fmt.Printf("Consumed: %s\n", data)
        }
}
~~~

利用goroutine，我们无需像操作系统中的做法，在生产者和消费者间添加各种互斥锁，也不需要我们手动去实现对信道的阻塞。例如，在执行“message := <- channel”时，程序从channel中取出数据时，会使自己处于阻塞态直到channel中返回数据。

这样，我们就可以实现一个“生产者与消费者”的经典问题，即父亲每秒生产一个苹果到盘子中，母亲每秒生产一个香蕉到盘子中，儿子当盘中有水果时，就会按水果生产的水果将水果吃掉。主函数如下：
~~~go
func main() {
    channel := make(chan string)
    // 创建父亲生产者和母亲生产者
    go producer("apple", channel)
    go producer("banana", channel)

    // 消费者开始消费
    consumer(channel)
}
~~~

输出结果
~~~
Produced: banana
Produced: apple
Consumed: banana
Consumed: apple
Produced: banana
Consumed: banana
...
~~~

### 总结
利用channel可以实现不同协程之间的通信，golang提倡“share memory by communicating”而不是“communicate by sharing memory”，这样可以避免竞争状况下复杂的互斥锁机制。

goroutine使得用户能在用户态实现一个轻量级的“线程创建”，而通道则负责完成这些goroutine间的通信。通道的独写操作类似于操作系统下P/V信号量，让系统无须陷入内核态就可以完成协程间的通信。
