---
title: 关于Web Worker的知识点
p: web/how-to-use-webworker
date: 2014-05-01 17:04:10
tags: [web, worker]
categories: [WEB]
---

## 背景
web worker 是运行在后台的 JavaScript，不会影响页面的性能。

*** 什么是 Web Worker？ *** 
当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。
web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

通过使用Web Worker， 我们可以在浏览器后台运行Javascript， 而不占用浏览器自身线程。
Web Worker可以提高应用的总体性能，并且提升用户体验。如果你想在自己的Web应用中使用Web Worker， 不妨来了解一下有关Web Worker的基础知识点。

<!--more-->

### 1. Web Worker 可以让你在后台运行Javascript
一般来说Javascript和页面的UI会共用一个线程，所以当点击一个按钮开始运行Javascript后，在这段代码运行完毕之前，页面是无法响应用户操作的，换句话来说就是被“冻结”了。而这段代码可以交给Web Worker在后台运行，那么页面在Javascript运行期间依然可以响应用户操作。后台会启动一个worker线程来执行这段代码，用户可以创建多个worker线程。所以你可以在前台做一些小规模分布式计算之类的工作，不过Web Worker有以下一些使用限制：

+ Web Worker无法访问DOM节点；
+ Web Worker无法访问全局变量或是全局函数；
+ Web Worker无法调用alert()或者confirm之类的函数；
+ Web Worker无法访问window、document之类的浏览器全局变量；

不过Web Worker中的Javascript依然可以使用setTimeout(),setInterval()之类的函数，也可以使用XMLHttpRequest对象来做Ajax通信。

### 2. 有两种Web Worker
Web workers可分为两种类型：专用线程dedicated web worker，以及共享线程shared web worker。 
Dedicated web worker随当前页面的关闭而结束；这意味着Dedicated web worker只能被创建它的页面访问。与之相对应的Shared web worker可以被多个页面访问。
在Javascript代码中，“Work”类型代表Dedicated web worker，而“SharedWorker”类型代表Shared web worker。

在绝大多数情况下，使用Dedicated web worker就足够了，因为一般来说在web worker中运行的代码是专为当前页面服务的。而在一些特定情况下，web worker可能运行的是更为普遍性的代码，可以为多个页面服务。在这种情况下，我们会创建一个共享线程的Shared web worker，它可以被与之相关联的多个页面访问，只有当所有关联的的页面都关闭的时候，该Shared web worker才会结束。相对Dedicated web worker，shared web worker稍微复杂些。

### 3. “Worker”对象代表Dedicated Web Worker
现在来看如何使用Dedicated web worker。下面的例子中用到了jQuery以及Modernizr作为Javascript库，然后往HTML页面中加入以下代码：
``` JS
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script type="text/javascript" src="script/modernizr.js"></script>
    <script type="text/javascript" src="script/jquery-2.0.0.js"></script>
    <script type="text/javascript">
    $(document).ready(function() {
        if (!Modernizr.webworker) {
            alert("This browser doesn't support Web Worker!");
            return;
        }
        $("#btnStart").click(function() {
            var worker = new Worker("script/lengthytask.js");
            worker.addEventListener("message", function(evt) {
                alert(evt.data);
            }, false);
            worker.postMessage(10000)
        });
    });
    </script>
</head>
<body>
    <form>
        <input type="button" id="btnStart" value="Start Processing" />
    </form>
</body>
</html>
```
这个HTML页面中有个按钮，点击后会运行一个Javascript文件。上面的代码中首先检测当前浏览器是否支持Web Worker，不支持的话，就跳出提醒信息。
按钮的点击事件中创建了Worker对象，并给它指定了Javascript脚本文件——lengthytask.js(稍后会有代码)，并且给Worker对象绑定了一个“message”事件。该事件会在后台代码（lengthytask.js）向页面返回数据时触发。“message”事件可以通过event.data来获取后台代码传回的数据。最后，postMessage方法正式执行lengthytask.js，该方法还可以向后台代码传递参数， 后台代码同样通过message事件获取该参数。

下面是lengthytask.js主要包含的代码：

``` JS
addEventListener("message", function(evt) {
    var date = new Date();
    var currentDate = null;
    do {
        currentDate = new Date();
    } while (currentDate - date < evt.data);
    postMessage(currentDate);
}, false);
```
以上代码在后台监听message时间，并获取页面传来的参数：10000；这里实际上是一个计时函数：在message事件被触发10000毫秒之后，把结果（currentDate）传给页面。
所以当点击“Start Processing”按钮，页面会在10秒钟后把当时的时刻alert出来。在这10秒钟内页面依然可以响应鼠标键盘事件。

