---
title: WEB性能优化问题
p: web/performance-one
date: 2014-01-09 08:21:49
tags: [web,performance,性能,优化]
categories: WEB
---

## 背景

谈谈前端性能优化问题 

<!--more-->

+ 代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。
+ 缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等
+ 请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载。
+ 请求带宽：压缩文件，开启GZIP，

#### 代码层面的优化
    用hash-table来优化查找
    少用全局变量
    用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能
    用setTimeout来避免页面失去响应
    缓存DOM节点查找的结果
    避免使用CSS Expression
    避免全局查询
    避免使用with(with会创建自己的作用域，会增加作用域链长度)
    多个变量声明合并
    避免图片和iFrame等的空Src。空Src会重新加载当前页面，影响速度和效率
    尽量避免在HTML标签中写Style属性

#### 移动端性能优化
    尽量使用css3动画，开启硬件加速。
    适当使用touch事件代替click事件。
    避免使用css3渐变阴影效果。
    可以用transform: translateZ(0)来开启硬件加速。
    不滥用Float。Float在渲染时计算量比较大，尽量减少使用
    不滥用Web字体。Web字体需要下载，解析，重绘当前页面，尽量减少使用。
    合理使用requestAnimationFrame动画代替setTimeout
    CSS中的属性（CSS3 transitions、CSS3 3D
transforms、Opacity、Canvas、WebGL、Video）会触发GPU渲染，请合理使用。过渡使用会引发手机过耗电增加
    PC端的在移动端同样适用




## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
