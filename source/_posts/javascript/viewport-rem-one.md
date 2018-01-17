---
title: WEB页面自适应解决方案
p: javascript/viewport-rem-one
date: 2016-07-15 10:14:46
tags: [web, viewport, media, rem, css, javascript]
categories: [h5]
---

## 背景

目录
1 viewport 缩放方案
2 rem 布局适配方案
2.1 动态设置 html 标签 font-size 大小
2.2 元素大小取值方法
2.3 rem 布局方案的开发方式
2.4 字体使用 px 为单位
3 相关参考

拿到设计稿后，如何进行布局还原？

如果只需要做非精确的响应式设计，那么使用媒体查询来实现就 OK 了。如果需要精确还原设计稿，则一般通过缩放来实现。常见方案有基于 viewport 和基于 rem 的缩放方案。

<!--more-->

### 1 viewport 缩放方案

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


## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
