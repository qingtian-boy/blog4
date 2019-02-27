---
title: mongodb
date: 2019-02-19 18:44:34
tags: mongodb
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识点回顾

能够记住redis的5种数据结构

​	string、hash、list、set(无序集合)、zset(有序集合)

能够运用set、get、incrby、decrby、del命令

​	操作string

​		set key value

​		get key

​	incrby 作用：自增N

​		incrby key n

​		注意：对已经存在key进行，key 的类型必须数字(int)类型，如果不存在会新增

​	incr 作用：自增1

​		incr key 

​		注意：对已经存在key进行，key 的类型必须数字(int)类型，如果不存在会新增

​	decrby 作用：自减 N

​		decrby key n

​	注意：对已经存在key进行，key 的类型必须数字(int)类型，如果不存在会新增

​	decr 作用：自减 1

​		decr key
<!-- more -->
​	注意：对已经存在key进行，key 的类型必须数字(int)类型，如果不存在会新增

能够运用hset、hmset、hmget、hgetall、hdel命令

​	hset key field value	作用：单个设置

​	hmset key field value field value field value ..... 作用：批量设置

​	hmget key field field ...	作用：获取hash表指定字段的值

​	kgetall key	作用：获取hash表所有值

​	hdel key field field ...	作用：删除hash表中的字段

能够运用lpush、lpop、rpush、rpop命令

lpush key value [value ...]	作用：从左压入队列(栈)

lpop key 作用：从左弹出队列(栈)第一个元素

rpush key value [value ...]	作用：从右压入队列

rpop key 	作用：从右弹出队列第一个元素

能够运用sadd、zadd命令(集合)

​	sadd 无序集合

​	sadd key value [value ...]

​	zadd 有序集合

​	zadd key socre value [socre value ...]

能够运用 keys、exists、select、ping、auth命令

keys * 代表查看所有key

keys key 代表查看当前key

select number 切换哪个数据库

ping 使用客户端向 Redis 服务器发送一个 PING ，如果服务器运作正常的话，会返回一个 PONG 

auth 作用：CONFIG SET requirepass secret_password   # 将密码设置为 secret_password

能够了解快照持久化与AOF持久化的原理

快照只是存储到硬盘中

AOF 存储方式：内存+硬盘

# 二、MongoDB 简介

中文开发文档：http://www.mongodb.org.cn/ 

官方：https://www.mongodb.com/

## 1、MongoDB

​	MongoDB是一个介于关系型数据库和非关系型数据库之间的产品，是非关系型数据库当中功能最丰富，最像关系型数据库，但是 MongDB 做不到关系型数据库的连表，外键等操作，它的存储数据方式有点类似于 JSON 格式，MogoDB叫做 Bson（big json）格式。

MongoDB是一个面向集合的，模式自由的文档型数据库。

MongoDB能很好地支持PHP。

MongoDB安全性是所有NoSql最好的。

MongoDB安装的文件比较大，占据了一定的硬盘空间。

## 2、MongoDB 与 MySQL 性能比较

下面是官方的 bench-mark 数据测试数据：

1、分别插入 100 万条记录，并对其做 100 个用户并发查询操作

![1543130214842](1543130214842.png)

MongoDB 分片(把大数据拆分为小数据进行复制)数据测试结论：138M/s

![1545269427078](1545269427078.png)

![1545269578005](1545269578005.png)

## 3、应用范围和限制

缺点：不支持连表查询，不支持 sql 语句，不支持事务存储过程等，所以不适合存储数据关系比较复杂的数据，一般主要是当做一个==数据仓库==来使用。

适用于：日志系统，股票数据等。

不使用于：电子商务系统等需要连多表查询的功能。

国内职场中应用 MongoDB 最广泛的网站(PHP+MongoDB)：

youku(优酷)、tudou(土豆)

# 三、需要掌握的几个点

==注意：==

​	==下文中出 shell># 代表着 Linux的命令窗口，MongoDB>代表着已经登陆了MongoDB==

## 1、文档

​	==文档==是 MongoDB 中数据的==基本单元==，类似==关系型数据库的行==，多个键值对有序地放置在一起便是文档。

MongoDB 中以文档的方式存取记录，如一条记录格式如下：

```json
{'username':'php33', 'age':18, email:'123456.gd@163.com', sex:'男'}
```

## 2、集合(表)

集合就是一组文档，==多个文档组成一个集合，集合类似于 MySQL 里面的表。==

模式自由（schema-free）：==意思是集合里面没有行和列的概念。==

