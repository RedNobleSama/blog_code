---
title: vue生命周期简答
toc: true
mathjx: true
cover: /2020/08/16/vue生命周期简答/head.png
tags:
  - Vue.js
categories:
  - 前端
  - Vue
abbrlink: 40255
date: 2020-08-16 21:32:58
update:
---

#### MVVM
MVVM是Model-View-ViewModel缩写，也就是把MVC中的Controller演变成ViewModel。Model层代表数据模型，View代表UI组件，ViewModel是View和Model层的桥梁，数据会绑定到viewModel层并自动将数据渲染到页面中，视图变化的时候会通知viewModel层更新数据。


#### Vue的生命周期
Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。

##### 它的生命周期中有多个事件钩子，让我们在控制整个vue实例的过程中更容易形成好的逻辑。
1. beforeCreate----创建前: 组件实例更被创建，组件属性计算之前，数据对象data都为undefined，未初始化。
2. created----创建后：组件实例创建完成，属性已经绑定，数据对象data已存在，但dom未生成，$el未存在
3. beforeMount---挂载前：vue实例的$el和data都已初始化，挂载之前为虚拟的dom节点，data.message未替换
4. mounted-----挂载后：vue实例挂载完成，data.message成功渲染。
5. beforeUpdate----更新前：当data变化时，会触发beforeUpdate方法
6. updated----更新后：当data变化时，会触发updated方法
7. beforeDestory---销毁前：组件销毁之前调用
8. destoryed---销毁后：组件销毁之后调用，对data的改变不会再触发周期函数，vue实例已解除事件监听和dom绑定，但dom结构依然存在

##### 简述生命周期
1. beforeCreate   实例准备开始创建
2. created 实例创建完成，数据初始化完成
3. beforeMount 实例挂在之前，无法访问DOM
4. mounted  实例挂载完成，可以访问真实DOM
5. beforeUpdate  数据发生更新，访问的是旧的DOM，和最新的数据
6. updated：DOM更i新完成，可以访问最新的DOM
7. activated：组件被激活
8. deactivated：组件被冻结  
9. beforeDestroy：组件实例即将销毁
10. destroyed：组件实例销毁完成
11. deactivated/beforeDestroy必须是keep-alive的组件才会有
