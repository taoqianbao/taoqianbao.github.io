---
title: CSS语法中position:sticky的介绍
p: css/position-sticky
date: 2016-01-01 10:15:41
tags: [CSS,position,sticky]
categories: CSS
---

## 背景

用户的屏幕越来越大，而页面太宽的话会不宜阅读，所以绝大部分网站的主体宽度和之前相比没有太大的变化，于是浏览器中就有越来越多的空白区域，所以你可能注意到很多网站开始在滚动的时候让一部分内容保持可见，比如，侧边栏的部分区域。position:sticky为此而生。

<!--more-->

## position:sticky用法

position:sticky是一个新的css3属性，它的表现类似position:relative和position:fixed的合体，在目标区域在屏幕中可见时，它的行为就像position:relative; 而当页面滚动超出目标区域时，它的表现就像position:fixed，它会固定在目标位置。

使用起来也非常简单：

``` CSS
//目前来说还需要用私有前缀～～
.sticky { 
    position: -webkit-sticky; position:sticky; top: 15px;
}
```

## 浏览器兼容性
由于这是一个全新的属性，以至于到现在都没有一个规范，W3C也刚刚开始讨论它，而现在只有webkit nightly版本和chrome 开发版(Chrome 23.0.1247.0+ Canary)才开始支持它。

另外需要注意的是，如果同时定义了left和right值，那么left生效，right会无效，同样，同时定义了top和bottom，top赢～～

## polyfill 
虽然其它浏览器尚不支持，但是实现起来其实不难，我们可以用js简单实现：

[点击这里查看DEMO](https://output.jsbin.com/tuxuyuj/1)

##### HTML
``` HTML
<div class="header"></div>  
```
#####  CSS

``` CSS
.sticky { position: fixed; top: 0; } 
.header { width: 100%; background: #F6D565; padding: 25px 0; }
```
##### JS
``` JS
var header = document.querySelector('.header'); 
var origOffsetY = header.offsetTop; 
function onScroll(e) { 
    window.scrollY >= origOffsetY ? header.classList.add('sticky') : header.classList.remove('sticky'); 
} 
document.addEventListener('scroll', onScroll);
```

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
