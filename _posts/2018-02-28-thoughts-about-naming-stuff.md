---
layout: post
title: 写代码而不是生成代码。关于命名的一些看法
date: 2018-02-28
tags: [译文，技术杂谈]
---

我现在写代码差不多有十年了，有时候觉得，"writing"这个词似乎变得比"code"更加重要。
我发现写代码很容易，但是写好代码却很困难。因为写好意味着你要创造一些有意义的东西，这些东西是要给别人看的。
机器可以识别丑陋的代码，但是人类却不行。我们的工作一个很重要的部分就是要确保我们的代码对同事是清晰可阅读的。

我写过[三本书](http://krasimirtsonev.com/#books)，可以这么说，找到一个合适的用语依然是我认为最困难的事情。
我们整天都在书写所以我们(开发者)或多或者也可以算是作家了。要成为一个优秀的作家是非常困难的。当然，作为开发人员，
不需要我们去讲故事，但是如何命名却是最困难的问题之一。
比如，
如何给HTML标签命名？
如何给JavaScript中的变量和函数命名？
如何给CSS中的类命名？

接下来我们来看下这些问题在前端开发中是如何存在的。

### 在HTML中
当我们讨论HTML的命名时，我们可能指的是[HTML语义](https://en.wikipedia.org/wiki/Semantic_HTML)。
HTML标签不仅构建了页面，而且还有实际意义。在一开始的时候HTML更多的是关于页面的介绍，近些年才逐渐演变成了一门语言，
比如，通常我们会认为\<b>标签的作用是把标签里面的字体变成粗体，然而，通过HTML5规范，我们现在赋予了它一些新的语义上的含义。
> The b element represents a span of text to which attention is being drawn for utilitarian purposes without conveying any extra importance and with no implication of an alternate voice or mood, such as key words in a document abstract, product names in a review, actionable words in interactive text-driven software, or an article lede.

大多数的HTML标签都有一些特殊含义。如果我们要创建一个导航栏，最好选择使用\<nav>标签而不是\<div>，因为\<div>没有任何含义。

不像CSS和JavaScript语言，HTML语言的标签数量是有限的(自定义的标签除外)。这样对于命名似乎就会容易很多，只要我们能够理解每个标签的含义，
我们就能够在合适的位置使用它们。比如，我们不能在页脚部分使用\<header>标签，如何想要展示一个无序列表，
最好使用\<ul>和\<li>标签而不是\<div>和\<p>标签。

关于好命名的一个特点是一个新手能否快速的理解代码。这里所说的理解指的是知道代码在干什么，能够自如的对代码进行更改。

好的HTML语义使我们的应用程序更容易访问，这一点现在很重要，因为我们的目标是越来越多的人拥有越来越多的设备。
正确命名HTML标记会大大增加我们产品的价值。

### 在CSS中
在CSS中进行命名将会更加困难。比如，需要给\<section>标签添加top border属性，可以有以下几种命名方式：
- .sectionBorder
- .borderedSection
- .addSectionBorder
- .isBorderedSection

一些开发人员有他们自己的关于如何使用这些类的想法，通常情况下我们都是根据个人喜好进行命名的。无所谓对与错，只要事先定义好命名规则，
然后整个项目组都遵从同样的规则就是OK的。

这里有些定义好的命名规范可供参考[SMACSS](https://smacss.com/), [BEM](http://getbem.com/introduction/) 或者 [SUIT](http://suitcss.github.io/)。
这些规范都遵从职责分离的原则。有网格系统类、排版和颜色类、应用程序特定的类。

我个人遵从的一个原则是先定义一些小类，然后通过组合的方式来达到预期的效果。
比如，我们前面提到过的给\<section>添加top border属性：
```
// in CSS
.sectionBorder{
  padding-top: 0.4rem;
  border-top: 2px solid #BADA55;
}

// in HTML
<section class="sectionBorder">...</section>
```
以上写法没有任何问题，但我更愿意写成以下这样：
```
// in CSS
.u-pt {
  padding-top: 0.4rem;
}
.u-bc-brandColor {
  border-color: #BADA55;
}
.u-bt {
  border-style: solid;
  border-top-width: 2px;
}

// in HTML
<section class="u-pt u-bc-brandColor u-bt"> ... </section>
```
<span style="color: red;">注意：u 前缀来源于 utility</span>

我们要尽量创建一些具有高可重用性的CSS类。当然有些情况下不得不定制一些不可重用的类，这里需要做个权衡，
但是也不能为每个CSS属性定义一个类，具体还是要根据应用程序的样式抽象出那些原子类。

### 在JavaScript中
关于如何书写JavaScript有很多的[风格指南](https://addyosmani.com/blog/javascript-style-guides-and-beautifiers)可供参考，
但是他们大多都是定义代码的书写规范，即 花括号应该写在哪里？ 哪里应该加空格？而有关变量，对象，函数或者类的命名却没有严格的规定。

**变量(Variables)**

变量是用来存储数据的，那么你可能会问"这个变量是用来存储什么样的数据？"
如果我们想要存储系统中的账户信息，那么应该使用users或administrators来命名。不推荐使用info或者data来命名，
因为它们的含义太笼统。所以在命名的时候应该尽量具体。

来看下面的代码：
```
function isAdmin(data) {
  if (data) {
    data = data.permissions || [];
    return data.includes('level2') && data.includes('level3');
  }
  return false;
}
```
我们将data传递给isAdmin并且希望"level2"和"level3"字符串在"permissions"数组中。
首先我们假设data是undefined，所以我们把它作为boolean值来使用，然后再将它从object转为数组并赋值给自己(这将降低代码的性能)。

更好的书写方式如下：
```
function isAdmin(user) {
  if (typeof user !== 'undefined') {
    let { permissions = [] } = user;
    return permissions.includes('level2') && permissions.includes('level3');
  }
  return false;
}
```
现在，我们对用户和他/她的权限做了明确的区分。


**函数(Functions and methods)**

在给函数命名前先问自己两个问题：
1. 函数是干什么的？
2. 函数返回什么？
比如有一个函数要改变应用程序的状态，通过第一个问题我们可以这样命名"transformData"，
然后通过第二个问题可以改为"getTransformData"。这样我们通过函数名不仅知道函数在干什么，而且知道函数返回什么。

函数的功能要尽量单一，即一个函数只做一件事情。尽量提高内聚度，降低耦合度。

**Classes**

As opposite to functions and methods I don’t like using verbs while defining classes. To be honest it’s actually difficult doing this. That’s because the classes are kind of mixture between both worlds “What data stores and what it does with it?”. If we have an utility class that checks if the user is an administrator we’ll probably go with UserCapabilities and a method isAdmin. UserChecking or CheckUserCapabilities sounds odd.

A good class name is the one that brings context and suggests only one action. If the name sounds generic then the class is probably doing more stuff then expected.

### Conclusion
How we programmers name stuff in our scripts definitely matter. I would say that this is always difficult and it will be always difficult. What is important is to push for better naming because this will help us finding problems earlier. It will help us maintaining our applications. We shouldn’t save words while writing code. Please don’t use ctrl instead of controller or v instead of view. Let’s be better writers (programmers)!