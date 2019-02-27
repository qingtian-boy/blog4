---
title: nodejs05
date: 2019-02-26 15:26:50
tags: nodejs
---

# 案例-实现文章列表的curd操作

需求：

get  / 或 /index：  展示文章列表页

get /add :  展示添加文章的模板

post /add :  添加数据入库操作

get  /upd :   取出数据，回显到模板中

post /upd: 完成编辑入库

get   /del:  用于删除文章的





## 设计路由

```javascript
//搭建一个web服务
var express = require('express');
var app = express();
//定义路由  正则匹配 /  或  /index
app.get(/^\/$|^\/index$/,function(req,res){
  res.send('index');
})

// app.get('/index',function(req,res){
//   //重定向到路由 '/'
//   res.redirect('/');
// })


app.get('/del',function(req,res){
  res.send('del');
})

app.get('/upd',function(req,res){
  res.send('upd');
})

app.get('/add',function(req,res){
  res.send('add');
})


app.listen(3000,function(){
  console.log('项目已启动');
});
```

<!-- more -->

正则： `/^\/$|^\/index$/ ` 图形化理解：

![1546653212924](/1546653212924.png)



## 添加文章的操作

1. 先安装模板引擎art-template,在项目中还要配置。
2. 配置body-parser，解析post参数
3. 安装mysql模块，操作数据库



## 展示文章列表页

1.先获取数据库数据

2.通过res.render输出模板并分配变量

3.在模板中通过each进行遍历数据



## 删除文章操作

1.设置a标签的链接地址   href="/del?id={ {val.id} }"  

2.路由匹配函数中

​	a.接收参数id

​	b.直接删除，成功重定向到首页



## 编辑文章的实现回显操作

1.设置a标签的链接地址   href="/upd?id={ {val.id} }"  

2.路由匹配函数中

​	a.接收参数id

​	b.通过id查询单条数据，分配到模板中



## 编辑入库操作

1.在匹配的路由中接收参数

2.实现编辑sql语句，进行入库



完整代码：

```javascript
//搭建一个web服务
var express = require('express');
var path = require('path');
var bodyParser = require('body-parser');
var artTemplate = require('art-template');
var expressArtTemplate = require('express-art-template');
var mysql = require('mysql');
var app = express();


//配置模板引擎art-template
app.set('views',path.join(__dirname,'views')) //配置视图的目录
app.engine('html',expressArtTemplate); //配置静态模板后缀为html
app.set('view engine','html'); //设置视图引擎

//配置body-parser中间件，获取post参数
app.use('/',bodyParser.urlencoded({extended:false}));


//配置数据库连接
var connection = mysql.createConnection({
  host:"127.0.0.1",
  port:3306,
  user:'root',
  password:'123456',
  database:"test"
});

//进行连接
connection.connect(function(err){
  if(err){
    throw err;
  }
  console.log('连接数据库成功');
});

//定义路由  正则匹配 /  或  /index
app.get(/^\/$|^\/index$/,function(req,res){
  //1.获取文章的数据
  var sql = "select * from article";
  //2.执行sql语句
  connection.query(sql,function(err,rows,fields){
    if(err){
      throw err;
    }
    //分配变量输出模板 rows:   [{},{}]
    res.render('index.html',{
      'rows':rows
    });
  })
  
})

// app.get('/index',function(req,res){
//   //重定向到路由 '/'
//   res.redirect('/');
// })


app.get('/del',function(req,res){
  //1.接收查询字符串的参数id
  var id = req.query.id;
  //2.编写sql语句，进行执行删除，重定向到首页
  var sql="delete from article where id = ?";
  var bind = [id];
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    res.redirect('/');
  });
  
})

//编辑，实现文章的回显操作
app.get('/upd',function(req,res){
  //1.接收id参数
  var id = req.query.id;
  //2.查询一条数据，进行回显
  var sql = "select * from article where id= " + id;
  connection.query(sql,function(err,rows){
    if(err){
      throw err;
    }
    //回显数据到模板中  [{}]
    res.render('upd.html',{
      "row":rows[0]
    })
  })
})

//完成编辑入库操作
app.post('/upd',function(req,res){
  //1.接受参数
  var id = req.body.id;
  var title = req.body.title;
  var content = req.body.content;
  //2.编写sql语句，入库
  var sql="update article set title = ? , content = ?  where id = ?";
  var bind = [title,content,id];
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    res.redirect('/');
  })
})

//展示添加模板的视图
app.get('/add',function(req,res){
  res.render('add.html');
})

//完成文章的添加入库操作
app.post('/add',function(req,res){
  //1.接受post参数
  var title = req.body.title;
  var content = req.body.content;
  //2.入库操作
  var sql = "insert into article(title,content) values(?,?)";
  var bind = [title,content];
  //执行sql语句
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    //说明入库成功，重定向到路由  / 
    res.redirect('/');

  })

})

app.listen(3000,function(){
  console.log('项目已启动http://127.0.0.1:3000');
});
```







