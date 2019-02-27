---
title: MySQL优化
date: 2019-02-22 18:37:58
tags: MySQL
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识点回顾

能够说出Nginx的作用

​	web服务器-->http或https协议

​	邮件服务器-->imap/pop3/smtp

​	负载均衡服务器(服务器集群)-->分发任务

​	高可用服务器-->可以随时替代其它服务器

能够实现nginx与php-fpm安装
<!-- more -->
​	nginx 安装方式2种？

​		可以使用yum 来安装 ：yum -y install nginx

​		源码编译安装：

​			./configure   检测环境并生一个makefile文件

​			make	读取makefile文件并生成一个二进制文件

​			make install	读取二进制文件并安装到我们指定目录下

​	php-fpm安装PHP（Lamp搭建已经安装）

​		可以使用yum(默认安装是5.3，希望安装是5.6或7.0以上，必须修改PHP的yum源) 来安装：yum -y install php

​		源码编译安装：

​			./configure

​			make

​			make install

能够实现nginx的虚拟主机配置

​	第一个虚拟主机必须在

```shell
......
http{
	......
    server {
        listen       80;
        server_name  localhost;
        root /mydata/wwwroot/itcast/www;
        index index.html index.htm index.php;

        location / {
            
        }

        location ~ \.php {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include fastcgi.conf;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            
        }
    }
}
```

能够了解nginx的缓存功能

​	缓存功能-->默认是关闭

​	开启缓存功能有什么作用？

​		开启缓存功能后，用户只是第1次请求比较慢，第2次请求会快很多

```shell
server {
  ......
  location ~ .*.(js|css|jpeg|png|gif|mp4|mp3|jpg){
     expires 10; #设置过期时间为10天
  }	
  ......
}
```

能够了解nginx的压缩功能

​	在配置文件中开启 gzip on;

```shell
http {
    gzip  on;	# 开启压缩
    gzip_types text/plain application/javascript   application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png video/mp4 video/mp3; # 指定压缩文件类型
    gzip_disable "MSIE [1-6]\.";	# 关闭ie6浏览器压缩
    gzip_min_length 1k;	# 指定压缩大小，只要这个文件达到 1kb大小就进行压缩
    gzip_buffers 4 16k;	# 指定压缩字节大小 以4倍速度压缩
    gzip_comp_level 2;	# 压缩的级别 1-9 数字越小 消耗CPU越大
}
```

能够了解负载均衡的原理与意义

​	意义存在？

​		解决高并

```
#开启负载均衡器
upstream web {
    #server ip地址 weight=1 max_fails=3 fail_timeout=20s;
    server 192.168.82.244:81(填写域名) weight=1 max_fails=3 fail_timeout=20s;
    server 192.168.82.244:82 weight=1 max_fails=3 fail_timeout=20s;
    #.......
}

```

能够解释session访问失效的原因

​	web架构采用集群(负载均衡)

能够理解session共享的原理

# 二、大型网站优化之MySql优化

## 1、优化和不优化的对比的

​	在业界当中我们有一个叫**大数据(big data)**的概念，所谓的大数据指代千万级别以上的数据作为起步的数据。所以我们现在需要对两张都具有**12000001**条记录的表进行查询对比，其中表名为**tbl_no**的表是没有做过任何优化的表，表名为**tbl_yes**的表是做过优化的表。这个实验的目的是观察具有优化和不具有优化的查询中速度的差别。

> 实验条件:
>
> 1)两张表的数据记录总数是相同的
>
> 2)两张表的数据字段结构也是一样的
>
> 3)查询的记录的where条件都是username='user999999'的用户

实验结果对比出现如下的结果:

1.1、表tbl_no查询条件为username='user999999'的用户耗时时间为：3.81秒

![1545442097460](1545442097460.png)

1.2、表tbl_yes查询条件为username='user999999'的用户耗时时间为：0.1秒

![1545442166827](1545442166827.png)

