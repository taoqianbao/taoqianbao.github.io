---
title: HTML5 ARIA role
p: h5/aria-role
date: 2018-01-17 15:55:17
tags: [ARIA,H5,HTML5]
categories: H5
---


## 前言
ARIA就是可以让屏幕阅读器准确识别网页中的内容，变化，状态的技术规范，可以让盲人这类用户也能无障碍阅读！

<!--more-->

## 正文

HTML5 aria-* and role

　　在video-js的demo中看到了很多aria-*，不知道干嘛的。google一下，发现aria的意思是*** Accessible Rich Internet Application ***。 Accessible一般是为不方便的人士提供的功能，比如windows的放大镜，语音朗读，高对比度主题等。

　　主要内容是说明并演示了HTML5针对html tag增加的属性：role 和 aria-*。

　　role的作用是描述一个非标准的tag的实际作用。比如用div做button，那么设置div 的 role=“button”，辅助工具就可以认出这实际上是个button。

　　ARIA Roles

　　Use the ARIA role attribute to indicate that a generic tag is playing the role of a standard widget like a button.

　　而aria-*的作用就是描述这个tag在可视化的情境中的具体信息。比如，

``` JS
　　<div role="checkbox" aria-checked="checked"$amp;>amp;$lt;/div>
```

　　辅助工具就会知道，这个div实际上是个checkbox的角色，为选中状态。

　　Add ARIA for screen readers

　　ARIA attributes provides semantic information to screen readers that is normally conveyed visually.

　　Note that using ARIA does not automatically implement the standard widget behavior, you'll still need to add focus management and keyboard navigation yourself.

## 小结
[https://www.w3.org/TR/aria-in-html/](https://www.w3.org/TR/aria-in-html/)

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
