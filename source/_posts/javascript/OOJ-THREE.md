---
title: OOJ-面向对象编程的三大特点-封装，继承，多态分析与实例
p: javascript/OOJ-THREE
date: 2010-06-01 10:19:05
tags:
    -   JS
    -   Javascript
    -   OOP
    -   OOJ
    -   H5
    -   WEB
categories: Javascript
---

今天谈谈面向对象编程的三个主要的关键词：封装、继承、多态。

javascript面向对象编程，既然是面向对象编程不得不提到面向对象的几大关键字：封装，继承，多态。

<!--more-->

### 首先：封装

在前几篇的文章中我们已经了解到JAVASCRIPT需要通过模拟实现一些类似C++,C#语言面向对象的特征。

今天我们重新温习加强理解下实现的要点与注意事项：

javascript中创建对象的模式中，一般认为通过闭包才算的上是真正意义上的封装，所以首先我们先来简单介绍一下闭包，看下面这个例子：

``` JS
<script type="text/javascript">
// <![CDATA[
  function myInfo(){
    var name ="Hello",age =27;
    var myInfo = "my name is" + name + "i am" + age +"years old";
    function showInfo(){
      alert(myInfo);
    }
    return showInfo;
  }
  var oldFish = myInfo();
  oldFish();
// ]]>
</script>
```

上面的代码段其实就是一个简单的闭包应用。 
简单解释一下：上面的函数myInfo中定义的变量，在它的内嵌函数showInfo中是可访问的（这个很好理解），但是当我们把这个内嵌函数的返回引用赋值给一个变量oldFish，这个时候函数showInfo是在myInfo函数体外被调用，但是同样可以访问到定义在函数体内的变量。 
总结一下闭包的原理吧：函数是运行在定义他们的作用域中而不是调用他们的作用域中。其实返回一个内嵌函数也是创建闭包最常用的一种方法！ 

如果觉得上面的解释太抽象的话，那么我们一起重塑上面的函数，看看这样是否层次鲜明一些：

``` JS

  var ioldFish = function(name,age){
    var name = name,age = age;
    var myInfo = "my name is" + name + "i am" + age +"">years old";
    return {
      showInfo: function(){
        alert(myInfo);  
      } 
    }
  }
  ioldFish("Hello",27).showInfo();
```
上例中的编码风格是ext yui中比较常见的，公私分明，一目了然。通过闭包，我们可以很方便的把一些不希望被外部直接访问到的东西隐藏起来，你要访问函数内定义的变量，只能通过特定的方法才可以访问的到，直接从外部访问是访问不到的，写的挺累，饶了一圈终于转回来了，封装嘛，不就是把不希望被别人看到的东西隐藏起来嘛！ 
上例如果转换成JQ的风格的话，应该如下例所写， 这样的封装模式属于门户大开型模式，里面定义的变量是可以被外部访问到的（下面的例子如果你先实例化一个对象，然后在函数外部访问对象的name或者age属性都是可以读取到的）当然这种模式下我们可以设置一些”潜规则”，让团队开发成员明白哪些变量是私用的，通常我们人为的在私有变量和方法前加下划线”_”，标识警戒讯号！从而实现”封装”！ 
``` JS
<script type="text/javascript">
// <![CDATA[
    var ioldFish = function(name,age){
      return ioldFish.func.init(name,age);  
    };
    ioldFish.func = ioldFish.prototype ={
        init:function(name,age){
            this.name = name;
            this.age = age;
            return this;
        },
        showInfo:function(){
            var info = "my name is" + this.name +"i am " +this.age+"years old";
            alert(info);
        }
    };
    ioldFish.func.init.prototype = ioldFish.func;
    ioldFish("Hello",27).showInfo();
    //var oldFish = new ioldFish("Hello",27);
    //alert(oldFish.name);
// ]]>
</script>
```
两种模式各有特点，大家看情况随机使用。

## 其次：继承

与闭包封装有点冲突，闭包封装不利于子类派生。

在javascript中继承一般分为三种方式：”类式继承”，”原型继承”,”掺元类”。下面简单的介绍一下三类继承方式的原理。 

*A.类式继承*：这个是现在主流框架中常用的继承方式，看下例：
``` JS
<script type="text/javascript">
// <![CDATA[
  var Name = function(name){
    this.name = name;
  };
  Name.prototype.getName = function(){
    alert(this.name);
  };
  var Fish = function(name,age){
    Name.call(this,name);
    this.age = age;
  };
  Fish.prototype = new Name();
  Fish.prototype.constructor = Fish;
  Fish.prototype.showInfo = function(){
   alert(this.age);
  }
  var ioldFish = new Fish("Hello",27);
  ioldFish.getName();
// ]]>
</script>
```