# 抽离文章列表中的路由部分（路由器中间件）



## 定义路由器中间件



express框架提供一个 ==expres.Router()==的方法，此方法返回的是一个路由对象，我们

 只需要把我们所设置的路由挂载到这个路由对象身上，最后通过app.use(router);中间件进行挂载即可

 route.php   route::get('/login','home/index/index')

具体步骤：

  定义一个路由模块router.js

  router.js

​    1.得到路由对象router

​    2.在路由对象身上设置一个路由

​    3.通过module.exports = router，暴露给外部

  最后外部要引入这个路由对象，var router = require('./router.js'),需要通过app.use(router),注册（挂载）到app身上去。



代码：

路由模块route.js

```javascript
var express = require("express");

//得到一个express的路由对象
var router = express.Router();
//我们把路由挂载这个对象 router 身上去就可以
//注意：通过路由对象 router和之前express的实例app,设置的路由的方式完全一样
router.get("/register",function(req,res){
  res.send("register");
});

router.get("/login",function(req,res){
  res.send("login");
});

router.get('/list',function(req,res){
  res.send('list');
});

//把此对象暴露给外部
module.exports = router;
```



可以在如webServer.js文件中引入上面的路由模块

```javascript
var express = require("express");
var app = express();

//引入路由模块
var router = require("./router.js");
//把路由对象挂载到中间件中，然后app对象才可以监听到路由的改变
app.use(router);
// app.get("/register",function(req,res){
//   res.send("register");
// });

// app.get("/login",function(req,res){
//   res.send("login");
// });

app.listen(3000, function () {
  console.log("请访问http://127.0.0.1");
});
```



## 抽离路由匹配的逻辑模块

先定义controller.js,内容如下

```javascript

var controller = {};

controller.login = function(req,res){
  res.send('login111');
}

controller.register = function(req,res){
  res.send('register22');
}


//暴露所有的函数
module.exports = controller;
```

在route.js中引入controller.js模块，内容如下：

```javascript
var express = require('express');
var app = express(); //框架的实例app

//抽离路由部分挂载到router对象身上
//1.通过express.Router得到路由对象
var router = express.Router();

//2.在路由对象router设置路由，
//3.引入controller.js
var controller = require('./controller.js');
router.get('/login',controller.login)
router.get('/register',controller.register)

//最后需要通过module.exports暴露出去，外面就可以通过requre进行接收使用
module.exports = router;


```

在入口文件index.js中开启服务，引入route.js模块

```javascript
var express = require('express');
var app = express(); //框架的实例app

var router = require('./route.js');
app.use('/',router);


app.listen(3000,function(){
  console.log('请访问http://127.0.0.1:3000');
});
```



最后开启服务在浏览器中请求：http://127.0.0.1:3000/register 







# 使用会话技术session完成退出和登陆功能

## 为什么需要session技术

答：因为==http是无状态==。

因为http的请求和响应是互相独立的，服务器无法识别两条http请求是否是同一个用户发送的。也就是说服务端并没有记录通信状态的能力。所以我们一般采用session和cookie来确定会话双方的身份。



注意：

- session是基于cookie存在的，第一次http请求时，session会进行初始化，同时会向浏览器cookie中写一个sessionID的标识。
- 第二次http请求时，就会带着cookie中的sessionID会话凭证，从而识别出是同一个客户端请求的。



## 安装express-session第三方扩展

