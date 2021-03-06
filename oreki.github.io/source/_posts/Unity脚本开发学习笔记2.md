---
title: Unity脚本开发学习笔记2
toc: true
mathjx: true
cover: /2019/09/01/Unity脚本开发学习笔记2/head.png
tags:
  - unity
categories:
  - 游戏开发
  - unity
abbrlink: 10133
date: 2019-09-01 16:45:51
update:
---

## 按键控制
Input.GetKey和Input.GetButton两种方法用于获取按键情况。

区别在于，Key有确定的KeyCode。KeyCode是Unity内置的枚举类，内含各种键盘、手柄按键，如空格键就由KeyCode.Space表示。

而Button是可以自定义详细配置的。在Edit-ProjectSettings-Input中可以对输入进行详细配置，为一种操作提供一个字符串别名。只要使用GetBotton(string str)就可以监测对应的输入了。

除了知道按键是否被按下之外，我们还需要确切地知道按键在哪一帧被按下，哪一帧被松开。 因此，这两个方法都有衍生的GetxxxDown和GetxxxUp。在对应按键被按下后的第一帧，GetKeyDown返回true，而到了第二帧，它就再次返回false了。同理，GetKeyUp也仅在松开按键后的第一帧返回true。

当然，这些方法都应该被放在Update()函数里，这样才能在每一帧对按键进行监测。

## 摇杆控制
确切地讲，这里应该是“轴”而不是摇杆。不过比起按键，摇杆更能体现“轴”的涵义，虽然按键确实也可以作为轴。

摇杆控制就比较麻烦了，因为按键只有按下与没按下两个状态，可以简单地用bool值表示，但摇杆有推移的距离，只能用浮点数表示。而且摇杆还有反向。

对于这些特性，GetKey之类的方法显然无法处理，所以需要使用Input.GetAxis。

Input.GetAxis的参数是称为axisName的字符串，可以在Edit-ProjectSettings-Input中定义。其返回的是一个-1到1之间的浮点数，Positive Botton使其返回正值，而Negative Botton让其返回负值。

Sensitivity和Gravity决定了按键触发/松开时返回值多快地上升/归零。

此外还有Dead值，这使得可以不对较小幅度的摇杆活动进行处理，一定程度上能够防止误触。Snap选项，其为true时如果同时触发了Positive Botton和Negative Botton，就返回0。

以上说的，是比较“完备”的摇杆控制，但是在很多时候我们并不需要这个“轴”返回浮点值。举个例子，《刺客信条：起源》中并没有以往的“奔跑”键，而是用摇杆推移的幅度决定移动速度，这就非常GetAxis。如果用键盘操作，WASD控制方向，是没有推移幅度这一说的，我们就当按下按键就是摇杆直接推到底，这就是Type属性为Key or Mouse Button而不是Joystick Axis的轴。无论如何，巴耶克并不是弹射起步，而是有gravity和sensitivity的作用。

但是，在东方的STG里就没有这些玩意，大家的运动都像加速度不存在一样。这种情况下，应当使用Input.GetAxisRaw方法，按键后返回的是1和-1这两个整数而非浮点数。

## 重写和成员隐藏
重写（Overriding，有人称为覆盖）是在派生类中重新实现基类中已有的方法和属性等，为了安全，基类中被重写的方法/属性需要有virtual修饰符（或者abstract、override），派生类重写的方法/属性需要有override修饰符。由于是重新“实现”，重写的方法签名/属性类型和名称必须和之前保持一致，也不能改变访问性，重写方法相当于只是重新编写了方法的函数体。

我的理解是，在派生类中重写之后，从基类继承下来的方法/属性就相当于不存在了。因此，不管我们把派生类的对象当成该类的对象还是其基类的对象，在直接调用方法时调用的都是重写之后的方法。

而成员隐藏则完全不同（Member Hiding，虽然也有人称其为覆盖），它虽然看上去是在派生类中用new修饰符重新定义了基类中已有的成员，但！是！ 从基类继承下来的成员并没有消失，而是被隐藏了。如果我们把派生类的对象作为其基类的对象（Upcasting，向上转型），那调用到的就是被隐藏的基类成员。

## 委托和事件

委托（delegate）可以被当作函数指针。要定义委托，首先需要用delegate关键字声明一个模板，模板展示了这种委托应该存放怎样的函数——用其返回类型以及参数列表来限定。

可以把委托名直接当成函数名来用。

委托支持“多播”，这使得我们可以把同一类型的函数集中放到一个委托里，只要调用这个委托，就是依次调用委托中函数列表里的函数。用+=和-=运算符把函数加入委托或从委托中删除。

最初看到多播委托这里，我还没有领会这个机制的用处，但很快我就看到了“事件”。

事件就是一种特殊的委托，或者说它非常类似于公共的多播委托。我们在定义委托的基础上使用event关键字定义一个事件。

对于同一个事件，不同的对象可能有不同的反应，也就有不同的处理函数。如果把唤起事件视作调用委托，那么一系列的事件处理函数就是委托的函数列表，要订阅或取消订阅事件的话，用+=和-=就好了。

## 协程和生成器

看到C#协程（Coroutine）里熟悉的yield，我想到的是此前在Python中学到过的生成器（Generator）。简直是一个套路，运行到yield时，函数返回，但下次调用该函数时不会再从头执行，而是从上次退出的地方之后开始执行。

实际上生成器就是协程的一种，称为半协程（Semi-coroutine），这是一种受限制的协程实现。
