---
title: MySQL优化02
date: 2019-02-25 18:19:22
tags: MySQL
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识点回顾

**能够说出慢日志查询的作用**

​	MySQL的慢查询日志是MySQL提供的一种日志记录，它用来记录在MySQL中响应时间超过阀值的语句，具体指运行时间超过long_query_time值的SQL。默认情况下，MySQL数据库并不启动慢查询日志，需要我们手动来设置这个参数，当然，不是调优需要的话，一般不建议启动该参数，因为开启后会影响性能。

**能够说出SQL语句缓存的作用**

​	缓存，减少对sql语句的访问
<!-- more -->

## 3、在PHP开启Sphinx的扩展

#### 3.1、安装 libsphinxclient

```shell
shell># cd ~
shell># cd /root/coreseek-3.2.14/csft-3.2.14/api/libsphinxclient
shell># ./configure --with-php-config=/usr/bin/php-config --with-sphinx=/usr/local/libsphinxclient
shell># make
shell># make install
```

### 3.2、安装 PHP 的扩展包

下载地址：http://git.php.net/?p=pecl/search_engine/sphinx.git;a=shortlog;h=refs/heads/php7

```shell
shell># cd ~
shell># cd /root/sphinx-1.3.3
shell># /usr/local/php/bin/phpize
shell># ./configure --with-php-config=/usr/local/php/bin/php-config
shell># make
shell># make install
```

### 3.3、修改 php.ini 配置

```shell
shell># vim /usr/local/php/etc/php.ini
#查找 extension_dir = "ext"
```

![1551058690163](1551058690163.png)

必须重启 php-fpm或apache

```shell
shell># pkill -9 php-fpm
shell># service php-fpm start
```

### 3.4、PHP操作 sphinx

![1551062368464](1551062368464.png)

```php
  1 <?php
  2         # 实例化对象
  3         $client  = new SphinxClient();
  4         # 设置服务器
  5         $client->SetServer("127.0.0.1", 9312);
  6         # 设置匹配规则
  7         $client->setMatchMode( SPH_MATCH_EXTENDED2 );
  8         # 搜索关键词
  9         $data = $client->Query('中心');
 10         $ids = array_keys( $data['matches'] );
 11         $ids = implode( ',', $ids );
 12         # 连接数据库
 13         $pdo = new PDO('mysql:host=127.0.0.1;dbname=text;charset=utf8;port==3306','root','123456');
 14         # 组装 SQL 语句
 15         $sql = "select * from documents where id in( $ids )";
 16         var_dump( $sql );exit;
 17         #$sql = "select * from documents where id in( ? )";
 18         # 执行SQL
 19         $query = $pdo->prepare( $sql );
 20         #$query->execute( array( $ids ) );
 21         $query->execute( );
 22         # 获取结果集
 23         $data =  $query->fetchAll( PDO::FETCH_ASSOC );
 24         var_dump( $data );
```

# 二、数据库三范式(3NF)和逆范式

​	在上个世纪==埃德加·科德博士==于1970年提出了一个名为==[科德十二定律]==的理论奠定了关系模型的基础，后来科德博士在 IBM 公司与相关的开发人员开发了闻名于全世界的关系型数据库 DB2，并在当时根据==[科德十二定律]==提出了数据库设计模型的相关规范，其规范总结起来主要有如下 3 点：

**原子性：**==即是把数据的共同特征或本质进行抽取，合理的地形成一个整体，且这个整体没有进一步拆分的必要性==

**唯一性：**==即为数据加入唯一性标识解决重复性的问题==

**依赖性(又称：关联性或者冗余性)：**==把多余的数据加入附属表，通过外键和主键进行关联操作==

==埃德加·科德博士==所提出的相关理论，被 DBA 数据库工程学认为是一项必修课，DBA 把埃德加·科德博士提出的模型理论又称为==数据库三范式==，尽管他的理论是非常科学和全面的，但是随着科技和互联网的发展，这些理论也受到了很多的批评。后来 LiveJournal 的开发团队发起了反关系型数据库的革命，得到了开发者的响应，并在其中产生了一种违背数据库三范式的思想，DBA 工程学把这种技术叫做 Lookup(==逆范式==)，就是人们俗称逆范式，在现实开发中对于大型网站来说逆范式的应用频率高于三范式，所以三范式在大型网站优化中是没有太大作用的。

