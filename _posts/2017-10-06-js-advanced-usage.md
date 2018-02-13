---
layout: post
title: JS高级用法
date: 2017-10-06
tags: [JavaScript]
---

####  1、重复定时器

```javascript
setTimeout(function() {
    // 处理中
    setTimeout(arguments.callee, 1000);
}, 1000)
```

这种模式链式调用了 setTimeout(), 每次函数执行的时候都会创建一个新的定时器，
第二个 setTimeout() 的调用使用了 arguments.callee 来获取对当前执行函数的引用，并为其设置另外一个定时器。
这样做的好处是在前一个定时器代码执行完之前，不会向队列插入新的定时器代码，确保不会有任何缺失的间隔。


#### 2、数组分块处理

```javascript
function chunk(array, process, context) {
    setTimeout(function() {
        var item = array.shift();
        process.call(context, item);

        if (array.length > 0) {
            setTimeout(arguments.callee, 1000);
        }
    }, 1000);
}
```

用法：

```javascript
var data = [12, 123, 234, 345, 456, 567];
function printValue(item) {
    console.log(item);
}
chunk(data, printValue);
```

数组分块的重要性在于他可以将多个项目的处理在执行队列上分开，在每个项目处理之后，给予其他的浏览器处理机会运行，
这样就可能避免长时间运行脚本的错误。


#### 3、节流函数

```javascript
function throttle(method, context) {
    clearTimeout(method.tID);
    method.tID = setTimeout(function () {
        method.call(context);
    }, 1000);
}
```

用法：

```javascript
function showTime() {
    console.log("nowDate:" + new Date().toLocaleDateString());
}

setInterval(function () {
    throttle(showTime);
}, 2000);
```

#### 4、自定义事件

```javascript
function EventTarget() {
    this.handlers = {};
}

EventTarget.prototype = {
    constructor: EventTarget,
    addHandler: function (type, handler) {
        if (typeof this.handlers[type] == "undefined") {
            this.handlers[type] = [];
        }

        this.handlers[type].push(handler);
    },
    fire: function (event) {
        if (!event.target) {
            event.target = this;
        }
        if (this.handlers[event.type] instanceof Array) {
            var handlers = this.handlers[event.type];
            for (var i = 0, len = handlers.length; i < len; i++) {
                handlers[i](event);
            }
        }
    },
    removeHandler: function (type, handler) {
        if (this.handlers[type] instanceof Array) {
            var handlers = this.handlers[type];
            for (var i = 0, len = handlers.length; i < len; i++) {
                if (handlers[i] == handler) {
                    break;
                }
            }

            handlers.splice(i, 1);
        }
    }
};
```

用法：

```javascript
function handleMessage(event) {
    alert("Message received: " + event.message);
}

var target = new EventTarget();
target.addHandler("message", handleMessage);
target.fire({type: "message", message: "Hello World"});
target.removeHandler("message", handleMessage);
```