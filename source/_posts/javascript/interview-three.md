---
title: 前端面试题整理之资深理论篇
p: javascript/interview-one
date: 2017-03-08 09:26:33
tags: [Javascript, 面试, 题目]
categories: Javascript
---

## 目录

本文重点讲解前端面试过程中遇到的偏向资深层面的问题，由于资深定义广泛，本人知识面也比较匮乏，所以有写的不当之处，还请各位过客体谅！

1. [前端面试题整理之基础问答篇](/2016/01/08/javascript/interview-one/)
2. [前端面试题整理之高级上机篇](/2017/01/08/javascript/interview-two/)
3. [前端面试题整理之资深理论篇](/2018/01/08/javascript/interview-three/)
4. [前端面试题整理之理想篇](/2018/01/08/javascript/interview-four/)

<!--more-->

## 问题

#### 如何解决跨域问题
1. JSONP
原理是：动态插入script标签，通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。
由于同源策略的限制，XmlHttpRequest只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，可以通过script标签实现跨域请求，然后在服务端输出JSON数据并执行回调函数，从而解决了跨域的数据请求。
优点是兼容性好，简单易用，支持浏览器与服务器双向通信。缺点是只支持GET请求。
JSONP：json+padding（内填充），顾名思义，就是把JSON填充到一个盒子里
``` JS
<script>
    function createJs(sUrl){

        var oScript = document.createElement('script');
        oScript.type = 'text/javascript';
        oScript.src = sUrl;
        document.getElementsByTagName('head')[0].appendChild(oScript);
    }

    createJs('jsonp.js');

    box({
       'name': 'test'
    });

    function box(json){
        alert(json.name);
    }
</script>
```

2. CORS
服务器端对于CORS的支持，主要就是通过设置 *** Access-Control-Allow-Origin ***来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。
通过修改document.domain来跨子域
将子域和主域的document.domain设为同一个主域.
前提条件：这两个域名必须属于同一个基础域名! 而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域
主域相同的使用document.domain。

3. 使用window.name来进行跨域
window对象有个name属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的。

4. 使用HTML5中新引进的window.postMessage方法来跨域传送数据
还有flash、在服务器上设置代理页面等跨域方式。个人认为window.name的方法既不复杂，也能兼容到几乎所有浏览器，这真是极好的一种跨域方法。


#### 谈谈你对webpack的看法
WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、JavaScript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源。

###### webpack的两大特色：
1. code splitting（可以自动完成）
2. loader 可以处理各种类型的静态文件，并且支持串联操作
webpack 是以commonJS的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。

###### webpack特效
webpack具有requireJs和browserify的功能，但仍有很多自己的新特性：
+ 对 CommonJS 、 AMD 、ES6的语法做了兼容
+ 对js、css、图片等资源文件都支持打包
+ 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
+ 有独立的配置文件webpack.config.js
+ 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
+ 支持 SourceUrls 和 SourceMaps，易于调试
+ 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活
+ webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快


###### 说说TCP传输的三次握手四次挥手策略
*** 三次握手 ***
为了准确无误地把数据送达目标处，TCP协议采用了三次握手策略。用TCP协议把数据包送出去后，TCP不会对传送后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了TCP的标志：SYN和ACK。

+ 第一次握手：发送端首先发送一个带SYN标志的数据包给对方。
+ 第二次握手：接收端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息。
+ 第三次握手：最后，发送端再回传一个带ACK标志的数据包，代表“握手”结束。 

若在握手过程中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。

*** 断开一个TCP连接则需要“四次挥手”： ***
+ 第一次挥手：主动关闭方发送一个FIN，用来关闭主动方到被动关闭方的数据传送，也就是主动关闭方告诉被动关闭方：我已经不会再给你发数据了(当然，在fin包之前发送出去的数据，如果没有收到对应的ack确认报文，主动关闭方依然会重发这些数据)，但是，此时主动关闭方还可以接受数据。

+ 第二次挥手：被动关闭方收到FIN包后，发送一个ACK给对方，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号）。

+ 第三次挥手：被动关闭方发送一个FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。

+ 第四次挥手：主动关闭方收到FIN后，发送一个ACK给被动关闭方，确认序号为收到序号+1，至此，完成四次挥手。


#### TCP和UDP的区别
+ TCP（Transmission Control Protocol，传输控制协议）是基于连接的协议，也就是说，在正式收发数据前，必须和对方建立可靠的连接。一个TCP连接必须要经过三次“对话”才能建立起来
+ UDP（User Data Protocol，用户数据报协议）是与TCP相对应的协议。它是面向非连接的协议，它不与对方建立连接，而是直接就把数据包发送过去！
UDP适用于一次只传送少量数据、对可靠性要求不高的应用环境。


#### Web Worker 和 WebSocket

##### worker主线程：   [查看详细讲解](/2014/05/01/web/how-to-use-webworker/)
Web Worker可以在后台执行脚本，而不会阻塞页面交互。Worker对象分为两种：专用式Web Worker和共享式Web Worker：专用式的Web Worker只能被当个页面使用，而共享式的Web Worker可以在被多个页面使用。

1. 通过 worker = new Worker( url ) 加载一个JS文件来创建一个worker，同时返回一个worker实例。
2. 通过worker.postMessage( data ) 方法来向worker发送数据。
3. 绑定worker.onmessage方法来接收worker发送过来的数据。
4. 可以使用 worker.terminate() 来终止一个worker的执行。

##### WebSocket
WebSocket是Web应用程序的传输协议，它提供了双向的，按序到达的数据流。他是一个Html5协议，WebSocket的连接是持久的，他通过在客户端和服务器之间保持双工连接，服务器的更新可以被及时推送给客户端，而不需要客户端以一定时间间隔去轮询。


#### Javascript垃圾回收方法
*** 标记清除（mark and sweep） ***
这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”。

垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了。

*** 引用计数(reference counting) ***
在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。
引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。

在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，也就是说只要涉及BOM及DOM就会出现循环引用问题。



## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
