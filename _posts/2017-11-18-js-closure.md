---
layout: post
title: 浅谈JS闭包
date: 2017-11-18
tags: [JavaScript]
---
* auto-gen TOC:
{:toc}

### 变量作用域
变量的作用域无非就两种：全局变量和局部变量。

我们都知道函数内部可以直接读取全局变量，但是在函数外部却无法读取函数内部的定义的局部变量。

那么如何从外部能够读取到函数内部的局部变量呢？

由于JavaScript语言的特性，正常情况下，这是办不到的，只有通过变通的方式来实现。

要解决这个问题，先要了解一个概念"链式作用域(Chain Scope)"

### 链式作用域(Chain Scope)

请看如下代码：

```
function f1() {
  var username = "Jack";

  function f2() {
    alert(username); // "Jack"
  }
}
```

在上面的代码中，函数f2被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。
但是反过来就不行，f2内部的局部变量，对f1就是不可见的。

这就是Javascript语言特有的"链式作用域"结构（chain scope），

子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，
反之则不成立。


既然f2可以读取f1中的局部变量，那么只要把f2作为返回值暴漏出去，我们不就可以在f1外部读取到
它的内部变量了吗！

代码如下：
```
function f1() {
  var username = "Jack";

  return function f2() {
    alert(username);
  }
}
var result=f1();
result(); // "Jack"
```


### 变量解析的过程

上面提到了链式作用域的概念，那么在函数执行的过程中，是如何在作用域链中查找变量的呢？

来看一个例子：
```
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}

var result = compare(5, 10);
```
以上代码先定义了compare函数，然后又在全局作用域中调用了它。当调用compare()时，会创建一个
作用域链对象。这个对象是一个有顺序的集合对象。

第一个对象：包含arguments, value1, value2的活动对象

第二个对象：包含result和compare的全局执行环境的变量对象

而闭包的实现实际上是因为变量解析会在作用域链中依次按序寻找对应属性所导致的。

下图展示了包含上述关系的compare()函数执行时的作用域链。

![](/images/posts/closure/1-1.png)


### 闭包的概念

各种专业文献上的"闭包"（closure）定义非常抽象，很难看懂。我的理解是，闭包就是能够读取其他函数内部变量的函数。

由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。

所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

### 闭包的使用场景
**1. 需要访问函数内部的变量**

**2. 通过循环给页面上多个dom节点绑定事件**

假如页面上有5个button,要给button绑定onclick事件，点击的时候，弹出对应button的索引编号。
请看如下代码：
```
for(var i = 0, len = btns.length; i < len; i++) {
    (function(num) {
        btns[num].onclick = function() {
            alert(num);
        }
    }(i))
}
```
Tip： **在js中，没有块级作用域 ，只有函数作用域。**可以采用“立即执行函数
Immediately-Invoked Function Expression (IIFE)”的方式创建作用域。

**3. 延续局部变量的寿命**

怎么来理解这句话呢？请看下面的代码：
```
function f1() {
	var n = 999;

	nAdd = function() {
		n += 1;
	}

	return function f2() {
		console.log(n);
	}
}
var result = f1();
result(); // 999
nAdd();
result(); // 1000
```
在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。
这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，
而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，
被垃圾回收机制（garbage collection）回收。

这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，
首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。
其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，
所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

**4. 用闭包模拟私有方法**

编程语言中，比如 Java，是支持将方法声明为私有的，即它们只能被同一个类中的其它方法所调用。

而 JavaScript 没有这种原生支持，但我们可以使用闭包来模拟私有方法。私有方法不仅仅有利于限制对
代码的访问：还提供了管理全局命名空间的强大能力，避免非核心的方法弄乱了代码的公共接口部分。

下面的示例展现了如何使用闭包来定义公共函数，并令其可以访问私有函数和变量。
这个方式也称为 模块模式（module pattern）：

请看如下代码：
```
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }
};

var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */
```
在上面的代码中，我们只创建了一个词法环境，
为三个函数所共享：Counter.increment，Counter.decrement 和 Counter.value

该共享环境创建于一个匿名函数体内。
这个环境中包含两个私有项：名为 privateCounter 的变量和名为 changeBy 的函数。
这两项都无法在这个匿名函数外部直接访问。必须通过匿名函数返回的三个公共函数访问。

这三个公共函数是共享同一个环境的闭包。
多亏 JavaScript 的词法作用域，它们都可以访问 privateCounter 变量和 changeBy 函数。


上面代码中将定义的匿名函数储存在一个全局变量makeCounter中，
这样可以通过makeCounter来创建多个计数器。

请注意两个计数器 counter1 和 counter2 是如何维护它们各自的独立性的。
每个闭包都是引用自己词法作用域内的变量 privateCounter 。

每次调用其中一个计数器时，通过改变这个变量的值，会改变这个闭包的词法环境。
然而在一个闭包内对变量的修改，不会影响到另外一个闭包中的变量。

**以这种方式使用闭包，提供了许多与面向对象编程相关的好处 —— 特别是数据隐藏和封装。**

### 使用闭包的注意点

1. 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，
否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，
将不使用的局部变量全部删除。

2. 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，
把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），
这时一定要小心，不要随便改变父函数内部变量的值。

### 关于this对象

在闭包中使用this对象也可能会导致一些问题。我们知道，this对象是在运行时基于函数的执行环境
绑定的：在全局函数中，this等于window，而当函数作为某个对象的方法调用时，this等于那个对象。
不过匿名函数的执行环境具有全局性，因此其this对象通常指向window。但有时候由于编写闭包的方式
不同，这一点可能不会那么明显。下面来看一个例子：
```
var name = "The Window";

var object = {
　　name : "My Object",

　　getNameFunc : function(){
　　　　return function(){
　　　　　　return this.name;
　　　　};
　　}
};

alert(object.getNameFunc()());
```
运行结果是："The Window"

再看下面这段代码：
```
var name = "The Window";

var object = {
　　name : "My Object",

　　getNameFunc : function(){
      var that = this;
　　　　return function(){
　　　　　　return that.name;
　　　　};
　　}
};

alert(object.getNameFunc()());
```
运行结果是："My Object"

本文参考
[学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)