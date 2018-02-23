---
layout: post
title: 什么是Web Worker？
date: 2017-03-23
tags: [技术杂谈]
---

简单点说，Web Worker就是一个运行在后台的JavaScript线程，不会影响页面的响应。

我们知道，JavaScript是单线程的脚本语言，即同一时刻只能做一件事情，否则会带来极其复杂的同步问题。比如JavaScript同时有两个线程，一个线程负责给某个DOM节点添加内容，另一个线程删除这个节点，这时，浏览器要以哪个线程为主呢？

所以，为了避免同步复杂性的问题，JavaScript从一诞生起就是单线程，这也是这门语言的特征。

JavaScript的单线程机制会带来一个问题，当有一些非常复杂的任务需要处理时，页面不得不需要等待任务处理完成才能响应用户的操作，这对于页面的响应及用户体验都会带来一些负面的影响，为了解决这个问题，同时也是为了利用多核CPU的计算能力，HTML5提出了Web Worker标准，允许JavaScript创建多个线程，但是新创建的这些线程将作为子线程并且完全受主线程的控制。并且不得操作DOM，其实本质上还是单线程。 所以，我们可以把一些费时的任务交给Web Worker创建的子线程在后台完成，而前台页面依然可以处理用户的响应。

由于Web Worker创建的线程是受限的子线程，所以会有一些使用限制：

* Web Worker无法访问DOM节点；
* Web Worker无法访问全局变量或是全局函数；
* Web Worker无法调用alert()或者confirm之类的函数；
* Web Worker无法访问window、document之类的浏览器全局变量；
不过Web Worker中的Javascript依然可以使用setTimeout(),setInterval()之类的函数，也可以使用XMLHttpRequest对象来做Ajax通信。

目前所有主流浏览器均支持 web worker，除了 Internet Explorer。

熟悉Angular的朋友应该都清楚，Angular1最被大家诟病就是它的脏检查机制，当scope的数据量过多时会严重影响性能。而Angular2正是借助WebWorker来把繁重的计算工作移入辅助线程，让界面线程不受影响。