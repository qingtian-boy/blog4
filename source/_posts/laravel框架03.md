---
title: laravel框架03
date: 2019-03-03 18:44:21
tags: laravel
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识回顾

HTTP请求与响应

Request

​	$request->all();	//接收所有参数

​	$request->input('key');	//接收指定参数

​	$request->key;	//接收指定参数，key代表$_POST['username']

​	$request->isMethod('post');	//判断当前请求是否为POST

​	$request->ajax();	//判断是否为ajax请求

​	$request->url();	//获取url

​	$request->path();	//获取uri

​	$request->port();	//获取端口

​	$request->server();	//获取服务器信息
<!-- more -->
响应

​	Response

​		例如：返回xml数据

​		$response->setContent($data)->header('Content-Type', 'text/xml');

响应跳转

​	redirect('url');	//跳转到指定url地址

​	redirect()->to('url');	//跳转到指定url地址

​	redirect()->back();		//返回上一个页面

会话操作

​	cookie与session

cookie

​	$response->cookie('key', 'value', 'time'[,.....])

​	$request->cookie('key');	//获取指定cookie

​	$request->cookie();		//获取所有cookie

session

​	$request->session()->put('key', 'value');	//设置session

​	$request->session()->all();		//获取所有session

​	$request->session()->get('key');	//获取指定session

​	$request->session()->forget('key')	//删除指定session

一次性的session

​	$request->session()->flash('key', 'value');	//设置一次session

​	$request->session()->get('key');	//获取session

​	$request->flash();		//批量设置一次性session

​	{ { old('key') } }	//获取批量设置的一次性session

DB类

​	DB执行原生SQL

​	DB::select()	//查询

​	DB::insert()	//添加

​	DB::update()	//更新

​	DB::delete()	//删除

DB查询构造器

​	DB::table('tableName')

​	DB::table('tableName')->get();	//查询所有

​	DB::table('tableName')->first();	//查询一条，它永远取出的数据是数据表第一条

​	DB::table('tableName')->where('id', '=', 1)->first();	//根据id查询

​	DB::table('tableName')->find(1);	//注意：表的主键一定是id

​	DB::table('tableName')->insertGetId( array() ); //添加一条，返回值是自增id

​	DB::table('tableName')->insert( array() );	//批量添加，成功返回true

​	DB::table('tableName')->where('id', '=', 1)->update( array() );	//更新数据

​	DB::table('tableName')->delete(1);	//主键名必须是id

​	DB::table('tableName')->where('id', '=', 1)->delete();

DB查询构造的联表查询

​	DB::table('tableName')->join('tableName1', 'tableName1.uid', '=', 'tableName.id')->select('username', 'email')->get();

# 二、Eloquent ORM模型

## 1、什么是ORM模型

> ORM是对象关系映射【Object Relational Mapping】，是一种面向对象的数据库操作数据的技术。
>
> 对象，指代的就是项目中实际的数据模型Model。
>
> 关系，指代的就是关系型数据库的数据表记录。
>
> 映射，是指通过逻辑代码把数据表的字段和模型对象的属性，进行一一对应的关联关系。
>
> 这种技术的核心是把数据表中的每一行数据记录都转换成一个数据对象的属性。
>
> 那么，操作数据对象的属性，就可以直接操作数据表中的每一行数据了。
>
> 简而言之，就是通过面向对象的方式来操作数据。

图示：

![1538734772231](1538734772231.png)

## 2、创建模型(ORM模型)

> Laravel框架中的模型基于DB查询构造器来实现的，所以在我们自定义模型中就算什么方法都不写，也可以直接调用到DB查询构造器里面的任何方法。

> 注意：
>
> ​	模型类名，建议和表名一致，去掉下划线，采用大驼峰写法【第一个单词的首字母大写】。
>
> ​	例如：
>
> ​		表名为：goods_attribute，则模型类名为：GoodsAttribute

语法：

```
php artisan make:model Models\User
```

![1546220264807](1546220264807.png)

![1546220295263](1546220295263.png)

> 注意：
>
> ​	Laravel框架中默认情况下，会自动根据模型的单词+s，作为我们当前操作的模型对应的表名，对于我们来说不太适合，所以我们可以通过模型的属性设置表名。例如：protected $table = 'user';
>
> ![1546220574174](1546220574174.png)
>
> ![1546220640079](1546220640079.png)

