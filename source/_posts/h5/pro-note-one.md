---
title: 实战H5项目笔记之双11活动页
p: h5/pro-note-one
date: 2016-11-15 10:28:21
tags: [h5,project,note,css,javascript]
categories: h5
description: 实战H5项目笔记
---

## 1. 背景

加入实体电商，开始第一个项目，双11活动。 每年参加双11活动，今年第一次自己参与，有点兴奋。

这种活动页面虽然用不上太复杂的逻辑，但也有很多后台项目涉及不到的细节，让我们踩了不少的坑。这里针对项目开发过程中涉及的一些主要技术点作一下总结回顾。

<!-- TOC -->

- [1. 背景](#1-背景)
    - [1.1. 项目特点](#11-项目特点)
    - [1.2. Slider 实现](#12-slider-实现)
    - [1.3. 移动屏幕适配](#13-移动屏幕适配)
        - [1.3.1. 高度优先、viewport 等比缩放适配方案](#131-高度优先viewport-等比缩放适配方案)
        - [1.3.2. 绝对定位元素的微调方法](#132-绝对定位元素的微调方法)
        - [1.3.3. 设计稿图片等比缩放方法](#133-设计稿图片等比缩放方法)
        - [1.3.4. 快速布局方法](#134-快速布局方法)
    - [1.4. HTML5 动画实现](#14-html5-动画实现)
        - [1.4.1. CSS3 动画](#141-css3-动画)
        - [1.4.2. 引入游戏引擎方案](#142-引入游戏引擎方案)
        - [1.4.3. 使用 svg/canvas 操作库](#143-使用-svgcanvas-操作库)
    - [1.5. 微信分享](#15-微信分享)
        - [1.5.1. 微信 jssdk 分享 API](#151-微信-jssdk-分享-api)
        - [1.5.2. 不走微信 jssdk 的取巧方法](#152-不走微信-jssdk-的取巧方法)
    - [1.6. 性能优化相关](#16-性能优化相关)
        - [1.6.1. 页面资源异步加载](#161-页面资源异步加载)
        - [1.6.2. webp 支持](#162-webp-支持)
    - [1.7. 应用内登陆/分享](#17-应用内登陆分享)
    - [1.8. 工程化问题](#18-工程化问题)
    - [1.9. 运维发布问题](#19-运维发布问题)
    - [1.10. 其他](#110-其他)
    - [1.11. 小结](#111-小结)
- [2. 关于作者](#2-关于作者)

<!-- /TOC -->



<!--more-->

### 1.1. 项目特点
- 多页滑动效果，页面多，动画元素细节多，动画效果简单
- 个人页(主页面)需 Flyme 账号登录，自有应用内要尽量实现免登录
- 将会在魅族主流应用和社区里推广
- 可分享到微信、微博等主流社交媒体

### 1.2. Slider 实现
多页翻屏滑动的效果有很多开源的实现，当然希望尽量的简单自己实现也不是很复杂。我们选择的是百度开源的 iSlider，iSlider 是一款非常优秀的翻页滑动组件。除此之外，还有很多成熟的开源实现可选用：

- swiper http://www.swiper.com.cn
- iscroll https://github.com/cubiq/iscroll
- fullpage.js https://github.com/alvarotrigo/fullPage.js/
- Scrollify https://github.com/lukehaas/Scrollify
- onepage-scroll https://github.com/peachananr/onepage-scroll

### 1.3. 移动屏幕适配
移动端屏幕适配常用的方案有如下三种：
- 固定高度，宽度自适应
- 固定宽度/高度，viewport 缩放
- rem 做宽度，viewport 缩放

#### 1.3.1. 高度优先、viewport 等比缩放适配方案
因为涉及多页大量的动画元素，只能是绝对定位来快速布局，我们采取了第二种方案：页面以 320x640 作为基础大小布局，在移动端根据实际的页面大小等比缩放。主要适配代码参考：
``` JS
+function () {
    function isMobile() {
        return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini|Mobi/i.test(navigator.userAgent);
    }

    function setScale() {
        var pageScale = 1;

        if (window.top !== window) {
            return pageScale;
        }

        var width = document.documentElement.clientWidth || 360;
        var height = document.documentElement.clientHeight || 640;
        if (width / height >= 360 / 640) {
            // 高度优先
            pageScale = height / 640;
        } else {
            pageScale = width / 360;
        }

        var content = 'width=' + 360 + ', initial-scale=' + pageScale 
          + ', maximum-scale=' + pageScale + ', user-scalable=no';
        document.getElementById('viewport').setAttribute('content', content);

        return pageScale;
    }

    if (isMobile()) {
        setScale();
    } else {
        try {
            document.getElementsByTagName('html')[0].classList.add('pc');
        } catch (e) {}
    }
}
```
这种方案以高度优先，以使得全部内容都能够出现在可视区域。但有一个问题是，在 webview 内宽高比大于 9/16，于是实际采用了高度的比例缩放，基础宽度缩放后会小于屏幕宽度，由于涉及动画的元素采用了绝对定位，这导致这些元素显示上偏左，右边出现较多的空白。

#### 1.3.2. 绝对定位元素的微调方法
对于这种问题，一种简单快速的解决方案：取得基础宽度与真实宽度的缩放比，将所有绝对定位的元素按照该缩放比重新计算 left 位移。主要代码参考：
``` JS
import $ from './libs/zepto';

const width = document.documentElement.clientWidth || 360;
const height = document.documentElement.clientHeight || 640;
let pageScale = 1;

if (width / height >= 360 / 640) {
    pageScale = height / 640;
}

const offset = (width - pageScale * 360) / 2;
// 每当页面切换后调用
export function positionFix(dom) {
    if (pageScale === 1 || !offset) {
        return;
    }

    // 为一个页面
    const $dom = $(dom);

    if ($dom.hasClass('position-fixed')) {
        return;
    }

    $dom.find('.text-box i, img').forEach((dom, i) => {
        let $this = $(dom),
            left;

        // 只修改绝对定位的元素
        if ($this.css('position') !== 'absolute') {
            return;
        }

        left = +($this.css('left').replace('px', ''));

        $this.css('left', (left + offset) + 'px');
        // console.log($(dom).css('left'));
    });

    $dom.addClass('position-fixed');
}
```

#### 1.3.3. 设计稿图片等比缩放方法
页面缩放解决了不同屏幕大小的布局一致性。另外涉及的一个问题是，设计稿图片大小如何进行等比缩放？
这个问题也很简单，图片引用直接设置 width 以缩放到合适大小；雪碧图上的图片则按照 360×640 的页面大小进行缩放：
- 对于绝对定位的元素，使用 <font color="red">transform: scale(0.333)</font> 进行变换缩放
- 对于流式布局的元素，使用 <font color="red">zomm(0.333)</font> 进行缩放

#### 1.3.4. 快速布局方法
还有一个值得一提的问题是，如何做到布局元素与设计稿完全一致？

这个问题的解决方法是：使用静态的设计稿图片作为全屏背景，通过调整各元素到对应位置，从而实现快速定位。我们的一位同学给出了这个方法，并且给出了一个让页面元素可拖动并设置最终位置(left/right值)的 jQuery 插件，这使得我们的页面布局变得简单而高效。

### 1.4. HTML5 动画实现
动画实现方案一般来说可以选择CSS3 动画、引入游戏引擎或使用 svg/canvas。
#### 1.4.1. CSS3 动画
该方案技术成本简单，任何前端开发者都能快速上手，但细节实现上工作量大。

从简单快速开始的角度来说，CSS3 动画是我们的唯一选择。使用 CSS3 动画需要特别注意一点：修改 DOM 会导致页面重绘，在移动端容易出现卡顿现象。所以应尽可能避免使用会修改 DOM 的 css 属性，只使用 <font color="red">transform</font> 实现动画变换效果。

我们的页面动画都是循环运动的，全部需要对照动效设计逐一还原实现，花费了大量的人力和时间成本。实现上主要使用了 <font color="red">animation</font>、<font color="red">keyframe</font>、<font color="red">transform</font> 属性。

此外，对于常见的入场显示/滑入等动画效果，只需要使用 <font color="red">transformc</font> 和 <font color="red">transition</font> 即可实现需求。对于 css 动画也有很多优秀而成熟的动画库可用，一些参考：

- animate.css https://github.com/daneden/animate.css
- magic.css https://github.com/miniMAC/magic
- Hover.css http://ianlunn.github.io/Hover/
- velocity.js https://github.com/julianshapiro/velocity
- anime.js https://github.com/juliangarnier/anime

下面简单介绍一下另外的两种动画实现方案。
#### 1.4.2. 引入游戏引擎方案
使用 h5 游戏引擎可大幅度降低工作量，能够相对容易地实现复杂动画效果，但需要经验避免入深坑，有较高学习成本，并且需要设计师深度配合。一些参考：

- Hilo https://github.com/hiloteam/Hilo
- Phaser https://github.com/photonstorm/phaser
- pixi.js https://github.com/pixijs/pixi.js
- melonjs https://github.com/melonjs/melonjs
- playcanvas https://github.com/playcanvas/engine
- LayaAir http://www.layabox.com
- 白鹭引擎 https://www.egret.com

#### 1.4.3. 使用 svg/canvas 操作库
使用 svg/canvas/webGL 实现的动画效果会比较好，但实现工作量较大，对实践经验也有较高的要求。成熟的相关库参考：

- createJs https://github.com/CreateJS
- snap.svg https://github.com/adobe-webplatform/Snap.svg
- svg.js https://github.com/svgdotjs/svg.js
- d3.js https://d3js.org
- threes.js https://threejs.org

### 1.5. 微信分享
由于第一次做这种活动页，没有特别注意到微信内分享的问题，直到上线时才发现，分享出去的效果实在太难看，这导致几位留守到最后的同事紧急探讨协调方案，几乎整晚没睡觉。

#### 1.5.1. 微信 jssdk 分享 API
微信内开发应注意这几点：

- 下载微信开发者工具（或 TBS Studio），以调试微信内页面
- 需要通过认证的公众号或订阅号，取得微信 jssdk 分享接口所需的 appId 和 signature
- 需要后端 API 管理 signature 签名的生成与缓存
- window.history.pushState/replaceState 修改了 URL 时需要重新生成 signature 签名。由于没有仔细阅读文档并意识到这一点，在这个问题上坑了比较多的时间。

我们最终协调到一个部门的订阅号，并使用他们已实现了的后端 token 签名生成 API 来实现 jssdk 的分享 API，在 nginx 层对该 API 代理转发解决跨域安全性相关问题。微信分享主要代码参考：

``` JS
const isWeixinBrowser = /micromessenger/.test(navigator.userAgent.toLowerCase());
const wxJsdk = '//res.wx.qq.com/open/js/jweixin-1.1.0.js';
const jsApiList = ['checkJsApi', 'onMenuShareTimeline', 'onMenuShareAppMessage', 'onMenuShareQQ', 'onMenuShareWeibo'];
let opts = {
    title: 'My Flyme 独家记忆',
    desc: '当时光凝固，当回忆定格。回首2016 ，我与 Flyme 的点点滴滴都在这里。这是属于我和 Flyme 的独家记忆。',
    link: '',
    imgUrl: ''
}, loadedwx = false;

function initShareEvent(wx) {
    const option = {
        ...opts,
        trigger: function (res) {console.log('trigger', res)},
        success: function (res) { console.log('已分享', res) },
        cancel: function (res) { console.log('已取消', res) },
        fail: function (res) { console.log(JSON.stringify(res)) } 
    };

    wx.onMenuShareAppMessage(option); // 分享给朋友
    wx.onMenuShareTimeline(option);   // 分享到朋友圈
    wx.onMenuShareQQ(option);         // 分享到QQ
    wx.onMenuShareWeibo(option);      // 分享到微博
    wx.onMenuShareQZone(option);      // 分享到QZone
}
function checkJsApi(wx) {
    wx.checkJsApi({
        jsApiList,
        success: () => initShareEvent(wx),
        error: err => console.log('checkJsApi error: ', err)
    });
}
function initConfig(wx) {
    // 特别注意，这里 link 必须使用当前页面的 URL 地址，否则会失败！
    opts.link = encodeURIComponent(document.location.href.split('#')[0]);
    return $.ajax({
        url: '/wechat_api/get_js_ticket?&url=' + opts.link,
        dataType:'jsonp', //指定为jsonp类型
        jsonp:'callback'
    }).done((res) => {
        wx.config({
            debug: process.env.NODE_ENV === 'development',
            appId: 'wx0000000000000000',
            nonceStr: res.data.nonceStr,
            timestamp: res.data.timestamp,
            signature: res.data.signature,
            jsApiList
        });
    });
}
export default options => {
    if (loadedwx || !isWeixinBrowser) { return }

    require([wxJsdk], (wx) => {
        loadedwx = true;
        $.extend(true, opts, options);
        initConfig(wx);
        wx.ready(() => checkJsApi(wx));
        wx.error((res) => console.error('出错了：', res.errMsg));
    });
}
```

#### 1.5.2. 不走微信 jssdk 的取巧方法
微信分享 API 需要公众号或订阅号，临时的活动开发可能来不及折腾，那么一个折中的办法是这样的：在页面头部 img 标签设置分享显示的图片，设置高度和宽度为 0。示例：
``` JS
<img src="assets/webp/share.webp" style="width:0;height:0;overflow:hidden"/>
```
微信会提取页面标题和第一张图片，作为朋友圈分享的标题和缩略图。使用 jssdk 方式分享到朋友圈的效果也是只有标题和缩略图，所以效果上没有区别。比较大的区别是，“发送给好友”时没有描述，描述位置变成了页面 URL 地址。

### 1.6. 性能优化相关
由于时间紧而且设计稿是逐步给到的，很多细节的优化都没法去做。最终上线的版本首屏大小约 1.3M，在弱网下的加载时间会比较久一些，可优化空间还比较大。这里探讨一下我们主要考虑到的几个点。

#### 1.6.1. 页面资源异步加载
一共二十多个页面，我们按每页一个 html 模板和一个 less 文件的方式，按页面分工开发，在 index.html 页面以 script 模板方式引入，由 fis3 实现模板嵌入。通过 ajax 拿到数据后，根据数据替换模板中的数据占位符，并进行页面切割，然后生成 iSlider 需要的数据配置项。这样做的好处是 html 内容未写入到 DOM 时，涉及的静态资源图片不会被加载。

iSlider 默认至少加载 3 个页面，每一时刻也最多保存三个页面实例。于是首屏加载了三个页面，这正好符合我们的目的。

另外要提到的一点是，弱网下不同图片下载的时差较大，会使得不同位置的图片动画断断续续地出现。为了避免这种不好的效果，我们使用了一个简单的图片预加载机制，在预加载完首屏涉及的图片资源后才隐藏 loading 显示页面。

#### 1.6.2. webp 支持

该项目涉及图片资源 500 多张，只有手绘文字图片做了雪碧图处理。现在的移动端基本都支持 webp，使用 webp 是必须的。实际上使用 webp 后，图片目录的大小减小了 60%。

以前大家都是用智图这种在线工具处理少量的图片，搜索了一下，居然没有找到现成的批量生成 webp 的工具库，于是写了一个批量生成方法。这两天整理完善了一下，算是造了一个小轮子，需要的同学可以关注下，地址在这里：

webp 批量转换：https://github.com/lzwme/webp-batch-convert

更新：有同学提到了，imagemin 提供的插件 imagemin-webp 也可以实现类似的需求。看了项目源码，实现的原理是一样的，其仓库地址为： https://github.com/imagemin/imagemin-webp

### 1.7. 应用内登陆/分享

由于要在 Flyme 自带的近十个主要应用内作推广入口，涉及到两个问题：应用内分享和应用内免登录。

在协调这一块时发现，各应用都是独自制定的各不相同的 webview 内相关 js 接口和规范，同一应用不同版本的实现也可能有差异，或者根本没有相关实现；有规范的文档也不够齐全，并且都没有示例参考；没有各应用的开发测试版本来做调试。于是花了不少时间各种咨询，踩了不少的坑，效果也还是不尽人意，最终只在魅族浏览器上做到了期望的效果。

没有统一规范，各自造轮子，于是这种跨部门跨应用的功能需求变得如此艰难。导致这种现象的存在因素很多，可知的一点是也和公司内前端人员处于边缘化地位的现实有关。过去的一年里，基础技术支撑部门技术平台做了一套 hybridApp 解决方案，@chemdemo 同学还将 JSBridge 部分抽离开源了出来：https://github.com/chemdemo/hybrid-js 。可能是缺少高层足够的相关意识和支持力度，并没有在各业务软件内得到广泛应用，反而主要靠内部前端圈间沟通传播。不过这套方案为了简洁只实现了很少的通用 API 和可扩展方法，并没有继续实现各种业务适用的通用性扩展功能，自然也没有我们想要的应用内分享和 Flyme 免登录这两个功能。

对于这个问题有两点总结： 
- 统一的公共 SDK 的重要性：避免重复造轮子，健壮且具有一致性的 API、完善的文档。
- 再好的文档不如一个 demo

### 1.8. 工程化问题
项目初期使用 webpack 进行构建，但由于我们平时的经验以 fis3 为主，webpack 过于灵活的配置特性使得一些工程化需求需要花时间探索。在我们接手项目后一起讨论了一下，果断转为熟悉的 fis3 构建体系。使用 fis3 主要解决的问题有：

- less 编译
- es6 编译
- js/css 压缩合并
- 页面模板嵌入
- 发布时 CDN 单独域名的适配
- 发布时符合内部运维体系线上发布规范的目录路径修正

相比较为灵活的 webpack，fis3 更注重流程化整体解决方案，简单的数十行配置即可实现各种工程化需求。不过 fis3 的发展现在似乎进入了一个瓶颈期/稳定期，社区中对于在 rollup 和 webpack 中大热的 tree-shaking 等技术几乎都没有什么反应。希望它不要没落了，能有更多的创新吧。有两点期望：走国际化路线，出英文文档与社区支持，向国际顶级项目看进；跟进参考业界最新的工程化理念，如有必要出个 fis4 也未尝不可。

### 1.9. 运维发布问题
由于涉及到跨部门合作，也没有太多的时间，项目起初放在了内部的 *** gitlab *** 平台，没有走 git+gerrit+jenkins+运维发布平台 这一内部完整的体系。这样做在前期省去了项目创建、各种权限申请等一堆需要协调沟通的事情，开发协作效率也比较高，但到了发布的阶段就突显出了问题：每次发布都需要由运维人员手动操作，协调发布很花费时间。

另外静态页面也没有独立出来，想当然地简单的扔到后端目录中，结果导致前端的修改需要后端也必须作修改发布，增加了前后端协调的时间成本。

于是，在首次发布后又进行的几次小迭代中，每次迭代发布都涉及到多人手动协调，十分的浪费时间。

总结一句话：项目构建尽量与已有的成熟的规范一致，以少走弯路。

### 1.10. 其他

最后列举一些移动端 H5 开发可参考的内容：

- 真机调试 https://github.com/jieyou/remote_inspect_web_on_real_device
- vConsole: 客户端内 H5 调试 https://github.com/WechatFE/vConsole
- 移动端布局终极解决方案 https://github.com/imochen/hotcss
- mobileHack https://github.com/RubyLouvre/mobileHack
- H5项目常见问题汇总及解决方案 https://github.com/FrontEndRoad/HTML5-FAQ

### 1.11. 小结
这是我们第一次尝试这种活动页，在如此紧凑的时间节点下，没有什么高大上的东西，更多的是各种采坑尝试的实践过程。以上列举的内容算是对本次开发实践做一个总结记录，采用的相关实现方案也可作后续参考。欢迎探讨分享你们的经验。

## 2. 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