## 2、业界中对“慢”的定义***[八秒原则]***

	在今天互联网当中有一个所谓的8秒原则，根据统计一个用户在8秒中之内如果不能打开一个网站，那么有99%的用户会选择关闭浏览器。
	
	其实8秒原则是相对论,因为如果一个网站打开的速度超过两秒达到3秒钟才能打开其实就已经无限在挑战用户的耐心了,也就说其实我们做优化是尽可能控制速度在2秒内完成,这样才能更好的留住用户,避免挑战用户的等待耐心。

![1535268224434](1535268224434.png)

然而我们说为什么数据库的查询速度慢就会影响用户等待的耐心呢?

## 3、PHP+MySql架构的请求原理

​	在开发的当中我们使用php+mysql的架构占据了主要的开发方式，这种架构形成的请求其实真正的结果执行最终发生在MySql数据库当中，其主要流程图如下所示：

![1](1.png)

​	如果这个请求的过程当中，数据库响应达到了?秒，那么也就就是说浏览器的客户在不断等待网站数据返回(打开)，也就是这样 8 秒原则的定律就会成型，用户关闭浏览器的可能性是99%的。

# 三、MySql优化的层面

1. 表设计层面：存储引擎,索引,列(字段)类型,范式规范（冗余）
2. 服务器层面：实现数据库主从复制(==读写分离==)
3. 编程层面：避免在循环中select数据表
4. 缓存层面：使用nosql技术、memcache、redis、mongodb
5. 系统层面：使用Linux操作系统作为服务器底层
6. 其他方面：硬件、集群服务器，靠谱的 idc 供应商，靠谱的技术团队

	 ​	天下没有免费的午餐，任何的优化其实都具有主观的性质存在而且每一种优化的方法基本上都需要付出一定的代价。世界上没有一个非常标准的优化技术给你作为参考，优化是一门永远不会停止课题，随着你的经验的积累，那么你的优化方法和见解就有可能不同

# 四、MySql存储引擎

## 1、什么是存储引擎

		MySQL中的数据是通过各种不同的技术格式存储在文件（或内存）中的。技术和本身的特性就称为"存储引擎"。每一种需求都使用不同的存储引擎，才能获取不同的数据库性能和功能上的需求。
	
		如果在生活把独门独户的楼房(纵向结构)和平房(平面结构)看作存储引擎的话，不同的建筑结构对于不同的需求来说就有不同的便利。对于送快递的人来说独门独户的楼房便于快递小哥的投递。对于装空调或者搬家具的师傅来说，平房自然更利于空调的安装和家具的进出。
	
		由此我们可以知道一个结论，选择不同的存储引擎就是选择不同的技术格式（结构），他们的特性决定他们对某种需求的优势，因此存储引擎的正确选择是做MySql优化的关键。

## 2、存储引擎的查看和默认项

​	在MySql数据库当中其实有5种存储引擎，并且会默认地帮用户选定其中一个，不同的数据库版本默认的选项是不一样的，所以我们可以通过以下的命令去了解当前MySql的存储引擎相关情况：

语法规则：show engines

执行命令效果如下：

![1535434493239](1535434493239.png)

常用的存储引擎一般是MyISAM和Innodb，MyISAM是默认的存储引擎(MySql5.5以下的版本)

> Memory其实以前也是一个很受欢迎的引擎，然而它现在已经被 Memcache 和 redis 取代它的地位了，因此使用的人就极少了，这个引擎是一个内存机制的引擎类似 Memcache，但性能不如 memcache 强大(了解)

## 3、MyISAM表的文件及其特点

在MySql中，MySql把所有的数据库用目录来表示，把数据用文件的方式来进行存储。

MySql数据库目录路径在：C:\phpStudy\PHPTutorial\MySQL\data **(根据自己安装MySql目录)**

建立一个名为demo的数据库

代码参看：code/demo.sql

![1535274580174](1535274580174.png)

查询C:\phpStudy\PHPTutorial\MySQL\data就会发觉多了一个名为demo的目录