## 3、ORM模型的属性

> protected	$table	设置表名
>
> protected	$primaryKey	设置主键字段名
>
> public	$timestamps = false;	关闭模型自动管理数据添加时间和更新时间功能
>
> protected	$fillable = ['字段1', '字段2'...];	//白名单属性，允许修改的字段列表
>
> protected	$guarded = ['字段1', '字段2'...];	//黑名单属性，不需要修改的字段列表
>
> 注意：$fillable属性和$guarded属性是相反的，所以它们不能同时使用。

## 4、查询数据

### 4.1、查询全部数据

#### 控制器代码

![1546220722445](1546220722445.png)

#### 定义路由

![1546220690969](1546220690969.png)

#### 效果

![1546220665698](1546220665698.png)

### 4.2、查询单条数据

#### 控制器代码

![1546221039085](1546221039085.png)

#### 定义路由

![1546221009495](1546221009495.png)

#### 效果

![1546220981333](1546220981333.png)

## 5、添加数据

> 在模型中添加数据可以使用DB查询构造器提供的insert和insertGetId方法。
>
> 除了上面两个方法以外，模型也提供了save和create方法。

```
注意：
	Laravel框架中的模型在添加、编辑数据的时候，会帮我们自动更新数据表中的created_at和updated_at字段的数据。
created_at和updated_at字段的数据类型是timestamp。
添加数据的时候，模型会自动帮我们填写created_at字段的值。
更新数据的时候，模型会自动帮我们填写updated_at字段的值。
所以，当我们使用模型添加、更新数据的时候，如果表中没有created_at和updated_at这两个字段，则直接报错。
如果需要这两字段，则直接在表中新增字段即可。
```

### 5.1、save添加数据

#### 控制器代码

![1546222941224](1546222941224.png)

#### 定义路由

![1546222910726](1546222910726.png)

#### 效果

![1546222867376](1546222867376.png)

![1546222884172](1546222884172.png)

可能会报以下错误

![1546222773981](1546222773981.png)

> 注意：
>
> ​	以上错误信息提示数据表中没有created_at与updated_at字段

解决方案

1、设置不自动维护这两字段

![1546222852371](1546222852371.png)

2、在数据表中增加这两个字段

### 5.2、create添加数据

#### 控制器代码

![1546223599778](1546223599778.png)

#### 定义路由

![1546223626693](1546223626693.png)

#### 效果

![1546223363303](1546223363303.png)

> 注意：
>
> ​	使用ORM模型中的create方法添加数据时必须要填写白名单，否则会报以下错误

![1546223556232](1546223556232.png)

在模型中设置白名单

![1546223532053](1546223532053.png)

## 6、更新数据

> 在ORM模型中更新数据，Laravel框架为我们提供了update与save方法。
>
> update是DB查询构造器的方法，所以在模型这里还是原来的操作方式。

### 6.1、save方法更新数据

#### 控制器代码

![1546223907303](1546223907303.png)

#### 定义路由

![1546223938776](1546223938776.png)

#### 效果

![1546223863621](1546223863621.png)

### 6.2、update方法更新数据

#### 控制器代码

![1546224184831](1546224184831.png)

#### 定义路由

![1546224159549](1546224159549.png)

#### 效果

![1546224105766](1546224105766.png)

## 7、删除数据

> Laravel框架中为我们提供了delete与destroy方法来删除数据

### 7.1、delete方法删除数据

#### 控制器代码

![1546224388814](1546224388814.png)

#### 定义路由

![1546224357462](1546224357462.png)

#### 效果

![1546224334631](1546224334631.png)

### 7.2、destroy方法删除数据

#### 控制器代码

![1546224567931](1546224567931.png)

#### 定义路由

![1546224534624](1546224534624.png)

#### 效果

![1546224506477](1546224506477.png)

## 8、软删除

> 在实际开发过程中，数据是无价的，对于现在的存储空间而言，宁愿多使用硬盘存储数据，也不愿意轻易删除。所以，Laravel框架中的模型内置了一个软删除功能，这个功能的使用，需要准备两样东西。
>
> 1.数据表中必须要有deleted_at字段，字段数据类型是timestamp
>
> ![1546225324206](1546225324206.png)
>
> 2.数据模型文件中，引入软件删除功能类组件softDeleteds

