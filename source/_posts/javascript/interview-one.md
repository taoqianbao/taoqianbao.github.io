---
title: 前端面试题整理之基础问答篇
p: javascript/interview-one
date: 2016-01-08 09:26:33
tags: [Javascript, 面试, 题目]
categories: Javascript
---

## 目录

1. [前端面试题整理之基础问答篇](/2016/01/08/javascript/interview-one/)
2. [前端面试题整理之高级上机篇](/2017/01/08/javascript/interview-two/)
3. [前端面试题整理之资深理论篇](/2018/01/08/javascript/interview-three/)
4. [前端面试题整理之理想篇](/2018/01/08/javascript/interview-four/)

<!--more-->

## 问题

1. CSS基础知识
2. JS基础知识
3. HTML基础知识
4. DOM/BOM基础知识

#### CSS中position的值， relative和absolute分别是相对于谁进行定位的？
absolute：生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。
fixed： （老IE不支持）生成绝对定位的元素，通常相对于浏览器窗口或 frame 进行定位。
relative： 生成相对定位的元素，相对于其在普通流中的位置进行定位。
static： 默认值。没有定位，元素出现在正常的流中
sticky： 生成粘性定位的元素，容器的位置根据正常文档流计算得出  
关于sticky，[更多介绍](/2016/01/01/css/position-sticky/)

#### XML和JSON的区别？
(1).数据体积方面。
    JSON相对于XML来讲，数据的体积小，传递的速度更快些。
(2).数据交互方面。
    JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。
(3).数据描述方面。
    JSON对数据的描述性比XML较差。
(4).传输速度方面。
    JSON的速度要远远快于XML。

#### 说说你对作用域链的理解
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。

#### 创建ajax过程
+ (1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.
+ (2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
+ (3)设置响应HTTP请求状态变化的函数.
+ (4)发送HTTP请求.
+ (5)获取异步调用返回的数据.
+ (6)使用JavaScript和DOM实现局部刷新.


#### 渐进增强和优雅降级
渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。


#### HTTP和HTTPS
HTTP协议通常承载于TCP协议之上，在HTTP和TCP之间添加一个安全协议层（SSL或TSL），这个时候，就成了我们常说的HTTPS。
默认HTTP的端口号为80，HTTPS的端口号为443。

#### 为什么HTTPS安全
因为网络请求需要中间有很多的服务器路由器的转发。中间的节点都可能篡改信息，而如果使用HTTPS，密钥在你和终点站才有。https之所以比http安全，是因为他利用ssl/tsl协议传输。它包含证书，卸载，流量转发，负载均衡，页面适配，浏览器适配，refer传递等。保障了传输过程的安全性

#### 对前端模块化的认识 
AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
AMD 是提前执行，CMD 是延迟执行。
AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。
[更多详细说明](/2015/01/01/javascript/modules-one/)




## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
