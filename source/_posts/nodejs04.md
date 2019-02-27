---
title: nodejs04
date: 2019-02-26 15:25:33
tags: nodejs
---


# 中间件 middleware



## 生活中的中间件



![中间件1](/middleware01.jpg)

从水库取水最终到用户家庭的整个过程，中间经过了很多环节，如反应沉淀池->过滤池->活性炭等。其中每个环节都可以当做是==中间件==，这样取水的环节非常清晰明了，同时也有利于工人的分工合作，当某个环节有问题时，也只需修复它本身的问题，不会影响到其他环节。

<!-- more -->

## 为什么需要中间件

- 中间件：在web开发中，主要做`业务解耦`。
- 业务解耦：即一个request到response的过程中，把一个==大功能拆分成某干个小功能==。这样有利于代码的维护。类似于MVC做法。



# express框架的中间件

我们这个express框架实质上就是==各种中间件叠加==组合使用。如下：

![](middleware02.png)



1. 在express中，中间件就是一个函数。
2. 在这个中间件函数中能做什么？

- 可以在程序请求后和响应前执行任何代码。（如判断用户是否登录等）

- 可以修改request请求对象和response响应对象的值

- 可以终结请求，不在往下执行。也可以继续执行下一个中间件

- ....


## express 中的中间件分类

1. 应用程序级中间件(普通中间件)

2. 路由器级中间件

3. 错误处理中间件


4. 内置中间件（Express.static()）

5. 第三方中间件（npm）



express中间件： http://www.expressjs.com.cn/guide/using-middleware.html 