![1535274600393](1535274600393.png)

创建表指定引擎为myisam

![1535274768447](1535274768447.png)

在C:\phpStudy\PHPTutorial\MySQL\data\demo目录下，产生以下文件：

![1535274872012](1535274872012.png)

在MySQL当中myisam引擎存储数据有3个文件组成

.frm文件用于存储建表的结构信息(字段信息)

.MYD用于储存表的记录信息

.MYI用于储存表的索引信息(例如：主键)

MyISAM存储引擎的特点：

建表结构，表记录信息和索引信息都独立存在的，由于MyISAM存储引擎的技术特点是结构和数据分离，因此可以更好地观察到该存储引擎的文件变化情况，因此大部分情况下我们做优化都是使用MyISAM引擎。

### 案例：

新建一张产品表goods，代码如下：

代码详细请参考：code/goods.sql

![1535275364839](1535275364839.png)

建表完成后,发觉C:\phpStudy\PHPTutorial\MySQL\data\demo下的文件有如下变化:

![1535275426889](1535275426889.png)

用复制数据的方法添加记录查看myisam引擎表的变化

### 3.1、向goods表添加3条记录

![1535275653677](1535275653677.png)

### 3.2、蠕虫(倍增)复制数据

![1535275735822](1535275735822.png)

反复执行的结果如下：

![1535276018279](1535276018279.png)

你会很快就可以插入几十万甚至几千万条数据

### 3.3、查看结果发现文件的大小变化如下图所示

![1535276036927](1535276036927.png)

myisam引擎的表，当数据发生变化的时候，保有.MYI文件和.MYD文件会发生变化，而.frm文件不会发生改变

## 4、Innodb存储引擎表的文件及其特点

在MySql中，MySql把所有的数据库用目录来表示，把数据用文件的方式来进行存储。

MySql数据库目录路径在：C:\phpStudy\PHPTutorial\MySQL\data (根据自己安装MySql目录)，因此无论是Innodb引擎或myisam引擎

MyISAM引擎的表数据都存在于该目录当中，所以同样地我们可以在这个目录中进行

Innodb引擎的表文件变化。

代码详细请参考:code/pros.sql

### 案例：

新建一张pros表，代码如下：

![1535276503249](1535276503249.png)

观察其实文件的变化如下：

![1535276586122](1535276586122.png)

问题来了：查看以上结果后你会发觉在Innodb只有一个.frm文件用于存储数据表的结构，那么Innodb的记录信息和索引文件存放哪里呢？

插数据来观察innodb的变化

### 4.1、向pros表添加数据

![1535276734027](1535276734027.png)

### 4.2、蠕虫(倍增)复制数据

![1535276802173](1535276802173.png)

反复执行的结果如下

![1535276981661](1535276981661.png)

### 4.3、查看结果发现文件的大小变化如下图所示

![1535279556491](1535279556491.png)

然而我们会发现如果我们继续倍增数据在C:\phpStudy\PHPTutorial\MySQL\data有一个文件ibdata1会发生改变

![1535279649772](1535279649772.png)

我们发觉产生了ibdata1文件，随着你对innodb引擎的数据表pros添加数据，这个文件的大小发生了变化，因为innodb是把所有表记录信息和索引信息都共享在一个文件当中。如果我们有10张innodb的表那么我们10张表的记录信息和索引信息都会存放到这个文件当中，因此我们要针对某一张innodb的表来做优化是很难的。所以我们证明了一点，做优化一般是针对myisam的表。

Innodb存储引擎的特点：**结构和数据共享**，目的是为了保证数据的完整性，**为追求数据完整性和安全性而生**的存储引擎。

## 5、MyISAM和Innodb的区别和应用场景

### Innodb

专注在==数据完整性和安全性方面==，例如：==事务等==，对查询，插入数据的效率支持就比较差

### MyISAM