上述子类Fish中并没定义getName方法，但是子类Fish的实例对象ioldFish依然调用到了该方法，这是因为子类Fish继承了父类Name中定义的getName方法。解释一下，这里子类Fish的prototype指到了父类的一个实例，在子类Fish中虽然没有申明getName方法，但是根据原型链原理，会向prototype指向的上一级对象中去查找是否有该方法，如果没找到该方法，会一直搜索到最初的原型对象。这其实也就是继承的原理了。这里特别说明一下，Fish.prototype.constructor = Fish;这句，由于默认子类的prototype应该是指向本身的，但是之前把prototype指向到了超类的实例对象，所以在这里要把它设置回来。当然这里可以把相关代码通过一个函数来组织起来，起到伪装extend的作用，这里不再阐述。 

*B.原型继承*，从内存性能上看优于类式继承。
``` JS
<script type="text/javascript">
// <![CDATA[
  function clone(object){
    var F = function(){};
    F.prototype = object;
    return new F();
  };
  var Name = {
    name:"who's name",
    showInfo:function(){
      alert(this.name);
    }
  };
  var Fish = clone(Name);
  //Fish.name = "Hello";
  Fish.showInfo();
// ]]>
</script>
```

很明显，原型继承核心就是这个clone函数，同样是原型链的原理，不同的是它直接克隆超类，这样的话子类就继承了超类的所有属性和方法.特别说一下，这类继承并不需要创建构造函数，只需要创建一个对象字变量，定义相应的属性和方法，然后在子类中只需要通过圆点”.”符号来引用属性和方法就可以了.

*C.掺元类*：把一些常用通用性比较大的方法统一封装在一个函数中，然后通过下面这个函数分派给要用到这些方法的类．还可以针对不同的类，选择性的传递需要的方法。 

``` JS
  function agument(receveClass,giveClass){
    if(arguments[2]){
      var len = arguments.length;
      for(i=2;i<len;i++){
        receveClass.prototype[arguments[i]] = giveClass.prototype[arguments[i]];
      }
    }
    else{
      for(method in giveClass.prototype){
        if(!receveClass.prototype[method]){
          receveClass.prototype[method] = giveClass.prototype[method];
        }
      }
    }
  };
  var Name = function(){};
  Name.prototype ={
    sayLike:function(){
      alert("i like oldfish");
    },
    sayLove:function(){
      alert("i love oldfish");
    }
  }
  var Fish = function(){};
  var ioldFish = new Fish();
  agument(Fish,Name,"sayLove");
  ioldFish.sayLove();
  ioldFish.sayLike();

```

多态：个人觉得这个比较抽象，很难言传，所以下面就从重载和覆盖两个方面来简单阐述一下。

重载：上面这个例子中agument函数初始带了两个参数，但是在后面的调用中，agument(Fish,Name,”sayLove”)同样可以带入任意多个参数，javascript的重载，是在函数中由用户自己通过操作arguments这个属性来实现的。

覆盖：这个很简单，就是子类中定义的方法如果与从超类中继承过来的的方法同名，就覆盖这个方法（这里并不是覆盖超类中的方法，注意一下），这里就不累赘了！

最后重点着墨说一下this和执行上下文，在前面举的封装例子中，this都是表示this所在的类的实例化对象本身，但是并不是千篇一律的，打个比方，通过HTML属性定义的事件处理代码，见如下代码：
``` JS

 var Name = function(name) {
    this.name = name;
    this.getName = function () {
        alert(this.name);
    }
 };
   var ioldFish = new Name("Hello"),
   btn = document.getElementById('btn');
   btn.onclick = ioldFish.getName;
   //btn.onclick = function(){ioldFish.getName.call(ioldFish)};
```
上例中点了按钮以后弹出框里并没有显示出实例对象的属性，这是因为this的执行上下文已经改变了，他现在所在的上下文应该是input这个HTML标签，但是该标签又不存在getName这个属性，所以自然无法输出这个属性的属性值了！从这个例子我们不难看出：执行上下文是在执行时才确定的，它随时可以变。

当然你可以去掉上面我注释掉的那段代码，通过call改变this的执行上下文，从而获取getName方法。apply方法同样可以实现改变执行上下文的功能，不过在prototype框架中发现了一个更为优美的实现方法bind。看一下这个方法的实现吧，不得不感叹先人的伟大……

``` JS
Function.prototype.bind = function(obj) {
    var method = this,
    temp = function() {
        return method.apply(obj, arguments);
    };
}
```
循序渐进,日有所学。学习语言基本靠实践。只要不断的写才能得到提高。


感谢老鱼的文章：本文引自：http://ued.alipay.com/2009/03/javascript-orient-object/  