**需求的提出：**

需求1：有一个学生叫张三，学号是 s123，年龄 23 岁，在清华大学念书

张三的考试成绩是：Linux 90分，PHP 编程 90分，英语 90分

需求2：有一个学生叫李四，学号是 s789，年龄 21 岁，在北京大学念书

李四的考试成绩是：历史 80分，英语 80分

需求3：有一个学生叫张三，学号是 s520，年龄 23 岁，在清华大学念书

张三的考试成绩是：艺术 70分，英语 70分

## 1、数据库三范式的应用案例

设计以上需求首先遵循数据库三范式的第 1 范式原子性设计表如下：==即是把数据的共同特征或本质进行抽取，合理地形成一个整体，且这个整体进行一步拆分的必要性==

![1544412892722](1544412892722.png)

为了解决两个学生都叫张三的问题，所以在系统中我们只能使用学号对他们进行唯一性的区分，把学号设置为主键，完成数据库的第 2 范式设计思想，设计表如下所示：

![1544413026540](1544413026540.png)

完成了以上设计后，我们需要把成绩和学科的数据看成冗余数据，这些数据与学生关联在一起，因此他们具有一定的关联性(依赖性)，因此根据第 3 范式的思想，==把多余的数据加入附属表，通过外键和主键进行关联操作，设计表如下所示：==

![1544413173350](1544413173350.png)

最终根据第 1，2，3 范式形成的结果如下所示：

![1544413222775](1544413222775.png)

把以上设计形成代码，参考\code\3NF\students.sql

设计学生表(students)，同时使用1NF和2NF，代码如下所示：

![1544413592707](1544413592707.png)

```sql
create table students
(
	sid varchar(10) primary key comment '学号,主键', #RT03883273
	name varchar(10) not null comment '学生的姓名',
	age tinyint unsigned comment '年龄', 
	school varchar(10) comment '学校的名称'
)charset=utf8;
```

![1551064881860](1551064881860.png)

设计成绩表(sorces),使用3NF，代码如下所示：

![1544413646447](1544413646447.png)

```sql
create table sorces
(
	sid varchar(10) comment '学号,主键', #RT03883273
	course varchar(10) comment '学科名称',
	sorce int comment '分数'
)charset=utf8;
#我们要对sorces这张表来设置外键
create index snum on sorces(sid);
```

![1551064908808](1551064908808.png)

填入学生信息的数据如下所示：

![1544413691852](1544413691852.png)

```sql
#先插入信息如下：
insert into students(sid,name,age,school)values('s123','张三',23,'清华大学');
insert into students(sid,name,age,school)values('s789','李四',21,'北京大学');
insert into students(sid,name,age,school)values('s520','张三',23,'清华大学');
```

执行结果如下：

![1551065094750](1551065094750.png)

填入学生成绩的数据如下所示：

![1544414125236](1544414125236.png)

```sql
#插入学生的成绩如下
insert into sorces(sid,course,sorce)values('s123','Linux',90);
insert into sorces(sid,course,sorce)values('s123','英语',90);
insert into sorces(sid,course,sorce)values('s123','PhP',90);
insert into sorces(sid,course,sorce)values('s789','历史',80);
insert into sorces(sid,course,sorce)values('s789','英语',80);
insert into sorces(sid,course,sorce)values('s520','艺术',70);
insert into sorces(sid,course,sorce)values('s520','英语',70);
```

执行结果如下：

![1551065225474](1551065225474.png)

把以上数据设计完成后，如何进行数据查询呢?这时我们就使用连表操作

获取学号为s123的相关数据信息，编写代码如下：

![1544414244778](1544414244778.png)

```sql
#使用左连接进行操作,students as stu,sorces as scr
select stu.sid,stu.name,stu.age,stu.school,scr.course,scr.sorce from 
students as stu left join sorces as scr on stu.sid=scr.sid where stu.sid='s123';
```

直接结果如下所示：

