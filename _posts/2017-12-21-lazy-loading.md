---
layout: post
title: IntersectionObserver实现图片懒加载
date: 2017-12-21
tags: [JavaScript]
---

### API

[https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

直接上源码：
```
<!DOCTYPE html>
<html>
    <header>
        <style>
            .list-item{
                height: 400px;
                margin: 5px;
                background-color: lightblue;
                list-style: none;
            }
        </style>
    </header>
    <body>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon1.png'></li>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon2.png'></li>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon3.png'></li>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon4.png'></li>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon5.png'></li>
        <li class="list-item"><img class="list-item-img" alt="loading" data-src='./images/icon6.png'></li>

        <script>
            var observer = new IntersectionObserver(function(changes) {
                console.log(changes);
                changes.forEach(function(element, index) {
                    // statements
                    if (element.intersectionRatio > 0 && element.intersectionRatio <= 1) {
                        element.target.src = element.target.dataset.src;
                    }
                });
            });


            function addObserver() {
                var listItems = document.querySelectorAll('.list-item-img');
                listItems.forEach(function(item) {
                    observer.observe(item);
                });
            }

            addObserver();
        </script>
    </body>
</html>
```

运行代码后发现，当滚动滚动轴时，只有当<li>区域完全显示出来后才会触发相应的下载图片的http请求。

### 兼容浏览器

**desktop**
![](/images/posts/lazyloading/1-1.png)

**Mobile**
![](/images/posts/lazyloading/1-2.png)