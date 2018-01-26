---
title: H5页面终端适配解决方案
tags: [h5,flex,media]
p: h5/terminal-adapter
date: 2016-11-23 09:43:44
categories: H5
---

## 前言

无线端应用日益增长，终端机型也发展多样化，前端开发者面临的适配方案也越来越多，如何选择最佳适配方案？    
<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
    - [基本概念解读](#基本概念解读)
        - [CSS尺寸单位](#css尺寸单位)
        - [屏幕（screen）](#屏幕screen)
        - [视口（viewport）](#视口viewport)
        - [缩放（scale）](#缩放scale)
    - [解决方案-viewport](#解决方案-viewport)
    - [解决方案-rem](#解决方案-rem)
        - [动态设置 html 标签 font-size 大小](#动态设置-html-标签-font-size-大小)
        - [元素大小取值方法](#元素大小取值方法)
        - [rem 布局方案的开发方式](#rem-布局方案的开发方式)
        - [字体使用 px 为单位](#字体使用-px-为单位)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->
<!--more-->

## 正文

### 基本概念解读
像素（pixels）、屏幕（screen）、视口（viewport）、缩放（scale）
设备像素、CSS像素、设备像素比（window.devicePixelRatio）、

#### CSS尺寸单位

1.px：绝对单位，页面按精确像素展示，英文（pixels）
2.em：相对单位，基准点为父节点字体的大小，如果自身定义了font-size按自身来计算（浏览器默认字体是16px），整个页面内1em不是一个固定的值。
3.rem：相对单位，可理解为”root em”, 相对根节点html的字体大小来计算，CSS3新加属性，chrome/firefox/IE9+支持。
4.vw：viewpoint width，视窗宽度，1vw等于视窗宽度的1%。
5.vh：viewpoint height，视窗高度，1vh等于视窗高度的1%。
6.vmin：vw和vh中较小的那个。
7.vmax：vw和vh中较大的那个。
8.%:百分比
9.in:寸
10.cm:厘米
11.mm:毫米
12.pt:point，大约1/72寸
13.pc:pica，大约6pt，1/6寸
14.ex：取当前作用效果的字体的x的高度，在无法确定x高度的情况下以0.5em计算(IE11及以下均不支持，firefox/chrome/safari/opera/ios safari/android browser4.4+等均需属性加么有前缀)
15.ch:以节点所使用字体中的“0”字符为基准，找不到时为0.5em(ie10+,chrome31+,safair7.1+,opera26+,ios safari 7.1+,android browser4.4+支持)


#### 屏幕（screen）

#### 视口（viewport）

#### 缩放（scale）




### 解决方案-viewport
在移动端，可以通过 viewport 缩放页面大小比率达到目的。

简单来说，即所有宽高像素与视觉稿输出相同，然后通过页面宽度与视觉稿的宽度比率，动态设置 viewport。缩放方案核心代码参考：
``` JS
(function () {
    var docEl = document.documentElement;
    var isMobile = window.isMobile /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini|Mobi/i.test(navigator.userAgent);

    function setScale() {
        var pageScale = 1;

        if (window.top !== window) {
            return pageScale;
        }

        var width = docEl.clientWidth || 360;
        var height = docEl.clientHeight || 640;
        if (width / height >= 360 / 640) {
            // 高度优先
            pageScale = height / 640;
        } else {
            pageScale = width / 360;
        }

        var content = 'width=' + 360 + ', initial-scale=' + pageScale 
          + ', maximum-scale=' + pageScale + ', user-scalable=no';
        document.getElementById('viewport').setAttribute('content', content);

        window.pageScale = pageScale;
    }

    if (isMobile) {
        setScale();
    } else {
        docEl.className += ' pc';
    }
})()
```
但是如果希望 PC 上也能显示，由于没有 viewport 的缩放概念，只能以固定值来设定，这个效果就不太好。




### 解决方案-rem
rem 布局适配方案被提到的比较多，在各大互联网企业产品中都有较为广泛的应用。

简单来说其方法为：

按照设计稿与设备宽度的比例，动态计算并设置 html 根标签的 font-size 大小；
css 中，设计稿元素的宽、高、相对位置等取值，按照同等比例换算为 rem 为单位的值；
设计稿中的字体使用 px 为单位，通过媒体查询稍作调整。
下面我们举个例子来说明。

#### 动态设置 html 标签 font-size 大小
第一个问题是 html 标签的 font-size 动态计算。这取决于如何约定换算比例，以页面宽度十等份为例，核心代码参考：
``` JS
(function(WIN) {
    var  setFontSize = WIN.setFontSize = function (_width) {
        var  docEl = document.documentElement; 
        // 获取当前窗口的宽度
        var  width = _width || docEl.clientWidth; // docEl.getBoundingClientRect().width;

        // 大于 1080px 按 1080
        if (width > 1080) { 
            width = 1080;
        }

        var  rem = width / 10;
        console.log(rem);

        docEl.style.fontSize = rem + 'px';

        // 部分机型上的误差、兼容性处理
        var  actualSize = win.getComputedStyle && parseFloat(win.getComputedStyle(docEl)["font-size"]);
        if (actualSize !== rem && actualSize > 0 && Math.abs(actualSize - rem) > 1) {
            var remScaled = rem * rem / actualSize;
            docEl.style.fontSize = remScaled + 'px';
        }
    }

    var timer;
    //函数节流
    function dbcRefresh() {
        clearTimeout(timer);
        timer = setTimeout(setFontSize, 100);
    }

    //窗口更新动态改变 font-size
    WIN.addEventListener('resize', dbcRefresh, false);
    //页面显示时计算一次
    WIN.addEventListener('pageshow', function(e) {
        if (e.persisted) { 
            dbcRefresh() 
        }
    }, false);
    setFontSize();
})(window)
```
另外，对于全屏显示的 H5 活动页，对宽高比例有所要求，此时应当做的调整。可以这么来做：
``` JS
function adjustWarp(warpId = '#warp') {
    // if (window.isMobile) return;
    const $win = $(window);
    const height = $win.height();
    let width = $win.width();

    // 考虑导航栏情况
    if (width / height < 360 / 600) {
        return;
    }

    width = Math.ceil(height * 360 / 640);

    $(warpId).css({
        height,
        width,
        postion: 'relative',
        top: 0,
        left: 'auto',
        margin: '0 auto'
    });

    // 重新计算 rem
    window.setFontSize(width);
}
```
按照这种缩放方法，几乎在任何设备上都可以实现等比缩放的精确布局。

#### 元素大小取值方法
第二个问题是元素大小的取值。

以设计稿宽度 1080px 为例，我们将宽度分为 10 等份以便于换算，那么 1rem = 1080 / 10 = 108px。

设计稿中，有一个图片大小为 460x210，相对页面位置 top: 321px; left: 70;。其换算方法：
``` JS
const px2rem = function(px, rem = 108) {
    let remVal = parseFloat(px) / rem;

    if (typeof px === "string" && px.match(/px$/)) { 
        remVal += 'rem';
    }

    return remVal;
}
```
由此得到该元素最终的 css 样式应为：
``` JS
.img_demo {
    position: absolute;
    background-size: cover;
    background-image: url('demo.png');
    top: 2.97222rem;
    left: 0.64814rem;
    width: 4.25926rem;
    height: 1.94444rem;
}
```
#### rem 布局方案的开发方式
通过以上方法，rem 布局方案就得到了实现。但是手动计算 rem 的取值显然不现实。
通过 less/sass 预处理工具，我们只需要设置 mixins 方法，然后按照设计稿的实际大小来取值即可。以 less 为例，mixins 参考如下：


``` JS
// px 转 rem
.px2rem(@px, @attr: 'width', @rem: 108rem) {
    @{attr}: (@px / @rem);
}

.px2remTLWH(@top, @left, @width, @height, @rem: 108rem) {
    .px2rem(@top, top, @rem);
    .px2rem(@left, left, @rem);
    .px2rem(@width, width, @rem);
    .px2rem(@height, height, @rem);
}
```
针对前文的示例元素，css 样式可以这样来写：

``` JS
.img_demo {
    position: absolute;
    background-size: cover;
    background-image: url('demo.png');

    .px2remTLWH(321, 70, 460, 210);
}

```
这里，宽和高可以直接通过设计稿输出的图片元素大小读取到；top/left 的取值，可以通过在 Photoshop 中移动参考线定位元素快速得到。


#### 字体使用 px 为单位
字体使用 rem 等比缩放会出现显示上的问题，只需要针对性使用媒体查询设置几种大小即可。

示例参考：
``` JS

// 字体响应式
@media screen and (max-width: 321px) {
    body {
        font-size: 13px;
    }
}

@media screen and (min-width: 321px) and (max-width: 400px) {
    body {
        font-size: 14px;
    }
}

@media screen and (min-width: 400px) {
    body {
        font-size: 16px;
    }
}
```


## 小结

[H5移动多终端适配全解 - 从原理到方案](https://zhuanlan.zhihu.com/p/25422063)
[CSS单位详解](https://www.w3.org/Style/Examples/007/units.en.html)


## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