![1545706370610](1545706370610.png)

使用explain查询索引的使用过程:复合数据库范式一般都能使用上索引

![1545706351198](1545706351198.png)

> **数据库3NF有明显的缺点：**
>
> - 逻辑相对复杂,因为需要连表操作
> - 但是索引存在于主表是主键,在附属表是外键(普通索引),因此会导致MYI文件特别大,造成空间过大
> - 连表操作越多消耗cpu越大
>
> 对于大数据来说,数据库范式的以上缺点的第3个是致命的

## 2、逆范式的应用案例(Lookup)

​	没有冗余的数据库未必是最好的数据库，有时为了提高运行效率，就必须降低范式标准，适当保留冗余数据。具体做法是： 在概念数据模型设计时遵守第三范式，降低范式标准的工作放到物理数据模型设计时考虑。降低范式就是增加字段，减少了查询时的关联，提高查询效率。

参考code/lookup.sql

![1544421225445](1544421225445.png)

```sql
create table lookups
(
	id int primary key auto_increment comment '主键',
	sid varchar(10) not null comment '学号',
	name varchar(10) not null comment '学生的姓名',
	age tinyint unsigned comment '年龄', 
	school varchar(10) comment '学校的名称',
	course varchar(10) comment '学科名称',
	sorce int comment '分数'
)charset=utf8;
```

插入数据：

![1544421252747](1544421252747.png)

```sql
#先插入信息如下：
insert into lookups(sid,name,age,school)values(null,'s123','张三',23,'清华大学','Linux',90);
insert into lookups(sid,name,age,school)values(null,'s123','张三',23,'清华大学','Mysql',80);
insert into lookups(sid,name,age,school)values(null,'s123','张三',23,'清华大学','Php',70);
insert into lookups(sid,name,age,school)values(null,'s789','李四',21,'北京大学','Linux',80);
insert into lookups(sid,name,age,school)values(null,'s789','李四',21,'北京大学','Mysql',60);
insert into lookups(sid,name,age,school)values(null,'s789','李四',21,'北京大学','Php',70);
insert into lookups(sid,name,age,school)values(null,'s520','张三',23,'清华大学','Linux',88);
insert into lookups(sid,name,age,school)values(null,'s520','张三',23,'清华大学','Mysql',78);
insert into lookups(sid,name,age,school)values(null,'s520','张三',23,'清华大学','PhP',60);
```



# 三、MySQL 的物理(系统逻辑)分表

​	在开发中把大数据拆分为若干小数据的技术分布到不同数据表文件中中的技术称为物理分表，在 MySQL5 以上的版本中物理分表主要有两种，一种叫水平分表，一种叫垂直分表。

==**垂直分表：**就是把数据按照不同的数据格式进行拆分，有 key 类型(键值对)的垂直分表和 hsah 类型(哈希表)的垂直分表，在 php 开发中应用不多。==

==**水平分表：**就是把数据按一定的范围根据不同的逻辑定义进行数据拆分，在开发中应用比较多==

物理水平分表主要有两种：一种称为 Range 分表，一种称为 List 分表。

业界有些说法也叫 Range 分区和 List 分区。

==Range分表：在开发中多应用于 id 主键当中==

==List分表：多用于时间字段当中==

## 1、使用 Range 形式的物理水平分表(切蛋糕)

​	Range 分表其实好比生活中的切蛋糕中的概念，是基于范围的数据切分，Range 主要是基于整数的分表。一般按主键进行范围分表，假设当前主键的最大值为 5000，有时希望用一个范围对数据进行拆分，比如把数据拆分为 5 个等份：

1-1000：是5000的第1个范围,把该范围的数据作为一张表

1001-2000：是5000的第2个范围,把该范围的数据作为一张表

2001-3000：是5000的第3个范围,把该范围的数据作为一张表

3001-4000：是5000的第3个范围,把该范围的数据作为一张表

4001-5000：是5000的第4个范围,把该范围的数据作为一张表

如果有以上的需求，那么就可以使用MySql的range分表技术，其原理图如下：

![1544424923053](1544424923053.png)

