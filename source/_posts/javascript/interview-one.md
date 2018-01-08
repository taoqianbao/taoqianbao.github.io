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

<!--more-->

## 问题

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


## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