- [第三方中间件列表-中文](http://www.expressjs.com.cn/resources/middleware.html)
- [第三方中间件列表-英文](http://expressjs.com/en/resources/middleware.html)

## 中间件执行顺序和特点

1. 根据中间件在代码中注册的顺序，依次匹配判断，如果匹配了就会执行对应的中间件代码。

2. 如果匹配到的中间件没有调用==res.end()==结束请求，则当前中间件必须调用 ==next() 方法==将控制权交==下一个中间件进行处理==，否则请求会被挂起（浏览器一直在转）。

   ==中间件执行流程==：中间件执行的两个结果：1、要么结束响应 2、要么next执行下一个中间件

   ![1546481717668](/1546481717668.png)

   

## 应用程序级中间件(普通中间件)的基本使用

- 通过 app 对象来注册中间件

  ```javascript
  app.use(path,callback) 和 app.METHOD(path,callback) 和 app.all(path,callback)  // method是http请求方法之一
  ```

  参数：

  ​	path: 是路由的url中的pathname部分,不写默认是=='/'==, 即路由到这个路径时使用这个中间件。

  ​	其中path是请求路径，不写默认是=='/'==, 即url中pathname部分。

  ​	callback是匹配到path路径对应处理的回调函数。有==3个参数==：req、res、next，其中calllback就是中间件函数

代码如下：

```javascript
var express = require('express');
var app = express();

//定义一个中间件 (任何请求方式，任何url都会经过)
//一般在此中间件中做一些最开始的初始化操作，如判断用户是否登录
app.use('/',function(req,res,next){
   console.log('先经过我');
  //  res.send('堵住路口');
   next(); //代表执行下一个中间件
})


//定义路由（路由函数也是中间件）
app.all('/',function(req,res){
  console.log('后经过我');
  res.send('index');
});

app.listen(3000,function(){
  console.log('请访问');
});
```

访问http://localhost:3000/index ，控制台结果：

```javascript
先经过我
后经过我
```



==注意==：中间件app.use(function(req,res,next){}) ，即所有的请求方式和请求的地址都会先触发此中间件，在此中间件一般做一些初始化操作（如：判断用户是否登录）



## 错误处理中间件

基本语法：

 ```javascript
   app.use('/',function (err, req, res, next) {
     console.error(err.stack) //打印出完整的错误信息
     res.status(500).send('Something broke!')
   })
 ```



- 错误中间件必须写==4个参数== err、req、res、next
- 在普通中间件中如果抛出一个错误（1.next(err) 2。throw new Error('错误信息')），都会直接出发错误中间件
- 会依次执行所有的普通中间件（应用层中间件），没有err抛出直接绕过错误中间件。会继续执行下一个普通中间件

![中间件1](/error-handling01.png)

- 当普通中间件调用 next(  new Error('错误') ) 或 throw err时，即抛出了错误对象，会直接进入错误处理中间件，同时跳过所有的普通中间件。

![中间件1](/error-handling02.png)

错误中间件两个作用：

- 跳转到404页面
- 把错误信息写入到日志文件中，供以后排查错误





- 普通中间件抛出错误

```javascript
next(new Error('错误'));   //或  throw err
```



- 定义错误中间件（4个参数）

```javascript
app.use(function (err, req, res, next) {
    console.og('我是错误中间件');
    console.error('error Info:',err.stack);
 	//status设置响应状态码，并通过send响应文本
    res.status(500).send('服务器错误');
})
```



完整代码：

```javascript
var express = require('express');
var app = express();

var path = require('path');


//定义一个普通中间件
app.use('/',function(req,res,next){
  console.log('先经过我1');
  next(); //执行下一个中间件
})

app.use('/',function(req,res,next){
  console.log('先经过我2');
  next(); //执行下一个中间件
})


//定义一个/login
app.get('/login',function(req,res){
  //触发错误中间件的两个条件：1.  next(err)    2. throw new Error('文件找不到')
  throw new Error('login.html文件找不到'); 
  //上面抛出错误，下面代码不会执行，直接执行我们定义的错误处理中间件
  res.sendFile(path.join(__dirname,'view/login.html'));
})

//定义路由（路由函数也是中间件）
app.get('/',function(req,res){
  res.send('index');
});

//定义一个错误中间件
app.use('/',function(err,req,res,next){
  console.log(err.message); //获取到上面抛出来的错误对象的错误信息
  res.send('404');
})

app.listen(3000,function(){
  console.log('请访问');
});
```





# 利用错误中间件响应404页面

完整代码：

```javascript

var express = require('express');
var path = require('path');
var fs = require('fs');

var app = express();
app.get('/login',function(req,res,next){
  console.log('111');
  if(!fs.existsSync(path.join(__dirname,'view/login222.html'))){
    var err = new Error('login.html文件不存在');
    next(err); //执行错误中间件
  }
  console.log('回来');
  console.log(req.code);
  if(!req.code){
    //上面判断为假，说明没有经过错误中间件，则没有code属性,则响应数据
    res.send('login');
  }
})

app.get('/register',function(req,res){
  res.send('register');
})

//定义错误中间件
app.use('/',function(err,req,res,next){
  req.code = 'error'; //给req对象设置一个属性code,代表是从错误中间件过去的
  console.log(222);
  //显示404错误页面
  res.end('40444444'); 
  // return false; //不行 ，还会跑到上面中间件去
})

app.listen(3000,function(){
  console.log('请访问http://127.0.0.1:3000');
})
```





# 使用Express.static()托管静态资源文件

1.基本使用

通过 Express 内置的 `express.static` 可以方便地托管静态文件，自动响应这些文件的内容，例如image、CSS、JavaScript 文件等。

将静态资源文件所在的目录作为参数传递给 `express.static` 中间件就可以提供静态资源文件的访问了。

例如，假设在 `public` 目录放置了images、CSS 和 JavaScript 文件，你就可以：

```javascript
//当访问静态资源文件的时候，都会去注册好的目录/public中去寻找，找到了并读取静态文件响应给客户端。
app.use('/',express.static(__dirname + '/public') ) ; 

```

现在，`public` 目录下面的文件就可以访问了。

```javascript
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
```

> 注：所有文件的路径都是相对于public目录存放的，因此，访问静态文件的url路径不要出现/public。



2. 也可以为 `express.static` 中间件函数创建的静态资源文件指定==虚拟路径前缀==（路径并不实际存在于文件系统中） 

```javascript

app.use('/mypublic',express.static(path.join(__dirname,'public')));
//如：html中访问图片<img src="/mypublic/images/dagong.jpg" alt="">
//这里的/mypublic是中间件虚拟路径,
//其中虚拟路径后面是跟着真实路径/images/dagong.jpg，再去拼接所托管的真实资源文件路径
//最终会这么拼接： path.join(__dirname,'public'))+/images/dagong.jpg 才会拼接一个完整的路径。
```

那么之前访问的url也要要带着虚拟路径/mypublic：

```java
http://localhost:3000/mypublic/images/kitten.jpg
http://localhost:3000/mypublic/css/style.css
http://localhost:3000/mypublic/js/app.js
```

官网地址：http://expressjs.com/zh-cn/starter/static-files.html 

完整代码：

```java

var express = require('express');
var app = express();

var path = require('path');

//使用express.static托管静态资源,指定静态资源路径即可
app.use('/public',express.static( path.join(__dirname,'public') ));

//创建路由/user
app.get('/user',function(req,res){
  //获取user.html模板内容并响应
  res.sendFile(path.join( __dirname,'views','user.html' ) );
});

//开启服务
app.listen(3000,function(){
  console.log('请访问http://127.0.0.1:3000');
});
```



在html静态文件中，访问静态文件的时候，一定要带上虚拟的路径才可以访问，如：

```
http://127.0.0.1:3000/public/css/style.css
```

- 其中url中的/public是中间件的虚拟路径，必须出现，这样才会经过上面定义的中间件，
- 要保证后面的css/style.css的资源文件在真实托管的目录中存在





# 在 express 中集成art-template模板引擎

## 介绍   

- 在 express框架 中, res对象具有一个`render()`方法来输出模板内容，但前提是要配置一个模板引擎

- 常见的nodejs模板引擎

  - jade(express默认，已升级为pug)
  - [art-template](https://aui.github.io/art-template/)  
  - [ejs](https://ejs.bootcss.com/)
  - [nunjucks](https://mozilla.github.io/nunjucks/)
  - ......

  这里我们使用[art-template](https://aui.github.io/art-template/) 模板引擎，原因是：1.语法简介  2.性能优越

  art-template官方网站：https://aui.github.io/art-template/ 

  

## art-template基本使用

  网址：https://aui.github.io/art-template/express/



==步骤==：

1. 项目中安装模板

   ```javascript
   npm install --save art-template
   npm install --save express-art-template
   ```

2. 引入上面两个模块和express模块

   ```javascript
   const artTemplate = require('art-template'); 
   const express_template = require('express-art-template');
   ```

   

3. 配置express框架模板引擎为art-template  

​    核心代码：

```javascript
//配置模板的路径
app.set('views', __dirname + '/views/');
//设置express_template模板后缀为.html的文件(不设这句话，模板文件的后缀默认是.art)
app.engine('html', express_template); 
//设置视图引擎为上面的html
app.set('view engine', 'html');
```

4. 定义路由，使用==res.render(filename,data)==方法响应模板内容，并分配变量

- 参数1:filename:

  模板的文件名，注意，模板文件是.html结尾，且默认在当前views目录中找模板

- 参数2:data

  分配的变量，要求是一个==json对象==格式。

```javascript
app.get('/detail',function(req,res){
    res.render('demo.html', {"name":"大锤"});
})   
```

完整代码：

```javascript
var express = require('express');
var app = express();
var path = require('path');
var moment = require('moment');

//1.引入安装的art-tempalte模板引擎的模块
var artTemplate = require('art-template');
var expressArtTemplate = require('express-art-template');

//2.配置express框架的模板引擎为art-template
app.set('views',path.join(__dirname,'view')); //设置视图所在的文件夹
app.engine('html',expressArtTemplate)// 设置模板文件的后缀为.html
app.set('view engine','html')

//定义一个过滤器dateFormat
artTemplate.defaults.imports.dateFormat = function(time,format = 'YYYY-MM-DD HH:mm:ss'){
  return moment.unix(time).format(format);
}


app.get('/detail',function(req,res){
  // res.send('detail');
  //3.输出模板内容res.render('模板文件'，分配的变量{});
  res.render('detail.html',{
    "name":"王大锤",
    "age":88,
    "users":[
      '小美','大美','真美'
    ],
    time:1546499716
  });
})

app.listen(3000,function(){
  console.log('请访问');
})
```



模板html中遍历数据：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
    <h2>detail模板</h2>
    大名：{ {name} }-- 年龄： { {  age } }
    <hr>
    大名：<%=  name %> -- 年龄：<%= age%>

    <hr>
    我的女儿：
   { {each users myvalue index} }
    <li>序号：{ {index+1} }-乳名：{ {myvalue} }</li>
   { {/each} }

   <hr>

   我的女儿：
   { {each users } }
    <li>序号：{ {$index+1} }-乳名：{ {$value} }</li>
   { {/each} }


   <hr>
   时间戳: { { time } }    
   <br>
   <!-- 第一个参数不需要传递默认是当前的time,直接从第二个参数开始传递 -->
   时间格式为：{ {time|dateFormat "YYYY-MM"} }

</body>
</html>
```

效果：

![1546501689104](/1546501689104.png)







## art-template模板中遍历数据

1. art-template同时支持模板的两种语法。

- ==标准语法==:写在一对`{ {  } }`中  (插件表达式)

  ```javascript
  { { name } }
  ```

- ==原始语法==:具有强大的逻辑处理能力。 

  ```javascript
  <%= name %>
  ```

  

官网地址：https://aui.github.io/art-template/docs/ 

模板语法：https://blog.csdn.net/gao_xu_520/article/details/78594792 



1. 渲染模板并分配变量：

```javascript
res.render('demo.html',{
    title:标题
    users:['大锤','小锤','中锤']
});

```

- ==标准语法==：

  - 输出变量

  ```html
  <body>
  	{ { name } }
  </body>
  ```

  - 判断变量

  ```html
  <body>
      { {if name} } 
          <span>{ {name} }</span>
      { {else} }
          <span>无名</span>
      { {/if} }
  </body>
  ```

  - 循环变量

    { {each 变量 值  索引  } }

    { {/each} }

  ```html
  <body>
  	{ {each users value index } }
  		<h1>{ { index+1 } } 元素{ { value } }</h1>
  	{ {/each} }
  </body>
  //或简写形式
  { {each users } }
  		<h1>{ { $index+1 } } 元素{ { $value } }</h1>
  { {/each} }
  
  ```

- ==原始语法==：

  - 输出变量

  ```
  <body>
  	<%= name %>
  </body>
  ```

  - 判断变量

  ```html
  <body>
  	<% if(name) {%>
  		<span><%= name %></span>
  	<% }else{%>
  		<span>无名</span>
  	<% }%>
  </body>
  ```

  - 循环变量

  ```html
  	<tr>
  		<% for(var i = 0; i < users.length; i++){ %>
  			 <% var item = tags[i]; %>
  			  <td><%= i+1 %></td>
  			  <td><%= item %></td>
  		<% } %>
  	</tr>
  ```



## art-template定义过滤器（模板函数） 

如定义一个时间格式的过滤器，把时间戳转化为日期格式。

基本使用：

1、如安装moment时间操作模块

```
npm install moment --save
```

2、基本使用，可以将时间戳转化为对应的日期格式

```javascript
var moment = require('moment');
console.log( moment.unix(1540602152).format("YYYY-MM-DD HH:mm:ss") );
```

3、定义过滤器语法

```javascript
var moment = require('moment');
//引入art-template对应的包
var artTemplate = require('art-template');
var ExpressartTemplate = require('express-art-template');

// 定义一个把毫秒值转化成时间格式的过滤器
//dateFormat是方法名
//定义一个过滤器dateFormat
artTemplate.defaults.imports.dateFormat = function(time,format = 'YYYY-MM-DD HH:mm:ss'){
  return moment.unix(time).format(format);
}
```

4、html模板中使用

```html
 <!-- 第一个参数不需要传递默认是当前的time,直接从第二个参数开始传递 -->
  时间格式为：{ {time|dateFormat "YYYY-MM"} }
```

时间插件moment用法参考： http://momentjs.cn/

# 使用body-parser中间件获取post请求参数

1. 回顾之前nodejs获取post参数的方式：

   之前获取用户的post提交参数，需要监听request对象的data和end事件，才可以完成。这样做太麻烦。

2. 现在我们可以借助一个第三方中间件`body-parser`,可以非常方便的获取post请求参数。



基本使用：

- 安装`npm install body-parser  --save ` 
- 引入`var  bodyParser  = require('body-parser')`
- 在 express 中配置

```javascript
//解析表单 application/x-www-form-urlencoded 格式编码的数据
//设extended: false会使用querystring模块解析参数
//并将post参数保存在req.body属性中
app.use('/',bodyParser.urlencoded({ extended: false }))
```

官方文档：https://www.npmjs.com/package/body-parser#bodyparserrawoptions

源码：https://github.com/expressjs/body-parser/blob/master/index.js

- 获取post参数

  html表单

```html
  <form method='post' action='/add'>
  	用户名： <input type="text" name='username' />
  	密码： <input type="text" name='password' />
      <input type="submit" value="提交" />
  </form>
```

 	 获取：

```javascript
  req.body.username
  req.body.password
```

  

==注==：获取get参数，即?号后面的参数：http://url?name=xiaobai ，可以直接使用==req.query.name==获取。







# 使用Nodejs操作mysql数据库

## 基本使用步骤

1. 在npm网站中https://www.npmjs.com/搜索mysql，安装mysql第三方模块，`npm install mysql `

2. 引入 mysql, `var mysql = require('mysql')`

3. 阅读文档，看连接接数据库，增删改查如何使用

   官方文档：https://www.npmjs.com/package/mysql/v/2.16.0#introduction



## 连接数据库代码

```javascript
var mysql = require('mysql'); 

//连接数据库参数配置
var connection = mysql.createConnection({
    host    :"localhost", //主机
    port    :'3306',	//端口
    user    :"root",	//用户名
    password:"123456",	//密码
    database:"test"		//数据库
});
//连接mysql
connection.connect(function(err){
    if(err){
        throw err;
    }
    console.log('connect success');
});
```

**连接常用选项**：

- `host`：要连接的数据库的主机名。（默认值： `localhost`）

- `port`：要连接的端口号。（默认值：`3306`）

- `user`：要进行身份验证的MySQL用户。

- `password`：MySQL用户的密码。

- `database`：用于此连接的数据库的名称（可选）。

- `charset`：连接的charset。这在MySQL的SQL级别（如`utf8_general_ci`）中称为“整理” 。如果指定了SQL级别的字符集（如`utf8mb4`），则使用该字符集的默认排序规则。（默认值：`'UTF8_GENERAL_CI'`）

- `socketPath`：要连接到的unix域套接字的路径。使用mac苹果电脑的同学需要设置mysql.sock的路径。

  如：`   socketPath: '/Applications/MAMP/tmp/mysql/mysql.sock' `



## 增删改查操作

- 查询

  ```javascript
  connection.query('select * from article',function(err,rows,fields){
        if(err){
            throw err;
        }
        //success  to do ...
  });
  ```

- 删除

  ```javascript
  connection.query('delete from article where id= 1',function(err,result){
        if(err){
            throw err;
        }
        //success  to do ...
  });
  ```

- 增加

  ```javascript
    var  sql = 'INSERT INTO article(id,title,content) VALUES(0,?,?)';
    var  bind = ['NBA', 'NBANBANBA']; // 数组元素和对应占位符的？号参数绑定
    connection.query(sql,bind,function(err,result){
        if(err){
            throw err;
        }
        //success  to do ...
    });
  ```

- 更新

  ```javascript
    var  sql = 'update  article set title = ?,content = ? where id = ? ';
    var  bind = ['CBA','CBACBACBA',1];
    connection.query(sql,bind,function(err,result){
        if(err){
            throw err;
        }
        //success  to do ...
    });
  ```



完整代码：

```javascript
var express = require('express');
var app = express();

//1.引入所安装的mysql模块
var mysql = require('mysql');

//2.链接数据库参数配置
var connection =  mysql.createConnection({
  host:"127.0.0.1",
  port:3306,
  user:"root",
  password:"123456",
  database:"test"
});
//3.链接mysql
connection.connect(function(err){
  //判断是否链接成功
  if(err){
    throw err;
  }
  console.log('链接mysql成功');
});

app.get('/index',function(req,res){
  //查询操作
  var sql = "select * from users"
  connection.query(sql,function(err,rows,field){
    //参数rows:返回的是一个数组[{},{},{}],其中每个元素都是一个json对象
    if(err){
      throw err;
    }
    console.log(rows.length);
    console.log(rows[2].phone);
  });
  res.send('查询');
})


app.get('/add',function(req,res){
  //入库操作
  var sql = "insert into users(username,age,phone) values(?,?,?)";
  var bind = ['切格瓦拉','34','110'];
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    console.log(result); //返回是一个对象
    //result.affectedRows:获取到受影响的函数
    //result.insertId:获取到入库成功的自增主键的值
    res.send('入库');
  })
})


app.get('/upd',function(req,res){
  //编辑操作
  var sql = "update users set username = ? , age = ?,phone = ? where uid = ? ";
  var bind = ['哇啦切割',44,119,7];
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    console.log(result);
      //result.affectedRows:获取到受影响的行数
  })
  res.send('编辑');

})

app.get('/del',function(req,res){
  //删除操作
  var sql = "delete from users where uid = ?";
  var bind = [5];
  connection.query(sql,bind,function(err,result){
    if(err){
      throw err;
    }
    console.log(result);
     //result.affectedRows:获取到受影响的行数
  });
  res.send('删除');
})

app.listen(3000,function(){
  console.log('请访问');
});


```

