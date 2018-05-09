---
layout: post
title: 前端面试题汇总之CSS(含答案)
date: 2018-04-29
tags: [面试]
---


1. 实现动画有哪些途径?
```
CSS3
JS帧动画,定时器,requestAnimateFrame
Canvas动画
SVG
图片
```

2. 如何理解CSS的盒子模型？
```
每个HTML元素都是长方形盒子。
（1）盒子模型有两种：IE盒子模型、标准W3C盒子模型；IE的content部分包含了border和pading。
（2）标准W3C盒模型包含：内容(content)、填充(padding)、边界(margin)、边框(border)。
（2）标准盒模型指定的width实际指内容(content)的宽，所以元素的宽=width+padding+border+margin
```

3. link和@import的区别？
```
（1）link属于XHTML标签，而@import是CSS提供的。
（2）页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载。
（3）import只在IE 5以上才能识别，而link是XHTML标签，无兼容问题。
（4）link方式的样式权重高于@import的权重。
（5）使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的。
```

4. CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS 3新增伪类有哪些？
```
id选择器（# myid）
类选择器（.myclassname）
标签选择器（div、h1、p）
相邻选择器（h1 + p）
子选择器（ul < li）
后代选择器（li a）
通配符选择器（ * ）
属性选择器（a[rel = ""external""]）
伪类选择器（a: hover, li: nth - child）
可继承： font-size font-family color, UL LI DL DD DT;
不可继承 ：border padding margin width height ;
优先级就近原则，样式定义最近者为准，载入样式以最后载入的定位为准。
优先级为：
       !important >  id > class > tag  
       important 比 内联优先级高
CSS3新增伪类举例：
p:first-of-type 选择属于其父元素的首个<p>元素的每个<p>元素。
p:last-of-type  选择属于其父元素的最后<p>元素的每个<p>元素。
p:only-of-type  选择属于其父元素唯一的<p>元素的每个<p>元素。
p:only-child    选择属于其父元素的唯一子元素的每个<p>元素。
p:nth-child(2)  选择属于其父元素的第二个子元素的每个<p>元素。
:enabled、:disabled 控制表单控件的禁用状态。
:checked  单选框或复选框被选中。
```

5. 如何居中div？
```
给div设置一个宽度，然后添加margin:0 auto属性
div {
    width:200px;
     margin:0 auto;
}
```

6. 如何居中一个浮动元素
```
确定容器的宽高，如宽500、高 300的层，设置层的外边距
.div { 
      width:500px ; height:300px;//高度可以不设
      margin: -150px 0 0 -250px;
      position:relative;相对定位
      background-color:pink;//方便看效果
      left:50%;
      top:50%;
}
```

7. 经常遇到的浏览器的兼容性有哪些？原因、解决方法是什么？
```
浏览器默认的margin和padding不同，解决方案是加一个全局的*{margin:0;padding:0;}来统一
```

8. 常用Hack的技巧
```
（1）IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用getAttribute()获取自定义属性；
（2）Firefox下，只能使用getAttribute()获取自定义属性。解决方法：统一通过getAttribute()获取自定义属性。
（3）IE下，even对象有x,y属性，但是没有pageX,pageY属性；
（4）Firefox下，event对象有pageX,pageY属性，但是没有x,y属性。解决方法是条件注释，缺点是在IE浏览器下可能会增加额外的HTTP请求数。
（6）超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不再具有hover和active了，解决方法是改变CSS属性的排列顺序：
L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}
```

9. 列出display的值，说明它们的作用
```
display的值：
block 像块类型元素一样显示。
none 缺省值。像行内元素类型一样显示。
inline-block 像行内元素一样显示，但其内容像块类型元素一样显示。
list-item 像块类型元素一样显示，并添加样式列表标记。
```

10. 为什么要初始化CSS样式？
```
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
最简单的初始化方法就是：* {padding: 0; margin: 0;} （笔者不建议这样）
淘宝的样式初始化： 
    body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
    body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
    h1, h2, h3, h4, h5, h6{ font-size:100%; }
    address, cite, dfn, em, var { font-style:normal; }
    code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
    small{ font-size:12px; }
    ul, ol { list-style:none; }
    a { text-decoration:none; }
    a:hover { text-decoration:underline; }
    sup { vertical-align:text-top; }
    sub{ vertical-align:text-bottom; }
    legend { color:#000; }
    fieldset, img { border:0; }
    button, input, select, textarea { font-size:100%; }
    table { border-collapse:collapse; border-spacing:0; }
    ```