### 1、在模型中引入软件删除类

![1546225409905](1546225409905.png)

### 2、删除数据

#### 控制器代码

![1546225615533](1546225615533.png)

#### 定义路由

![1546225580135](1546225580135.png)

#### 效果

![1546225543999](1546225543999.png)

### 3、获取被软件删除数据

#### 控制器代码

![1546226710493](1546226710493.png)

#### 定义路由

![1546226744940](1546226744940.png)

#### 效果

![1546226765736](1546226765736.png)

> 注意：
>
> ​	一旦开启了软件删除，我们之前所学的所有的查询方式都是只能查询正常的数据，被软删除的数据，查不出来的。
>
> ​	如果想查询所有数据(同时包含被软件删除后的数据)，需要在查询的时候，调用==withTrashed==方法
>
> ​	如果只希望查询被软件删除的数据，则调用==onlyTrashed==即可，正常数据是不会被查询出来。

### 4、恢复被软删除的数据

作业

# 三、关联模型

## 1、简介

​	关联模型主要是通过关系型数据库的主外键【foreign key】来声明两个模型之间的关系，通过模型的关系实现连表查询效果。

​	一个模型对应一个数据表，在关系型数据库中，表与表之间可以因为主外键的关系来确定两个表的关系。

关系：

一 对 一

一 对 多

多 对 多

上面这三种关系，我们也可以通过模型的属性进行关联。

在以往的开发中，我们连表操作都是通过SQL语句的join来实现。

但是，这种实现的方式，在多表查询的时候，**很容易出现字段冲突**。

而Laravel框架中的模型，提供了关联模型的功能，通过这个功能可以实现多表查询，同时还可以完美避免多表之间的字段冲突问题。

## 2、使用关联模型进行多表操作

> 关联模型的操作主要分两步：
>
> 1.在模型文件中声明当前模型与其他模型的关系(一对一、一对多、多对多)
>
> Laravel框架中的模型，提供了以下方法来声明模型的关系
>
> hasOne	用于 一对一、多对一的场合
>
> belongsTo	用于多对一、一对多的场合
>
> hasMany	用于多对一、两个多对一就是多对多
>
> 2.在模型以外其他地方使用模型关系来进行多表关联操作

### 2.1、新建comment模型

```cmd
php artisan make:model Models\Comment
```

![1546227132925](1546227132925.png)

代码如下：

![1546227171977](1546227171977.png)

### 2.2、在user模型中声明关联关系

#### user模型代码

![1546228247340](1546228247340.png)

#### 控制器代码

![1546228283227](1546228283227.png)

#### 定义路由

![1546228198860](1546228198860.png)

#### 效果

![1546228163299](1546228163299.png)

### 2.3、在comment模型中声明关联关系

#### comment模型代码

![1546228653643](1546228653643.png)

#### 控制器代码

![1546228698082](1546228698082.png)

#### 定义路由

![1546228728594](1546228728594.png)

#### 效果

![1546228617730](1546228617730.png)

# 四、综合功能：管理员管理模块

## 1、项目分析

1. 管理员的添加

2. 管理员的列表

3. 管理员的删除

4. 管理员的编辑

5. 管理员的登陆与退出

## 2、初始化项目

### 2.1、新建项目

```cmd
composer create-project --prefer-dist laravel/laravel php33blog 5.4.*
```

![1546238028551](1546238028551.png)

![1546238350595](1546238350595.png)

### 2.2、新建数据库

```sql
CREATE DATABASE `php33blog` CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_general_ci';
```

![1546238076740](1546238076740.png)

### 2.3、配置数据库

![1546238411397](1546238411397.png)

### 2.4、删除一些不必要的文件

Laravel内置的欢迎页面 resource/views/welcome.blade.php
Laravel内置的Auth控制器目录  app\Http\Controllers\Auth目录

### 2.5、配置智能提示插件

https://packagist.org/packages/barryvdh/laravel-ide-helper

#### 2.5.1、下载插件

```cmd
composer require barryvdh/laravel-ide-helper ^2.4.3
```

![1546238567517](1546238567517.png)

#### 2.5.2、 配置config/app.php

