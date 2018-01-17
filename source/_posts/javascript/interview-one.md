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


### 前端基础

+ prototype和__proto__的关系是什么
+ meta viewport原理
+ 域名收敛是什么
+ float和display：inline-block；的区别
+ 前端优化策略列举
+ 首屏、白屏时间如何计算
+ 闭包
+ 作用域链
+ ajax如何实现、readyState五中状态的含义
+ jsonp如何实现
+ 怎么处理跨域
+ restful的method解释
+ get和post的区别
+ 事件模型解释
+ 编写一个元素拖拽的插件
+ 编写一个contextmenu的插件
+ 编写web端cookie的设置和获取方法
+ 兼容ie６的水平垂直居中
+ 兼容ie的事件封装
+ h5和原生android的优缺点
+ 编写h5需要注意什么
+ xss和crsf的原理以及怎么预防
+ css优先级
+ 如何实现点击radio的文字描述控制radio的状态（通过label实现）
+ delegate如何实现



### 框架原理

#### angularjs

angular的directive怎么写
angular的脏检查（双向绑定）是如何实现的
依赖注入如何实现
scope如何实现
$parse模块如何实现（主要自己写了一个类似的库）

#### react

react在setState后发生了什么（直接说了setState源码）
flux解释
对react有什么了解（直接说了react中虚拟dom内部表示，mount过程源码和同步过程源码）
jsBridge

如何说服对方使用jsBridge
requirejs

amd和cmd区别，怎么了解到这些区别的，是否是去看了规范
requirejs那些经常用的方法，然后对其进行解释
weex

weex实现大致原理（只写过demo，面试管很好没有难为我，只问了这一个问题）

http协议

accept是什么，怎么用
http协议状态码，302和303的区别
前端缓存如何实现、etag如何实现、etag和cache-control的max-age的优先级哪个比较高以及为什么、cache-control和expire优先级哪个比较高以及为什么
node

Buffer模块是干什么的
Stream是什么，使用的两种模式
http模块如何将异步处理方式实现成同步处理方式，具体解析请参考http模块如何将异步处理转成同步处理
其他问题

utf8和gbk的区别
知道页面上某个点的坐标，如何获取该坐标上的所有元素
angular、react和jQuery适合哪些应用场景（建议查看各个框架产生背景）
7点15分小于180度的夹角是多少
大数相加
给５升和６升的水杯如何倒出３升的水
一班喜欢足球的人60%，喜欢排球的70%，喜欢篮球的80%，求喜欢足球和排球的占多少
前端异常监测如何实现
直播点赞按钮的冒泡功能如何实现
js的uglify如何实现
项目架构、如何带人（自己带过一个小团队）
前端工程化方面做了哪些东西
面试中的收获

最开始面试时只阅读过angular源码，阿里一面完后面试官对我说react用的不熟悉没关系，弄懂原理也可以，之后三天疯狂阅读react源码，对于react中虚拟dom内在表示、mount过程、setState的同步过程有了清晰的认识。
面试官建议去阅读node的http模块和Stream模块源码，其中node-v0.1.100的http模块源码已经阅读完，并且写了一个基于net模块的http模块。node-v6.9.1的Stream模块源码现在还在阅读中。
初步了解了前端异常监测，并且了解了百姓网、腾讯和阿里在前端异常监测的一些方案和框架。
阅读了大量前端工程化方面的博文，对前端工程化有了进一步的理解。
了解了angular和react产生背景。



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

#### 什么是Etag？
当发送一个服务器请求时，浏览器首先会进行缓存过期判断。浏览器根据缓存过期时间判断缓存文件是否过期。
情景一：若没有过期，则不向服务器发送请求，直接使用缓存中的结果，此时我们在浏览器控制台中可以看到 200 OK(from cache) ，此时的情况就是完全使用缓存，浏览器和服务器没有任何交互的。
情景二：若已过期，则向服务器发送请求，此时请求中会带上①中设置的文件修改时间，和Etag
然后，进行资源更新判断。服务器根据浏览器传过来的文件修改时间，判断自浏览器上一次请求之后，文件是不是没有被修改过；根据Etag，判断文件内容自上一次请求之后，有没有发生变化。
情形一：若两种判断的结论都是文件没有被修改过，则服务器就不给浏览器发index.html的内容了，直接告诉它，文件没有被修改过，你用你那边的缓存吧—— 304 Not Modified，此时浏览器就会从本地缓存中获取index.html的内容。此时的情况叫协议缓存，浏览器和服务器之间有一次请求交互。
情形二：若修改时间和文件内容判断有任意一个没有通过，则服务器会受理此次请求，之后的操作同①
① 只有get请求会被缓存，post请求不会


#### Expires和Cache-Control
Expires要求客户端和服务端的时钟严格同步。HTTP1.1引入Cache-Control来克服Expires头的限制。如果max-age和Expires同时出现，则max-age有更高的优先级。

``` JS
    Cache-Control: no-cache, private, max-age=0
    ETag: abcde
    Expires: Thu, 15 Apr 2014 20:00:00 GMT
    Pragma: private
    Last-Modified: $now // RFC1123 format
```


## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
