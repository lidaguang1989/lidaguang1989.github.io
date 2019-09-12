---
layout: post
title: 全栈开发博客系统(nodejs + vuejs + mongodb)
date: 2019-09-12
tags: [全栈]
---

本篇文章将会介绍如何使用nodejs+vuejs构建个人博客。

主要分三部分内容：
1. 环境准备
2. 博客后端管理系统（admin）
3. 后端服务（主要提供admin及web端接口）
4. 博客前端展示（web）

### 环境准备
- nodejs
  
    直接去官网下载最新的稳定版就好，以下为下载链接：    
    https://nodejs.org/en/download/
- vue-cli
    
    这是一个强大的构建工具，使用它可以很方便的管理一个vue的项目，并且不需要更多的webpack配置。建议全局安装：
    ```
    npm install --global vue-cli
    ```
    
- mongodb

    后端要用到的数据库。直接去官网下载对应系统的版本就好，注意要下载server版。下载地址：    
    http://downloads.mongodb.com/
    
### 博客后端管理系统

#### 项目创建
首先创建一个基本的vue后端项目，可以使用以下命令：    
```
    vue create admin
```
对于出现的一些选项，直接选择默认就可。
创建好后的目录结构如下：

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e124f895fb0c?w=787&h=387&f=png&s=48189)

#### router集成
使用vue-cli可以很方便的集成路由：
```
vue add router
```

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e11d85d9e500?w=490&h=64&f=png&s=3696)
注意，默认会采用history模式，为了以后方便些，这里要选择(n)即用hash模式，而不是浏览器的history模式。

#### element-ui集成
为了方便，后端的页面我们采用element-ui提供的一些组件实现，所以要把element-ui集成进来：
```
vue add element
```
提示选项，全部选择默认就可。   

好了，到目前为止，后端项目的基本结构就算构建完成了。可以通过以下命令启动：
```
npm run serve
```
启动后的默认页面如下：

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e111a57faf12?w=1351&h=693&f=png&s=59305)

#### 页面基本布局
可以采用element-ui的layout-container，直接把样例代码copy过来放到Home.vue 的template里面就可以。
但是这里有一点要注意，由于在切换左侧菜单的时候，我们希望只有右侧是变动的，左侧应该保持不变，所以这里需要用到`<router-view>`。把右侧变动的部分挂载到`<router-view>`这里。
![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e10759a53d0d?w=623&h=325&f=png&s=30664)

### 新建分类页面
首先新建一个CategoryEdit.vue文件，然后添加路由。
路由这里需要注意，右侧的可变动的页面应该作为Home组件的子路由。
这样，CategoryEdit这个页面的内容就会显示在我们之前定义的`<router-view>`的位置。

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e1d6b28b89ef?w=506&h=177&f=png&s=14487)

好了，这样当我们在浏览器输入"http://localhost:8080/#/categories/create", CategoryEdit页面就会显示出来了。具体的页面内容在这里不介绍了。都是用的element-ui的组件。

由于篇幅有限，其他的页面在这里也不详细一一介绍了，具体源码已开源到[GitHub](https://github.com/lidaguang1989/myblog)。

最终的项目页面结构会是下面这样：

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e20b2c2f77dc?w=1356&h=691&f=png&s=50656)

共分成三部分：分类管理、文章管理及用户管理。

对于页面中使用的接口，会在接下来的server部分介绍。