MyISAM是==追求性能而生==的。**因此我们说的MySql优化其实很多时候就是针对myisam引擎而言**，但优化是要付出代价的，你要优化就要牺牲掉某些东西，如果你当前的业务要求安全性很高那么你就别谈什么优化了，也不能选择myisam了。

### 两种存储引擎的应用场景(引擎的选择)

大量查询(select)或者写入(update,insert,delete)就选择==MyISAM==。例如：博客系统，论坛等。

数据涉及利益和金钱交易，就需要很严谨和具有安全性，例如：销售、财务、股票等系统，就选择==Innodb==。

1.商城中的支付选择哪个引擎比较合理？

答：Innodb

2.商城中的产品录入选择哪个引擎比较合理？

答：MyISAM

# 五、MySql的explain执行计划

## 1、什么是执行计划

explain是一个MySql性能显示的工具，它显示了MySql如何使用索引来处理select语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。在开发当中我们一般用explain来查看索引的使用情况，explain你可以把它理解成为一个查看索引使用情况的工具，但官方对它的定义是explain是一个==mysql执行计划(ex=execute)==，不过这种概念的东西不需要过分的纠结，反正知道有这么一回事就可以了。

语法规则：explain [select 语句]

执行 explain select * from tbl_no where username='itcast999999' 结果如下：

![1535434841802](1535434841802.png)

explain执行计划最重要的选项：

![explain](explain.png)

explain执行计划分析tbl_yes

![1535434964942](1535434964942.png)

比对explain扫描tbl_yes和tbl_no的结果你会发现其实只要使用索引，那么速度就会很快，而且只要使用到索引那么就一定不会是全表扫描all的方式，并且扫描的记录行数也大大缩减，因此这个就是为什么tbl_yes比tbl_no快的主要原因，关键一点就是tbl_yes有使用到索引。

# 六、索引的概述

## 1、为什么要使用索引

	数据库的索引其实就好像书本的目录一样，假设你拿一本新华字典来查一个字，如果新华字典的拼音字母的目录不见了，笔画部首的目录被撕掉了，这时你去工这个字你就慢了。如果重新把这个目录给你，你查找这个字的速度肯定快了很。索引就相当于这个目录的作用，它能提高select语句查找记录的速度。

![3](3.png)

在Mysql当中尽管索引查找的速度非常快，然而Mysql的任何一种优化都是需要付出一定代价的，添加索引会占据很多的磁盘的空间,==也会降低写操作(delete , update , insert)语句的效率==。**其实索引我们很早就接触过，主键就是一种索引。主键也是所有索引当中最高效率的。**

![1535435283822](1535435283822.png)

在数据库当中,索引优化有一个原则:使用上索引就一定会快,并且使用上索引就不会是ALL

## 2、常见索引的分类

在Mysql的常用引擎Innodb和MyISAM当中它们的索引结构是一种叫Btree的类型，因为它们是根据一种叫“**二叉树**”的算法创建出来的。在Mysql当中常见的索引使用有4种，分别为:

主键索引( Primary Key )

普通索引( Key )

唯一性索引(Unique)

全文索引(FullText):这个索引其实被sphinx(迅搜)取代了其地位

特殊的索引有1种：**前缀索引**

## 3、查看索引的方法

### 3.1、语法规则:show index from [表名称]

使用命令: show index from tbl_yes

![1535435361493](1535435361493.png)

primary是一个主键索引,它作用在id字段中

uname的普通索引,它作用在username字段中

### 3.2、语法规则:show create table [表名称]

使用命令:show create table tbl_yes

![1535435451816](1535435451816.png)

以上方法其实是通过查看建表的结构来查看索引的相关信息

# 七、主键索引

主键索引的原则和场景:

在开发当中我们往往需要对一些重复性的数据进行区分，那么这时我们就需要使用主键索引，主键索引既不能为null,也不能重复,更不能为空字符串。在id字段中出现最多,通常还会配合自增长一起使用,主键索引也是所有索引当中查询速度最快的索引。

