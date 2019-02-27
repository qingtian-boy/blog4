---
title: nodejs02
date: 2019-02-26 15:16:35
tags: nodejs
---

# 模拟 Apache 实现静态资源服务器，响应html内容

> 这里我们使用http模块模拟 `Apache服务器 `响应静态资源（html、img、css、js、...）响应给用户。



需求：

请求 / 或 /index, 展示对应index.html的内容

请求  /login展示对应login.html的内容

请求 / 或 /register, 展示对应register.html的内容



<!-- more -->

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







# 获取get请求参数   



get方式的请求参数，参数是追加在请求url地址?的后面   

？后面的参数叫做==查询字符串==

```
http://localhost:3000/index?id=66&name=zhangsan
```

通过`req.url`就可以获取上面路径信息，结果如下：

```
 /index?id=66&name=zhangsan
```

不过上面路径`/index`和参数`name=zhangsan`是在一个字符串中，不便于获取参数，这是我们可以借助nodejs的内置==url模块==.

### url模块

- 官网地址：http://nodejs.cn/api/url.html

- url模块可以很方便地解析用户请求的url中的参数

- 具体使用

  加载模块 `var url = require('url');`

  调用`parse()`方法解析url地址

```javascript
// 语法：url.parse(urlString[, parseQueryString[, slashesDenoteHost]]);
var url = url.parse(req.url, true); // 设置true可以把参数初始化为json对象格式，否则是查询字符串格式
console.log(url);
```

上面打印的结果：

```java
Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '?id=66&name=zhangsan',
  query: { id: '66', name: 'zhangsan' },
  pathname: '/index',
  path: '/index?id=66&name=zhangsan',
  href: '/index?id=66&name=zhangsan' 
}
```

url各属性对应的路径位置：

```javascript
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                            href                                             │
├──────────┬──┬─────────────────────┬─────────────────────┬───────────────────────────┬───────┤
│ protocol │  │        auth         │        host         │           path            │ hash  │
│          │  │                     ├──────────────┬──────┼──────────┬────────────────┤       │
│          │  │                     │   hostname   │ port │ pathname │     search     │       │
│          │  │                     │              │      │          ├─┬──────────────┤       │
│          │  │                     │              │      │          │ │    query     │       │
"  https:   //    user   :   pass   @ sub.host.com : 8080   /p/a/t/h  ?  query=string   #hash "
│          │  │          │          │   hostname   │ port │          │                │       │
│          │  │          │          ├──────────────┴──────┤          │                │       │
│ protocol │  │ username │ password │        host         │          │                │       │
├──────────┴──┼──────────┴──────────┼─────────────────────┤          │                │       │
│   origin    │                     │       origin        │ pathname │     search     │ hash  │
├─────────────┴─────────────────────┴─────────────────────┴──────────┴────────────────┴───────┤
│                                            href                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
(请忽略字符串中的空格，它们只是为了排版需要)
```

- 重点掌握==pathname==和==query==两部分
  - pathname：请求的路径
  - query：请求的参数
- url对象的pathname属性，获取不包含请求字符串的url
- url对象的query属性中包含的就是查询字符串的键值对对象，也就是我们所需要的get请求参数



# queryString模块

具体使用：

```javascript
//加载模块
const querystring = require('querystring');
// 把 id=66&name=zhangan 查询字符串，转换为一个 json 对象
postBody = querystring.parse('id=66&name=zhangan');
console.log(postBody);

```

结果：

```json
{
  id: '66',
  name: 'zhangan'
}
```













# Buffer类型

## Buffer类型介绍



1. **什么是Buffer**

- Buffer也叫缓冲，主要用来解决进行`数据传输`的，存储数据的格式是`字节数组`
- 当我们要把一大块数据从一个地方传输到另外一个地方的时候可以通过 Buffer 对象进行传输，通过 Buffer 每次可以传输小部分数据，直到所有数据都传输完毕。



2. **Buffer类型介绍**

- JavaScript 语言没有读取或操作二进制数据流的机制。