### 后端服务
这部分采用[nodejs](https://nodejs.org/en/docs/) + [mongodb](https://docs.mongodb.com/)实现。

#### 项目构建
1. 首先在根目录（与admin同级的目录）新建文件夹server。
2. 新建一个package.json文件，可以使用以下命令初始化一个
    ```
    npm init
    ```
    全部选择默认即可。
3. 依赖安装。后端主要会用到以下依赖。
```
    "cors": "^2.8.5",       // 允许跨域请求
    "express": "^4.17.1",   // 后端框架
    "mongoose": "^5.6.12",  // 数据库
    "nodemon": "^1.19.2",   // 当文件变更后会自动重启后端服务
```
4. 以上依赖安装完后新建一个index.js文件。
```
const express = require('express')
const cors = require('cors')
const app = express()

// 允许跨域
app.use(cors())
app.use(express.json())

app.listen('3000', async(req, res) => {
  console.log("http://localhost:3000")
})
```
5. 通过以下命令就可以启动后端服务了：
```
    nodemon start index.js
```

#### 路由定义
新建一个router文件夹，然后新建admin文件夹作为admin端的接口，平级可以再建一个web文件夹作为web端的接口。在admin文件夹下新建index.js文件
```
module.exports = app => {
  const express = require('express')
  const router = express.Router()

  // 获取资源
  router.get('/getData', async (req, res) => {
    res.send("hello world")
  })
  
  app.use('/admin/api/rest', router)
}
```
注意这里导出的是一个函数，这样做有个好处就是可以传递参数进来，这里我们传入了app作为参数。

在根目录的index.js文件中需要引入一下：
```
require('./routers/admin/index')(app) // 直接执行函数并传入app作为参数
```

当在浏览器中输入"http://localhost:3000/admin/api/rest/getData" 时，会看到"hello wrold"。

#### 连接数据库
接下来新建一个文件夹db，然后新建db.js文件:
```
module.exports = app => {
  const mongoose = require('mongoose')

  mongoose.connect('mongodb://localhost:27017/myblog', {
    useNewUrlParser: true
  })
}
```
mongodb安装完后默认会在27017端口启动，myblog是我们给数据库的命名。    



然后在index.js文件中需要引入db.js。
```
require('./db/db')(app) 
```

#### 创建模型
新建models文件夹，在文件夹下新建Category.js作为分类的模型。
```
const mongoose = require('mongoose')

const schema = new mongoose.Schema({
  title: {type: String}
})

module.exports = mongoose.model('Category', schema)
```
暂时只定义一个分类名称 "title"

#### 数据查询
在routers/admin/index.js中通过以下代码就可以查询categories表中的数据。
```
  // 获取分类列表
  router.get('/categories', async (req, res) => {
    const res = await Category.find()
    res.send(res)
  })
```

当在浏览器中输入 "http://localhost:3000/admin/api/rest/categories" 即可得到categories表的数据。

好了，通过以上的介绍，我们应该能够实现一些简单的增删改查操作。

#### 中间件
在开发的过程中，我们一定会遇到一个问题。

前面提到，后端admin主要包括三大模块：分类管理、文章管理、用户管理。

每一个模块都会涉及增删改查操作。如果我们为每一个模块都定义一套自己的增删改查接口，势必会产生很多重复代码，而且如果是分类比较多的情况，重复代码会更加严重。所以这里可以考虑自定义一个中间件。

首先新建一个文件夹middleware，然后新建resource.js文件（我们可以把每个分类都理解为资源），统一对资源进行增删改查操作，唯一的不同是资源名称。
```
module.exports = options => {
  return async (req, res, next) => {
    const inflection = require('inflection')
    const modelName = inflection.classify(req.params.resource)
    req.Model = require(`../models/${modelName}`)
  
    next()
  }
}
```
这里用到了inflection库，需要先安装一下，然后用inflection.classify把传入的参数转为单数形式，作为模型的名称。

然后将routers/admin.index.js改为以下方式
```
  // 获取资源
  router.get('/', async (req, res) => {
    const data = await req.Model.find()
    res.send(data)
  })

  app.use('/admin/api/rest/:resource', resourceMiddleware(), router)
```

好了，由于篇幅有限，其他的一些内容就暂不介绍了，详细代码可以参考[GitHub](https://github.com/lidaguang1989/myblog)。

### 博客前端展示（web）
#### 项目构筑
这部分和admin差不多，新建web项目，需要集成router，但不需要element-ui，具体可以参照上文后端管理系统的项目构筑介绍。

#### 主题选择
主题部分我是直接从[Jekyll Themes](http://jekyllthemes.org/)主题库中选取的一个，由于时间没那么多，再加上是自己做着玩，所以偷个懒。在这里贴出[原作者 Liberxue](https://github.com/Liberxue/liberxue.github.io)。

#### 访问量统计
访问量统计这里用到了一个免费的开源库[不蒜子](http://busuanzi.ibruce.info/)，非常轻量级，使用起来也很简单。

#### 主页面

![](https://user-gold-cdn.xitu.io/2019/9/11/16d1e6fdbe57bddf?w=1366&h=1465&f=png&s=121842)

### 结尾
好了，暂时就先介绍这么多吧，还有很多内容就不一一展开了。如果大家有疑问可以留言。

由于本人是前端出身，做了六七年前端了。后端也是刚刚接触不久，所以上文前端部分介绍的可能会少一些，相对后端会多些，如果有哪些错误的地方欢迎指正。

## 源码已经开源到[GitHub](https://github.com/lidaguang1989/myblog), 如果您觉着对自己还有些帮助，希望能给个[Star](https://github.com/lidaguang1989/myblog)。