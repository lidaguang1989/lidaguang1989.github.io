---
layout: post
title: 前端面试题汇总之HTML(含答案)
date: 2018-05-01
tags: [面试]
---



1. 什么是HTML语义化？
```
根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。
```

2. 简述一下你对HTML语义化的理解。
```
（1）HTML语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析；
（2）即使在没有样式CSS的情况下也能以一种文档格式显示，并且是容易阅读的；
（3）搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，有利于SEO；
（4）使阅读源代码的人更容易将网站分块，便于阅读、维护和理解。
```

3. 写HTML代码时应注意什么？
```
尽可能少的使用无语义的标签div和span；
在语义不明显时，既可以使用div或者p时，尽量用p, 因为p在默认情况下有上下间距，对兼容特殊终端有利；
不要使用纯样式标签，如：b、font、u等，改用css设置。
需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）；
使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td；
表单域要用fieldset标签包起来，并用legend标签说明表单的用途；
每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。
```

4. Doctype作用？严格模式与混杂模式如何区分？它们有何意义?
```
（1）<!DOCTYPE>声明位于HTML文档中的第一行，处于<html>标签之前，用于告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
（2）标准模式的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。
（3）如果HTML文档包含形式完整的DOCTYPE，那么他一般以标准模式呈现。对于HTML4.01文档，包含严格DTD的DOCTYPE常常导致页面已标准模式呈现。DOCTYPE不存在或者格式不正确会导致文档已混杂模式呈现。
```

5. 请描述一个网页从开始请求到最终显示的完整过程？
```
一个网页从请求到最终显示的完整过程一般可以分为如下7个步骤：
（1）在浏览器中输入网址；
（2）发送至DNS服务器并获得域名对应的WEB服务器IP地址；
（3）与WEB服务器建立TCP连接；
（4）浏览器向WEB服务器的IP地址发送相应的HTTP请求；
（5）WEB服务器响应请求并返回指定URL的数据，或错误信息，如果设定重定向，则重定向到新的URL地址；
（6）浏览器下载数据后解析HTML源文件，解析的过程中实现对页面的排版，解析完成后在浏览器中显示基础页面；
（7）分析页面中的超链接并显示在当前页面，重复以上过程直至无超链接需要发送，完成全部数据显示。
```

6. HTML5 为什么只需要写 <!DOCTYPE HTML>？
```
（1）HTML5不基于SGML，因此不需要对DTD进行引用，但是需要DOCTYPE来规范浏览器的行为（让浏览器按照它们应该的方式来运行）；
（2）HTML4.01基于SGML，所以需要对DTD进行引用，才能让浏览器知道该文档所使用的文档类型。
```

7. 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
```
***行内元素***
a - 锚点
em - 强调
strong - 粗体强调
span - 定义文本内区块
i - 斜体
img - 图片
b - 粗体
label - 表格标签
select - 项目选择
textarea - 多行文本输入框
sub - 下标
sup - 上标
q - 短引用
***块元素***
div - 常用块级
dl - 定义列表
dt，dd，ul- 非排序列表
li，ol-排序表单
p-段落
h1，h2，h3，h4，h5-标题
table-表格
fieldset - form控制组
form - 表单
***空元素***
br-换行
hr-水平分割线
```

8. 常见的浏览器内核有哪些？
```
Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等。
Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
EdgeHTML内核：Microsoft Edge。  [此内核其实是从MSHTML fork而来，删掉了几乎所有的IE私有特性]
```

9. html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 ?
```
***新增了以下的几大类元素***
内容元素，article、footer、header、nav、section。
表单控件，calendar、date、time、email、url、search。
控件元素，webworker, websockt, Geolocation。
***移出的元素有下列这些****
显现层元素：basefont，big，center，font, s，strike，tt，u。
性能较差元素：frame，frameset，noframes。
HTML5已形成了最终的标准，概括来讲，它主要是关于图像，位置，存储，多任务等功能的增加。
新增的元素有绘画 canvas ，用于媒介回放的 video 和 audio 元素，本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失，而sessionStorage的数据在浏览器关闭后自动删除，此外，还新增了以下的几大类元素。
内容元素，article、footer、header、nav、section。
表单控件，calendar、date、time、email、url、search。
控件元素，webworker, websockt, Geolocation。
***移出的元素有下列这些***
显现层元素：basefont，big，center，font, s，strike，tt，u。
性能较差元素：frame，frameset，noframes。
***新的技术***
canvas,svg,webworker, websocket, Geolocation......
```

10. HTML5的离线存储怎么使用？能否解释一下工作原理？
```
在用户没有连接英特网时，可以正常访问站点和应用；在用户连接英特网时，更新用户机器上的缓存文件。
`原理`：HTML5的离线存储是基于一个新建的 `.appcache` 文件的缓存机制（并非存储技术），通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储下来。之后当网络处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
使用方法：
(1) 在页面头部像下面一样加入一个 manifest 的属性；
(2) 在 cache.manifest 文件里编写离线存储资源；
     CACHE MANIFEST
     #v0.11
     CACHE：
       js/app.js
       css/style.css
     NETWORK:
       resource/logo.png
     FALLBACK：
       / /offline.html
(3) 在离线状态时，操作 window.applicationCache 进行需求实现
```
[https://segmentfault.com/a/1190000000732617](https://segmentfault.com/a/1190000000732617)

11. iframe 有哪些缺点？
```
（1）iframe会阻塞主页面的Onload事件；
（2）搜索引擎的检索程序无法解读这种页面，不利于SEO；
（3）iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
（4）使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好通过JavaScript动态给iframe添加src属性值，这样可以绕开以上两个问题。
```

12. HTML5的form如何关闭自动完成功能？
```
给不想要提示的 form 或下面某个 input 设置为 'autocomplete = off'。
```

13. 如何实现浏览器内多个标签页之间的通信？（阿里）
```
调用 localStorage、cookies 等本地存储方式
```

14. 如何在页面上实现一个圆形的可点击区域？
```
(1) map + area 或者 svg
(2) border-radius
(3) 纯js实现，需要求一个点在不在圆上的简单算法、获取鼠标坐标等等
```

15. 实现不使用 `border` 画出 `1px` 高的线，在不同浏览器的标准模式与怪异模式下都能保持一致的效果？
```
"<div style="height:1px; overflow:hidden; background:#ccc"></div>"
```

16. 网页验证码是干什么用的？是为了解决什么安全问题？
```
可以防止：恶意破解密码、刷票、论坛灌水，有效防止某个黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试，实际上用验证码是现在很多网站通行的方式，我们利用比较简易的方式实现了这个功能。这个问题可以由计算机生成并评判，但是必须只有人类才能解答。由于计算机无法解答CAPTCHA的问题，所以回答出问题的用户就可以被认为是人类。
```