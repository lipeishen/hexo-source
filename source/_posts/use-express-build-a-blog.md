title: use Express build a blog
date: 2014-04-02 18:24:21
tags: Express
categories: Express
---
<!--more-->
## 前言
  Npm 提供了大量的第三方模块，其中不乏许多Web框架，我们没有必要重复发明轮子，因而选择使用Express作为开发框架，因为它是目前最稳定、使用最广泛，而且Node.js官方推荐的唯一一个Web开发框架。

 ```
##安装Express
 命令:  npm install  -g  express
-g 说明是在全局模式下安装，这样我们就可以在命令行中使用它。
Express 在初始化一个项目时需要指定模板引擎，默认支持Jade和 Ejs ，为了降低学习难度，我们推荐使用 ejs，但是express 3.X之后就不再默认支持 ejs了，所以安装express时最好装3.0以下的版本。
如果想使用 3.x 的版本
解决方案是
安装一个插件

    npm install express-partials

安装完之后会在node_modules 文件夹里面多出一个文件夹 express-partials
app.js代码里面添加这俩行代码即可运行正常

    partials=require('express-partials');
    app.use(partials());

我们还是再express 3.0 一下开发吧，不用麻烦了。

通过一下命令建立网站基本结构：
`express  -e  ejs  microblog`
完成之后安装依赖
 `cd microblog  && npm install`
它自动安装了ejs 和express 。这是因为在package.json 文件中配好了，所有指定依赖的模块都会自动安装。
package.json 文件中的 配置如下：
 ![package.json](./public/img/QQ1.jpg)
 
接下来，运行 node app.js  就看到 服务器启动了，在浏览器中输入地址 http://localhost:3000  就可以看到express的欢迎界面了。
关闭服务器用 Ctrl+ C 

##MongoDB 数据库的安装和配置

开发过程中使用的数据库是 mongdb ,一种开源的非关系型的数据库，它没有表、行的概念，也咩有固定的模式和结构，数据以文档的形式存储。
安装mongodb 请去 http://www.mongodb.org/ 查看如何安装，也可以按照这篇博客文章去安装 http://blog.csdn.net/yuyunli1989/article/details/16989715

##连接mangodb数据库
装好之后，在package.json 里面增加一行代码：
` ,”mongodb”: “>=0.9.9”`
然后运行`npm install` 跟新依赖的模块。
接下来的工作是创建settings.js 文件，用于保存数据库的连接信息，数据库名为 microblog ，settings.js的内容如下：
`module.exports = {
  cookieSecret: 'microblogbyvoid',
  db: 'microblog',
  host: 'localhost',
};`
其中，db 是数据库的名称，host 是数据库的地址。cookieSecret 用于Cookie加密与数据库无关。
接下来在models 子目录中创建db.js 内容是：

  

`var settings = require('../settings');
var Db = require('mongodb').Db;
var Connection = require('mongodb').Connection;
var Server = require('mongodb').Server;
module.exports = new Db(settings.db, new Server(settings.host, Connection.DEFAULT_PORT, {}));
`以上代码通过module.exports 输出了创建的数据库连接

##会话支持
对于开发者来说，我们无须关心浏览器端的存储，需要关注的仅仅是如何通过这个唯一的标示符来识别用户。Express提供了会话中间件，默认情况是把用户信息存储到内存中，我们已经有了mongodb ，不妨把会话信息存储到数据库中，便于持久维护，为了使用这个功能，我们首先获得一个connect-mongo 的模块，在package.json 中添加一行代码：
`，“connect-mongo”:”>=0.1.7”`
运行npm install 获得模块。

 打开 app.js 添加以下内容：
`var settings = require('./settings');`

  var MongoStore = require('connect-mongo');`

此处输入 app.configure(function(){
  
app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.bodyParser());
  app.use(express.methodOverride());
  app.use(express.cookieParser());
  app.use(express.session({
    secret: settings.cookieSecret,
    store: new MongoStore({
      db: settings.db
    })
  }));
  app.use(express.router(routes));
  app.use(express.static(__dirname + '/public'));
});

其中express.cookieParser() 是Cookie解析的中间件。Express.session() 提供会话支持，设置它的store 参数为MongoStore 实例，把会话信息存储到数据库中，以免丢失。
```
以上就是express框架和mangodb数据库的 常用的配置信息
