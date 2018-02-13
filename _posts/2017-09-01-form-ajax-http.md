---
layout: post
title: Form表单提交，Ajax请求，$http请求的区别
date: 2017-09-01
tags: [JavaScript]
---

做过前端同学想必都避免不了要和后台server打交道。而以下这三种与后台交互的方式想必大家都不陌生。

**Form表单提交，Ajax请求，Angular的$http请求**

以前一直搞不清楚什么时候应该用哪种方式请求数据，正好最近在做文件上传相关业务，顺便对这三种方式的使用场景及区别做个简单总结。

## 一，用法

以下是三种请求方式的API详细介绍：

Form：
[https://www.w3schools.com/html/html_forms.asp](https://www.w3schools.com/html/html_forms.asp)

Ajax：
[http://api.jquery.com/jQuery.ajax/](http://api.jquery.com/jQuery.ajax/)

Angular $http:
[https://angular.io/api/common/http](https://angular.io/api/common/http)


## 二，区别

### 1. Form VS Ajax, $http

#### 提交方式

form表单通常是通过在HTML中定义的action，method及submit来进行表单提交，另外也可以通过在js中调用submit函数来进行表单提交。

具体的提交方式有很多种，比如可以通过封装成XMLHttpRequest对象进行提交，这里就不一一详述了。

而另外两种请求(Ajax，$http)都是基于XMLHttpRequest进行的。

#### 页面刷新

Form提交，更新数据完成后，需要转到一个空白页面再对原页面进行提交后处理。哪怕是提交给自己本身的页面，也是需要刷新的，因此局限性很大。

Ajax，$http都可以实现页面的局部刷新，整个页面不会刷新。

#### 请求由谁来提交

Form提交是浏览器完成的，无论浏览器是否开启JS，都可以提交表单。

Ajax，$http是通过js来提交请求，请求与响应均由js引擎来处理，因此不启用JS的浏览器，无法完成该操作。

#### 是否可以上传文件

最初，ajax出于安全性考虑，不能对文件进行操作，所以就不能通过ajax来实现文件上传，但是通过隐藏form提交则可以实现这个功能，所以这也是用隐藏form提交的主要用途。

后来XMLHttpRequest引入了FormData类型，使得通过Ajax也可以实现文件上传，稍后会详细介绍。


### 2. Ajax VS $http

#### 默认Content-type类型

Ajax默认的Content-type是 x-www-form-urlencoded

$http默认的Content-type是 application/json

#### async

Ajax支持同步通信(async:false)

$http不支持async:false

#### 参数处理

Ajax在post数据之前jQuery会对数据进行序列化，转换成字符串："a=1&b=2"这种形式，然后把post的参数拼接到url上发送。

$http不会对数据做参数处理，数据将以json格式发送。

这样就会导致一个问题：有些请求通过Ajax可以请求成功，但是通过$http却失败了。

当然，我们可以通过做一些处理来实现这两种请求的统一。

可以从angular层面解决，把angular的post请求也按照jquery的方法做些改变，如下：

http://victorblog.com/2012/12/20/make-angularjs-http-service-behave-like-jquery-ajax/

https://github.com/petersirka/total.js/issues/26


## 三，通过Ajax提交文件

前面讲过，可以通过传统的form表单实现文件上传，不过传统的form表单提交会导致页面刷新，但是在有些情况下，我们不希望页面被刷新，

这种时候我们都是使用Ajax的方式进行请求的。

这时候就要用到一个对象FormData。

FormData类型其实是在XMLHttpRequest Level 2定义的，它是为序列化表以及创建与表单格式相同的数据提供的。

目前几乎所有的主流的浏览器都已经支持这个对象了。参见

https://developer.mozilla.org/zh-CN/docs/Web/API/FormData

以下为代码实现：
```javascript
<form id="id_form">
    <input type="text" name="name"/>
    <input type="number" name="age"/>
    <input type="file" name="fileName"/>
    <input type="button" value="sumbit" onclick="onSubmit();"/>
</form>

function onSubmit(){
    /* 可以通过两种方法获取文件 */
    // // 方法一： 直接获取表单值
    // var formData = new FormData(document.getElementById("id_form"));

    // 方法二：通过FormData的append方法动态添加属性
    var nameVal = $("input[name='name']").val();
    var ageVal = $("input[name='age']").val();
    var files = document.querySelector('input[type=file]').files;
    var formData =  new FormData();
    formData.append("nameKey", nameVal);
    formData.append("ageKey", ageVal);
    formData.append("fileKey", files[0]);

    /* 可以通过两种方式实现文件上传 */
    // // 方法一： 通过构建一个XMLHttpRequest对象实现上传
    // var req = new XMLHttpRequest();
    // req.open("post", "http://xxx/public/upload", false);
    // req.send(formData);

    // 方法二：通过Ajax实现上传
    $.ajax({
        url:"http://xxx/public/upload",
        type:"POST",
        data:formData,
        processData:false,
        contentType:false,
        success:function(data){
            console.log("response success: ", data);
        },
        error:function(error){
            alert("response error: ", error);
        }
    });
}

```