如果把 5000 条记录看成蛋糕，那么拆分的数据就是把蛋糕切成了 5 等份

### 1.1、创建表的时候指定 range 分表的范围

代码详细请参考：code/sql/range.sql

语法规则：

```sql
partition by range (字段)(
	partition 分表名称 values less than (范围)
)
```

**例子：如何为表创建一个名为cake1000和cake2000的分表**

![1544425122223](1544425122223.png)

```sql
create table cakes
(
	id int unsigned primary key auto_increment,
	cakename varchar(16)
)charset=utf8 partition by range(id)(
    #分表名称cake1000,范围是id=1 to id=999
	partition cake1000 values less than(1000),
	#分表名称cake1000,范围是id=1000 to id=1999
	partition cake2000 values less than(2000)
);
```

创建后数据库文件结构如下所示：

![1551077373411](1551077373411.png)

假设我们希望把数据放到 cake1000 这张分表当中，那么我应该如何插入？

分析可知：由于这个分表是从 id 的范围来分，如果 id 的范围是 1-999 那么就会处于 cake1000当中，所以我们插入数据如下：

![1544425640854](1544425640854.png)

```sql
insert into cakes(id,cakename)values(333,'蛋糕333');
insert into cakes(id,cakename)values(666,'蛋糕666');
insert into cakes(id,cakename)values(999,'蛋糕999');
insert into cakes(id,cakename)values(1666,'蛋糕1666');
```

假设我们插入的数据的 id = 2001，那么会出现怎么样的情况呢？

![1544425698416](1544425698416.png)

```sql
insert into cakes(id,cakename)values(2001,'蛋糕2001');
```

如果超过了表的分区范围那么逻辑分表就会报错

![1545709893727](1545709893727.png)

然而我们的数据是分布在不同的分表当中，如果我们使用 select count(*) from cakes 能统计出记录总数吗？

答：肯定可以的，因为逻辑分表的功能和算法是开发者帮我们实现了。

![1545709792397](1545709792397.png)

### 1.2、添加 Range 分表

如果我们添加一个数据超过了分表范围就会报错，如果我们依然希望插入 id = 2001 这数据，应该怎么解决？

答：通过修改表添加一个新的分表进行数据的插入

语法规则：

```sql
alter table 表名 add partition (
    partition 分表名称  values  less than (范围)
)
```

![1544426160814](1544426160814.png)

```sql
alter table cakes add partition(
	partition cake3000  values less than(3000)
)
```

### 1.3、删除 Range 分表

代码详细请参考:code/del_range.sql

语法规则：

```sql
alter table 表名 drop partition 分表名称;
```

![1544426707708](1544426707708.png)

```sql
-- 删除分表
alter table cakes drop partition cake3000;
```



问题：cake3000 分表被删除后，存在 cake3000 分表中的数据是否会丢失？

答：数据会丢失。

## 2、使用 List 形式的物理水平分表(分季度)

​	List 分表其实简单地理解为把一年按季度的进行分表的应用即可，因为实际开发当中 List分表主要用于年度季度报表的应用比较多，List 分表也是按范围的分表技术。假设当前要把 1年的数据进行季度拆分，那么 1 年可以分为 4 个季度：

3、4、5 月份为春季

6、7、8月份为夏季

9、10、11月份为秋季

12、1、2月份为冬季

这时 Range 分表其实无法达到这个功能，如果需要满足当前这个需求则可以选择 List 分表技术，原理图如下：

![1544427515707](1544427515707.png)

### 2.1、创建表的时候指定 List 分表的范围

代码详细请参考:code/sql/list.sql

语法规则：

```sql
partition by list (条件语句)(
	partition 分表名称  values  in (范围)
)
```

![1544427633598](1544427633598.png)

```sql
create table goods
(
	id int unsigned comment '不能在list分表使用索引',
	proname varchar(10) not null,
	source varchar(20) comment '原料',
	`money` varchar(10) not null,
	addtime datetime not null comment '2018-03-25 11:22:33'
)charset=utf8 partition by list( month(addtime) )(
	partition spring values in(3,4,5), #春季
	partition summer values in(6,7,8), #夏季
	partition autumn values in(9,10,11), #秋季
	partition winter values in(12,1,2) #冬季
);
```