## 1、在创建表的时候创建主键索引

代码详细请参考:code/pk1.sql

语法规则:primary key关键字

![1535285833692](1535285833692.png)

插入一些数据测试主键

![1535285842983](1535285842983.png)

使用explain执行计划查看主键是否会被使用上:

使用命令explain select * from pk1 where id=2;和explain select * from pk1 where name='itcast.cn';

进行对比,结果如下所示:

![1535286102953](1535286102953.png)

## 2、在已有表中创建主键索引

代码详细请参考:code/pk2.sql
语法规则:alter table 表名 add primary key(字段名称)

![1535286201623](1535286201623.png)

```mysql
-- 创建一张名为pk2的表
create table pk2(
	id int unsigned,
	username varchar(16)
)engine=myisam charset=utf8;
-- 首先把id变成主键
alter table pk2 add primary key(id);
```

执行结果如下：

![1535286335249](1535286335249.png)

有时候我们在创建主键索引的时候,遇到id字段我们还需要配合设置

整型数据类型的自增长,这时可以使用以下语法在已有的表中进行建立

语法规则:Alter table 表名 modify 字段名 字段类型 auto_increment

![1535287735397](1535287735397.png)

```mysql
-- 还需要为id设置自动增长的属性
alter table pk2 modify id int unsigned auto_increment;
```

执行后,效果如下：

![1535286539222](1535286539222.png)

## 3、删除表中的主键索引

代码详细请参考:code/pk3.sql

语法规则:Alter table 表名 drop primary key;

主键删除的详细步骤:

### 3.1、主键含有自动增长的特性,需要首先删除自增长的属性,否则就会报错

![1535287817405](1535287817405.png)

```mysql
-- 删除主键索引
alter table pk2 drop primary key;
```

执行效果如下

![1535287804846](1535287804846.png)

### 3.2、使用Alter table语句修改去除主键字段的自动增长属性

![1535287879454](1535287879454.png)

```mysql
-- 删除自动增长就是重新修改id字段为int就行了
alter table pk2 modify id int unsigned;
```

执行结果如下

![1535287865035](1535287865035.png)

### 3.3、使用删除主键的语法规则对主键进行删除

![1535287906205](1535287906205.png)

```mysql
-- 删除主键索引
alter table pk2 drop primary key;
-- 查看建表语句
show create table pk2;
```

执行结果

![1535287963716](1535287963716.png)

# 八、唯一性索引

唯一索引的原则和场景：

```
在开发当中，有时我们在网站注册会员时需要对用户名进行唯一性约束，这时唯一性索引就可以起到约束的作用，同时唯一索引也能提高数据查找的速度，因此唯一性索引在开发中十分广泛使用。
```

## 1、在创建表的时创建唯一索引

代码详细请参考：code/unique1.sql

![1535436845430](1535436845430.png)

```mysql
create table unique1(
	username varchar(16) DEFAULT NULL,
	password char(32) DEFAULT NULL,
	UNIQUE KEY uni_name (username)
)ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

![1535436786125](1535436786125.png)

第一次插入数据

![1535436877059](1535436877059.png)

第二次插入相同数据

![1535436919159](1535436919159.png)

以上例子给我们的启示是可以在注册用户的时候使用唯一性进行唯一性约束。

使用explain执行计划查看唯一性索引是否会被使用上:

![1535437198517](1535437198517.png)

> 注意：
>
> ```
> 数据表中只有一条记录时，索引是不起作用。
> ```

## 2、在已有表中创建唯一索引

代码详细请参考：code/unique2.sql

语法规则:create unique index 索引的名称 on 表名(字段名称)

![1535437854357](1535437854357.png)

![1535437881056](1535437881056.png)

```mysql
create table unique2(
	username varchar(16) DEFAULT NULL,
	password char(32) DEFAULT NULL
)ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

添加唯一索引

![1535437695285](1535437695285.png)

![1535437971537](1535437971537.png)

