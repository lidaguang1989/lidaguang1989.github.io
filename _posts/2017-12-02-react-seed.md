---
layout: post
title: React入门-从项目搭建到部署
date: 2017-12-02
tags: [React]
---

前一段时间做了一个小项目，时间比较紧，就一个人月。最终希望能够通过微信公众号链接启动应用。
项目的业务细节就不多说了，主要是想分享一下做这个项目技术方面的一些经验。

### 技术选型
参考范围大致三种：AngularJS，Angular，React。

对于那些使用Angular不是太久的人来说，可能会有些蒙圈，AngularJS和Angular不是一个东西吗？

没错，它们是一个东西，但也不是一个东西。好了，废话少说，首先说明一下AngularJS和Angular的区别。

引用官方文档中的一句话
> Angular is the name for the Angular of today and tomorrow. AngularJS is the name for all v1.x versions of Angular.

这回很清晰了。

**Angular：指的是 v2.x 及以后的版本**

**AngularJS：特指 v1.x**

由于Angular从2.0以后版本更新比较大，所以为了做区分，只能想出这么一招了。

好了，回归正题，这三种技术怎么选择？

以下是我当时的考虑：

**AngularJS**：由于之前两个项目一直在用AngularJS，所以使用起来并不陌生，相对buffer也会少很多(前面说了，时间比较紧，只有一个月)。碰到问题还可以和大家商量一下。
但是AngularJS性能及体积是个问题，尤其是要在手机端运行。

**Angular**: 性能不是问题，体积也还好。但是接触比较少，之前只做过一个Web端的项目。

**React**: 前一段时间刚刚研究过，性能，体积都不是问题。但是同Angular一样，没有实际项目经验啊，出了问题就只能百度了。

经过以上的一番权衡，其实我心里是想用React的。其中一个原因是经过前一段时间的学习，很想通过一个项目积累一些React的开发经验，
另外一个原因是React无论从性能上（得益于Virtual DOM）还是思想上（组件化）都更为先进。

基于以上两点原因，所以在和项目经理确定技术选型的时候，我极力推荐React，虽然可能会存在一些buffer，但是从可持续化发展的角度考虑（自己瞎猜的...），最终项目经理同意了。

### 项目框架搭建
在GitHub上看了很多的boilerplate工程，或多或少都和自己的需求有些出入。
所以，最终自己通过React官方的create-react-app cli搭建了一个seed工程。

**1. 准备环境**

[node.js](https://nodejs.org/en/)

**2. 安装creat-react-app cli**
```
npm install -g create-react-app
```

**3. 创建工程**
```
create-react-app react-seed
```
然后进入项目根目录react-seed安装相关依赖
```
cd react-seed
npm install
```

**4. 暴露配置项**

由于采用create-react-app创建的项目webpack等配置信息都是封装好的，所以为了灵活修改相关配置，可以通过以下命令让封装好的配置文件暴露出来。
```
npm run eject
```

创建好的项目目录如下：
![](/images/posts/react-seed/1-1.png)

**5. 本地服务启动**
```
// 启动本地server用于开发
npm run start
```
将会在本地 3000 端口启动


### redux集成
熟悉React的可能都知道，React是单向数据流，所以有些情况下单单依赖React自身无法实现组件之间的通信，这时就需要[Redux](http://cn.redux.js.org/index.html)登场了。

本文不介绍Redux的语法及思想，只说明如何集成。

首先需要安装Redux相关依赖，由于不同版本之间可能存在兼容性问题，所以安装的时候最好制定版本：
```
npm install redux@3.7.2 --save
npm install redux-thunk@2.1.0 --save
npm install react-redux@5.0.6 --save
```
然后就可以在项目中引入redux了，可以像如下方式组织目录结构：
![](/images/posts/react-seed/1-2.png)

### 路由集成
[react-router](http://react-guide.github.io/react-router-cn/docs/API.html)
```
npm install react-router@3.0.5 --save
```
路由支持嵌套，代码如下：
```
const routes = (
    <Router history={hashHistory}>
        <Route path="/home" component={Layout} onEnter={onEnter}>
            <IndexRoute getComponent={home} onEnter={onEnter}/>
            <Route path="/home" getComponent={home} onEnter={onEnter}/>
            <Route path="/detect" getComponent={detect} onEnter={onEnter}/>
            <Route path="/about" getComponent={about} onEnter={onEnter}/>
        </Route>
        <Route path="/unauthorized" getComponent={unauthorized}/>
        <Redirect from="*" to="/home" />
    </Router>
);

export default routes;
```

### 国际化方案
采用的是常用的[react-intl](https://github.com/yahoo/react-intl/wiki#intro-guides)
```
npm install react-intl@2.4.0 --save
```

### UI组件集成
基于React的UI组件在这里推荐两个，一个是蚂蚁金服的[Ant Design](https://ant.design/)；另外一个是[Material-UI](http://www.material-ui.com)。

两个都很不错，本人使用的是Ant Design。
```
npm install antd@2.13.10 --save
```

### fetch集成
关于请求数据有很多种方式，本人用的是[fetch](https://github.com/github/fetch)。
```
npm install fetch@1.1.0 --save
```
可以简单封装一下，如下：
```
import 'whatwg-fetch'
import loggerService from './logger'

let notAuthorizedCounter = 0;
let fetchService = {
    fetch: (url, method, header, body) => {
        if (!header) {
            header = {}
        }

        return fetchService[method.toLowerCase()](url, header, body).catch(function(exception) {
            loggerService.log('fetchService failed:', exception);

            // token过期，重新获取token并发起请求
            if (exception.code === '401' || exception.code === '403') {
                notAuthorizedCounter++;
                // 最多重试3次
                if (notAuthorizedCounter > 2) {
                    notAuthorizedCounter = 0;
                    loggerService.warn("401 or 403 received. Max attemps reached.");
                    return;
                } else {
                    return fetchService.fetch(url, method, header, body);
                }
            }
        });
    },
    get: (url, header) => {
        return fetch(url, {
            method: 'GET',
            headers: header
        }).then((response) => {
            return response.json();
        });
    },
    post: (url, header, body) => {
        header['Content-Type'] = 'application/json';
        return fetch(url, {
            method: 'POST',
            headers: header,
            body: JSON.stringify(body)
        }).then((response) => {
            return response.json();
        });
    }
};
export default fetchService;
```

### 项目部署
首先对项目进行打包。
```
npm run build
```
可以通过以下命令在本地环境运行打包后的项目。
```
serve -s build
```

注意：如果项目中引入路由的时候使用的是BrowserRouter,
那么当执行npm run build打包之后，本地打开index.html文件，会出现空白页面。

原因是BrowserRouter是用浏览器history对象的方法去请求服务器，
如果服务器没有配置相对应的路由去指向对应的页面，路由会找不到资源。

BrowserRouter会变成类似这样的路径  http://111.230.139.105:3001/detail/9459469，这样的路径在访问服务器时，服务器会认为是请求查找某个接口数据。

所以这时候必须使用HashRouter，这时候访问具体页面时就是http://111.230.139.105:3001/#/detail/9459469



_____________________________________________________________________
项目源码已经开源到github上，喜欢的给个Star支持下吧！（^_^）

[react-seed](https://github.com/lidaguang1989/react-seed.git)