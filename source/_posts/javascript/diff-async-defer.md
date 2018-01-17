---
title: JS中async与defer区别
p: javascript/diff-async-defer
date: 2014-01-07 10:48:32
tags: [javascript, async, defer]
categories: Javascript
---

## 前言

JS中async与defer区别

<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
- [如何选用？](#如何选用)
- [兼容性](#兼容性)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->


<!--more-->

## 正文

先来试个一句话解释仨，当浏览器碰到 script 脚本的时候：

- script
``` JS 
<script src="script.js"></script>
```
解释1：没有 defer 或 async，浏览器会立即加载并执行指定的脚本，“立即”指的是在渲染该 script 标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。
解释2：script 脚本不设置任何属性。HTML文档解析过程中，遇到script文档时，会停止解析HTML文档，发送请求获取script文档（如果是外部文档的话）。脚本执行后，才恢复HTMl文档解析。

- script async 
``` JS 
<script async src="script.js"></script>
```
解释1：有 async，加载和渲染后续文档元素的过程将和 script.js 的加载与执行并行进行（异步）。
解释2：设置async属性后，在HTML解析的同时，下载script文档。script文档下载完成后，HTMl解析会暂停，来执行script文档。

- script defer
``` JS
<script defer src="myscript.js"></script>
```
解释1：有 defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），但是 script.js 的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。
解释2：设置defer属性后，在HTML解析的同时，下载script脚本。但只有在HTML解析完成后，才执行script文档。同时，defer属性保证脚本按照其在文档中出现的顺序执行。

接着，我们来看一张图咯：
![图1 浏览器解释1](/imgs/javascript/browser-lifecycle.jpeg)


然后从实用角度来说呢，首先把所有脚本都丢到 </body> 之前是最佳实践，因为对于旧浏览器来说这是唯一的优化选择，此法可保证非脚本的其他一切元素能够以最快的速度得到加载和解析。


蓝色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的；绿色线代表 HTML 解析。

此图告诉我们以下几个要点：

- defer 和 async 在网络读取（下载）这块儿是一样的，都是异步的（相较于 HTML 解析）
- 它俩的差别在于脚本下载完之后何时执行，显然 defer 是最接近我们对于应用脚本加载和执行的要求的
- 关于 defer，此图未尽之处在于它是按照加载顺序执行脚本的，这一点要善加利用
- async 则是一个乱序执行的主，反正对它来说脚本的加载和执行是紧紧挨着的，所以不管你声明的顺序如何，只要它加载完了就会- 立刻执行
- 仔细想想，async 对于应用脚本的用处不大，因为它完全不考虑依赖（哪怕是最低级的顺序执行），不过它对于那些可以不依赖任何脚本或不被任何脚本依赖的脚本来说却是非常合适的，最典型的例子：Google Analytics

![图2 浏览器解释2](/imgs/javascript/browser-lifecycle-en.png)


## 如何选用？
通常情况下，尽可能的使用async属性，然后考虑defer，都不适用时才不设置任何属性。选用规则：

- 如果脚本是模块化的，并且不依赖其他脚本，那么使用async。
- 如果脚本依赖其他脚本或被其他脚本依赖，那么使用defer。
- 如果脚本比较小，并且被一个async脚本依赖，那么使用行内脚本，并放置在async脚本之前。

## 兼容性

带有defer属性的脚本执行也不一定按照顺序执行，有风险；
IE9及以下浏览器在实现defer属性上存在糟糕的bug，比如无法保证脚本的执行顺序。如果需要支持<=IE9，不建议使用defer，如果执行顺序非常重要的话，不要使用任何属性。


## 小结
Chrome为了更快的页面加载，引入了两项JavaScript新技术：script streaming 和 code caching。简而言之，前者优化脚本文档的解析(Chrome 41)，后者缓存编译后的代码(Chrome 42)。

![图3 浏览器stream](/imgs/javascript/streaming.png)

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
