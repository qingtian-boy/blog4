---
title: nodejs03
date: 2019-02-26 15:22:56
tags: nodejs
---
# express框架

- 什么是 express ？
  - 基于 Node.js 平台开发的 "web 开发框架" ，就是一个 node.js 模块 
  - web 开发框架：专为web开发而生，让web开发更加简单和高效的一个工具。
  - express的作用：它提供一系列强大的特性，帮助你创建各种 Web 和移动设备应用所需要的功能。
  - express 同时也是 Node.js 的一个模块，也需要通过npm进行安装
- 为什么学习 express 框架？
  - 为了让我们基于 Node.js 开发web应用程序更高效。
- express 官方网站
  - 英文 http://expressjs.com/
  - 中文 http://www.expressjs.com.cn/
- express 网站如何查阅？
  - 带领同学们一起看一下

# express 特点

1. 实现了路由功能    
2. 中间件（函数）功能
3. 对 req 和 res 对象的扩展了其他功能   
4. 可以集成其他模板引擎。如[art-template](http://aui.github.io/art-template/docs/syntax.html)、ejs。默认模板引擎为jade(Jade已更名升级为[Pug](https://www.npmjs.com/package/pug) )
<!-- more -->


# express 基本使用-启动一个web服务器

1. 安装 express

- https://npmjs.com 搜索express，安装。按照[官方文档](http://www.expressjs.com.cn/4x/api.html)一步一步进行
  1. 创建 package.json文件
  2. 安装 express 模块：`npm install express --save `



2. 参考代码,演示hello world

```javascript
//1.加载express
var express = require('express');

//2.得到express实例(类似于创建一个 server 对象）
var app = express();

//3.设计路由，匹配路由/,响应数据
app.get('/',function(req,res){
    //res.end('hello world');
    res.send('hello world'); //默认底层会自动设置编码格式为utf8,所以不用单独设置响应头
     
});

//4.开启服务，监听端口3000
app.listen(3000,function(){
	console.log('请访问localhost:3000');
});

```

1. express创建web服务总结

   1.引入 express 模块
   2.创建 express 实例（一般叫 app ）
   3.设置路由匹配
   4.启动服务

   

2. 注意

- 在 express 中，request 对象 和 response 对象一样使用，同时这两个对象express框架还额外添加了其他好用的功能
- res.send() 是 res.end()的扩展，res之前的方法都是可以用的。
- res.send() 和 res.end() 区别：
  - res.send() 参数可以是 a Buffer object、a String、 an object、 an Array.
  - res.end() 参数类型只能是 Buffer 对象或者是字符串
  - res.send() 会自动发送更多的响应报文头，其中就包括 Content-Type: text/html; charset=utf-8，所以没有乱码
  - res.end() http://www.expressjs.com.cn/4x/api.html#res.end
  - res.send() http://www.expressjs.com.cn/4x/api.html#res.send



# express之request对象

- request对象：http://www.expressjs.com.cn/4x/api.html#req 

# express之response对象

- response对象：http://www.expressjs.com.cn/4x/api.html#res





# res.sendFile读取并响应文件内容

参考地址：http://www.expressjs.com.cn/4x/api.html#res.sendFile

```javascript
//1.引入express框架
var express = require('express');
var fs = require('fs');
var path = require('path');

//2.得到框架express的实例app
var app = express();
//3.定义匹配的路由，处理对应的逻辑
app.get('/',function(req,res){
  //之前的做法：1.先读取文件index.html内容   2. 把内容给res.end响应出去
  fs.readFile(path.join(__dirname,'view/index.html'),'utf8',function(err,data){
    if(err){
      throw err;
    }
    res.send(data);
  })
  
});

app.get('/index',function(req,res){
  //如果是匹配/index,则重定向到路由 '/' 中
  res.redirect('/');
});

//定义get请求，匹配pathname等于/login路由   
app.get('/login',function(req,res){
  fs.readFile(path.join(__dirname,'view/login.html'),'utf8',function(err,data){
    if(err){
      throw err;
    }
    res.send(data);
  })
})

//定义get请求，匹配pathname等于/register路由   
app.get('/register',function(req,res){
 //使用express的res.sendFile方法，读取内容并响应给客户端浏览器
 res.sendFile( path.join(__dirname,'view/register.html') );
})


//4.启动服务
app.listen(3000,function(){
  console.log('请访问....');
})


```





# express 中的路由

## 什么是路由

路由可以理解成根据请求的不同URL，映射到不同处理程序上。

路由语法结构：

```javascript
app.METHOD (PATH, HANDLER)
```

其中：

- `app` 是 `express` 的实例。
- `METHOD` 是 [HTTP 请求方法](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)。
- `PATH` 是请求的路径。是请求url路径中的pathname部分      http://127.0.0.1:3000/register?name=xb&age=18，其中pathname就是==/register==
- `HANDLER` 是在路由匹配时执行的函数。

使用：

```javascript
app.get('/',function(req,res){
	// 匹配路由'/'逻辑写在这里; 
});

app.get('/login',function(req,res){ 
   // 匹配路由'/login'的逻辑写在这里;
});
```

## exprese中路由的分类

- app.get(path,callback)

  若`get请求方式`中的req.url 中的 pathname部分和 `path`完全匹配，则执行callback的回调函数中的逻辑

  ```javascript
  app.get('/index',function(req,res){
      res.send('hello world);
  });
  ```

  其中path也可以定义==正则==来匹配

  ```javascript
  app.get(/^\/index\/\d{2}$/, function (req, res) {
    res.send('hello world');
  });
  ```

  

- app.post(path,callback)

  若`post请求方式`中的req.url 中的 pathname部分和 `path`完全匹配，则执行callback的回调函数中的逻辑

  ```javascript
  app.post('/index',function(req,res){
      res.send('hello world);
  });
  ```

- app.all(path,callback)

  1. 在进行路由匹配的时候不限定方法，什么请求方法都可以
  2. 请求路径中pathname要求与 `path` ==完全匹配==

  ```javascript
  app.all('/index',function(req,res){
  	res.end('hello world');
  });
  
  // http://localhost:9000/index 		匹配
  // http://localhost:9000/index/		匹配
  // http://localhost:9000/index/a/b/c 	不匹配
  // http://localhost:9000/indexa/b/c 	不匹配
  ```

    ==注意==：路径后面斜杠可缺省，都是可以匹配的。





- app.use(path,callback)

  1. 在进行路由匹配的时候不限定方法，什么请求方法都可以
  2. 请求路径中pathname==第一部分==只要与` path` 匹配即可，==并不要求完全匹配==

  ```javascript
  app.use('/index',function(req,res){
  	res.end('hello world');
  });
  
  // http://localhost:9000/index 			匹配
  // http://localhost:9000/index/ 		匹配
  // http://localhost:9000/index/a/b/c 	匹配
  // http://localhost:9000/indexa/b/c 	不匹配
  // 注意：匹配的/index后面必须有斜杠/
  ```





==注意==:app.all和app.use的区别：

- 相同点：不限定请求方法
- 不同点：
  - all,要求pathname和path部分完全匹配
  - use,要求pathname第一部分与path完全匹配即可

​     


## 路由参数绑定

```javascript
// 通过 req.params 获取路由中的参数
app.get('/index/:year/:month/:day', function (req, res) {
  // console.log(req.params);
  res.send(req.params);   
});
```

如下：

![1536482746366](/param.png)



## 获取查询字符串参数

如：

请求地址：http://127.0.0.1:3000/register/2020/01/02?name=zhu

```javascript
app.get('/register/:year/:month/:day',function(req,res){
  console.log(req.params); //获取参数
  console.log("年：",req.params.year); //获取路由参数
  console.log("大名：",req.query.name); //获取？后面的查询字符串参数
  res.send('register');
});
```








# 安装nodemon模块后台运行nodejs服务

作用：可以让nodejs服务在后台运行，只要修改文件，它会实时重启nodejs服务。我们只需刷新浏览器即可

全局安装nodemon（当做命令使用）： 

```\
npm --registry https://registry.npm.taobao.org install nodemon -g
```

启动：

```
nodemon  XXX.js 
```

 

# 给学生扩展git

资料见笔记