### 2.2、尝试插入数据到夏季的分表当中

![1544427818602](1544427818602.png)

```sql
insert into goods (id,proname,source,`money`,addtime)
	values (100,'蛋糕','糖,奶,奶油,裱花纸',23,'2018-06-25 11:22:33');
insert into goods (id,proname,source,`money`,addtime)
	values (100,'蛋糕','糖,奶,奶油,裱花纸',23,'2018-07-25 11:22:33');
insert into goods (id,proname,source,`money`,addtime)
	values (200,'蛋糕','糖,奶,奶油,裱花纸',23,'2018-08-25 11:22:33');
```

### 2.3、删除 List 分表

代码详细请参考:code/sql/list.sql

语法规则：

```sql
alter table 表名 drop partition 分表名称;
```

![1544427983133](1544427983133.png)

> 注意：
>
> ​	分表被删除后，数据会丢失。==不要随意操作==

### 2.4、添加 List 分表

代码详细请参考:code/sql/list.sql

语法规则：

```sql
alter table 表名 add partition (
    partition 分表名称  values  in (范围)
)
```

![1544428132942](1544428132942.png)

```sql
-- 添加分表
alter table goods add partition(
	partition xiatian values in(6,7,8)
);
```

list 分表有 1 缺点，就是在 MySQL5.1.73 版本，建议 id 字段设置为普通索引，因为唯一性索引和主键在 list 分表当中不起作用，且无法添加

![1551080063141](1551080063141.png)

```sql
-- 对 List 分表进行主键的添加和唯一索引的添加
alter table goods add primary key(id);
create unique index uni_id on goods(id);
```

这时我们建立普通索引

![1551080090594](1551080090594.png)

```sql
-- 对 list 分表添加变通索引
create index normal_id on goods(id);
```

# 四、MySQL 的 Grant 用户授权

服务器地址如下：

master：185.225.139.195(主)--线上服务器

slave：192.168.82.244(从)

在 MySQL 中默认存在一个本地用户访问的机制，其他服务器是无法访问的另一台服务器的MySQL 数据库，如下所示：

命令：

```
C:\Users\Administrator>mysql -uroot -p123456 -h185.225.139.195
```

![1545721827855](1545721827855.png)

如果希望当前服务器被其他服务器访问就需要使用 Grant 用户授权技术

**语法规则：**

```sql
-- 授权语句
mysql> GRANT ALL PRIVILEGES ON *.* TO '授权用户名'@'被授权服务器的IP' IDENTIFIED BY '授权密码';
-- 立即生效
mysql> FLUSH PRIVILEGES;
```

以上 SQL 语句必须在 master 服务器中执行

![1545721936156](1545721936156.png)

如果授权成功效果,通过命令查看mysql中的系统用户表：

```
mysql> select host,user from mysql.user where user=授权用户名
```

![1551081713958](1551081713958.png)

再次使用-h选项,并用Grant授权用户和密码登录在Slave中访问Master服务器效果如下：

![1545722165207](1545722165207.png)

代表 slave 已经可以通过 long 的授权用户和密码访问 master

# 五、binlog 日志

​	在 MySQL 中我们所有的写操作(alter create insert update delete)都可以通过文件进行保存，这个文件叫 binlog 日志，binlog 日志主要用于做 MySQL 的==主从复制==，其主要函数如下：

1. show variables like 'log_bin' : 该函数主要用于查看binlog日志是否已经开启
2. show master status : 该函数主要查看用于Master数据库正在使用的binlog日志情况
3. show slave status \G: 该函数主要用于Slave数据库是否已经同步

以上函数如果不用于主从复制和读写分离的配置当中,等同没用

# 六、读写分离和主从复制

## 1、什么是主从复制和读写分离

​	MySQL 主从复制(Master to Slave)与读写分离可以说是数据库当中重要的环节，也是目前世界上所有大型网站当中必须拥有的优化手段。

读写分离的目的很简单：就是让 Master 服务器负责写操作，让 Slave 负责读工作，因此两台服务器就能专注于某项工作，达到负载均衡的效果，其原理如下：

