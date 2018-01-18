---
title: 代码实现无障碍的对话框
p: javascript/code-snippet-dialog
date: 2013-03-17 15:50:54
tags: [javascript,dialog]
categories: Javascript
---

## 前言

全方面考虑对话框对可访问性的影响。
多数情况下，它是可访问性的一个灾难。输入焦点未能正确处理以及屏幕阅读器无法感知内容变化。其实，使对话框可访问并非如此困难，你只需要理解几行代码的作用。

<!-- TOC -->

- [前言](#前言)
- [正文](#正文)
    - [首先考虑 ARIA](#首先考虑-aria)
    - [如何将焦点设置在对话框上？](#如何将焦点设置在对话框上)
    - [如何限制焦点(Trapping focus)？](#如何限制焦点trapping-focus)
    - [如何恢复焦点(Restoring focus)？](#如何恢复焦点restoring-focus)
    - [最后退出对话框](#最后退出对话框)
    - [DEMO示例](#demo示例)
- [小结](#小结)
- [关于作者](#关于作者)

<!-- /TOC -->

<!--more-->

## 正文

### 首先考虑 ARIA 

如果你想要屏幕阅读器用户感知到弹出了一个对话框，那么你需要学习一些ARIA role知识。ARIA role [1]为HTML元素提供了额外的语义，让浏览器与屏幕阅读器以更具描述性的方式进行沟通。ARIA定义了大量的角色，改变了屏幕阅读器感知页面中不同元素的方式。与对话框有关的角色有两个：dialog和alertdialog 。

在大多数情况下， 我们使用dialog。将一个元素的role属性设为此值，浏览器则会把该元素看作为一个对话框。

``` JS
<div id="my-dialog" role="dialog">
    <-- Your dialog code here -->
</div>
```

当role属性值为dialog的元素可见时，浏览器会告知屏幕阅读器一个对话框已打开。这可以让屏幕阅读器用户意识到，他们已经不在页面的常规流中了。

对话框应有描述标签（label）。你可以使用aria-label属性来指明描述文本或者使用aria-labelledby属性来指明包含描述文字的元素的ID。示例如下：

``` JS
<div id="my-dialog" role="dialog" aria-label="New Message">
    <-- Your dialog code here -->
</div>

<div id="my-dialog" role="dialog" aria-labelledby="dialog-title">
    <h3 id="dialog-title">New Message</h3>
    <-- Your dialog code here -->
</div>
```
在第一个例子中，aria-label属性用于指定一个仅用于屏幕阅读器的标签。当对话框的label无需可见时，你可以使用此方法。在第二个例子中，aria-labelledby属性用于指定包含对话框标签的元素的ID。由于对话框有一个可见的标签，关联复用比再重复一遍更妥。当对话框显示时，屏幕阅读器会报读对话框的标签。

alertdialog role是对话框的一种特殊类型，目的是为了吸引用户的注意力。你可以把它看作是当你尝试删除一些东西时弹出的确认对话框（confirmation dialog ）。alertdialog的交互相比而言较少。它的主要目的是让用户感知到一个操作已执行。与此相比，dialog 可能是供用户输入信息的区域，比如写一封电子邮件或即时消息。

当一个alertdialog显示时，屏幕阅读器会查找描述文字来报读。建议使用aria-describedby来指定需要朗读的文本。这个属性与aria-labelledby 类似，其值是包含欲朗读内容的元素的ID。如果未指定aria-describedby，那么屏幕阅读器将试图找出能起描述作用的文本，往往会选择元素内第一段文本内容。例如：

``` JS
<div id="my-dialog" role="alertdialog" aria-describedby="dialog-desc">
    <p id="dialog-desc">Are you sure you want to delete this message?</p>
    <-- Your dialog code here -->
</div>
```
此示例使用一个元素包含了描述文本。这样做可以确保在对话框显示时，会报读正确的文本。

即使你不设置这些额外的属性，仅对对话框设置适当的role，应用的可访问性也会得到极大地提高。


### 如何将焦点设置在对话框上？
创建无障碍对话框的下一步就是管理焦点。当一个对话框出现时，焦点应在对话框内，这样用户才可以使用键盘继续浏览。焦点设置在对话框内的确切位置，在很大程度上取决于对话框本身的目的。如果确认对话框（confirmation dialog ）内有一个“继续”按钮和一个“取消”按钮，那么你可以将焦点默认设置在“取消”按钮上。如果对话框是用来让用户输入文字的，那么你可以将焦点默认设置在文本输入框内。如果你实在不知道将焦点设在何处，将焦点设置在能代表对话框的元素上是个不错的选择。

由于多数情况下，我们使用<div>元素来表示一个对话框，那么可以将焦点默认设置在该<div>上。你需要将该元素的tabIndex属性设置为-1，这样这个元素才能获得焦点。这个属性值允许你使用JavaScript将焦点设置到该元素，但不会将该元素插入到正常的Tab键顺序中。也就是说用户将无法按TAB键将焦点设置在对话框上。直接在HTML中设置或通过JavaScript设置都可以。在HTML中设置：

``` JS
<div id="my-dialog" role="dialog" tabindex="-1" aria-labelledby="dialog-title">
    <h3 id="dialog-title">New Message</h3>
    <-- Your dialog code here -->
</div>
```
通过JavaScript设置：

``` JS
var div = document.getElementById("my-dialog");
div.tabIndex = -1;
div.focus();
```

一旦将tabIndex设置为-1，元素就可以调用focus()，就像任何其他的可聚焦元素一样。这样用户就可以按Tab键在对话框中导航了。

### 如何限制焦点(Trapping focus)？
对话框的另一个可访问性问题是要确保焦点不能跳出对话框。一般来说，如果对话框是模态的，其焦点应无法逃脱对话框。当对话框打开时，如果按tab键将焦点设置到对话框背后的页面元素中，那么对于键盘用户来说将焦点重新返回到对话框内是相当困难的。因此，我们最好使用一些JavaScript以避免这种情况发生。

基本思路是使用事件捕获（event capturing）侦听focus事件，这种方法由Peter-Paul Koch[2]推广，如今已在JavaScript库中广泛使用。由于focus不冒泡（bubble），你无法在事件流的冒泡阶段捕捉到它。相反，你可以通过使用事件捕获方法捕获页面上的所有focus事件。之后，你只需确定获得焦点的元素是否在对话框中。如果没有，则将焦点设置在对话框上。代码是非常简单的：

``` JS
document.addEventListener("focus", function(event) {
    var dialog = document.getElementById("my-dialog");
    if (dialogOpen && !dialog.contains(event.target)) {
        event.stopPropagation();
        dialog.focus();
    }
}, true);
```
代码监听document的focus事件，用以在目标元素接收到它们之前截获所有这类事件。假设对话框打开时，变量dialogOpen的值为true。当focus事件发生时，这个函数截获事件，并检查对话框是否是打开的，如果是的话，再检查接收焦点的元素是否在对话框内。如果两个条件都满足，则重新将焦点设置在对话框上。这样焦点就会在对话框的尾部和起始处循环。这就不会tab出对话框，键盘用户就很难再迷失方向。

如果你使用JavaScript库的话，focus事件委托的方法也可以实现同样的效果。如果不使用JavaScript库，同时需要支持Internet Explorer 8及更早的版本，可以使用focusin事件代替(译者注：focusin和focusout支持事件冒泡)。

### 如何恢复焦点(Restoring focus)？
对话框的最后一个焦点难题：当对话框关闭时，将焦点返回至页面的主体部分。思路很简单：为了打开对话框，用户可能激活了一个链接或一个按钮。此时焦点转移到对话框中，用户完成一些任务后，然后退出对话框。焦点应该重新设回至为了打开对话框而点击的链接或按钮上，以便可以继续浏览网页。在Web应用程序中经常忽视这个问题，但效果是天壤之别。

与其他部分一样，少量代码即可实现效果。所有浏览器都支持document.activeElement ，返回当前具有焦点的元素。你只需获得这个值，然后显示对话框，关闭对话框时，将焦点返回到该元素。例如:

``` JS
var lastFocus = document.activeElement,
    dialog = document.getElementById("my-dialog");

dialog.className = "show";
dialog.focus();
```
这段代码的重点是它记录了最后的焦点元素。这样一来，对话框被关闭时，将焦点设置在它上面：

``` JS
lastFocus.focus()
```
总体而言，只会在你已有的对话框代码上增加几行代码即可实现。


### 最后退出对话框
最后一个问题是要为用户提供一个快速简便的方法来退出对话框。最好的办法是使用Esc键关闭对话框。这是对话框在桌面应用程序中的退出方式，所以用户非常熟悉这种方式。只需监听Esc键是否被按下，然后退出对话框，如：

``` JS
document.addEventListener("keydown", function(event) {
    if (dialogOpen && event.keyCode == 27) {
        // close the dialog
    }
}, true);
```
Esc键的keyCode值是27，所以你只需在keydown事件中查找它。一旦监听到，关闭对话框并将焦点设置回之前的焦点元素上。


### DEMO示例

[jsbin](http://jsbin.com/saloraj)

``` JS
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>

  <div class="content">
    Page Content
    <button id="openDialog">打开dialog</button>
  </div>
  
<div id="my-dialog" role="dialog" tabindex="-1" aria-labelledby="dialog-title">
    <h3 id="dialog-title">New Message</h3>
    <!-- Your dialog code here -->
    <p id="dialog-desc">Are you sure you want to delete this message?</p>
  <button id="closeDialog">关闭dialog</button>
</div>
</body>
</html>
```

CSS部分

``` css
* {
  margin:0;
  padding:0;
  font-size:12px;
}
button {
  width: 100px;
  height:30px;
}

#my-dialog {
  width: 400px;
  height: 300px;
  border: solid 1px red;
  position:absolute;
  display: none;
  top:50%;
  left:50%;
  margin-top: -150px;
  margin-left: -200px;
}
#my-dialog.show {
  display: block;
}
#dialog-title {
  font-size:14px;
  background: #d4d3de;
  height: 40px;
  line-height: 40px;
}
```

JS部分
``` JS
var lastFocus = document.activeElement,
    dialogOpen = false,
    dialog = document.getElementById("my-dialog"),
    btnOpenDG = document.getElementById("openDialog"),
    btnCloseDG = document.getElementById("closeDialog");
//div.tabIndex = -1;
//div.focus();


btnOpenDG.addEventListener("click", function(){
  showDialog()
});

btnCloseDG.addEventListener("click", function(){
  closeDialog()
})

document.onload += function(){
  
  document.addEventListener("focus", function(event) {
    var dialog = document.getElementById("my-dialog");
    if (dialogOpen && !dialog.contains(event.target)) {
        event.stopPropagation();
        dialog.focus();
    }
}, true);
  
  
  document.addEventListener("keydown", function(event) {
    if (dialogOpen && event.keyCode == 27) {
        // close the dialog
        resetDialog()
    }
}, true);
  
}

function showDialog(){
  lastFocus = document.activeElement;
  dialog.className = "show";
  dialog.focus();
  dialogOpen = true;
}

function resetDialog(){
  lastFocus.focus();
  dialogOpen = false;
  dialog.className = '';
}

function closeDialog(){
  resetDialog()
}

```
## 小结

[WAI-ARIA （W3C）](http://www.w3.org/WAI/intro/aria.php)
[focus和blur事件委托](http://www.quirksmode.org/blog/archives/2008/04/delegating_the.html) by Peter-Paul Koch（Quirksmode）

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 
