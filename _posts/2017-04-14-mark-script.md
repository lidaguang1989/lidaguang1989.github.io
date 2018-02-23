---
layout: post
title: 使用script嵌入代码时的注意事项
date: 2017-04-14
tags: [JavaScript]
---

在使用\<script>嵌入JavaScript代码时，记住不要在代码中的任何地方出现"</script>"字符串。

例如浏览器执行下面代码会报错：
```
<script type="text/javascript">
    function sayHello() {
        console.log("</script>");
    }
</script>
```
浏览器会报以下错误：

**Uncaught SyntaxError: Invalid or unexpected token**

因为按照解析嵌入式代码的规则，当浏览器遇到字符串"</script>"时，会认为那是结束的</script>标签。

而通过转义符可以解决这个问题，例如：
```
<script type="text/javascript">
    function sayHello() {
        console.log("<\/script>");
    }
</script>
```