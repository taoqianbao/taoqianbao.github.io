---
title: 小程序开发之入门基础知识
tags: [小程序]
p: miniapp/how-to-study
date: 2018-02-08 09:07:52
categories: 小程序
---

## 前言

公司开始大力推广微信小程序，由于有过小程序的开发经验，自当起到带头作用，承担起学习引入人的作用。
本文主要是适合基础无前端开发经验人群，有意向开发微信小程序的伙伴阅读。
所有内容来自互联网和自我读后感，有任何差错，请忽略....

<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
    - [01. 小程序之入门篇](#01-小程序之入门篇)
    - [02. 小程序之目录结构](#02-小程序之目录结构)
    - [03. 小程序之生命周期](#03-小程序之生命周期)
    - [04. 小程序之数据绑定](#04-小程序之数据绑定)
    - [05. 小程序之触控事件](#05-小程序之触控事件)
    - [06. 小程序之基础组件篇之视图容器](#06-小程序之基础组件篇之视图容器)
    - [07. 小程序之基础组件篇之基础内容](#07-小程序之基础组件篇之基础内容)
    - [08. 小程序之基础组件篇之表单组件](#08-小程序之基础组件篇之表单组件)
    - [09. 小程序之基础组件篇之导航组件](#09-小程序之基础组件篇之导航组件)
    - [10. 小程序之其它组件](#10-小程序之其它组件)
    - [11. 小程序之拓展篇之调试工具](#11-小程序之拓展篇之调试工具)
    - [12. 小程序之拓展篇之样式库weui-wxss](#12-小程序之拓展篇之样式库weui-wxss)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->

<!--more-->

## 正文
其实入门教程系列，再好也比不了[官方文档](https://mp.weixin.qq.com/)，写的很全，向导也很到位，但是就当写读后感，再提炼下吧。

心急吃不了豆腐，但是，还是先尝试Hello World开发吧。


### 01. 小程序之入门篇

*** 环境搭建 ***
首先跳入[简易教程](https://mp.weixin.qq.com/debug/wxadoc/dev/index.html)，根据向导走一遍流程，我们通过开发者工具快速创建了一个 QuickStart 项目，这里完全靠官方文档走一遍，是不是很轻松？


*** 项目配置 ***
接下来，我们讲一讲小程序的代码构成要素： [官方资料](https://mp.weixin.qq.com/debug/wxadoc/dev/quickstart/basic/file.html)

![配置文件图](/imgs/miniapp/miniapp-fileintro.png)

*** JSON配置 ***
这里主要有 
- app.json
- project.config.json
- page.json

*** app.json *** 是对当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。
其他配置项细节可以参考文档 [小程序的配置 app.json](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/config.html)

![app.json配置项列表](/imgs/miniapp/miniapp-app-config.png)

*** 工具配置 project.config.json *** 其他配置项细节可以参考文档 [开发者工具的配置](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/edit.html#%E9%A1%B9%E7%9B%AE%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

*** 页面配置 page.json *** 其他配置项细节可以参考文档 [小程序的配置 page.json](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/config.html)

*****************************

### 02. 小程序之目录结构

框架提供了自己的视图层描述语言 WXML 和 WXSS，以及基于 JavaScript 的逻辑层框架，并在视图层与逻辑层间提供了数据传输和事件系统，可以让开发者可以方便的聚焦于数据与逻辑上。

- JSON配置
- WXML模版
- WXSS样式
- JS逻辑交互

MVVM
![MVVM](/imgs/miniapp/miniapp-mvvm1.png)

*****************************


### 03. 小程序之生命周期

小程序生命周期
- APP生命周期
- 页面生命周期
*****************************
*** APP生命周期示例 ***

``` JS 
App({
  onLaunch: function(options) {
    // Do something initial when launch.
  },
  onShow: function(options) {
      // Do something when show.
  },
  onHide: function() {
      // Do something when hide.
  },
  onError: function(msg) {
    console.log(msg)
  },
  globalData: 'I am global data'
})
```
*****************************
*** 页面生命周期示例 ***

``` JS
//index.js  
Page({
  data: {
    text: "This is page data."
  },
  onLoad: function(options) {
    // Do some initialize when page load.
  },
  onReady: function() {
    // Do something when page ready.
  },
  onShow: function() {
    // Do something when page show.
  },
  onHide: function() {
    // Do something when page hide.
  },
  onUnload: function() {
    // Do something when page close.
  },
  onPullDownRefresh: function() {
    // Do something when pull down.
  },
  onReachBottom: function() {
    // Do something when page reach bottom.
  },
  onShareAppMessage: function () {
   // return custom share data when user share.
  },
  onPageScroll: function() {
    // Do something when page scroll
  },
  onTabItemTap(item) {
    console.log(item.index)
    console.log(item.pagePath)
    console.log(item.text)
  },
  // Event handler.
  viewTap: function() {
    this.setData({
      text: 'Set some data for updating view.'
    }, function() {
      // this is setData callback
    })
  },
  customData: {
    hi: 'MINA'
  }
})
```
*****************************

### 04. 小程序之数据绑定

*** 数据传值 ***

- 页面内
- 页面与页面之间
- 父组件与子组件之间

*****************************

![视图驱动视图更新](/imgs/miniapp/miniapp-mvvm2.png)

简单的讲，对象状态化，只要对象状态发送变化，就通知页面更新视图元素。 通过以下三个步骤实现：

- 识别哪个UI元素被绑定了相应的对象。
- 监视对象状态的变化。
- 将所有变化传播到绑定的视图上。

  注意数据流向是单向的，即视图变化不会影响对象状态。

以下将展示小程序提供的更加多元化的复杂的数据绑定方式。
``` JS
index.wxml
<!--数据绑定--内容-->
<view>{{message}}</view>

<!--数据绑定--组件属性-->
<view id="item-{{id}}">组件属性id-{{id}}</view>

<!--数据绑定---控制属性-->
<view wx:if="{{condition}}">控制属性{{condition}}</view>

<!--数据绑定---三元运算-->
<view hidden="{{flag ? true : false}}">Hidden--{{flag}}</view>

<!--数据绑定---算数运算-->
<view>{{a + b}} + {{c}} + d</view>

<!--数据绑定---逻辑判断-->
<view wx:if="{{length > 5}}">6</view>

<!--数据绑定---字符串运算-->
<view>{{"Hello  " + name}}</view>

<!--数据绑定---数组组合-->
<view wx:for="{{[zero, 1, 2, 3, 4, 5, 6]}}">{{item}}</view>

<!--数据绑定---对象-->
<!--最终组合成的对象是{for: 1, bar: 2}-->
<template is="objectCombine" data="{{for: x, bar: y}}"></template>

<!--数据绑定---扩展运算符 ... 来将一个对象展开-->
<!--最终组合成的对象是{a: 1, b: 2, c: 3, d: 4, e: 5}-->
<template is="objectCombine" data="{{...obj1, ...obj2, e: 5}}"></template>

<!--数据绑定---对象的 key 和 value 相同-->
<!--最终组合成的对象是{foo: 'my-foo', bar:'my-bar'}-->
<template is="objectCombine" data="{{foo, bar}}"></template>
```

对应的逻辑层代码如下:

``` JS
index.js
Page({
  data:{
    //内容
    message:'Hello MINA!',
    //组件属性
    id: 0,
    //控制属性
    condition: true,
    //三元运算
    flag:false,
    //算数运算
    a: 1,
    b: 2,
    c: 3,
    //逻辑判断
    length: 6,
    //字符串运算
    name: 'MINA',
    //数组组合
    zero: 0,
    //对象
    x: 0,
    y: 1,
    //对象展开
    obj1: {
        a: 1,
        b: 2
    },
    obj2: {
        c: 3,
        d: 4
    },
    e: 5,
    //对象key和value相同
    foo: 'my-foo',
    bar: 'my-bar'
  },
})
```

所以视图上的数据都必须用过事件传递给对象，只有用户操作视图，才能获取到数据，并更新对象状态。调用this.setData（）方法实现视图的部分渲染。如下图：
![视图-对象](/imgs/miniapp/miniapp-view-object.png)

### 05. 小程序之触控事件
主要是页面交互事件的处理
[官方文档](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/event.html)

### 06. 小程序之基础组件篇之视图容器

视图容器组件主要有：
- view
- scroll-view
- swiper
- movable-view
- cover-view

[官方文档之视图容器](https://mp.weixin.qq.com/debug/wxadoc/dev/component/view.html)

*** WXML模版 ***
WXML(WeiXin Markup Language), [语法介绍](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/)

视图层
- 数据绑定
- 条件渲染
- 列表渲染
- 事件

*** WXSS样式 ***
小程序布局

*** CSS盒子布局 ***
布局的传统解决方案，基于盒状模型，依赖 display属性 + position属性 + float属性。
它对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。

![css盒子模型](/imgs/css/css-box.png)

*** 弹性盒子Flex布局 ***
Flex是Flexible Box的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。 
[Flex布局示例](https://github.com/taoqianbao/tqb-miniapp-flex)

*** Tips:CSS样式的优先级 ***
选择器的优先权:
（外部样式）External style sheet <（内部样式）Internal style sheet <（内联样式）Inline style

计算规则：
1.  内联样式表的权值最高 1000
2.  ID 选择器的权值为 100
3.  Class 类选择器的权值为 10
4.  HTML 标签选择器的权值为 1

Tips:
CSS 优先级法则:
    A  选择器都有一个权值，权值越大越优先；
    B  当权值相等时，后出现的样式表设置要优于先出现的样式表设置；
    C  创作者的规则高于浏览者：即网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式；
    D  继承的CSS 样式不如后来指定的CSS 样式；
    E  在同一组属性设置中标有“!important”规则的优先级最大

**************************


### 07. 小程序之基础组件篇之基础内容
基础组件：icon,text,rich-text,progress
[官方文档之基础内容篇](https://mp.weixin.qq.com/debug/wxadoc/dev/component/icon.html)

### 08. 小程序之基础组件篇之表单组件
表单组件：button,checkbox,form,input,label,picker,picker-view,radio,slider,switch,textarea
[官方文档之表单组件篇](https://mp.weixin.qq.com/debug/wxadoc/dev/component/button.html)

### 09. 小程序之基础组件篇之导航组件
[官方文档之页面导航](https://mp.weixin.qq.com/debug/wxadoc/dev/component/navigator.html)

[官方文档之页面路由](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/route.html)

页面路由
在小程序中所有页面的路由全部由框架进行管理。

主要了解的概念：
- 页面栈
- getCurrentPages()
- 路由方式：

[页面路由示例代码下载](https://github.com/taoqianbao/tqb-miniapp-guide)
![示例图](/imgs/miniapp/miniapp-route-demo.png)

Tips:
- navigateTo, redirectTo 只能打开非 tabBar 页面。
- switchTab 只能打开 tabBar 页面。
- reLaunch 可以打开任意页面。
- 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
- 调用页面路由带的参数可以在目标页面的onLoad中获取。


### 10. 小程序之其它组件
主要有：媒体组件、地图、画布、其它开放能力


### 11. 小程序之拓展篇之调试工具
如何调试小程序，在开发过程中至关重要，下面图就是：

主要有三种调试工具：
1. 小程序开发工具
![小程序开发工具调试窗口](/imgs/miniapp/miniapp-debug1.png)

2. 手机端调试窗口：
![小程序开发工具调试窗口](/imgs/miniapp/miniapp-debug2.jpeg)
![小程序开发工具调试窗口](/imgs/miniapp/miniapp-debug3.jpeg)
![小程序开发工具调试窗口](/imgs/miniapp/miniapp-debug4.jpeg)

3. 抓包工具 charles等 

*****************************
### 12. 小程序之拓展篇之样式库weui-wxss
[样式框架](https://github.com/Tencent/weui-wxss)


*****************************


## 小结
到此，您对项目还有哪些问题吗？

以下是整理的资源:

[官方开发文档](https://mp.weixin.qq.com/debug/wxadoc/dev/)
 - [页面路由官网资料](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/route.html)


学习资料
 - [样式优选级](http://www.cnblogs.com/xugang/archive/2010/09/24/1833760.html)
 - [flex布局语法](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
 - [flex布局实战](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)


[开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/devtools.html)

示例代码
 - [页面路由示例代码下载](https://github.com/taoqianbao/tqb-miniapp-guide)
 - [页面布局示例代码下载](https://github.com/taoqianbao/tqb-miniapp-flex)

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