```json
MongoDB> db.class.insert( { 'username':'php33', 'age':18 } )
MongoDB> db.class.insert( { 'username':'php34' } )
```

> 注意：
>
> ​	MongoDB 中的==集合不用创建、没有数据结构==，所以可以放不同格式的文档。

## 3、数据库

多个集合可以组成数据库。一个MongoDB实例可以承载多个数据库，它们之间完全独立。

MongoDB 中的数据库和 MySQL 中的数据库概念类似，只是==无需创建==。

==一个数据库中可以有多个集合。==

==一个集合中可以有多个文档(MongoDB里面的每一个文档，代表mysql的数据表中的每一行数据)。==

# 四、安装MongoDB

## 1、安装

### 1.1、创建仓库(yum 源)

```shell
shell># vim /etc/yum.repos.d/mongodb-org-3.4.repo
```

![1545270632088](1545270632088.png)

### 1.2、把下面的内容复制到文件中 保存退出

```shell
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

![1545270830675](1545270830675.png)

![1545270813392](1545270813392.png)



### 1.3、yum安装 如图安装完成

```shell
shell># yum install -y mongodb-org
```

![1545270917034](1545270917034.png)

安装成功后如下图所示：

![1545271915890](1545271915890.png)

## 2、启动 MongoDB

```shell
shell># service mongod start	# 启动
shell># service mongod stop		# 停止
shell># service mongod restart	# 重启
shell># service mongod status	# 查看状态
```

![1545272000718](1545272000718.png)

# 五、MongoDB 的数据库相关命令

## 1、mongo 命令

命令功能：用于登录 MongoDB 的命令行

命令格式：`mongo [-u 登陆名][-p 登陆密码][ip:端口/数据库]`

例如：mongo -uroot -p123456 localhost:27017/admin

```shell
# 连接 mongodb
shell># mongo -uroot -p123456 localhost:27017/admin
```

![1545272270723](1545272270723.png)

登陆成功后如下图所示：

![1545272369804](1545272369804.png)

> 注意：
>
> ​	MongoDB 默认是没有用户与密码
>
> ![1545272231820](1545272231820.png)

## 2、show dbs 命令

命令作用：查看所有数据库

相当于 MySQL 中的 show databases(); 命令

```mongodb
# 显示所有数据库
MongoDB> show dbs;
```

![1545272619102](1545272619102.png)

> 说明：
>
> ​	local 是本地系统数据库，默认是显示的，它不能被删除，否则 MongoDB会直接崩溃
>
> ​	其实，还有一个 admin 管理员数据库，默认是隐藏的
>
> ​	show databases;  或 show dbs;

## 3、use 命令

命令作用：选择/创建数据库

例如：use php33

> 说明：
>
> ​	如果 php33数据库存在，use 命令就是相当于MySQL中的 use 命令，选择数据库。
>
> ​	如果 php33数据库不存在，use 命令就是相当于MySQL中的 create database php33命令。

案例1：选择 admin 数据库

```mongodb
# 选择 admin 数据库，因为 admmin 这个数据库本身就存在
MongoDB> use admin
```

![1545272959598](1545272959598.png)

案例2：创建 php33数据库

```mongodb
# 创建 php33 数据库
MongoDB> use php33
```



## 4、db 命令

命令作用：显示当前正在操作的数据库

说明：相当于 MySQL 中的 select database(); 命令。

```mongodb
# 查看当前操作的数据库
MongoDB> db
```

![1545273746390](1545273746390.png)

## 5、show tables 命令

命令作用：显示当前数据库的集合(表)

```mongodb
# 选择数据库，如果 php33 不存在，则创建，存在则选择
MongoDB> use php33
# 添加数据 class就相当于 mysql 数据库中的表名
MongoDB> db.class.insert({name:'long', age:18})
# 显示所有表名(集合)
MongoDB> show tabels
```

![1550545702326](1550545702326.png)

也可以使用 show collections 来替代，效果如下：

```mongodb
# 显示所有表名(集合)
MongoDB> show collections
```

![1545273975283](1545273975283.png)

## 6、db.help() 命令

命令作用：显示数据库的帮助文档信息

```mongodb
# 显示帮助文档
MongoDB> db.help()
```

![1545274203545](1545274203545.png)

## 7、db.dropDatabase() 命令

命令作用：删除当前正在操作的数据库

==注意：删除数据库会让集合和文档全部丢失==

```mongodb
# 删除数据库
MongoDB> db.dropDatabase()
```

![1545274645062](1545274645062.png)

==注意：绝对不能删除 local 数据库，因为这是一个系统数据库，删除则 MongoDB崩溃==

## 8、db.集合名称.help() 命令

命令作用：显示当前操作数据库中的集合相关帮助文档信息

例如：显示 php33数据库中 class1 这个集合帮助信息

```mongodb
# 显示集合帮助指令
MongoDB> db.class1.help()
```



# 六、在 MongoDB 中实现聚合查询

## 1、insert 命令

命令格式：db.集合名称.insert({ bson 数据 });

集合名称：在 MongoDB 中集合相当于MySQL中的表，这个表是==无需创建==，如果表==不存在就会自动创建==；如果存在就是选择

案例1：在 php33中创建一个集合为：class2

```mongodb
# 创建 class2 集合
MongoDB> db.class2.insert({name:'dachui', age:12})
```

![1550546357132](1550546357132.png)

添加会默认创建一个_id的字段，并且是对象类型

```mongodb
# 查询所有数据
MongoDB> db.class2.find()
```

![1545275959686](1545275959686.png)

案例2：定义一个 _id 为 300 的文档(记录)

```mongodb
# 自定义 _id 的值为 300
MongoDB> db.class2.insert({_id:300, name:'php33', age:6})
```

![1550546564479](1550546564479.png)

案例3：在 class3 集合插入 1000 条学生记录

```mongodb
# 批量添加数据到 class3 集合中
MongoDB> for(var i=1; i<=1000; i++){ db.class3.insert( {_id:i,name:'sut'+i,lesson:'Laravel'} ) }
```

![1545276302619](1545276302619.png)

查看结果如下：

```mongodb
# 查看所有数据
MongoDB> db.class3.find()
```

![1545276348089](1545276348089.png)

![1545276397668](1545276397668.png)

## 2、find() 命令

命令格式：db.集合名称.find({条件})[.limit().skip().count(true)]

案例1：查询 class3 集合中 _id 大于 997 的信息记录

==口诀：比较操作符一定在字段之内，$set 修改器一定在字段以外，$or 和 $and 必须使用[]==

```mongodb
# 相当于编写： select * from class3 where _id > 997
# 按条件查询数据
MongoDB> db.class3.find( {_id:{'$gt':997}} )
```

![1545276870479](1545276870479.png)

案例2：查询 class3 中 _id 小于或者等于 10 的文档

```mongodb
# 相当于编写：select * from class3 where _id <=10
MongoDB> db.class3.find( {_id:{'$lte':10}} )
```

![1545277115173](1545277115173.png)

案例3：查询 class3 中 _id 小于等于 5 的文档，并且只显示前面的 3 条记录

```mongodb
# 相当于编写：select * from class3 where _id <=5 limit 3
MongoDB> db.class3.find( {_id:{'$lte':5}} ).limit(3)
```

![1545277305450](1545277305450.png)

案例4、查询 _id 小于等于 5 的文档，跳过前面 2 条记录，显示后面的 3 条记录

```mongodb
# 相当于编写：select * from class3 where id<=5 limit 2,3
MongoDB> db.class3.find( {_id:{'$lte':5}} ).skip(2).limit(3)
或者
MongoDB> db.class3.find( {_id:{'$lte':5}} ).skip(2)
```

![1545277509133](1545277509133.png)

![1545277653654](1545277653654.png)

案例5、统计 class3 集合有多少数据(文档)

```mongodb
# 相当于编写：select count(*) from class3
MongoDB> db.class3.find().count(true)
```

![1545277825401](1545277825401.png)

案例6、查询 _id 小于等于7的文档，并且跳过前面 3 条记录，统计当前查询出多少条记录(个文档)

```mongodb
# 相当于编写：select count(*) from class3 where _id <= 7 limit 3,4
MongoDB> db.class3.find( {_id:{'$lte':7}} ).skip(3).limit(4).count(true)
```

![1545278229397](1545278229397.png)

> 注意：
>
> ​	如果在 MongoDB 中没有使用 count(true)，是无法把 skip 和 limit 看成一个条件
>
> ![1545278590277](1545278590277.png)

## 3、使用 $or 操作符

符号的作用：相当于 MySQL 当中 or 操作符

 案例1：查询 _id 为1 或者 _id 大于或等于 997 的文档

```mongodb
# 相当于编写：select * from class3 where _id=1 or _id>=997
MongoDB> db.class3.find( {'$or':[{_id:1},{_id:{'$gte':997}}]} )
```

![1545288313734](1545288313734.png)

案例2：查询 _id = 10 或者 name = stu99 或者 _id = 100 的记录(文档)

```mongodb
# 相当于编写：select * from class3 where _id=10 or name='stu99' or _id=100
MongoDB> db.class3.find( {'$or':[{_id:10},{name:'stu99'},{_id:100}]} )
```

![1545288515340](1545288515340.png)

案例3：查询 _id 小于等于 5 或者 name 为 stu100 文档

```mongodb
# 相当于编写：select * from class3 where _id<=5 or name='stu100'
MongoDB> db.class3.find( {'$or':{_id:{'$lte':5}},{name:'stu100'}} )
```

![1545288655860](1545288655860.png)

## 4、使用 $and 操作符

符号的作用：相当于 MySQL 中 and 操作符

```mongodb
# 相当于编写：select * from class3 where name='stu100' and lesson='Laravel'
MongoDB> db.class3.find( {'$and':[{name:'stu100'},{lesson:'Laravel'}]} )
```

![1545288885222](1545288885222.png)

## 5、把文档进行排序操作，使用 sort() 命令

语法规则：db.集合名称.find().sort( {字段:-1/1} )

参数说明：

- 1：代表升序排列，相当于 asc 的操作，默认为 asc
- -1：代表降序排列，相当于 desc 的操作

案例：查找 _id 小于 10 的文档，按照 _id 字段降序(从大到小)排列

```mongodb
# 相当于编写：select * from class3 where _id<10 order by _id desc
MongoDB> db.class3.find( {_id:{'$lt':10}} ).sort( {_id:-1} )
```

![1545289209616](1545289209616.png)

## 6、update 命令和 $set 修改器

命令格式：db.集合名称.update( {条件},{'$set':{字段名:值}}, false, true )

> 说明：
>
> ​	第3个参数表示关闭只修改单行记录功能，false 表示修改可以发生在多行记录中，默认值为：true
>
> ​	第4个参数表示启动批量修改功能，默认值为：false

案例：修改 _id < 7 的数据 lesson 为Tp5

```mongodb
# 相当于纺车：update class3 set lesson='Tp5' where _id<7
MongoDB> db.class3.update( {_id:{'$lt':7}},{'$set':{lesson:'Tp5'}}, false, true )
```

![1545289769521](1545289769521.png)

## 7、remove() 命令

语法规则：db.集合名称.deleteMany( {条件} )

注意：如果条件为空，则代表删除集合所有的文档

案例1：删除 class2 集合中的 name='dachui' 的数据

```mongodb
# 相当于编写：delete from class2 where name='dachui'
MongoDB> db.class2.deleteMany( {name:'dachui'} )
或
MongoDB> db.class2.remove( {name:'dachui'} )
```

![1545290209931](1545290209931.png)

案例2：删除 class2 中所有数据(文档)

```mongodb
# 相当于编写：delete from class2
MongoDB> db.class2.deleteMany({})
或
MongoDB> db.class2.remove({})
```

![1545290413366](1545290413366.png)

![1545290571640](1545290571640.png)

## 8、$in 操作符

$in 操作相当于 MySQL 中 in 操作语句，不过 in 在 MongoDB 中是非常快速，因为 $in 一定可以使用上索引，语法规则如下：

db.集合名称.find( {字段名:{'$in':[...]}} )

案例：查询 _id 为 33，55，77，99 的记录

````mongodb
# 相当于编写：select * from class3 where _id in(33,55,77,99)
MongoDB> db.class3.find( {'_id':{'$in':[33,55,77,99]}} )
````

![1545291014014](1545291014014.png)

# 七、MongoDB 中索引和执行计划

索引目的提高查询的效率，如果在查询中能够使用上定义的索引就表示就是一个速度很快的查询

## 1、创建索引

语法格式：db.集合名称.ensureIndex( {字段:1},{索引的属性} )

{字段:1}：表示在某个字段中设置索引，1 表示 true

案例1：为 class2 的 name 字段添加普通索引

```mongodb
# 添加普通索引
MongoDB> db.class2.ensureIndex( {name:1} )
```

![1545292039895](1545292039895.png)

案例2：为 class3 的 name 字段添加唯一性索引

```mong
# 添加唯一索引
MongoDB> db.class3.ensureIndex( {name:1},{unique:true} )
```

![1545292396966](1545292396966.png)

测试唯一性索引的约束功能：

```mongodb
MongoDB> db.class3.insert({_id:1, name: 'stu7', lesson:'stu7'})
```

![1545292554281](1545292554281.png)

## 2、执行计划 explain

执行计划是查看一个 find() 是否可以使用上索引

语法格式：db.集合名称.find( {条件} ).explain()

```mongodb
# 查看是否使用索引
MongoDB> db.class3.find({name:'sut101'}).explain()
```

 使用到索引如下图：

![1545292866158](1545292866158.png)

没有使用到索引如下图：

```mongodb
# 查看是否使用索引
MongoDB> db.class3.find({lesson:'Tp5'}).explain()
```

![1545293101299](1545293101299.png)

# 八、与索引相关的其他指令

## 1、getIndexes() 命令

命令的作用：用于查询一个集合当中的索引有哪些

案例：查询 class3 中使用了哪些索引

```mongodb
# 查看使用了什么索引
MongoDB> db.class3.getIndexes()
```

![1545293264905](1545293264905.png)

## 2、dropIndex()命令

命令作用：删除集合中指定的索引

案例：删除 class3 中的索引，索引名称 name_1 的索引

```mongodb
# 删除索引
MongoDB> db.class3.dropIndex( "name_1" )
```

![1545293368187](1545293368187.png)

![1545293412955](1545293412955.png)

# 九、MongoDB 的安全权限验证

MongoDB号称世界上 NoSQL 产品中最安全的产品，MongoDB 拥有权限验证机制和加密功能，如果希望启动MongoDB 的安全验证，要遵循以下步骤的顺序：

## 1、切换到隐藏数据库 admin 当中

```mongodb
MongoDB> use admin
```

![1545293509270](1545293509270.png)

## 2、添加用户并指定权限

```mongodb
MongoDB> db.createUser({user:"root",pwd:"123456",roles:[{role:"root",db:'admin'}]})
```

![1545293937898](1545293937898.png)

> db.createUser({user:<username>,pwd:< password>,roles:[{role:<root>,db:<dbName>}]})
>
> 参数说明：
>
> ​	<username>：用户名
>
> ​	<password>：密码
>
> ​	<root>：权限
>
> ​	<dbName>：数据库名

==注意：密码必须要用引号引住==

当我们添加完用户后，会出现一个system.users 的集合，专门用于存放 MongoDB 的管理员数据

```mongodb
MongoDB> show tables
```

![1545294211386](1545294211386.png)

查询该集合，发现 root 用户存放在该集合当中

```mongodb
MongoDB> db.system.users.find()
```

![1545294226817](1545294226817.png)

## 3、使用 exit 命令退出 MongoDB

```mongodb
MongoDB> exit
```



建议为了包含 Mongodb.conf 不被其他用户修改，所以要停止

```shell
shell># service mongod stop
```

![1545294318114](1545294318114.png)

## 4、修改 /etc/mongodb.conf 文件

### 4.1、打开

```shell
shell># vim /etc/mongodb.conf
```

![1545294351709](1545294351709.png)

### 4.2、找security选项

中的#去掉，并在其下一行，空两个空格，加上`authorization: enabled`，如下

![1545294493374](1545294493374.png)

保存并退出（:x）

### 4.3、重启 MongoDB服务

```shell
shell># service mongod start
```

![1545294526648](1545294526648.png)

## 5、重新登陆

```shell
shell># mongo
```

![1545294590691](1545294590691.png)

但是无法操作，代表验证权限开启成功，如果希望正常操作则需要使用mogo命令的权限验证功能：

```shell
shell># mongo -uroot -p123456 localhost:27017/admin
```

![1545294695743](1545294695743.png)

# 十、修改 root 密码的方法

更新运行该方法的数据库上的用户配置文件。对字段的更新完全取代了先前字段的值。这包括对用户roles阵列的更新。

```
语法：db.updateUser(username，update[pwd:密码，writeConcern])
```

案例：将 root 用户密码修改为：654321

```mongodb
MongoDB> db.updateUser('root',{pwd:'654321',roles:[{role:'root',db:'admin'}]})
```

![1545294918493](1545294918493.png)

重新登陆

```shell
shell># mongo -uroot -p123456 localhost:27017/admin
```

![1545294984479](1545294984479.png)

> 注意：
>
> ​	登陆成功后，密码被修改了，如果用户没有退出，还是可以继续操作MongoDB

# 十 一、PHP 中开启 MongoDB 扩展

## 1、安装

### 1.1、下载

下载地址：https://pecl.php.net/package/mongodb

```shell
shell># wget https://pecl.php.net/get/mongodb-1.5.3.tgz
```

### 1.2、上传到服务器

可以使用 rz 上传或 ftp 上传

> 注意：必须得安装了 vsftpd  或  lrzsz，我们这里以 rz 为例

### 1.3、解压与进入目录

```shell
shell># tar -zxvf mongodb-1.5.3.tgz
shell># cd mongodb-1.5.3
```

![1545295950675](1545295950675.png)

![1545295972696](1545295972696.png)

### 1.4、生成编译文件

```shell
shell># /usr/local/php/bin/phpize
```

![1545296007456](1545296007456.png)

### 1.5、进行软件配置和环境检测

```shell
shell>#./configure --with-php-config=/usr/local/php/bin/php-config
```

![1545296096087](1545296096087.png)

### 1.6、编译软件并且进行安装

```shell
shell># make && make install
```

![1545296135106](1545296135106.png)

成功如下图所示：

![1545296263204](1545296263204.png)

## 2、配置

### 2.1、修改 php.ini 加载 MongoDB 组件

```shell
shell># vim /usr/local/php/etc/php.ini
```

![1545296332588](1545296332588.png)

### 2.2、结束PHP进程

```shell
shell># pkill  -9  php-fpm
```

![1545296355144](1545296355144.png)

### 2.3、重新启动PHP进程

```shell
shell># service php-fpm start
```

![1545296374813](1545296374813.png)

### 2.4、检查是否添加成功

### 在站点目录下创建一个phpinfo.php文件，输入以下内容

![1545296427123](1545296427123.png)



# 十二、PHP 操作MongoDB

![1545296806764](1545296806764.png)

## 1、使用php链接Mongodb

开发文档：http://php.net/manual/zh/book.mongodb.php

代码参看：code/client.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

![1545297061688](1545297061688.png)

效果如下：

![1545297072099](1545297072099.png)

## 2、在mongodb中查询数据

代码参看：code/findAll.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

代码如下图所示：

![1545297931901](1545297931901.png)

效果如下图所示：

![1545297954876](1545297954876.png)

## 3、在mongodb中使用操作符

### 3.1、案例1

查询 class3 中 _id 大于或等于 996 的文档

代码参看：code/findWhere.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

代码如下图所示：

![1545298318773](1545298318773.png)

效果如下图所示：

![1545298337783](1545298337783.png)

### 3.2、案例2

查询 class3 中 _id >= 998 或者 name = stu88 的文档，且把文档按照降序(desc)排列

代码参看：code/findOr.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

代码如下图所示：

![1545298757704](1545298757704.png)

效果如下图所示：

![1545298740797](1545298740797.png)

### 3.3、案例3

修改 class3 中 _id 小于或等于 9 的记录中的 lesson 字段的值为 swoole

代码参看：code/update.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

修改前：

```mongodb
MongoDB> db.class3.find({_id:{'$lte':9}})
```

![1545299300349](1545299300349.png)

代码如下图所示：

![1545299905867](1545299905867.png)

效果如下图所示：

![1545299791078](1545299791078.png)

修改成功后：

```mongodb
MongoDB> MongoDB> db.class3.find({_id:{'$lte':9}})
```

![1545299933586](1545299933586.png)

### 3.4、案例4(作业)

http://php.net/manual/zh/book.mongodb.php

删除 _id 为 11,33,55 的数据

代码参看：code/delete.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

```
MongoDB> db.class3.remove({_id:{'$in':[11,33,55]}})
```

代码如下图所示：



效果如下图所示：

```mongodb
MongoDB> db.class3.find({_id:{'$in':[11,33,55]}})
```



# 十三、总结

登陆mongoDB使用什么指令

```shell
shell># mongo
#设置密码使用什么方式？
shell># mongo -u<username> -p<password> ip:port/databaseName
```

查看所有数据库

​	show dbs

进入/创建使用什么指令？

​	use databaseName

显示当前操作的数据库？

​	db

查看所有集合(表)？

​	show tables

查看指令帮助信息？

​	db.help()

​	db.集合名称.help()

删除当前的数据库

 	db.dropDatabase()

例如：在class4集合中添加一条数据

​	db.class4.insert( {name:'long', age:8} )

修改数据

​	db.class4.update({_id:8},{'$set':{age:18}})

删除除数据

​	db.class4.remove({_id:8})

查看所有数据

​	db.class4.find()

排序

​	db.class4.find().sort( {_id:-1} ) //降序

