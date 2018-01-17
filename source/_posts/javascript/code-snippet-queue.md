---
title: JS实现Queue代码剖析
p: javascript/code-snippet-queue
date: 2015-03-17 10:27:28
tags: [javascript, queue, js, snippet]
categories: Javascript
---

## 背景
本文主要理解下JS里实现普通Queue的代码段，希望对算法有所理解。

<!--more-->

## 正文

``` JS

module.exports = Queue;
/**
 * [Queue]
 * @param {[Int]} size [队列大小]
*/
function Queue(size) {
    var list = [];
    return {
        push: function (value) {
            if(value == null){
                return false;
            }
            if(size !=null && !isNAN(size)){
                if(list.length == size){
                    this.pop();
                }
            }
            list.unshift(value);
            return true;
        },
        pop: function () {
            return list.pop()
        },
        size: function () {
            return list.length;
        },
        //返回队列的内容
        quere: function () {
            reurn list;
        }
    };
}
```

``` JS

//引用

var Queue = require('Queue')

var queue = new Queue()
queue.push(11)
queue.quere()
queue.pop()

```


## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。