- 由于express框架自身不带session和cookie机制，
- 这里需要借助第三方模块`express-session`中间件来实现。
- 注意：之前`express-session`的老版本要想使用cookie需要单独安装`cookie-parser中间件`来实现,
- `express-session`从1.5.0版开始， 已经集成cookie相关组件，不再需要使用[`cookie-parser`中间件](https://www.npmjs.com/package/cookie-parser)来使该模块工作。



### 配置session参数

1. 安装, ` npm install express-session --save

2. 引入,`var session = require （'express-session'）  `

3. 使用中间件`app.use()`配置session和cookie相关参数

   ```javascript
   app.use( session(options) );
   ```

   其中options是一个json对象格式，有如下选项

   - **name**： 写入cookie中的session会话名称，默认值是`'connect.sid'`

   - **secret**：一个 String 类型的字符串，作为服务器端生成 session 的签名(加密用的)。主要防止用户篡改cookie中的值。

   - **cookie：**与php中的cookie的设置一样，设置发送到客户端 的一些 属性。

     -  默认值为一个对象

       ```json
       { 	
           path: '/' ,  // 默认为'/'，即域的根路径。
           httpOnly: true,  // 默认为true,设置cookie只读，即不能通过document.cookie来获取
           secure: false,  // 设为true可适用于https网站的安全传输。若域名是http开头，网站只能设为false，否则cookie不写携带到后台服务器中。
           maxAge: null , //session会话保存在cookie中的有效期，单位：毫秒值， 默认为null,即设置的当前会话永不失效，一般都是需要设置有效期的，如php中的session有效期默认为24分钟。
           //设置有效期为24分钟，说明：24分钟内，不访问就会过期，如果24分钟内访问了。则有效期在初始化为24分钟。
       }
       ```

      


   官方文档：https://www.npmjs.com/package/express-session 



### session会话基本操作

通过==req.session==可以对session会话进行相关操作

1. 设置session、获取session、销毁session

==设置session==：

```javascript
req.session.键名 = 值； 
```

==获取session==:

```javascript
console.log(req.session.键名); //获取session
```

==销毁当前session会话==:

```javascript
req.session.destroy(function(err){
    if(err){
          throw err;
    }
})

//清除指定的session
req.session.键名 = null;
```

2. 查看session剩余的有效期

```
req.session.cookie.maxAge/1000; //除以1000转化为秒
```



整体代码：

```javascript
var express = require('express');
//1.引入session
var session = require('express-session');

var app = express();

//2.设置session一些初始化配置操作
//通过app.use中间件设置session的参数
app.use(session({
  name:'SESSIONID',  // session会话名称在cookie中的值
  secret:'%#%￥#……%￥', // 必填项，用户session会话加密（防止用户篡改cookie）
  cookie:{  //设置session在cookie中的其他选项配置
    path:'/',
    secure:false,  // true 只针对于域名https协议
    maxAge:60000*24,  //设置有效期为24分钟，说明：24分钟内，不访问就会过期，如果24分钟内访问了。则有效期在初始化为24分钟。
  }
}));

app.get('/set',function(req,res){
  //设置session
  req.session.uid = 1001;
  req.session.username = 'admin';
  res.send('session已设置')
})

app.get('/get',function(req,res){
  //获取session
  console.log(req.session.uid);
  console.log(req.session.username);
  res.send('获取session')
})


app.get('/delui',function(req,res){
  //删除一个session
  req.session.username = null;
  res.send('删除username');
})

app.get('/delsess',function(req,res){
  //删除整个session会话
  req.session.destroy(function(err){
    if(err){
      throw err;
    }
  });
  res.send('删除session会话');
})


app.get('/youxiaoqi',function(req,res){
  // console.log(req.session.cookie);
  console.log(req.session.cookie.maxAge);
});

app.listen(3000,function(){
  console.log('请访问');
})


```








# 实现文章管理登录和退出功能



## 完成登录操作

1. 定义get路由/login,展示登录的模板

2. 点击登录表单的按钮：

   a:在post匹配的/login路由的函数中，先接收参数

   b:判断用户名和密码是否匹配

   ​	失败：还要要回显到登录页面，并分配一个message,  主要是用户模板中判断message从而可以使用alert()提示，最后换成lyaer。 ==注意==：使用layer之前先引入jquery

   ​	

   ​	成功：设置用户的信息到session,重定向到后台列表页



## 完成退出操作

1. 设置退出的链接地址 get   /logout 
2. 清除session，重定向登录页





# 防止登录翻墙

思路： 核心是在中间件中判断是否有session

哪些路由需要判断session?哪些不需要？

答案：除开 /login 和 /logout不需要判断session,其他都需要。





#  密码加密

参考地址：https://www.npmjs.com/package/md5

1.安装  npm install md5  --save

2.项目中引入

3.匹配密码的时候，需要给password进行md5加密



抽离路由和控制器模块图解:
![1546653212924](/抽离路由和控制器模块图解.png)

防翻墙定义中间件执行流程:
![1546653212924](/防翻墙定义中间件执行流程.png)

中间件在栈中执行的逻辑:
![1546653212924](/中间件在栈中执行的逻辑.png)

中间件的执行流程和特点:
![1546653212924](/中间件的执行流程和特点.png)

中间件在栈中的执行顺序:
![1546653212924](/中间件在栈中的执行顺序.png)
