---
layout: post
title: 前端面试题汇总之JavaScript(含答案)
date: 2018-04-28
tags: [面试]
---
* auto-gen TOC:
{:toc}

### 入门级
1. 介绍一下 JS 的基本数据类型
```
Undefined、Null、Boolean、Number、String
```

2. push()-pop()-shift()-unshift()分别是什么功能？
```
push 方法: 将新元素添加到一个数组末尾位置并返回数组的新长度值。
unshift 方法: 将指定的元素插入数组开始位置并返回数组的新长度值。
pop 方法: 移除数组中的最后一个元素并返回该元素。
shift 方法: 移除数组中的第一个元素并返回该元素。
```

3. JavaScript 有几种类型的值？能否画一下它们的内存图？
```
栈：原始数据类型（Undefined，Null，Boolean，Number，String）
堆：引用数据类型（对象、数组、函数）
```

4. eval 是做什么的？
```
它的功能是把对应的字符串解析成JS代码并运行；
应该避免使用eval，因为不安全，非常耗性能（2次，一次解析成js语句，一次执行）。
```

5. null 和 undefined 有何区别？
```
typeof undefined //"undefined"
typeof null //"object" 
undefined == null  // true
undefined !== null // true
```

6. JavaScript 代码中的 "use strict"; 是什么意思？使用它的区别是什么？
```
use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,使JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为。
默认支持的糟糕特性都会被禁用，比如不能用with，也不能在意外的情况下给全局变量赋值;
全局变量的显示声明,函数必须声明在顶层，不允许在非函数代码块内声明函数,arguments.callee也不允许使用；
消除代码运行的一些不安全之处，保证代码运行的安全,限制函数中的arguments修改，严格模式下的eval函数的行为和非严格模式的也不相同;
提高编译器效率，增加运行速度；
为未来新版本的Javascript标准化做铺垫。
```

7. document.write 和 innerHTML 有何区别？
```
document.write 只能重绘整个页面
innerHTML 可以重绘页面的一部分
```

8. 如何判断当前脚本运行在浏览器还是 node 环境中？（阿里）
```
通过判断 Global 对象是否为 window ，如果不为 window ，当前脚本没有运行在浏览器中
```

9. 判断一个对象是不是数组类型
```
instanceof 检查一个对象是否是制定构造函数的实例 可检查整个原型链
Array.isArray(obj)  不检查整个原型链
```

10. 数据类型判断
```
typeof: 可用来检测基本数据类型'string', 'number', 'boolean', 'undefined'
instanceof: Object instanceof constructure
toString: 可以用来检测Object、Number、Array、Date、Function、String、Boolean、Error和RegExp
```

11. 数据本地存储
```
localStorage
indexDB
cookie
seesionStorage
```

12. 常用的浏览器内核
```
Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等。
Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
EdgeHTML内核：Microsoft Edge。  [此内核其实是从MSHTML fork而来，删掉了几乎所有的IE私有特性]
```



### 中级难度
1. call和apply的作用是什么？区别是什么？
```
call和apply的功能基本相同，都是实现继承或者转换对象指针的作用；唯一不通的是前者参数是罗列出来的，后者是存到数组中的.
```

2. ["1","2","3"].map(parseInt) 的答案是多少？
```
[1,NaN,NaN]
因为 parseInt 需要两个参数(val,radix)，其中 radix 表示解析时用的基数。
map 传了3个(element,index,array)，对应的 radix 不合法导致解析失败。
相当于如下代码：
parseInt("1“, 0)
parseInt(“2”, 1)
parseInt(“3”, 2)
```

3. 事件是什么？IE与火狐的事件机制有何区别？如何阻止冒泡？
```
(1)我们在网页中的某个操作（有的操作对应多个事件）。
   例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
(2)事件处理机制：IE是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
(3)ev.stopPropagation();（旧ie的方法 ev.cancelBubble = true;）
```

4. new 操作符具体干了什么呢？
```
(1)创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
(2)属性和方法被加入到 this 引用的对象中。
(3)新创建的对象由 this 所引用，并且最后隐式的返回 this 。
var obj = {};
obj.__proto__ = Base.prototype;
Base.call(obj);
```

5. JavaScript 中，有一个函数，执行对象查找时，永远不会去查找原型，这个函数是哪个？
```
hasOwnProperty
JavaScript 中 hasOwnProperty 函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
使用方法：object.hasOwnProperty(proName)其中参数object是必选项，一个对象的实例。proName是必选项，一个属性名称的字符串值。
```

6. 谈一谈你对 ECMAScript6 的了解？
```
因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。
也就是说，ES6就是ES2015
```

7. Canvas和SVG的比较
```
Canvas
依赖分辨率
不支持事件处理器
弱的文本渲染能力
能够以 .png 或 .jpg 格式保存结果图像
最适合图像密集型的游戏，其中的许多对象会被频繁重绘
SVG
不依赖分辨率
支持事件处理器
最适合带有大型渲染区域的应用程序（比如谷歌地图）
复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
不适合游戏应用
```

8. 谈谈你对 JavaScript 中的模块规范 CommonJS、AMD、CMD 的了解？
![](/images/posts/interview/1-1.png)

9. 跨域解决办法
- [七种前端跨域解决方案](https://lidaguang1989.github.io/2016/09/cors/)

10. 浏览器解析过程
![](/images/posts/interview/1-2.png)

11. [jsonp的原理与实现](https://segmentfault.com/a/1190000007665361)



### 高级难度
1. 介绍一下 JavaScript 原型，原型链，它们有何特点？
```
每个对象都会在其内部初始化一个属性，就是prototype(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。
关系：instance.constructor.prototype = instance.__proto__
特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本，当我们修改原型时，与之相关的对象也会继承这一改变。
当我们需要一个属性时，JavaScript引擎会先看当前对象中是否有这个属性，如果没有的话，就会查找它的prototype对象是否有这个属性，如此递推下去，一致检索到Object内建对象。
```

2. JavaScript 如何实现继承？
```
(1)构造继承
(2)原型继承
(3)实例继承
(4)拷贝继承
//原型prototype机制或apply和call方法去实现较简单，建议使用构造函数与原型混合方式。
function Parent() { 
    this.name = 'song';
}
function Child() {
    this.age = 28;
}
Child.prototype = new Parent(); //通过原型,继承了Parent
var demo = new Child();
alert(demo.age);
alert(demo.name); //得到被继承的属性
```

3. 什么是闭包(closure)，为什么要用它？
- [浅谈JS闭包](https://lidaguang1989.github.io/2017/11/js-closure/)


4. Ajax的原理
- [图解Ajax工作原理](https://www.cnblogs.com/ygj0930/p/6126542.html)

5. 前端性能优化方案
- [移动前端性能优化方案](https://lidaguang1989.github.io/2016/05/performance-optimization/)

6. JS设计模式
- [http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
- [http://www.alloyteam.com/2012/10/commonly-javascript-design-patterns-simple-factory-pattern/](http://www.alloyteam.com/2012/10/commonly-javascript-design-patterns-simple-factory-pattern/)
- [http://www.runoob.com/design-pattern/design-pattern-tutorial.html](http://www.runoob.com/design-pattern/design-pattern-tutorial.html)

7. [谈谈对Web Worker的理解](https://qiutc.me/post/the-multithread-in-javascript-web-worker.html)