- Buffer 类型的对象类似于==字节数组==，但 Buffer 的大小是固定的、且在 V8 堆外分配物理内存。 Buffer 的大小在被创建时确定，且无法调整。（ buf.length 是固定的，不允许修改 ）

- Buffer 是全局的，所以使用的时候无需 require() 的方式来加载

  

- 看一下什么是 Buffer? 什么是 Stream（流），看图解

  Buffer:

  ![](/Buffer.png)

  Stream:

  ![](/Stream.png)

  

## Buffer类型基本操作

常见的 API 介绍

1. Buffer 对象和字符串互相转换

```javascript
// 1. 通过 Buffer.from() 创建一个 Buffer 对象
//  创建一个字节数组
var array = [0x68, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0xe4, 0xb8, 0x96, 0xe7, 0x95, 0x8c];
//  把字节数组变为Buffer 对象
var buf = Buffer.from(array);
//  把buffer对象变为可读的utf8编码的字符串格式
console.log(buf.toString('utf8')); // hello 世界

// 2 通过字符串来创建一个 Buffer 对象
// Buffer.from(string[, encoding]) 
var buf = Buffer.from('你好世界！ Hello World!');
console.log(buf); // <Buffer e4 bd a0 e5 a5 bd e4 b8 96 e7 95 8c ef bc 81 20 48 65 6c 6c 6f 20 57 6f 72 6c 64 21>
console.log(buf.toString('utf8')); 

```

2. 拼接多个 Buffer 对象为一个Buffer对象

```javascript
var buf1 =Buffer.from([0x68, 0x65]); // he
var buf2 =Buffer.from([0x6c, 0x6c,]); // ll
var buf3 =Buffer.from([0xe7, 0x95, 0x8c])// 界

var buf = Buffer.concat([ buf1,buf2,buf3 ]);
console.log(buf.toString('utf8'));
```



3. 获取字符串对应编码的字节个数

```javascript
// Buffer.byteLength(string[, encoding])

var len = Buffer.byteLength('你好世界Hello', 'utf8');
console.log(len); // 17
```

注意：在utf8编码下，一个中文占==3==个字节

# 获取post请求参数

分析：

1. 获取用户 post 提交的数据

- 因为 post 提交数据的时候，数据量可能比较大，所以会分多次进行传输，每次传输一部分数据，直到接收所有数据
- 此时要想在服务器中获取用户提交的数据，就必须监听 req对象 的 ==data 事件==（浏览器每传输一部分数据到服务器就会触发一次 data 事件）
- 那么，什么时候才表示浏览器把所有数据都提交到服务器了呢？就是当 req对象的 ==end 事件==被触发的时候。此时才代表post数据传输完毕。



1. 核心代码

```javascript
var http = require("http");
var fs = require('fs');
var path = require('path');
//引入查询字符串的模块querystirng
var querystring = require("querystring");

var server = http.createServer((req,res)=>{
  req.url = req.url.toLowerCase();
  req.method = req.method.toLowerCase();
  //判断是否是/login且是get请求
  if(req.url == '/login' && req.method == 'get'){
    //读取静态文件login.html并响应
    fs.readFile(path.join(__dirname,'view','login.html'),'utf8',(err,data)=>{
      if(err){
        throw err;
      }
      res.end(data);
    })
  }else if(req.url == '/login' && req.method == 'post'){
    //由于post提交的数据较大，每次只会提交过来一部分数据都会触发req对象的data事件
    //只有数据全部传输完毕之后，会触发req对象的end事件
    var arr = []; // 用于存储chunk
    req.on('data',(chunk)=>{
      //chunk是个buffer对象
      // console.log('data');
      //把chunk存入到数组arr中，
      arr.push(chunk);
    });
    req.on('end',()=>{
      //需要把data事件中的buffer对象数据转化为utf8格式数据
      // console.log('end');
      //把多个小buffer对象拼接成一个大的buffer对象
      var buf = Buffer.concat(arr);
      var postData = buf.toString('utf8');
      //console.log(postData); // 返回的是一个查询字符串的格式username=admin&password=123456
      console.log( querystring.parse(postData) ); //{ username: 'adminadmin', password: '12345678' }
    });

    res.end('post');
  }
});

server.listen(3000,()=>{
  console.log("请访问http://127.0.0.1:3000");
})
// http path fs url=>req.url   querystring=>name=dabai&age=24

//获取post参数
//1.监听req对象的data事件，在此事件中把每次传输过来的chunk对象（buffer对象），存进数组中
//2.监听req对象的end事件，此事件只要被触发，说明post数据已经接收完毕，此时，我们把数组中的buffer对象连接成一个大的buffer对象
//3.把大的buffer对象调用toSting(),返回的是查询字符串的格式，
//最后借助querystirng可以把查询字符串变为一个json格式的对象，方便获取数据
```