![1544431037842](1544431037842.png)

​	由上图可知，读写分离主要是让两台服务器负责不同的工作，并且 Master 服务器需要把写入的数据同步到 Slave 服务器中，达到数据一致的结果，而这个数据同步的过程主要是通过 binlog 日志完成，DBA 工程学中把这个过程称为==主从复制==。

> 注意：主从复制和读写分离是一个整体，是密不可分的

## 2、主从复制和读写分离的配置步骤

主从首要条件：①关闭 selinux ②关闭 iptables ③ master 已经对 slave 进行 grant 授权

### 2.1、建立数据库

需要在 master 服务器和 slave 服务器都建立一个同名的数据,如：php33

```sql
mysql> create database php33 charset utf8;
```

![1544432626308](1544432626308.png)

### 2.2、开启 binlog 日志

在 master 服务器中开启 binlog 日志和设置要发生主从同步数据库(my.cnf)

binlog日志文件名称：log-bin=mysql-bin

日志文件ID：server-id=1

需要同步数据库：binlog-do-db=php33

![1551082940565](1551082940565.png)

> 参数说明：
>
> #mysql的bin-log 日志配置选项，假设 做读写(主从)分离，这个选项在==从服务器必须关闭==
>
> log-bin=mysql-bin
>
> #主服务器的id，这个 id 不一定设为 1，只要主从不一样就行
>
> server-id=1
>
> #要做同步的数据库名字，可以是多个数据库，之间用分号分割
>
> binlog-do-db=php33

保存并退出(:x)，重启mysqld服务器

```
shell># service mysqld restart
```

![1544432861831](1544432861831.png)

登录 master 查看一些 binlog 日志的相关函数：

```mysql
mysql> show master status;
```

![1544450808777](1544450808777.png)

### 2.3、在slave服务器中，修改配置文件实现同步master中的数据

修改配置文件：

![1544451440031](1544451440031.png)

重启 mysql 服务：

```
shell># service mysqld restart
```

登陆 mysql 配置从服务器Slave

```sql
shell># mysql -uroot -p123456

mysql> change master to master_host='185.225.139.195',master_port=3306,master_user='long',master_password='654321',master_log_file='mysql-bin.000007',master_log_pos=106;
```

![1544450763942](1544450763942.png)

> 参数说明：
>
> #主服务器的IP地址
>
> master_host='185.225.139.195'
>
> #主服务器的 mysql 端口
>
> master_port=3306
>
> #grant 授权的用户名(账号)
>
> master_user='long'
>
> #grant 授权的用户密码
>
> master_password='654321'
>
> #主服务器上的bin-log日志
>
> master_log_file='mysql-bin.000007'
>
> #主服务器上的 bin-log日志值
>
> master_log_pos=106

启动 slave(从服务器)

```sql
mysql> start slave;
```

![1544450873427](1544450873427.png)

> 说明：
>
> mysql> stop slave;	-- 停止
>
> mysql> reset slave;	-- 重置
>
> mysql> start slave;	-- 启动

登录slave服务器中，查看show slave status\G，看到如下选项代表主从复制同步成功：

```mysql
mysql> show slave status\G
```

![1544450980833](1544450980833.png)

### 2.4、测试

创建数据表看是否能同步，成功如下图所示：

![1544451157801](1544451157801.png)

> 注意：
>
> ​	如果开发中写的操作发生slave当中，主从同步马上宣告失败，因此开发中，我们必须清楚 master 和 slave 服务器，slave 我们只是让它负责 select，但是 slave 是可以 insert 数据的，但是清楚风险所在

# 七、总结

3nf

逆范式

分表

​	水平分表 Range

​	水平分表 list(按季度分表)

MySQL用户授权

读写分离(主从复制)

​	开启binlog日志

​	注意：在从服务器不需要操作添加、修改、删除这些

```php
<?php
    //连接数据库
    $type = $_GET['type'];
    if( $type == 'select' ){
        //连接数据库从服务器
        .....
        //查询语句
    }else{
        //连接数据库(主服务器)
        ......
        //添加、删除、修改
	}
```

