---
title: Unity脚本开发学习笔记1
toc: true
mathjx: true
cover: /2019/08/28/Unity脚本开发学习笔记1/head.png
tags:
  - unity
categories:
  - 游戏开发
  - unity
abbrlink: 9941
date: 2019-08-28 12:40:30
update:
---

## Awake() 和 Start()
这两个函数用于做一些前置性工作。

不同之处在于，Awake()只要脚本被加载就会执行，即使脚本没有被使能（也就是勾上）。而Start()则在Awake()之后执行，且脚本必须被使能。

## Update() 和 FixedUpdate()
Update()很简单，每一帧调用一次。但显然，每一帧的渲染时间不会是一致的，即调用Update()的间隔时间不确定，这就导致如果我们把物理效果放到Update()里，会导致物理效果不太流畅。

为了保证物理运动与时间之间的紧密联系，应该使用FixedUpdate()。顾名思义，这是“固定的”Update，即间隔时间可以保持一致。间隔时间由Time.fixedDeltaTime决定，默认的时间是0.02s，一秒钟调用50次。

## 坐标与向量
Unity使用左手坐标系，三轴的相对位置依照下图所示。与数学立体几何中的一般表示不同，在这里Z轴代表的是“深度”而不是“高度”。

![](坐标与向量.png)

Unity包含了一些用于进行向量运算的方法，它们基本被放在Vector2和Vector3类里。

Vector3里包含了一些常用向量，以(0,0,1)为forward（依照上面的坐标手势，中指指向前面），有up、down、left、right、back，以及(0,0,0)的zero和(1,1,1)的one。Vector2里没有z轴，故没有forward和back。

## 游戏对象、组件，及其开关
一个游戏对象（GameObject）可以拥有多个组件（Component）。

比如一个简单的点光源，其本身是一个GameObject。首先它有一个Transform组件标示其位置、旋转、缩放等信息，还得有一个Light组件让它发光。也许，还有若干用于控制这个点光源的脚本组件。

如果我们关闭这个游戏对象（我们把它叫做激活/停用），那么这个灯整个就从场景中消失了。用游戏对象的SetActive(bool value)方法来实现这一点。

需要注意的是，如果游戏对象有多层结构，将一个父对象停用并不会使其子对象停用（虽然效果上，子对象也消失了）。查看一个对象到底是在层级中被激活还是本身确实在场景里激活了，可以用游戏对象的activeInHierarchy()和activeSelf()方法来确定。

要关掉灯，除了让灯凭空消失，我们当然还有更正常的做法。每一个组件都有一个bool类型的enabled标志位，只要把这个标志位设为false，就能去使能（禁用）这个组件。如将点光源的Light组件禁用，光是没了，但灯依然还在原处。
