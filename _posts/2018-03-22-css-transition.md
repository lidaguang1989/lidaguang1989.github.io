---
layout: post
title: CSS Transition 简介
date: 2018-03-23
tags: [CSS]
---


### CSS Transitions简介
CSS Transitions是最简单的一种创建CSS动画的方式。

CSS Transitions共有以下几种属性：

- transition-property              需要设置过渡效果的 CSS 属性的名称
- transition-duration              完成过渡效果需要多少秒或毫秒
- transition-timing-function       速度效果的速度曲线（比如: linear, ease）, 默认为ease
- transition-delay                 定义过渡效果需要延时多少秒开始

以下是transition属性的简写形式：
```
.container {
  transition: property
              duration
              timing-function
              delay;
}
```

### CSS Transition应用示例
以下代码实现了一个CSS 动画：
```
.one,
.three {
  background: rgba(142, 92, 205, .75);
  transition: background 1s ease-in;
}

.two,
.four {
  background: rgba(236, 252, 100, .75);
}

.circle:hover {
  background: rgba(142, 92, 205, .25); /* lighter */
}
```

请看Glitch上的例子[https://flavio-css-transitions-example.glitch.me](https://flavio-css-transitions-example.glitch.me)

当鼠标悬停在 .one 和 .three 元素上时，紫色圆圈的背景会有一个过渡动画效果，但是当鼠标悬停在黄色圆圈上时却没有动画效果，
因为没有针对他们设置transition属性。

<div class="glitch-embed-wrap" style="height: 563px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/flavio-css-transitions-example?path=style.css&amp;previewSize=99&amp;sidebarCollapsed=true" alt="flavio-css-transitions-example on glitch" style="height: 100%; width: 100%; border: 0;"></iframe>
</div>

### transition-timing-function属性

transition-timing-function属性用来指定动画的加速曲线类型。

下列常用值可以直接使用：

- linear
- ease
- ease-in
- ease-out
- ease-in-out

下面这个[例子](https://flavio-css-transitions-easings.glitch.me/)展示了这些值的实际运行效果。

<div class="glitch-embed-wrap" style="height: 739px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/flavio-css-transitions-easings?path=style.css&amp;previewSize=50&amp;previewFirst=true&amp;sidebarCollapsed=true" alt="flavio-css-transitions-easings on glitch" style="height: 100%; width: 100%; border: 0;"></iframe>
</div>

你可以使用[cubic bezier curves](https://developer.mozilla.org/en-US/docs/Web/CSS/single-transition-timing-function)定制一个更加复杂的加速曲线。这个工具是非常先进的，但是它们的基本原理
都是基于贝塞尔曲线。

### CSS Transitons在Browser DevTools中的使用
Browser DevTools提供了一种非常友好的方式使动画非常直观可视化。

Chrome：

<img src="https://d33wubrfki0l68.cloudfront.net/490e1198bea7aa1e584110a8ea5ebde300daab6c/feda6/css-transitions/devtools-chrome.png" alt="Debug CSS Transitions in Chrome DevTools">

Firefox:

<img src="https://d33wubrfki0l68.cloudfront.net/77b27f3dae56741b8c8d72003df761438957e59a/e65c7/css-transitions/devtools-firefox.png" alt="Debug CSS Transitions in Firefox DevTools">

### 哪些属性可以应用Transition功能
```
background
background-color
background-position
background-size
border
border-color
border-width
border-bottom
border-bottom-color
border-bottom-left-radius
border-bottom-right-radius
border-bottom-width
border-left
border-left-color
border-left-width
border-radius
border-right
border-right-color
border-right-width
border-spacing
border-top
border-top-color
border-top-left-radius
border-top-right-radius
border-top-width
bottom
box-shadow
caret-color
clip
color
column-count
column-gap
column-rule
column-rule-color
column-rule-width
column-width
columns
content
filter
flex
flex-basis
flex-grow
flex-shrink
font
font-size
font-size-adjust
font-stretch
font-weight
grid-area
grid-auto-columns
grid-auto-flow
grid-auto-rows
grid-column-end
grid-column-gap
grid-column-start
grid-column
grid-gap
grid-row-end
grid-row-gap
grid-row-start
grid-row
grid-template-areas
grid-template-columns
grid-template-rows
grid-template
grid
height
left
letter-spacing
line-height
margin
margin-bottom
margin-left
margin-right
margin-top
max-height
max-width
min-height
min-width
opacity
order
outline
outline-color
outline-offset
outline-width
padding
padding-bottom
padding-left
padding-right
padding-top
perspective
perspective-origin
quotes
right
tab-size
text-decoration
text-decoration-color
text-indent
text-shadow
top
transform.
vertical-align
visibility
width
word-spacing
z-index
```


Browser DevTools使得你可以直接在页面中实时编辑修改而无需重新加载代码。


原文：[https://flaviocopes.com/css-transitions](https://flaviocopes.com/css-transitions/)