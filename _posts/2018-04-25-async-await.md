---
layout: post
title: async/await 执行顺序详解
date: 2018-04-25
tags: [JavaScript]
---


随着async/await正式纳入ES7标准，越来越多的人开始研究据说是异步编程终级解决方案的async/await。但是很多人对这个方法中内部怎么执行的还不是很了解，本文是我看了一遍技术博客
[理解 JavaScript 的 async/await](https://segmentfault.com/a/1190000007535316)（如果对async／await不熟悉可以先看下这篇文章）后拓展了一下，我理了一下await之后js的执行顺序，希望可以给别人解疑答惑，先简单介绍一下async／await。

1. async/await 是一种编写异步代码的新方法。之前异步代码的方案是回调和 promise。
2. async/await 是建立在 promise 的基础上。
3. async/await 像 promise 一样，也是非阻塞的。
4. async/await 让异步代码看起来、表现起来更像同步代码。这正是其威力所在。

### async怎么处理返回值
```
async function testAsync() {
    return "hello async";
}
let result = testAsync();
console.log(result)
```
输出结果：
```
Promise {[[PromiseStatus]]: "resolved", [[PromiseValue]]: "hello async"}
```

从结果中可以看到async函数返回的是一个promise对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。

如果asyn函数没有返回值
```
async function testAsync1() {
    console.log("hello async");
}
let result1 = testAsync1();
console.log(result1);
```

结果
```
hello async
Promise {[[PromiseStatus]]: "resolved", [[PromiseValue]]: undefined}
```

结果返回Promise.resolve(undefined)。

### await做了什么处理
从字面意思上看await就是等待，await等待的是一个表达式，这个表达式的返回值可以是一个promise对象也可以是其他值。

> 很多人以为await会一直等待之后的表达式执行完之后才会继续执行后面的代码，实际上await是一个让出线程的标志。await后面的函数会先执行一遍，然后就会跳出整个async函数来执行后面js栈（后面会详述）的代码。等本轮事件循环执行完了之后又会跳回到async函数中等待await
后面表达式的返回值，如果返回值为非promise则继续执行async函数后面的代码，否则将返回的promise放入promise队列（Promise的Job Queue）

### async/await 执行顺序

先看一个例子
```
function testSometing() {
    console.log("执行testSometing");
    return "testSometing";
}

async function testAsync() {
    console.log("执行testAsync");
    return Promise.resolve("hello async");
}

async function test() {
    console.log("test start...");
    const v1 = await testSometing();//关键点1
    console.log(v1);
    const v2 = await testAsync();
    console.log(v2);
    console.log(v1, v2);
}

test();

var promise = new Promise((resolve)=> { console.log("promise start.."); resolve("promise");});//关键点2
promise.then((val)=> console.log(val));

console.log("test end...")
```

输出结果：
```
test start...
执行testSometing
promise start..
test end...
testSometing
执行testAsync
promise
hello async
testSometing hello async
```

当test函数执行到

> const v1 = await testSometing()；

的时候，会先执行testSometing这个函数打印出“执行testSometing”的字符串，然后因为await会让出线程就会区执行后面的

> var promise = new Promise((resolve)=> { console.log("promise start.."); resolve("promise");});//关键点2

然后打印出“promise start..”接下来会把返回的这promise放入promise队列（Promise的Job Queue），继续执行打印“test end...”，等本轮事件循环执行结束后，又会跳回到async函数中（test函数），等待之前await 后面表达式的返回值，因为testSometing 不是async函数，所以返回的是一个字符串“testSometing”，test函数继续执行，执行到

> const v2 = await testAsync();

和之前一样又会跳出test函数，执行后续代码，此时事件循环就到了promise的队列，执行promise.then((val)=> console.log(val));then后面的语句，之后和前面一样又跳回到test函数继续执行。

这个就是在async/await 函数之后js的执行顺序，我们再看一个列子把testSometing函数前面加上async
```
async function testSometing() {
    console.log("执行testSometing");
    return "testSometing";
}

async function testAsync() {
    console.log("执行testAsync");
    return Promise.resolve("hello async");
}

async function test() {
    console.log("test start...");
    const v1 = await testSometing();
    console.log(v1);
    const v2 = await testAsync();
    console.log(v2);
    console.log(v1, v2);
}

test();

var promise = new Promise((resolve)=> { console.log("promise start.."); resolve("promise");});//3
promise.then((val)=> console.log(val));

console.log("test end...")
```

输出结果：
```
test start...
执行testSometing
promise start..
test end...
promise
testSometing
执行testAsync
hello async
testSometing hello async
```

和上一个例子比较发现promise.then((val)=> console.log(val));先与console.log(v1);执行了，原因是因为现在testSometing函数加了async，返回的是一个Promise对象要要等它resolve，所以将当前Promise推入队列，所以会继续跳出test函数执行后续代码。之后就开始执行promise的任务队列了，所以先执行了promise.then((val)=> console.log(val));因为这个Promise对象先推入队列；

最后再来看一个例子：
```
async function async1() {  
    console.log("async1 start");  
    await  async2();  
    console.log("async1 end");  
  
}  
async  function async2() {  
   console.log( 'async2');  
}  
console.log("script start");  
setTimeout(function () {  
    console.log("settimeout");  
},0);  
async1();  
new Promise(function (resolve) {  
    console.log("promise1");  
    resolve();  
}).then(function () {  
    console.log("promise2");  
});  
console.log('script end');  
```

结果：
```
script start
async1 start
async2
promise1
script end
promise2
async1 end
settimeout
```

我们来挨个分析一下，
```
await  async2();//执行这一句后，输出async2后，await会让出当前线程，将后面的代码加到任务队列中，然后继续执行test()函数后面的同步代码  
```

执行到setTimeout函数时，将其回调函数加入队列(此队列与promise队列不是同一个队列，执行的优先级低于promise)。继续执行

创建promise对象里面的代码属于同步代码，promise的异步性体现在then与catch处，所以promise1被输出，然后将then函数的代码加入队列，继续执行同步代码，输出script end。

至此同步代码执行完毕，开始从队列中调取任务执行，由于刚刚提到过，setTimeout的任务队列优先级低于promise队列，所以首先执行promise队列的第一个任务，即执行async1中await后面的代码，输出async1 end。

然后执行then方法的部分，输出promise2。最后promise队列中任务执行完毕，再执行setTimeout的任务队列，输出settimeout。



至此，该题的输出结果分析完毕了，这类的执行结果可以用一句话总结，**先执行同步代码，遇到异步代码就先加入队列，然后按入队的顺序执行异步代码，最后执行setTimeout队列的代码**。


**队列任务优先级：promise.Trick()>promise的回调>setTimeout>setImmediate**

### 写在最后
写到这里大家应该已经清楚了使用async/await进行异步操作时js的执行顺序，在研究过程中还发现了js的任务队列执行顺序的问题，就下次再述。如果大家对有什么意见或建议可以指出。本篇是我第一次发布文章，希望大家不吝赐教。

[原文链接](https://segmentfault.com/a/1190000011296839)