### 4. “SharedWorker”对象代表Shared Web Worker
前面的代码使用的是dedicated web worker。 这一节会用shared web worker代替dedicated web worker，来区别两者的不同。下面是同一个例子的shared web worker版本：
``` JS
addEventListener("message", function(evt) {
    var date = new Date();
    var currentDate = null;
    do {
        currentDate = new Date();
    } while (currentDate - date < evt.data);
    postMessage(currentDate);
}, false);
```
请注意加黑的代码，这里创建了一个SharedWorker对象，并把message事件绑定在shared worker的port对象上；同样由port对象发起postMessage， 开始执行后台代码sharedlengthytask.js。
下面是sharedlengthytask.js的主要代码：
``` JS
var port;
addEventListener("connect", function(evt) {
    port = evt.ports[0];
    port.addEventListener("message", function(evt) {
        var date = new Date();
        var currentDate = null;
        do {
            currentDate = new Date();
        } while (currentDate - date < evt.data);
        port.postMessage(currentDate);
    }, false);
    port.start();
}, false);
```
使用SharedWorker对象的后台代码需要绑定connect和message事件， connect事件会在页面上的port被start时触发。之后的message事件的回调函数与之前的基本相同，最后port调用postMessage方法把结果传回给页面。

### 5. Web Worker使用XMLHttpRequest与服务端通信
有些情况下，web worker还需要与服务器进行交互。比如页面可能需要处理来自数据库中的信息，我们就需要使用Ajax技术与服务器交互，下面代码包含了web worker如何从服务端获取数据：
``` JS
addEventListener("message", function(evt) {
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "lengthytaskhandler.ashx");
    xhr.onload = function() {
        postMessage(xhr.responseText);
    };
    xhr.send();
}, false);
```
上面的代码向服务端的asp.net服务lengthytaskhandler.ashx发出GET请求。并注册了获取数据后的onload事件。下面的代码是服务端的lengthytaskhandler.ashx:
``` C#
namespace WebWorkerDemo {
    public class LengthyTaskHandler: IHttpHandler {
        public void ProcessRequest(HttpContext context) {
            System.Threading.Thread.Sleep(10000);
            context.Response.ContentType = "text/plain";
            content.Response.Write("Processing successful!");
        }
        public bool IsReusable {
            get {
                return false;
            }
        }
    }
}
```
如你所见，ProcessRequest模拟了一个长时间运行的任务，并返回了“Processing successful！”的消息。

### 6. 通过Error事件捕捉错误信息
当我们把越来越复杂的逻辑加到Web Worker里时，错误处理机制是必不可少的。而Web Worker恰恰提供了error事件，供开发者捕捉错误信息。下面的代码展示了如何绑定error事件：
``` JS
$("#btnStart").click(function() {
    var worker = new Worker("scripts/lengthytask.js");
    worker.addEventListener("error", function(evt) {
        alert("Line #" + evt.lineno + " - " + evt.message + " in " + evt.filename);
    }, false);
    worker.postMessage(10000);
});
```
如上可见， Worker对象可以绑定error事件；而且evt对象中包含错误所在的代码文件（evt.filename）、错误所在的代码行数（evt.lineno）、以及错误信息（evt.message）。

### 7. 通过terminate()方法终止Web Worker
有些情况下，我们可能需要强制终止执行中的Web Worker。Worker对象提供了terminate()来终止自身执行任务，被终止的Worker对象不能被重启或重用，我们只能新建另一个Worker实例来执行新的任务。

## 兼容性

### 浏览器支持
所有主流浏览器均支持 web worker，除了 Internet Explorer。

### 如何检测 Web Worker 支持
在创建 web worker 之前，请检测用户的浏览器是否支持它：
``` JS
if (typeof(Worker) !== "undefined") {
    // Yes! Web worker support!
    // Some code.....
} else {
    // Sorry! No Web Worker support..
}
```

## 完整的 Web Worker 实例代码

``` JS
//demo_workers.js
var i=0;
function timedCount()
{
    i=i+1;
    postMessage(i);
    setTimeout("timedCount()",500);
}
timedCount();
```
我们已经看到了 .js 文件中的 Worker 代码。下面是 HTML 页面的代码：
``` html
<!DOCTYPE html>
<html>
<body>
    <p>
    	Count numbers: <output id="result"></output>
    </p>
    <button onclick="startWorker()">Start Worker</button>
    <button onclick="stopWorker()">Stop Worker</button>
    <br />
    <br />
    <script>
    var w;

    function startWorker() {
        if (typeof(Worker) !== "undefined") {
            if (typeof(w) == "undefined") {
                w = new Worker("demo_workers.js");
            }
            w.onmessage = function(event) {
                document.getElementById("result").innerHTML = event.data;
            };
        } else {
            document.getElementById("result").innerHTML = "Sorry, your browser does not support Web Workers...";
        }
    }

    function stopWorker() {
        w.terminate();
    }
    </script>
</body>
</html>
```

## 总结

Web Worker可以在后台执行脚本，而不会阻塞页面交互。Worker对象分为两种：专用式Web Worker和共享式Web Worker：专用式的Web Worker只能被当个页面使用，而共享式的Web Worker可以在被多个页面使用。另外，本文还介绍了Web Worker的错误处理机制，以及使用Ajax与服务端交互。

## 文献

1. [点击查看转转原文](http://www.developer.com/lang/jscript/7-things-you-need-to-know-about-web-workers.html)

2. [参考W3C](http://www.w3school.com.cn/html5/html_5_webworkers.asp)

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