```php
'providers' => [
    // ...
    Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
],
```

![1546238615619](1546238615619.png)

#### 2.5.3、生成_ide_helper.php文件

```cmd
php artisan ide-helper:generate
```

![1546238657663](1546238657663.png)

### 2.6、运行项目

```cmd
php artisan serve
```

![1546238710745](1546238710745.png)

## 3、完成后台首页功能

### 3.1、新建控制器

```cmd
php artisan make:controller Admin\IndexController
```

![1546238896186](1546238896186.png)

代码如下：

![1546238963676](1546238963676.png)

### 3.2、定义路由

![1546239088361](1546239088361.png)

### 3.3、视图

#### 3.3.1、复制视图文件

将视图文件复制到resources/views/admin/index目录下，并修改后缀为.blade.php

![1546239200394](1546239200394.png)

#### 3.3.2、复制静态资源文件

将静态资源文件复制到public/back目录。注意：没有的目录需要手动创建

![1546239529808](1546239529808.png)

#### 3.3.3、修改静态资源路径

![1546239905678](1546239905678.png)

效果如下：

![1546239922926](1546239922926.png)

## 4、完成欢迎页面功能

### 4.1、定义方法

![1546240032346](1546240032346.png)

### 4.2、定义路由

![1546240102858](1546240102858.png)

### 4.3、视图

#### 4.3.1、复制视图文件

将视图文件复制到resources/views/admin/index目录下，并修改后缀为.blade.php

![1546240196530](1546240196530.png)

#### 4.3.2、修改静态资源路径

![1546240292225](1546240292225.png)

效果如下：

![1546240303178](1546240303178.png)

#### 4.3.3、修改后台首页

修改resources/views/admin/index/index.blade.php文件，代码如下

![1546240397409](1546240397409.png)

效果如下：

![1546240376296](1546240376296.png)

## 5、显示管理员列表页

> 开发步骤
>
> 1. 数据表的创建
>
> 2. 创建控制器【资源控制器】
>
> 3. 创建数据模型【模型】
> 4. 定义路由
> 5. 视图

### 1、创建数据表