==注意==：把上面的 id=66&name=zhangan 查询字符串，转换为一个 json 对象。这里需要使用到一个nodejs内置的`querystring`内置模块





# NPM - Node Package Manager - Node 包管理器



## NPM介绍



**1. NPM 是什么**？

- npm（全称Node Package Manager，即node包管理器）。

- npm是Node.js默认的软件包管理器件。安装完毕node后，会默认安装npm

- npm本身也是基于Node.js开发的包

- 查看当前npm版本：`npm -v`

- 更新全局的npm版本：`npm install npm@latest -g`   

- [npm 官方网站](https://www.npmjs.com/)，这是NPM 包管理库，存储了大量的JavaScript代码库

- [npm 官方文档](https://docs.npmjs.com/)，可以查看npm命令的具体使用

  

## NPM 使用

基本步骤：

1. 在 https://www.npmjs.com/ 网站找到需要的包，进行下载
2. 在项目的根目录下，执行`npm install 包名`安装
3. 注意：通过`npm install 包名`本地安装的包，会自动下载到当前目录下的`node_modules`目录下，如果该目录不存在，则创建，如果已存在则直接把包下载进去。
4. 在代码中通过 `require('模块');` 会自动从node_modules目录中找到并加载该模块  







## NPM常用命令介绍

- npm安装模块

npm install xxx ：利用 npm 安装xxx模块到当前命令行所在node_modules目录；
npm install  xxx -g：利用npm安装全局模块xxx；（当前全局安装后在代码中就不可以通过require进行引入，安装的模块，仅能当做命令使用。若要在项目中使用，一定要本地安装） 

- 本地安装时将模块写入package.json中：

npm install xxx：安装但不写入package.json；  只会产生package-lock.json  ,相当于记录了所有包的下载地址，下次安装同样的包时不用找地址，可提供下载速度。 
npm install xxx –-save ：安装并写入package.json中的dependencies==生产环境==依赖中；==其中--save等价于-S==
npm install xxx –-save-dev：安装并写入package.json中的devDependencies开发环境依赖中。==其中--save-dev等价于-D==

npm install --production : 只安装生产环境dependencies指定的依赖，而不安装devDependencies

安装包的时候指定版本号： npm instal XXX==@版本号== 



- npm 删除模块

npm uninstall xxx：删除xxx模块； 

npm uninstall  xxx -g：删除全局模块xxx； 

------

## NPM 全局安装包

1. **什么是 npm 全局安装**？

- -g：global 全局
- ` install 包名 -g` npm 全局安装指的是把包安装成了一个命令行工具。
- 且全局安装的包不可以在代码中通过require() 的方式来加载。==仅能当作一个全局的命令来使用==

```javascript
  // 通过npm全局安装mime
  npm install mime -g

  //安装完毕后可以在window命令行中直接使用
  mime a.txt 命令来查看对应的结果
```



2. **npm 全局安装实际做了2件事**：

- 下载包到一个指定的目录`C:\Users\username(Administrator)\AppData\Roaming\npm\node_modules`
- 创建一段命令行执行的代码。` C:\Users\username\AppData\Roaming\npm\mime -> C:\Users\username\AppData\Roaming\npm\node_modules\mime\cli.js`

==注意==：上面的目录，每个同学可能存储的位置不一样。

所以，全局安装只是为了可以==当做命令使用==仅此而已，若需在项目中使用还需本地项目安装



创建自己的npm包： https://blog.csdn.net/Marksinoberg/article/details/73196164





## 可设置全局安装的包目录和缓存目录

默认安装的包和缓存存在以下目录：

C:\Users\Administrator\AppData\Roaming 



再重新设置默认的缓存和全局安装的模块目录,不设置默认是c盘

```
npm config set cache d:/nodejs/node_cache     // node_cache需要手工创建
npm config set prefix  d:/nodejs/node_global   // node_global需要手工创建
//最好设置好检查一下
npm config get cache
npm config get prefix
```

设置全局安装模块的路径到环境变量，目的，让全局安装的模块命令可用

修改系统环境变量的path选项，把上面d:/nodejs/node_global设置进去

![1546241216030](/1546241216030.png)



# package.json 文件



## package.json 文件的作用？

1. package.json 文件是一个包说明文件（项目描述文件），用来管理组织一个项目所使用的包。
2. package.json 文件是一个 json 格式的文件，==注意==：json格式的文件时不可以写注释。
3. 位于当前项目的根目录下,和node_modules目录平级

## package.json 文件中常见的选项

1.**常见项**

- name: 包的名称，由小写英文字母、数字和下划线组成，不能包含空格。

- description：包的简要说明。

- author：包的作者

- version：符合语义化版本识别规范的版本

- keywords：项目关键字，数组格式，通常用于搜索。

- maintainers：维护者数组，每个元素包含name、email（可选）、web（可选）字段。

- contributors：贡献者数组，格式与maintainers相同。包的作者应该是贡献者数组的第一个元素。

- licenses：许可证数组，每个元素要包含type（许可证的名称）和url（链接到许可证文本的地址）字段。

  软件开源协议：https://www.oschina.net/news/74999/how-to-choose-a-license 

- repositories：仓库托管地址数组，每个元素要包含type（仓库的类型，如git）、url（仓库的地址）和path（相对于仓库的路径，可选）字段。

- ==dependencies==：记录==线上环境项目所依赖的包==，由包名称和版本号组成 。 npm install XXX --save

- ==devDependencies==：记录==开发环境项目所依赖的包==，由包名称和版本号组成，一般用于记录开发时所使用的第三方插件或工具包。 npm install XXX --save-dev

- ==main==： main属性指定了程序的主==入口文件==。意思是，如果你的模块被命名为foo，用户安装了这个模块并通过require("foo")来使用这个模块，那么require返回的内容就是main属性指定的文件中 通过module.exports暴露出来的对象。 如果没有main属性，会默认把index.js作为包的入口文件。

  

  

2. **创建一个 package.json 文件**

- 手动创建一个
- 通过 `npm init` 命令根据引导创建 package.json 文件。
- 或直接通过 `npm init -y` 或 `npm init -yes`  命令直接创建一个包含默认信息的package.json文件



==注意==

- 通过 `npm init -y`  创建 package.json 文件时，执行命令所在的文件夹如果包含中文等特殊字符，会直接报错。(高版本的node是不会报错)

  

package.json文件说明：

- [package.json](https://docs.npmjs.com/files/package.json)
- [Using a package.json](https://docs.npmjs.com/getting-started/using-a-package.json)

其中[package.json](https://docs.npmjs.com/files/package.json)文件中的script段是可用于设置在cmd中执行的命令

如：定义一个命令

```json
{
    "scripts":{
        "dev"："echo hello world"
    }
}
```

可直接在window命令行中执行上面的dev执行：

```
npm run dev
```





# 淘宝NPM源的使用



由于npm 命令安装的包。其所在的服务器都在国外，安装较慢,可以临时切换到淘宝镜像进行下载，可提高下载速度。



> #临时使用

```
npm --registry https://registry.npm.taobao.org install  xxx
```

可参考：https://www.cnblogs.com/dahe1989/p/7068046.html









# 模块（Modules）和包（Packages）的关系



Nodejs 中除了它自己提供的核心模块外如http、fs，我们也可以==自定义模块==，也可以使用npm安装的第三方的模块。Nodejs 中**第三方模块由包组成**，可以通过包来对一组具有相互依赖关系的模块进行统一管理。 

包和模块的关系：

![1541692505245](/1541692505245.png)

==注意==：
- 包含众多模块

- 包的说明文件-package.json 

  - package.json必须在包的顶层目录下，该文件描述了此包依赖哪些模块等信息，和node_modules平级

  - [==重点==]Node.js在require加载非内置模块，会首先检查包中package.json文件的==main字段指定的js文件==，将其作为包的程序入口文件进行加载，如main字段不存在，会尝试寻找==index.js==作为包的默认入口文件。 

    ```javascript
    var mime = require('mime');
    console.log(mime);
    //require加载第三方包的顺序：
    //1.先去node_moduels里面找同名的包
    //2.找到包下面名为package.json文件，
    //3找到main属性指定的js入口文件进行加载，如果main属性不存在，则会默认找到index.js作为require的入口文件
    ```

  - 同时package.json满足CommonJS规范用来描述包的的有关信息。

  CommonJS规范、AMD规范、CMD规范介绍

  参考链接：https://www.cnblogs.com/chenguangliang/p/5856701.html 




# 模块

## 什么是模块

一个文件就是一个模块，如index.js、config.json都算是模块 

**模块化的好处**：

- 一个模块就是一个各自的作用域。模块和模块之间不会出现变量"污染"
- 模块化开发效率高、可维护性好
- 模块化可以做到职责分离，每个模块实现一个独立的功能



**Node中模块分类**：

- ==核心模块==（Nodejs内置模块）  
  - Node内置提供的模块，包括http、fs、path、url、queryString等.,他们在Node 进程启动时就直接加载进内存。
  - 核心模块拥有最高的加载优先级，如果有模块与其命名冲突，Node.js总是加载核心模块
- ==文件模块== （自定义模块）
  - 用户自己编写的模块如config.js，或config.json。运行时动态加载，需要完整经历`路径分析`、`文件定位`、`编译执行` 
- ==第三方模块==
  - 通过npm install命令进行安装的，会把下载的包直接放到node_modules目录中。






## 文件模块（自定义模块）的加载顺序 

1. 载入文件模块，一般使用的是相对路径（也可以用绝对路径）

```javascript
var myMod = require('./a')  //js的后缀可以省略,建议加上
```

文件模块如果不显式指定文件模块扩展名，则会按照.js 、 后.json的扩展名顺序检查模块是否存在 。

- 所有的模块加载都是采用`require('模块名')`进行加载

- 无论是 核心模块 还是 文件模块，第一次加载后都会缓存起来，第二次加载（第二次require）的时候直接从缓存中读取即可。即多次require同一个模块，只会加载执行一次

- 文件模块可以通过 require 指定路径的方式来加载（路径可以是文件路径 或 目录）

  `require('./a/b.js')` 通过指定相对路径来加载模块

  `require('/a/b.js')` 或 `require('c:\a\b.js')` 通过指定绝对路径来加载 

  > 注意：`require`加载模块的时候，相对路径永远相对于当前模块

加载顺序：

1. 先找当前目录下同名的==a.js==，如果找不到再找同名的==a.json==文件。若两者同时存在则 .js文件优先级更高。

==注意==：若不指定路径则会找node内置模块。找不到再去找npm安装的模块，==所以加载自定义模块一定要加路径==。



# 导出模块module.exports 

- Node中require函数中存在一个module对象，其拥有一个exports属性。

- 每个模块可以独立于一个环境,此模块被require加载时，Node.js 会使用一个如下的==函数包装器==（自执行函数）将其包装： 

  

- ==require函数==加载模块==核心结构代码==：

```javascript
function require(...) {
 	var module = {
     	 exports:{}
    };
     // 函数包装器(自执行匿名函数)
     (function () {
        
         //我们的模块代码实际上是写在这里
         // 我们只需要把我们自己写的模块的代码挂载到module.exports对象身上。
        function add(a,b){
          return a+b;
        }
         
        module.exports = add;
     })();
    	
      //return暴露给外部，供外部使用
      return module.exports; 
}

```

可看出==module.exports== 就是 require()函数加载模块时的返回值，所以我们只需要往其身上挂载一些东西。最终会return返回。从而得到我们所挂载的东西。



如：定义一个数据库配置文件dbconfig.js模块

```javascript
var config = {
  "host":'127.0.0.1',
  "port":'3306',
  "username":'root',
  "password":'123456'
};

//导出模块代码
module.exports = config;
```

require加载上面的模块：

```javascript
var dbconfig = require('./dbconfig.js');
console.log(dbconfig);
console.log(dbconfig.username); // root
console.log(dbconfig.password); // 123456
```



# 小结

- 创建web服务器，响应静态资源（html、css、js、image）,在nodejs中都要匹配静态资源的url，读取内容在响应。

- 获取get参数：

  ```
  http://127.0.0.1:3000/login?username=dachui&age=18
  //1.获取地址 req.url /login?username=dachui&age=18
  //2.借助url模块，把url地址解析成一个对象。 var myurl = url.parse(req.url，true); 
  //3.两个重要属性：myurl.pathname 为 /login .  myurl.query 为{"username":dachui,"age":18}   
  
  ```

- 接收post参数

  - 由于post传递的数据比较大，分批传输

  - 每次传输一部分数据，都会触发req对象data事件

  - 当req对象的end事件被执行了，说明post数据就代表接收完毕

    - end事情做的事情： 把在data事件中buffer拼接一个大的buffer对象，最后把大的buffer变为一个查询字符串格式  username=admin&password=123456 
    - 借助querystring模块把查询字符串变为对象

    

- npm命令

  - npm install  XX@版本  --save  安装生产环境依赖
  - npm install  XX  --save-dev  安装开发环境依赖

- npm全局安装

  - 全局安装仅仅是当做一个全局的命令去使用
  - 全局安装的模块，在项目中不可以通过require进行加载。

- 临时切换到淘宝镜像进行下载

  - npm --registry https://registry.npm.taobao.org  install mime --save

- 模块的分类

  - 内置模块（http、fs、url、path、querystring）

  - 自定义模块（dbconfig.js），要注意加载的机制，加载自定义模块，==必须指定模块的路径==

  - 第三方模块（通过npm下载），把包下载node_modules目录里面

    ```
    var mime = require('mime'); 
    //第三方加载模块的加载机制
    //1.会去node_modules目录中去找与模块mime同名的目录
    //2.进入到mime目录，找到pageage.json文件main属性指定的js文件做为包的入口文件进行加载。
    //3.如果没有找到main属性，则会以index.js文件做为包的入口文件
    ```

- 暴露自定义模块  

  ```
  module.exports = 需要暴露的成员。
  ```

  







# Common System Errors - 常见错误号

err.code有以下几个值：

- EACCES (Permission denied)
  - An attempt was made to access a file in a way forbidden by its file access permissions.
  - 访问被拒绝
- EADDRINUSE (Address already in use)
  - An attempt to bind a server (net, http, or https) to a local address failed due to another server on the local system already occupying that address.
  - 地址正在被使用（比如：端口号备占用）
- EEXIST (File exists)
  - An existing file was the target of an operation that required that the target not exist.
  - 文件已经存在
- EISDIR (Is a directory)
  - An operation expected a file, but the given pathname was a directory.
  - 给定的路径是目录
- ENOENT (No such file or directory)
  - Commonly raised by fs operations to indicate that a component of the specified pathname does not exist -- no entity (file or directory) could be found by the given path.
  - 文件 或 目录不存在
- ENOTDIR (Not a directory)
  - A component of the given pathname existed, but was not a directory as expected. Commonly raised by fs.readdir.
  - 给定的路径不是目录

