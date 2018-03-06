---
title: Javascript入门指引
tags: [js,入门]
p: javascript/js-starter
date: 2016-02-27 16:03:30
categories: Javascript
---

## 前言

JavaScript，一种高级编程语言，通过解释执行，是一门动态类型，面向对象（基于原型）的直译语言。它已经由ECMA（欧洲电脑制造商协会）通过ECMAScript实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、FireFox、Safari、Opera）支持。JavaScript是一门基于原型、函数先行的语言，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本，数组，日期以及正则表达式等，不支持I/O，比如网络，存储和图形等，但这些都可以由它的宿主a环境提供支持。

<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
    - [背景](#背景)
    - [前端级别思考](#前端级别思考)
    - [推荐书目](#推荐书目)
    - [在线课程](#在线课程)
    - [视频教程](#视频教程)
    - [应用推荐](#应用推荐)
    - [标准文档](#标准文档)
    - [在线资源](#在线资源)
    - [实践项目](#实践项目)
    - [参考资料](#参考资料)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->

<!--more-->

## 正文

### 背景

虽然JavaScript与Java这门语言不管是在名字上，或是在语法上都有很多相似性，但这两门编程语言从设计之初就有很大的不同，JavaScript的语言设计主要受到了Self（一种基于原型的编程语言）和Scheme（一门函数式编程语言）的影响[5]。在语法结构上它又与C语言有很多相似（例如if条件语句、while循环、switch语句、do-while循环等）。

在客户端，JavaScript在传统意义上被实现为一种解释语言，但在最近，它已经可以被即时编译（JIT）执行。随着最新的HTML5和CSS3语言标准的推行它还可用于游戏、桌面和移动应用程序的开发和在服务器端网络环境运行，如Node.js。

1995年，当时在网景公司就职的布兰登·艾克正为Netscape Navigator 2.0浏览器开发的一门名为LiveScript的脚本语言，后来网景公司与昇阳电脑公司组成的开发联盟为了让这门语言搭上java这个编程语言“热词”，将其临时改名为“JavaScript”，日后这成为大众对这门语言有诸多误解的原因之一。

JavaScript推出后在浏览器上大获成功，微软公司在不久后就为IE浏览器推出了JScript，以与处于市场领导地位的网景产品同台竞争。JScript也是一种JavaScript实现，这两个JavaScript语言版本在浏览器端共存意味着语言标准化的缺失，对这门语言进行标准化被提上了日程，在1997年，由网景、昇阳、微软、宝蓝等公司组织及个人组成的技术委员会在ECMA（欧洲计算机制造商协会）确定定义了一种名叫ECMAScript的新脚本语言标准，规范名为ECMA-262。JavaScript成为了ECMAScript的实现之一。

完整的JavaScript实现应该包含三个部分，即ECMAScript（语言核心）、DOM（文档对象模型）、BOM（浏览器对象模型）。

### 前端级别思考

    Winter：P5 看承担 P6 看深度 P7 看体系 P8 看规划 P9 看创造

- P5(前端开发工程师)：独立执行，娴熟运用
- P6(高级前端开发工程师)：主动执行，辅助团队
- P7(技术专家)：融会贯通，自有一套
- P8(高级技术专家)：锐意进取，运筹帷幄
- P9(资深技术专家)：无中生有。可以看下 @玉伯 [『从前端技术到体验科技（附演讲视频）』](https://zhuanlan.zhihu.com/p/32782686) 以及他们的[『 参加第一届蚂蚁体验科技大会 SEE Conf 2018 是什么体验？』](https://www.zhihu.com/question/263685257)
- P10(研究员)越往上越不要自我局限，我们首先是工程师，而前端只是你的一个出发点，不要成为你的界限。




### 推荐书目

[深入浅出JavaScript（中文版）](https://book.douban.com/subject/5337333/)
[Eloquent JavaScript](https://book.douban.com/subject/5402021/)
[JavaScript权威指南(第6版)](https://book.douban.com/subject/10549733/)
[JavaScript高级程序设计（第3版）](https://book.douban.com/subject/10546125/)
[JavaScript语言精粹](https://book.douban.com/subject/3590768/)
[JavaScript模式](https://book.douban.com/subject/11506062/)
[你不知道的JavaScript（上卷）](https://book.douban.com/subject/26351021/)
[jQuery基础教程（第3版）](https://book.douban.com/subject/10569608/)
[锋利的jQuery](https://book.douban.com/subject/10792216/)
[You Don't Know JS (book series)](https://github.com/getify/You-Dont-Know-JS)
    
### 在线课程
        
[W3School JS教程](http://www.w3school.com.cn/js/)
[JS 菜鸟教程](http://www.runoob.com/js/js-tutorial.html)
[JS 廖雪峰教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
[JS 阮一峰教程](http://javascript.ruanyifeng.com/)
[MDN 重新介绍JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)
[JavaScript Tutorials](http://www.htmldog.com/guides/javascript/)                
        
        
### 视频教程

[精通JavaScript开发](http://study.163.com/course/introduction.htm%3FcourseId%3D224014%23/courseDetail)
[JavaScript视频教程](http://www.jikexueyuan.com/course/javascript/2-0-0-0/)                
[Youtube JavaScript Tutorial for Absolute Beginners](https://www.youtube.com/watch%3Fv%3DXL9Ri8pO68w)
                
### 应用推荐
(IOS&English ONLY)
[Learn JavaScript by Sololearn Inc](https://appsto.re/cn/RsAY4.i)
[L2Code JavaScript – Learn to Code JavaScript by SparkNET Interactive](https://appsto.re/cn/Vvt4X.i)
[Mimo: Learn how to code through interactive tutorials and quizzes! by Johannes Berger](https://appsto.re/cn/C4TLdb.i)
[Learn JavaScript by Examples by Pretonic](https://appsto.re/cn/5lwBab.i)               
        
### 标准文档        
[Airbnb JS代码规范](https://github.com/airbnb/javascript")        
[Javascript in one pic 中文版](https://github.com/coodict/javascript-in-one-pic/blob/master/README-zh.md)
[W3C CSS CURRENT STATUS](https://www.w3.org/standards/techs/css%23w3c_all)        
[MDN JavaScript基础](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics)        
            
### 在线资源
[大前端导航](http://www.daqianduan.com/nav)
[Frontend Stuff](https://github.com/moklick/frontend-stuff)
[Frontend directory](https://frontend.directory/)
[Frontend Interview Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)

### 实践项目                
[GitHub - sethvincent/javascripting: Learn JavaScript by adventuring around in the terminal.](https://github.com/sethvincent/javascripting)
            
[FCC中文JS系列关卡](https://freecodecamp.cn/challenges/comment-your-javascript-code)                

[初窥Linux 之 我最常用的20条命令](http://blog.csdn.net/ljianhui/article/details/11100625)


### 参考资料
[维基百科 JavaScript](https://zh.wikipedia.org/wiki/JavaScript)


## 小结

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
