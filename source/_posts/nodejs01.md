---
title: nodejs01
date: 2019-02-26 15:04:45
tags: nodejs
---

SPA =  simple  page application (单页应用程序)

nodejs(6天)+Vue项目(8天=语法+项目驱动+nodejs作为服务端提供api数据） 

# nodejs概述 
<!-- more -->
## nodejs介绍

- Node.js 是一个基于 Chrome V8 引擎的 JavaScript `开发平台`，使得JavaScript成为服务端开发语言得以实现。  

- 何为开发平台？有对应的编程语言、有语言运行Runtime时、有能实现特定功能的API（SDK：Software Development Kit）。    apache -php    tomcat  - java     nodejs- js

- 该平台使用的编程语言是 JavaScript 语言

- 基于 node.js 可以开发控制台程序（命令行程序、CLI程序）、桌面应用程序（GUI -借助 node-webkit、electron 等框架实现）、==Web 应用程序（网站）== 

  


node.js 相关网站

- [node.js官方网站](https://nodejs.org/)
- [node.js中文网](http://nodejs.cn/)
- [node.js 中文社区](https://cnodejs.org/)



## node.js 有哪些特点？

1. 事件驱动(当事件被触发时，执行传递过去的回调函数)

2. 单线程 (JavaScript运行机制)

3. 非阻塞 I/O 模型，当执行异步I/O操作时，不会阻塞。（I/O=input/output） (JavaScript运行机制)

   IO:磁盘读写IO,网络IO

4. 拥有世界最大的开源库生态系统 —— npm（Nodejs Package Manager即node包管理器）




## 浏览器、nodejs和其他服务器之间的关系

![1536031822769](/1536031822769.png)





# 安装nodejs

由于Node.js平台是在后端运行JavaScript代码，所以，必须首先在本机安装Node环境 

1. 下载地址

- [当前版本](https://nodejs.org/en/download/)
- [历史版本](https://nodejs.org/en/download/releases/)



2. 官网术语解释

- LTS 版本：Long-term Support 版本，长期支持版，即稳定版。
- Current 版本：Latest Features 版本，最新版本，新特性会在该版本中最先加入。

> 注意：

- 安装完毕后通过命令：`node -v`来查看版本确定是否安装成功,安装成功也内置了npm命令，也可通过`npm -v`查看版本号。

```shell
Microsoft Windows [版本 10.0.17134.345]
(c) 2018 Microsoft Corporation。保留所有权利。

C:\Users\Administrator>node -v
v10.15.0

C:\Users\Administrator>npm -v
6.4.1

```

- 如需要可以配置环境变量。可以让node和npm命令在任意目录都可使用（==安装时默认配置了环境变量==）
  - 步骤：我的计算机->右键属性->高级系统设置->高级->环境变量->path



# 编写nodejs程序

回顾：之前所编写的javascript代码都是在==浏览器中==运行的，因此可以直接在浏览器的控制器console中编写js代码并运行，如下：

![1536041388874](/1536044363749.png)



现在我们编写的javascrip代码都是在==Node环境中==执行，执行方式有两种：

- 方式一：在window命令行环境中输入指令`node`并回车，进行==node的交互式环境==,编写javascript代码执行即可。其中node交互式环境也称之为==REPL==(Read Eval Print Loop-读取评估打印循环 )，按两次`ctrl+c`,可退出REPL环境。



先输入node指令，进入node的交互式环境，在输入js代码。

```powershell
C:\Users\Administrator>node
> 1+1
2
```



- 方式二：把javascript代码写在后缀为==.js==的文件中，如有一个hello.js文件，在window命令行中输入`node hello.js`即可执行。

  

如在`hello.js`中编写以下代码:

```javascript
var a = 1;
var b = 1;
console.log(a+b); 
```

window命令行执行： ==node 文件名.js==    

> 可以按住ctrl+shift+鼠标右键，进入当前目录的cmd命令

```powershell
C:\path>node hello.js
2
```

![1546140459950](/1546140459950.png)



>  思考:之前在浏览器中的的window、history、location、document等对象在node环境还支持吗？

答:不支持了，因为window、history、location等对象是基于浏览器的，而现在我们是基于node环境。

```powershell
# node环境中输入会报错
> history
ReferenceError: history is not defined
> window
ReferenceError: window is not defined
>
```

> 注：在浏览器环境中全局对象是`window`，在node环境中全局对象变为`global`



# 通过nodejs 读写文件

官方文档：http://nodejs.cn/api/fs.html 

- 使用到的文件系统模块`var fs = require('fs');`

- **写文件**：`fs.writeFile(file, data[, options], callback);`

  - 参数1：要写入的文件路径，**必填**。
  - 参数2：要写入的数据，**必填**。
  - 参数3：写入文件时的选项，比如：文件编码，选填。
  - 参数4：文件写入完毕后的回调函数，**必填**。
  - 写文件注意：
    - 该操作采用==异步==执行
    - 如果文件存在则替换原内容
    - 默认写入的文件编码为utf8
    - 回调函数有1个参数：err，表示在写入文件的操作过程中是否出错了。
      - 如果出错了`err != null`，否则 `err === null`

- **读文件**：`fs.readFile(file[, options], callback)`

  - 参数1：要读取的文件路径，**必填**。
  - 参数2：读取文件时的选项，比如：文件编码utf8。选填。
  - 参数3：文件读取完毕后的回调函数，**必填**。
  - 读文件注意：
    - 该操作采用==异步==执行
    - 回调函数有两个参数，分别是err和data
    - 如果读取文件时没有指定编码，那么返回的将是二进制数据，如果指定了编码如utf8，那么会根据指定的编码返回对应的字符串数据。

  

写文件：

```javascript
// --------------------------------- 写文件 -----------------------------

//导入内置文件模块fs(file System)
var fs = require('fs');
//定义一个字符串
var str = 'hello world 你好';
//写入文件（文件不存在则自动创建）
fs.writeFile('data.txt',str,'utf8',function(err){
	console.log(err); // 成功返回null
	if(err){
        //抛出错误信息
		throw err; 
	}
	console.log('写入ok');
});
```

> ==注==：writeFile写入文件是先把文件内清空在写入，如果要追加写入的话可以使用`appendFile`函数



读文件：

```javascript
var fs = require('fs');
fs.readFile('./data.txt','utf8',function(err,data){
	if(err){
         console.log(err); //打印错误信息
		throw err; //抛出异常
	}
	//打印读取到的文件数据 
	console.log(data);
});
```



 ==注意==

1. 异步操作无法通过 try-catch 来捕获异常，只能通过回调函数的 err参数 来判断是否出错。

2. 只有同步操作可以通过 try-catch 来捕获异常。

3. 不要使用 `fs.existsSync(path, callback)` 来判断文件是否存在，直接判断 err 即可

4. 文件操作时的路径问题

   - ==__dirname==, 表示的永远是"当前被执行的js的`所在目录`"
   - __filename, 表示的是"被执行的js的完成文件名（含路径)"

5. path模块的join方法

   - `path.join()` 方法使用平台特定的分隔符把全部给定的 `path` 片段连接到一起，并==规范化生成的路径==。 

      ```javascript
      var path = require('path');
      console.log( path.join(__dirname+'direction','111.txt') );
      ```

   

5. 错误优先。nodejs规定，异步回调函数，若有错误对象err，其作为异步回调函数的第一个参数。



# 小结

- 异步读取文件 ：fs.readFile

- 异步写入数据到文件： fs.writeFile  

- 异步写入数据追加到文件中： fs.appendFile

- 异步和同步函数的区别：

  ​	-异步函数： 必定需要传入一个对应的==回调函数==来处理对应的成功业务逻辑
         - 同步函数：同步直接接收函数的返回结果





# 通过node.js 编写服务器程序 

步骤：

1. 加载http模块
2. 创建http服务，
3. 服务端对象监听request 请求事件，用于监听客户端的请求
4. 启动http服务，监听端口

参考代码：

```javascript
// 1. 加载http模块
var http = require('http');

// 2. 创建http服务，设置监听request 请求事件的处理程序
var server = http.createServer();

// 3. 服务端对象监听request 请求事件，用于监听客户端的请求
server.on('request',function (req, res) {
  //req-请求对象 , res-响应对象
  //处理客户端请求逻辑
  console.log('有人请求了~~');
  res.end(); //必须结束响应
});

// 4. 启动http服务，开始监听3000端口
server.listen(3000, function () {
  console.log('服务已经启动，请访问： http://localhost:3000');
});
```



> 注意：


1. 在监听request事件中，最后一定要`res.end()`结束响应。

2. 浏览器显示中文可能是乱码，需设置响应头告诉浏览器显示时所使用的编码，要在==res.end()之前设置==

   ```javascript
   res.setHeader("Content-type","text/plain;charset=utf-8"); #响应为纯文本
   res.setHeader("Content-type","text/html;charset=utf-8");  #响应为html格式，能够解析html标签
   ```

3. Chrome 浏览器默认无法手动设置编码，需要安装"Set Character Encoding"扩展。



# http.createServer创建http服务

文档：http://nodejs.cn/api/http.html#http_http_createserver_options_requestlistener

语法：

```
http.createServer([options][, requestListener])
```

- 第二个参数`requestListener`，是一个自动添加到[`'request'`](http://nodejs.cn/s/2qCn57)事件的方法。


  成功返回一个新的 [`http.Server`](http://nodejs.cn/s/jLiRTh)实例。



之前代码是使用==server.on('request',callback)==来监听请求事件，由于http.createServer第二个参数也是个request监听请求事件。所以可以直接把request的请求事件的监听函数callback传递给==http.createServer的第二个参数==即可。

代码如下：

```javascript
// 1. 加载http模块
var http = require('http');

//2,3步骤写在一起，可直接把request请求事件的回调函数直接定义在createServer函数中
var server = http.createServer(function(req,res){
	console.log('有人请求了');
	res.end('hello world');
})


// 4. 启动http服务，开始监听3000端口
server.listen(3000, function () {
  console.log('服务已经启动，请访问： http://localhost:3000');
});
```



# request请求和response响应对象



一个网站的请求和响应过程:

![](client-server.png)

具体请求和响应的信息：

![](/HTTPMsgStructure2.png)



http模块创建服务：

```javascript
http.createServer([options][, requestListener])
```

其中`requestListener`,就是监听request请求事件的回调函数，回调函数有两个参数==req==和==res==：

```javascript
// 1. 加载http模块
var http = require('http');

//2,3步骤写在一起，可直接把request请求事件的回调函数直接定义在createServer函数中
var server = http.createServer(function(req,res){
	res.end('hello nodejs');
})

// 4.启动http服务，监听3000端口
server.listen(3000, function () {
  console.log('服务已经启动，请访问： http://localhost:3000');
});
```



文档：http://nodejs.cn/api/http.html#http_event_request

- req： 为请求对象，属于类型 ==<http.IncomingMessage>==，会把客户端请求过来的各种信息，封装在此对象中，如：
  - `req.method`：获取http请求方法
  - `req.url`：获取完整的url请求路径

- res： 为响应对象，对象类型 ==<http.ServerResponse>==，把需要响应的各种信息设置此在对象中即可，如：
  - `res.statusCode`:设置响应状态码
  - `res.statusMessage` :设置响应状态码对应的信息
  - `res.setHeader()`：设置响应头 
  - 同时设置上面三者信息：

  ```
  response.writeHead(statusCode[, statusMessage][, headers])
  ```

  writeHead的使用注意事项:http://nodejs.cn/api/http.html#http_response_writehead_statuscode_statusmessage_headers

  - 响应的数据`res.write('content')`
  - 结束响应`res.end()`，一个请求必须要结束，否则浏览器会被挂起。










#  创建http服务实现不同请求，响应不同内容

## 需求说明

不同的url响应不同的内容：

- 请求 / 或 /index，输出index内容
- 请求 /login，输出login内容
- 请求 /register，输出register内容

## 参考代码

```javascript
// 1.引入http服务模块
var http = require('http');
// 2.创建服务，设置监听request事件的回调函数
var server = http.createServer(function(req, res) {
	req.url = req.url.toLowerCase(); //把url转为小写，在赋值给req.url属性
    if (req.url == '/' || req.url == '/index') {
    	res.end('index');
    }else if(req.url == '/login' ){
    	res.end('login');
    }else if(req.url == '/register' ){
    	res.end('register');
    }else{
    	res.end('404 Not Found');
    }
});
// 3.启动http服务，监听3000端口
server.listen(3000, function() {
    console.log('请访问http://localhost:3000');
});

```

启动服务,输入http://localhost:3000/login 会响应login内容。



作业：随便找几个nodejs内置的模块，找到对应的方法进行练习：

==推荐看以下几个模块==：

- fs
- path
- queryString 查询字符串模块
- url 处理url的模块
- http    req和res对象











# 模拟 Apache 实现静态资源服务器，响应html内容

>  这里我们使用http模块模拟 `Apache服务器 `响应静态资源（html、img、css、js、...）响应给用户。



步骤：

1. 创建view视图目录，在里面创建index.html、login.html、register.html文件。 
2. 创建public资源目录，再去public中创建image、css、js目录存放静态资源

   3.创建web_server.js文件，参考代码如下：

```javascript
// 1.引入http服务模块
var http = require('http');
//加载文件模块
var fs = require('fs');
//加载path模块，处理路径问题
var path = require('path');

// 2.创建服务，监听request事件的回调函数
var server = http.createServer(function (req, res) {
  //获取url路径并转化为小写
  req.url = req.url.toLowerCase();
  if (req.url == '/' || req.url == '/index') {
    fs.readFile(path.join(__dirname, 'view', 'index.html'), function (err, data) {
      if (err) {
        throw err;
      }
      res.setHeader('Content-type', "text/html;charset=utf-8");
      res.end(data);
    });

  } else if (req.url == '/login') {
    fs.readFile(path.join(__dirname, 'view', 'login.html'), function (err, data) {
      if (err) {
        throw err;
      }
      res.setHeader('Content-type', "text/html;charset=utf-8");
      res.end(data);
    });
  } else if (req.url == '/register') {
    fs.readFile(path.join(__dirname, 'view', 'register.html'), function (err, data) {
      if (err) {
        throw err;
      }
      res.setHeader('Content-type', "text/html;charset=utf-8");
      res.end(data);
    });
  } else if (req.url == '/public/css/style.css') {
    fs.readFile(path.join(__dirname, 'public/css/', 'style.css'), function (err, data) {
      if (err) {
        throw err;
      }
      //css文件的mime类型是 image/css,默认浏览器可以解析，不用指定
      //res.setHeader('Content-type', "text/css");
      res.end(data);
    });
  }else if (req.url == '/public/js/index.js') {
    fs.readFile(path.join(__dirname, 'public/js/', 'index.js'), function (err, data) {
      if (err) {
        throw err;
      }
      //js文件的mime类型是 application/x-javascript,默认浏览器可以解析，不用指定
      //res.setHeader('Content-type', "application/x-javascript");
      res.end(data);
    });
  }else if (req.url == '/public/images/zhilin.jpg') {
    fs.readFile(path.join(__dirname, 'public/images/', 'zhilin.jpg'), function (err, data) {
      if (err) {
        throw err;
      }
      //image文件的mime类型默认浏览器可以解析，不用指定
      res.end(data);
    });
  } else {
    res.writeHead(404, 'Not Found');
    res.end('404 Not Found');
  }
});

// 3.监听3000端口
server.listen(3000, function () {
  console.log('请访问http://localhost:3000');
});
```

补充知识点：

1. path 模块的 join() 方法



> 注意：

- 1、注意在发送不同类型的文件时，要设置好对应的`Content-Type`，其中js、css、image浏览时默认可以识别
  - [Content-Type参考 OSChina](http://tool.oschina.net/commons)
- 2、HTTP状态码参考
  - [w3org参考](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
  - [菜鸟教程参考](http://www.runoob.com/http/http-status-codes.html)
- 3、在html页面中写相对路径'./' 和 绝对路径 '/'的含义 。
  - 网页中的这个路径主要是告诉浏览器向哪个地址发起请求用的
  - './' 表示本次请求从相对于当前页面的请求路径（即服务器返回当前页面时的请求路径）开始
  - '/' 表示请求从根目录开始



- 



# node.js 错误调试

1. 当开启服务后，在浏览器中输入地址，如果出现浏览问题，首先要先看 服务器控制台是否报错。如果报错，直接根据服务器报错信息进行排错。
2. 打开浏览器开发者工具中的 “网络” 部分，即network,查看请求是否成功发出去了
3. 看一下请求报文是不是和我们想的一样
4. 查看响应状态码