```sql
CREATE TABLE `admin`  (
  `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `username` varchar(150) NOT NULL COMMENT '登录帐号',
  `password` varchar(255) NOT NULL COMMENT '登录密码',
  `avatar` varchar(255) NULL DEFAULT NULL COMMENT '头像',
  `sex` tinyint(4) NULL DEFAULT 1 COMMENT '性别(1:男,2:女)',
  `nickname` varchar(100) NULL DEFAULT NULL COMMENT '昵称',
  `phone` char(15) NOT NULL COMMENT '手机号码',
  `email` varchar(150) NOT NULL COMMENT '邮箱地址',
  `role_id` tinyint(4) NULL DEFAULT NULL COMMENT '角色ID',
  `note` text NULL COMMENT '备注',
  `created_at` timestamp NULL DEFAULT NULL COMMENT '创建时间',
  `updated_at` timestamp NULL DEFAULT NULL COMMENT '更新时间',
  `deleted_at` timestamp NULL DEFAULT NULL COMMENT '删除时间',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT '禁用时间',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `username`(`username`) USING BTREE,
  UNIQUE INDEX `phone`(`phone`) USING BTREE,
  UNIQUE INDEX `email`(`email`) USING BTREE
) ENGINE = InnoDB COMMENT = '管理员表';
```

![1546240478795](1546240478795.png)

### 2、创建控制器(资源控制器)

```cmd
php artisan make:controller Admin\AdminController --resource
```

![1546240518330](1546240518330.png)

代码如下：

![1546240549068](1546240549068.png)

```
Laravel会为资源控制器创建7个方法，用于操作数据的增删查改；资源控制器就是为了解决命名不统一的问题而设置一种遵循了restFul接口编程命名规范的方法名。
index	用于展示数据列表【只能使用get方式访问】
show	用于展示一条数据【只能使用get方式访问】
create	用于显示添加表单页面【只能使用get方式访问】
store	用于添加(保存)数据【只能使用post方式访问，接收create页面post提交过来的数据】
edit	用于显示编辑表单页面【只能使用get方式访问】
update	用于更新数据【只能使用put方式访问，接收edit页面post提交过来的数据】
destroy	用于删除数据【只能使用delete方式访问，用于删除数据】
```

### 3、创建模型

```cmd
php artisan make:model Models\Admin
```

![1546240599831](1546240599831.png)

代码如下：

![1546240909397](1546240909397.png)

### 4、定义路由(声明资源路由)

```php
语法：Route::resource('地址', '控制器类名');
```

> 注意：
>
> ​	第二个参数不需要加方法名称

![1546240974113](1546240974113.png)

当我们在路由文件中声明了资源路由，则Laravel会自动生成7个对应着资源控制器的路由地址，通过在命令行中使用 **php artisan route:list** 可以查看：

![1546241008828](1546241008828.png)

### 5、视图

#### 5.1、复制视图文件

复制到resources/views/admin/admin目录中。注意：如果目录不存在，需要手动创建。

![1546241090774](1546241090774.png)

#### 5.2、修改静态资源路径

![1546241244925](1546241244925.png)

#### 5.3、载入视图文件

修改app/Http/Controllers/Admin/AdminController.php文件中的index方法，代码如下：

![1546241152048](1546241152048.png)

效果如下：

![1546241279029](1546241279029.png)

#### 5.4、修改添加管理员按钮

![1546242266075](1546242266075.png)

## 6、完成管理员添加功能

> 开发步骤
>
> 1. 控制器代码
>
> 2. 视图
>
> 3. 控制器中接收表单信息
>    1. 验证数据【Validation验证类验证数据】
>    2. 文件上传
>
> 4. 控制器中调用模型保存数据
>    1. 验证数据成功，则保存数据，并跳转到管理员列表
>    2. 验证数据失败，则返回错误信息，并跳转到上一页

### 1、控制器代码

app/Http/Controllers/Admin/AdminController.php

![1546242303044](1546242303044.png)

### 2、视图

#### 2.1、复制视图文件

复制视图文件到resources/views/admin/admin目录中，并重命名

![1546242388782](1546242388782.png)

#### 2.2、修改静态资源路径

![1546242657458](1546242657458.png)

效果如下：

![1546242674559](1546242674559.png)

#### 2.3、调整添加管理员的表单信息

新增两个输入框，分别是昵称和头像，模板代码如下：

![1546242842841](1546242842841.png)

效果

![1546242858798](1546242858798.png)

#### 2.4、修改表单name属性与form提交 

![1546243051699](1546243051699.png)

![1546243119726](1546243119726.png)

![1546243219093](1546243219093.png)

#### 2.5、在form表单中设置token值

> 注意：
>
> ​	Laravel框架中除了get请求类型不需要设置token值，其它都必须设置，可以使用以下两种方法：
>
> ​	{ {csrf_field()} }
>
> ​	<input type="hidden" name="_token" value="{ {csrf_token()} }">

### 3、控制器中接收表单信息

> 默认情况下，我们使用工匠指令生成的控制器都已经自动引入了Request请求操作类，所以我们直接使用all方法接收所有参数即可。

#### 3.1、控制器代码

app/Http/Controllers/Admin/AdminController.php

![1546243402291](1546243402291.png)

效果

![1546243412889](1546243412889.png)

#### 3.2、验证数据[Validation安装包验证数据]

> 使用Validation验证数据，有三种方式：
>
> 1. 通过当前控制器对象的validate方法来使用
>
> 2. 通过Validator静态类来使用(我们这里使用第二种)
>
> 3. 通过Request请求操作类来使用(第三种验证方式的时候，类似于第二种)

##### 3.2.1、控制器代码

![1546244368528](1546244368528.png)

![1546244403919](1546244403919.png)

##### 3.2.2、视图

使用layer弹框显示错误信息，官网地址：http://layer.layui.com

resources/views/admin/admin/add.blade.php

![1546245221124](1546245221124.png)

提交表单数据时，有可能报以下错误：

![1546244514165](1546244514165.png)

> 注意：
>
> ​	因为没有引入jquery.form.js文件

引入jquery.form.js文件

![1546244644057](1546244644057.png)

效果如下：

![1546244655134](1546244655134.png)

#### 3.3、数据入库

引入模型类

![1546246513791](1546246513791.png)

数据入库

![1546246555522](1546246555522.png)

效果如下：

![1546246481243](1546246481243.png)

# 五、总结

自己总结