```mysql
-- 添加唯一索引
create unique index uni_name on unique2(username);
```

查看是否创建成功

![1535438056447](1535438056447.png)

```mysql
-- 查看创建语句
show create table unique2;
```

插入数据

![1535438220363](1535438220363.png)

```mysql
-- 插入数据
insert into unique2(username,password) values('itcast.cn', md5('123456'));
insert into unique2(username,password) values('itheima.com', md5('123456'));
insert into unique2(username,password) values('youlongit.com', md5('123456'));
```

## 3、删除表中的唯一索引

代码详细请参考：code/unique2.sql

语法规则: alter table 表名 drop index 索引的名称

![1535438469843](1535438469843.png)

```mysql
-- 删除唯一索引
alter table unique2 drop index uni_name;
```

执行结果

![1535438677149](1535438677149.png)

## 4、唯一索引的注意事项

代码详细请参考:code/unique1.sql

### 4.1、在唯一索引当中，null是可以重复的

![1535438962378](1535438962378.png)

```mysql
-- 插入null值
insert into unique1(username,password) values(null, md5('123456'));
```

连续插入结果如下

![1535438995940](1535438995940.png)

发觉唯一性索引对NULL值不起任何的约束作用,这个是唯一性索引的缺点

### 4.2、在唯一索引当中，空字符串是不能重复

![1535439202867](1535439202867.png)

```mysql
-- 插入空值
insert into unique1(username,password) values('', md5('123456'));
```

执行结果如下：

![1535439273084](1535439273084.png)

# 九、普通索引

普通索引的原则和场景：

在开发当中，如果我们仅仅是为了提升查询的效率，但不需要用到任何的约束行为，就可以使用普通索引。

## 1、创建普通索引

代码详细请参考：code/normal.sql

![1535444090243](1535444090243.png)

![1535444129974](1535444129974.png)

```mysql
-- 创建表
create table normal(
	id int unsigned primary key auto_increment,
	name varchar(10),
	type varchar(10)
)charset=utf8 engine=myisam;
```

插入数据

![1535444151983](1535444151983.png)

![1535444179532](1535444179532.png)

```mysql
-- 插入数据
insert into normal(name,type) values('IT', 'itcast.cn');
insert into normal(name,type) values('IT', 'itheima.com');
insert into normal(name,type) values('IT', 'youlongit.com');
```

name字段目前没有建立索引，使用explain分析结果如下：

![1535444351380](1535444351380.png)

![1535444302668](1535444302668.png)

```mysql
 -- 分析
 explain select * from normal where name="IT";
```

如果按照name名称进行搜索，就会产生全表扫描，整个效率就太差了。但是我们必须为name字段设置普通索引。

语法规则:create index 索引名称 on 表名称(字段名称)

![1535451675678](1535451675678.png)

```mysql
 -- 为name字段设置普通索引
 create index index_name on normal(name);
```

执行效果如下：

![1535451910676](1535451910676.png)

使用explain执行计划查看普通索引是否会被使用上:

![1535451963955](1535451963955.png)

## 2、删除普通索引

语法规则：alter table 表名 drop index 索引名称

![1535452373626](1535452373626.png)

![1535452450182](1535452450182.png)

```mysql
 -- 删除普通索引
 alter table normal drop index index_name;
```

查看是否删除成功：

![1535452582281](1535452582281.png)

# 十、总结

索引

​	主键索引

​		一张表只能有一个主键，内容不可以重复

​	唯一索引

​		一张表可能有多个，内容不能重复，可添加null（多次添加），空字符串(只能添加一次)

​	普通索引

​		一张表可以有多个，内容是否可以重复？可以重复

​	复合索引(联合)

执行计划(explain)

​	type：类型（all、ref、const、index、range）

​		从小到大：const、ref、range、index、all

​	possibe_keys：索引名称(可能使用到的索引)

​	key：查询时使用到索引

​	key_len：索引的长度

mysql存储引擎

​	MyISAM、InnoDB、memory
