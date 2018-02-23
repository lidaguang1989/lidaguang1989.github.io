---
layout: post
title: encodeURI与decodeURI
date: 2017-04-24
tags: [JavaScript]
---

Global对象的ecodeURI方法可以对URI进行编码，与其类似的还有一个方法encodeURIComponent方法。

相应的对URI的解码方法也有两个：decodeURI、decodeURIComponent, 下面将对这四个方法的用法做个简要介绍。



**encodeURI**：只对URI中的空格进行编码，所以decodeURI()方法主要用于对整个URI进行编码。

**encodeURIComponent**：会对URI中的所有非标准字符进行编码。

所以decodeURIComponent()方法主要用于对URI中的某一段进行编码。比如对跟在URI后面的查询字符串的参数进行编码。

举例说明：
```
var uri = "http://www.cnblogs.com/lidgblogs/p/test 7124770.html";
console.log(encodeURI(uri)); // http://www.cnblogs.com/lidgblogs/p/test%207124770.html
console.log(encodeURIComponent(uri)); // http%3A%2F%2Fwww.cnblogs.com%2Flidgblogs%2Fp%2Ftest%207124770.html
```

**decodeURI**: 只能对使用encodeURI编码的字符进行解码。

**decodeURIComponent**: 只能对使用encodeURIComponent编码的字符进行解码。

举例说明：
```
var uri = "http%3A%2F%2Fwww.cnblogs.com%2Flidgblogs%2Fp%2Ftest%207124770.html";
console.log(decodeURI(uri)); // http%3A%2F%2Fwww.cnblogs.com%2Flidgblogs%2Fp%2Ftest 7124770.html
console.log(decodeURIComponent(uri)); // http://www.cnblogs.com/lidgblogs/p/test 7124770.html
```