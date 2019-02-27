---
title: restful
date: 2019-02-26 14:39:12
tags: restful
---

# RESTful Api架构风格概述

## 什么是RESTful

REST即`Representational State Transfer`的缩写，可译为"表现层状态转化”。（先不管此鸟语），其中ful是形容词风格。

`RESTful`是目前最流行的 API 设计规范，主要用于web中前后端数据接口的设计。



## 使用RESTful目的

- 可以正确的使用web标准，优雅的使用HTTP本身的特性。 

- 使用合适的HTTP的方法（get|post|put|delete）对资源（URI）进行操作(增删改查)

- 一个URl定位服务器中的一个资源(数据)，

- HTTP中的方法（get|post|put|delete）进行资源的操作



图一：

<!-- more -->

![1540132180692](/api.jpg)



一句话概括restful作用：

- 看urI知道要操作什么数据(资源)   /api/user    /api/article/
- 看http方法知道怎么操作数据  put /api/user/8   ,  delete   /api/user/8





| 动词   | URI      | 操作 | 作用         |
| ------ | -------- | :--- | ------------ |
| GET    | /users   | 获取 | 获取所有用户 |
| GET    | /users/2 | 获取 | 获取指定用户 |
| POST   | /users   | 创建 | 创建一个用户 |
| PUT    | /users/3 | 更新 | 更新用户信息 |
| DELETE | /users/4 | 删除 | 删除用户     |





# RESTful架构风格的特点

REST最大的几个特点可归类为：

- 资源(数据)
- 统一接口(post、delete、put、get)
- URI （定位到数据）
- 无状态



### 资源（数据）

所谓`资源`,就是网络上的一个实体，可以理解为就是`数据`。

比如：一段文本、一张图片、一个视频等。

而在前后端数据交互的时候，上面的数据或动态的数据我们一般存储在数据库中。所以我们可以把数据库中的数据也可以称之为是`资源`。

JSON是现在最常用的资源传输格式。 



### 统一接口

RESTful架构风格规定，数据的操作，即CRUD(create, read, update和delete,即数据的增删查改)操作，分别对应于HTTP方法：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源，这样就统一了数据操作的接口，仅通过HTTP方法，就可以完成对数据的所有增删查改工作。



通常就是五种 HTTP 方法（动词），对应 CRUD 操作，即    

- GET（READ）：从服务器获取资源。成功建议状态码返回200，表示为OK
- POST（CREATE）：在服务器创建一个资源。成功建议状态码返回201,表示为Created,代表资源已创建
- PUT（UPDATE）：在服务器更新资源。成功建议状态码返回200。
- DELETE（DELETE）：从服务器删除资源。成功建议状态码返回204 ，表示为No Content，一般多用200来响应，因为如果返回204，我们返回的json数据，客户端是获取不到的。

![1544856823627](/1544856823627.png)