11. CSS是怎样定义权重规则的？
```
以下是权重的规则：标签的权重为1，class的权重为10，id的权重为100，以下例子是演示各种定义的权重值：
/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
#id1{
}
/*权重为100+1=101*/
#id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
} 
如果权重相同，则最后定义的样式会起作用，但是应该避免这种情况出现。
```
[https://www.w3cplus.com/css/css-specificity-things-you-should-know.html](https://www.w3cplus.com/css/css-specificity-things-you-should-know.html)

12. 如何理解表现与内容相分离？
```
表现与结构相分离简单的说就是HTML中只有标签元素 表现完全是由CSS文件控制的。
```

13. 如何定义高度为1px的容器？
```
div{
	heigh：1px; 
	width:10px; 
	overflow:hidden
} 
IE 6下这个问题是默认行高造成的，overflow:hidden | zoom:0.08 | line- height:1px这样也可以解决。
```

14. Firefox下文本无法撑开容器的高度，如何解决？
```
清除浮动 .clear{ clear:both; height:0px; overflow:hidden;}
```

15. cursor:hand在FF下不显示小手，如何解决？
```
目前没有hand这个属性，使用cursor; pointer; 替代
```

16. 在IE中内容会自适应高度，而FF不会自适应高度，怎么办？
```
在要自适应高度的层中加一个层，样式为
.clear{clear:both;font-size:0px;height:1px}，
这样解决有一个小小的问题，高度会多一个像素。还有一种解决方法，给当前层加上一个伪类。
#test:after {
    content: ""."";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
```

17. 用纯 CSS 创建一个三角形的原理是什么？
```
把上、左、右三条边隐藏掉（颜色设为 transparent）
#demo {
  width:0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

18. 如何设计一个满屏“品”字布局？
```
简单的方式：
  上面的div宽100%，
  下面的两个div分别宽50%，
  用float或inline使其不换行。
```

19. ::before 和 :after中双冒号和单冒号 有什么区别？解释一下这2个伪元素的作用。
```
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。
伪元素由双冒号和伪元素名称组成。双冒号是在css3规范中引入的，用于区分伪类和伪元素。但是伪类兼容现存样式，浏览器需要同时支持旧的伪类，比如:first-line、:first-letter、:before、:after等。
对于CSS2之前已有的伪元素，比如:before，单冒号和双冒号的写法::before作用是一样的。
提醒，如果你的网站只需要兼容webkit、firefox、opera等浏览器，建议对于伪元素采用双冒号的写法，如果不得不兼容IE浏览器，还是用CSS2的单冒号写法比较安全。
```

20. 介绍一下 Sass 和 Less 是什么？它们有何区别？
```
Sass (Syntactically Awesome Stylesheets)是一种动态样式语言，语法跟css一样(但多了些功能)，比css好写，而且更容易阅读。Sass语法类似与Html，属于缩排语法（makeup），用意就是为了快速写Html和Css。
Less一种动态样式语言. 将CSS赋予了动态语言的特性，如变量，继承，运算， 函数. LESS 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可一在服务端运行 (借助 Node.js)。
区别：
(1))Sass是基于Ruby的，是在服务端处理的，而Less是需要引入less.js来处理Less代码输出Css到浏览器，也可以在开发环节使用Less，然后编译成Css文件，直接放到项目中，也有Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。
(2)变量符不一样，less是@，而Scss是$，而且变量的作用域也不一样，后面会讲到。
(3)输出设置，Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。
(4)Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。
```

21. CSS display:none和visibility:hidden的区别
```
visibility:hidden隐藏，但在浏览时保留位置
display:none视为不存在，且不加载
```

22. position的值及区别
```
Static
这个是元素的默认定位方式，元素出现在正常的文档流中，会占用页面空间。不能使用top，bottom，left，right和z-index。
Relative
相对定位方式，相对于其父级元素（无论父级元素此时为何种定位方式）进行定位，准确地说是相对于其父级元素所剩余的未被占用的空间进行定位（在父元素由多个相对定位的子元素时可以看出），且会占用该元素在文档中初始的页面空间，即在使用top，bottom，left，right进行移动位置之后依旧不会改变其所占用空间的位置。可以使用z-index进行在z轴方向上的移动。
Absolute
绝对定位方式，脱离文档流，不会占用页面空间。以最近的不是static定位的父级元素作为参考进行定位，如果其所有的父级元素都是static定位，那么此元素最终则是以当前窗口作为参考进行定位。可以使用top，bottom，left，right进行位置移动，亦可使用z-index在z轴上面进行移动。当元素为此定位时，如果该元素为内联元素，则会变为块级元素，即可以直接设置其宽和高的值；如果该元素为块级元素，则其宽度会由初始的100%变为auto。
注意：当元素设置为绝对定位时，在没有指定top，bottom，left，right的值时，他们的值并不是0，这几个值是有默认值的，默认值就是该元素设置为绝对定位前所处的正常文档流中的位置。（可能我没有描述的很清楚，建议自己写个示例看看效果）
Fixed
绝对定位方式，直接以浏览器窗口作为参考进行定位。其它特性同absolute定位。
```

23. float和position: absolute的区别
```
使用float脱离文档流时，其他盒子会无视这个元素，但其他盒子内的文本依然会为这个元素让出位置，环绕在周围。典型的就是图文混排中文字环绕图片的效果了。
而对于使用absolute脱离文档流的元素，其他盒子与其他盒子内的文本都会无视它。
```
[http://www.cnblogs.com/linxiong945/p/4052737.html](http://www.cnblogs.com/linxiong945/p/4052737.html)


24. 介绍所知道的CSS hack技巧(如：_， *， +， \9， !important 之类)
- [史上最全的CSS hack方式一览](https://blog.csdn.net/freshlover/article/details/12132801)