- PATCH（UPDATE）：也是在服务器更新资源。PATCH方法是新引入的，是对PUT方法的补充，用来对已知资源进行局部更新。参考[rfc5789](https://tools.ietf.org/html/rfc5789)



还有几个不常用的动词

- HEAD：获取资源的元数据。
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。



### URI    

可以用一个URI（统一资源定位符）指向资源，即每个URI都对应一个特定的资源。要获取这个资源，访问它的URI就可以，因此==URI==就成了每一个==资源的====定位符==。

一般的，每个资源至少有一个URI与之对应，最典型的URI即URL（统一资源标识符）。



### 无状态    

所谓无状态的，即所有的资源，都可以通过URI定位，而且这个定位与其他资源无关，也不会因为其他资源的变化而改变。

有状态和无状态的区别，举个简单的例子说明一下。

如查询员工的工资，如果查询工资是需要登录系统，进入查询工资的页面，执行相关操作后，获取工资的多少，则这种情况是`有状态`的，因为查询工资的每一步操作都依赖于前一步操作，如前置操作不成功，后续操作就无法执行。



如果输入一个url即可得到指定员工的工资，则这种情况是`无状态`的，因为获取工资不依赖于其他资源或状态，且这种情况下，员工工资是一个资源，由一个url与之对应，可以通过HTTP中的`GET`方法得到资源，这是典型的RESTful风格。



# 使用TP5框架实现restful API架构

`application/config.php`定义路由：

```php
use think\Route;
// Route::any("/users",'api/users/users');
Route::get('/users/[:id]','api/users/read'); // get获取一个或所有的资源
Route::post('/users','api/users/create'); // post新增资源
Route::put('/users/:id','api/users/update'); // put更新资源
Route::delete('/users/:id','api/users/delete'); // delete删除资源
```

控制器建议直接继承Rest，最适合Restful。

```php
<?php 
namespace app\api\controller;
//建议继承Rest
use think\controller\Rest;
use think\Db;

class UserController extends Rest{

	//get /api/user/[:id]  获取资源
	public function read($id = 0){
		$id = $id?:0;
		if($id==0){
			$data = Db::name('user')->select();
		}else{
			$data = Db::name('user')->find($id);
		}
		// $data = request()->get();  //接受get所有的参数

		//响应数据
		$response = ['code'=>200,'message'=>'success','data'=>$data];
		return json($response,200);
	}

	//post /api/user  新增资源
	public function create(){
		//获取所有的post参数
		$data = request()->post();
		$data['password'] = md5($data['password']); //密码加密处理
		$flag = Db::name('user')->insert($data);
		if($flag){
			$response = ['code'=>'200','message'=>'success','data'=>''];
			return json($response,201); // 201表示服务器新增资源
		}else{
			$response = ['code'=>'-1','message'=>'fail','data'=>''];
			return json($response); // 201表示服务器新增资源
		}
	}

	//put /api/user/:id  修改资源
	public function update($id){
		//获取put方式传递的参数
		$data = request()->put();
		//更新数据
		$flag = Db::name('user')->where('user_id','=',$id)->update($data);
		if($flag){
			$response = ['code'=>'200','message'=>'success','data'=>''];
			return json($response); 
		}else{
			$response = ['code'=>'-3','message'=>'fail','data'=>''];
			return json($response); 
		}
	}

	//delete /api/user/:id  修改资源
	public function delete($id){
		//删除资源
		$flag = Db::name('user')->delete($id);
		if($flag){
			$response = ['code'=>'200','message'=>'success','data'=>''];
			// 204表示服务器资源已经被删除了，注意不可以写204，否则客户端获取不到数据
			return json($response); 
		}else{
			$response = ['code'=>'-2','message'=>'fail','data'=>''];
			return json($response); // 201表示服务器新增资源
		}
	}
	
}

```

==注意：==

- put和delete和post的参数方式都是一样的，参数都是通过body进行设置的

- 只有get请求方式，才是把参数放到url后面 url?id=2

  

# 接口请求工具postman使用

此工具可以模拟http的各种请求方法，包括设置参数、设置请求头、查看响应头和响应数据等操作都非常便捷。

- 下载postman工具 , https://www.getpostman.com/
- 注册账户（可选）
- 进行接口的请求

![1540181098060](/1540181098060.png)



#  后台响应json数据的约定

一般响应的json格式其中包含三个字段：状态码、状态码信息、数据

```
$data = ['code'=>200,'message'=>'success',data=>'数据'];
//echo json_encode($data);
return json($data,200)
```

![1544859877359](/1544859877359.png)





# postman传递请求的参数

![1544861431185](/1544861431185.png)









# 使用$.ajax进行各种请求

```html
<body>
	<h2>api接口测试</h2>
	<button id="btn">请求</button>
</body>
<script type="text/javascript">
	//模拟get\post\put\delete四种请求
	$("#btn").click(function(){
		$.ajax({
			type:'get', // 可以是/get/post/put/delete
			url:"/users/6",
			data:{"name":"大王","age":18},
			success:function(res){
                //数据完全响应回来会触发这里
				console.log(res);
			},
            beforeSend:function(){
                //数据未完全响应回来会触发这里
            },
		  dataType:"json"
		});
	});
</script>
```



完整代码：

1. 设置路由：

```
Route::group('admin',function(){
	//后台文章的操作相关路由
	Route::get('/test/api','admin/test/api');
})
```

2. 模板代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script type="text/javascript" src="{:config('static_admin')}/js/jquery.js"></script>
</head>
<body>
	<h1>api接口测试</h1>
	<button id="btn">发送</button>
</body>
<script type="text/javascript">
	$("#btn").click(function(){
		//put请求
		$.ajax({
			"type":"put", //可以是get|post|put|delete
			"url":"{:url('/api/user/5')}",
			"data":{"username":"laogui"},
			"dataType":"json", 
			"success":function(res){
				//res是服务端响应回来的数据
				console.log(res);
			}
		});


		//delete请求
		// $.ajax({
		// 	"type":"delete", //可以是get|post|put|delete
		// 	"url":"{:url('/api/user/6')}",
		// 	"dataType":"json", 
		// 	"success":function(res){
		// 		//res是服务端响应回来的数据
		// 		console.log(res);
		// 	}
		// });



		//post请求
		// $.ajax({
		// 	"type":"post", //可以是get|post|put|delete
		// 	"url":"{:url('/api/user')}",
		// 	"data":{"username":'sigui',"password":'777777'},
		// 	"dataType":"json", 
		// 	"success":function(res){
		// 		//res是服务端响应回来的数据
		// 		console.log(res);
		// 	}
		// });


		//get请求
		// $.ajax({
		// 	"type":"get", //可以是get|post|put|delete
		// 	"url":"{:url('/api/user/1')}",
		// 	"dataType":"json", 
		// 	"success":function(res){
		// 		//res是服务端响应回来的数据
		// 		console.log(res);
		// 	}
		// });
	});
</script>
</html>
```













# 几个建议



- api域名 http://api.example.com ,即以api打开
- api版本 https://api.example.com/v1/api ，通过v1指定版本，参考[github](https://api.github.com/)
- 响应数据最好为json，避免XML,形成良好的约定规范。   ==json==、xml、text、jsonp、html
- api授权auth
  - 假设有一个接口：获取个人的工资
    - http://api.xxx.com/salary/1 
    - 有些接口需要授权，如需要在后面带一个token,服务端会有一个专门的api接口，来提供token，这个token一般是有有效期。(如微信公众号接口，微信有效期是两个小时)
    - 有token之后，接口的url地址：http://api.xxx.com/salary/1/gfgsdfgs45g4s6df6ds5f6g
    - 有了上面的token之后，就可以请求一些比较敏感的数据。

如商城后面的qq登录。 或者是其他的api，如微信开发官方提供的一些接口，同样也是需要token进行授权。

- 会首先询问是否要授权。
- 当我们同意授权之后就可以获取到token。
- 获取到token，就可以继续调用其他的有关token的api接口了

# HTTP响应状态码

在用户发出请求，服务端对请求进行响应时，给予正确的HTTP响应状态码，有利于让客户端正确区分遇到的情况。HTTP状态码共分为5种类型：

| 分类 | 分类描述                                                     |
| ---- | ------------------------------------------------------------ |
| 1**  | 相关信息，服务器收到请求，需要请求者继续执行操作             |
| 2**  | 成功，操作被成功接收并处理                                   |
| 3**  | 重定向，需要进一步的操作以完成请求                           |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求  404-not found    403-forbidden |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误               |

[状态码参考-英文](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

[状态码参考-中文](http://tool.oschina.net/commons?type=5)



# 小结

综合上面的解释，我们总结一下什么是RESTful架构：

- 每一个URI代表一种资源
- 客户端和服务器之间，传递这种资源的某种表现层；
- 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。
  - get 获取数据
  - post 新增数据
  - put 更新数据
  - delete 删除资源




参考地址：

github-restful参考：https://api.github.com/

restful:http://www.ruanyifeng.com/blog/2014/05/restful_api.html

restful编写指南:https://blog.igevin.info/posts/restful-api-get-started-to-write/

restful:https://www.cnblogs.com/artech/p/3506553.html

[状态码参考-英文](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

[状态码参考-中文](http://tool.oschina.net/commons?type=5)



