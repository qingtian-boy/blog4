---
title: MySQL优化01
date: 2019-02-24 19:04:12
tags: MySQL
---
# 目录

[TOC]

# 一、昨日回顾 

## 1、知识点回顾

**能够说出innodb存储引擎的索引结构**

InnoDB使用 B+Tree 的方式存储索引。

InnoDB 的一个表可能包含多个索引，每个索引都使用 B + 树来存储。而索引包括聚集索引和二级索引，聚集索引使用的主键作为索引键，包含表的所有字段。二级索引只包含索引键和聚集索引键(主键)的内容，不包括其他字段。每一个索引都是一棵 B + 树，每棵 B + 树由很多页面组成，而每个页面大小一般为 16k。从 B + 树的组织结构来看，B + 树的页面可分为：
<!-- more -->
叶子节点：B 树层次为 0 的页面，存储记录的所有内容

非叶子节点：B 树层次大于 0 的页面，只存储索引键和页面指针。

一棵典型的 B + 树结构：

![1550914934202](1550914934202.png)

从上图可知，相同层次的页面是用一个双向链接表连接起来的。

一般情况下，从 B + 树的最左边叶子节点开始，一直向右扫描，就可以得到 B + 树的从小到大的所有数据。因此，对于叶子节点，有如下特征：

页内数据是按索引键排序的。

页面的任一记录的索引键值不小于其左兄弟页面的任何记录。

**能够说出 innodb 存储引擎的特性**

完全支持事务的ACID特性

InnoDB支持的是行级锁，在进行写操作时需要的资源更少，支持的并发更多

行级锁是由存储引擎层实现的

阻塞和死锁

​	阻塞：一个事务中的锁需要等待另一个事务中的锁释放，形成的是阻塞

​	死锁：两个或两个以上的事务在执行中相互占用了对方的资源

**能够说出 innodb 存储引擎之外的存储引擎**

存储引擎查看：show engines;

MyISAM 存储引擎、MEMORY 存储引擎

**能够运用explain 查看执行计划**

mysql> explain select * from tbl_yes where username='user999999';

**能够掌握执行 explain 之后的结果表达的意思**

type 类型：const、ref、range、indexhe、all （从小到大）

possibe_key：可能使用到索引

key：使用到的索引

key_len：索引长度

**能够理解什么样的字段适合创建索引**


# 二、MySQL全文索引(了解)

## 1、like查询的索引原则

代码详细请参考：code/like.sql

### 1.1、创建articles_like表，并且把title字段设置为普通索引

![1535453217390](1535453217390.png)

![1535453200084](1535453200084.png)

```mysql
#创建一张名为articles_like的表
CREATE TABLE articles_like(
    id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
    title VARCHAR(200),#标题
    body TEXT, #内容
    INDEX tit (title)
) charset=utf8 engine=myisam;
```

### 1.2、为articles_like表插入测试数据

![1535453266371](1535453266371.png)

![1535453306845](1535453306845.png)

```mysql
#为articles_like表添加一些数据
INSERT INTO articles_like (title,body) VALUES('MySQL Tutorial','DBMS stands for DataBase ...'),('How To Use MySQL Well','After you went through a ...'),('Optimizing MySQL','In this tutorial we will show ...'),('1001 MySQL Tricks','1. Never run mysqld as root. 2. ...'),('MySQL vs. YourSQL','In the following database comparison ...'),('MySQL Security','When configured properly, MySQL ...');
```

执行结果如下:

![1535453361299](1535453361299.png)

### 1.3、使用like语句查询标题中含有mysql关键字的数据库记录

使用命令:select * from articles_like where title like '%mysql%';
执行效果如下:

![1535453495408](1535453495408.png)

我们的title字段是有索引的，但是这时使用linke进行模糊搜索，这个索引是否可以被使用？

### 1.4、使用explain执行计划查看like语句是否使用到索引

使用命令：explain select * from articles_like where title like ‘%mysql%';

![1535453795162](1535453795162.png)

发觉like语句使用不了索引，原因就在于MySql数据库中，如果使用like条件语句当中出现%查询关键字之前，那么将无法使用到索引。所以以下情况都是不可能使用到索引的：

select * from articles_like where title like '%mysql%';     // 这种是没使用索引

select * from articles_like where title like '%mysql';	// 没有使用索引

![1535454110844](1535454110844.png)

如果把%放在查询关键字之后，使用上索引的可能性比较高，而不是100%能使用上。

以下情况like有可能用得索引,但搜索出来结果不是你想要的:
使用命令: explain select * from articles_like where title like 'mysql%';  --使用到索引
执行结果如下:

![1535454271119](1535454271119.png)

如果%号出现在搜索关键字之后,那么like是可以使用关键字的,然而查询的结果是会丢失记录的:

![1535454374754](1535454374754.png)

但是如果我们真的需要使用到索引,有必须保证查询结果的完整性,那么我们应该怎么办呢?
全文索引应运而生

## 2、全文索引(FullText)

全文索引是为取代==like==而生，然而mysql自带的全文索引==不支持中文==。

全文索引的原则和场景：

​	全文索引能很好取代like的查询，且功能比like强大，但MySql自带的全文索引FullText在MySql5.6以下的版本并不支持中文，自带的全文索引可以使用在一些英文的小型网站当中。

代码详细请参考:code/fulltext.sql
如果我们需要查看articles表中title或者body字段含有database关键字的数据记录,那么我们如果我们要满足以上的要求,就必须使用全文索引

### 2.1、创建全文索引

语法规则: fulltext(一个或者多个字段名称)
注意:一般使用fulltext的时候很少使用一个字段作为全文索引,因为使用给一个字段全文索引的意义就失去了

![1535454850691](1535454850691.png)

![1535455109316](1535455109316.png)

```mysql
#创建articles表
CREATE TABLE articles (
    id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
    title VARCHAR(200),#标题
    body TEXT, #内容
    fulltext(title,body) #把title和body字段同时定义全文索引
) charset=utf8 engine=myisam;
```

### 2.2、为articles表插入测试数据

![1535455170536](1535455170536.png)

![1535455204368](1535455204368.png)

```mysql
#为articles表插入测试数据
INSERT INTO articles (title,body) VALUES('MySQL Tutorial','DBMS stands for DataBase ...'),('How To Use MySQL Well','After you went through a ...'),('Optimizing MySQL','In this tutorial we will show ...'),('1001 MySQL Tricks','1. Never run mysqld as root. 2. ...'),('MySQL vs. YourSQL','In the following database comparison ...'),('MySQL Security','When configured properly, MySQL ...');
```

### 2.3、全文索引的布尔模式查询原则

语法规则：match(全文索引的字段列) against( '+关键字' in boolean mode)

2.3.1、查看articles表中**title或者body字段含有database关键字**的数据记录

![1535455396327](1535455396327.png)

![1535455442816](1535455442816.png)

```mysql
-- 查看articles表中title或者body字段含有database关键字的数据记录
select * from articles where match(title,body) against('database');
```

2.3.2、查看articles表中**title或者body字段含有mysql关键字**的数据记录

![1535455520119](1535455520119.png)

![1535455598107](1535455598107.png)

```mysql
-- 查看articles表中title或者body字段含有mysql关键字的数据记录
select * from articles where match(title,body) against('mysql');
```

正常来说应该查询出6条记录，为什么一条也查不出来的呢？

原因是全文索引是根据算法生成的，支持一种叫布尔查询的模式，如果不使用布尔查询有可能查询的结果不准确，因此使用全文索引必须使用布尔查询模式，使用布尔查询模式修改上面的查询语句如下所示：

![1535455811481](1535455811481.png)

执行结果如下所示,发觉查询正确了:

![1535456173701](1535456173701.png)

```mysql
-- 使用布尔查询模式查找articles表中title或者body字段含有mysql关键字的数据记录
select * from articles where match(title,body) against('+mysql' in boolean mode);
```

我们发现全文索引可以完成like的功能，但是它会使用到索引吗？

使用explain查看全文索引是否有使用到索引：

![1535456229378](1535456229378.png)

![1535456264505](1535456264505.png)

```mysql
explain select * from articles where match(title,body) against('+mysql' in boolean mode);
```

执行explain语句发现fulltext索引是可以被使用，like是可以完全使用fulltext来取代的，但是fulltext不支持中文，所以只能应用在英文网站中。

# 三、全文索引(xunsearch 迅搜)

## 1、简介

URL地址：http://www.xunsearch.com/site/about

Xunsearch 是一个高性能、全功能的全文检索解决方案。

Xunsearch 旨在帮助一般开发者针对既有的海量数据，快速而方便地建立自己的全文搜索引擎。

Xunsearch 中文译名为“迅搜”，代码中的经常被缩写为 XS，既是英文名称的缩略也是中文声母缩写。 这儿的“迅”是快速的意思，至少包含了两层涵义：其一代表了搜索结果的响应能力，其二则为二次开发难度、速度。

## 2、特色优势

我们追求的最大特色是：快，搜索响应快，开发上手快。

- 支持海量数据，高速搜索响应，敬请参见 Xapian 里的 Scalability。 据描述单库最多支持 40 亿条数据，在 5 亿网页大约 1.5TB 的数据中，非缓存情况下检索时间不超过 1 秒。

- 健壮的后端守护程序，内置缓存设计，事件模型基于 libevent。

- 内置专为搜索而自主开发的 scws 中文分词，搜索效果好，又能保障查全率。

- 后端采用稳定高效的 C/C++ 开发，前端采用流行的 PHP 脚本语言，堪称最佳组合。

- 极低的开发难度，接口简单易用，而且文档规范、全面。

- 与 Lucene, Sphinx 之类相比较，Xunsearch 提供了更丰富而必需的功能，开发周期更短。

- 功能强大，内置了大量只有商业、大型搜索引擎才提供的功能。支持包括字段检索、结果高亮、 字段排序、布尔语法、区间检索、聚合搜索、相关搜索、权重微调、拼音搜索、 搜索建议等等专业搜索引擎具备的功能。

## 3、安装

### 3.1、下载

```
shell># wget http://www.xunsearch.com/download/xunsearch-full-latest.tar.bz2
```

### 3.2、解压安装包

```
shell># tar -xjf xunsearch-full-latest.tar.bz2
```

### 3.3、进入目录，并执行 setup.sh脚本安装

```
shell># cd xunsearch-full-1.4.12/
shell># sh setup.sh
```

> ==注意：==
>
> 有可能报错误，如下图所示：
>
> ![1544343518940](1544343518940.png)

#### 3.3.1、解决方法

##### 3.3.1.1、下载

```shell
shell># wget http://ftp.gnu.org/gnu/gcc/gcc-4.8.2/gcc-4.8.2.tar.bz2
```

##### 3.3.1.2、解压

```shell
shell># tar -jxvf gcc-4.8.2.tar.bz2
```

##### 3.3.1.3、进入目录，并下载供编译需求的依赖项

```shell
shell># cd gcc-4.8.0
shell># ./contrib/download_prerequisites　
```

##### 3.3.1.4、建立一个目录供编译出的文件存放，并进入目录

```shell
shell># mkdir gcc-build-4.8.2
shell># cd gcc-build-4.8.2
```

##### 3.3.1.5、生成Makefile文件

```shell
shell># ../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
```

##### 3.3.1.6、编译

```shell
shell># make -j4
```

> 说明：
>
> ​	-j4选项是make对多核处理器的优化，如果不成功请使用 make，相关优化选项可以移步至参考文献[2]。

> 注意：有可能会报以下错误
>
> ![1544344001023](1544344001023.png)
>
> 使用 yum -y install glibc-devel.i686 glibc-devel 解决

##### 3.3.1.6、安装

```shell
shell># make install
```

### 3.4、启动

```shell
// 监听在本地回环地址 127.0.0.1 上
shell># /usr/local/xunsearch/bin/xs-ctl.sh -b local start
// 监听在所有本地 IP 地址上
shell># /usr/local/xunsearch/bin/xs-ctl.sh -b inet start
// 监听在指定 IP 上
shell># /usr/local/xunsearch/bin/xs-ctl.sh -b a.b.c.d start
// 分别监听在 tmp/indexd.sock 和 tmp/searchd.sock
shell># /usr/local/xunsearch/bin/xs-ctl.sh -b unix start
```

> 说明：
>
> ​	有必要指出的是，关于搜索项目的数据目录规划。搜索系统将所有数据保存在 $prefix/data 目录中。 如果您希望数据目录另行安排或转移至其它分区，请将 $prefix/data 作为软链接指向真实目录。

> 注意：
>
> ​	在启动的过程中，可能会报以下错误
>
> ![1544344193035](1544344193035.png)

#### 3.4.1、解决方法

##### 3.4.1.1、复制 libstdc++.so.6.0.18 文件到 /usr/lib64 

```shell
shell># cp /usr/local/lib64/libstdc++.so.6.0.18 /usr/lib64 
```

##### 3.4.1.2、删除 libstdc++.so.6 文件

```shell
shell># rm -rf /usr/lib64/libstdc++.so.6
```

##### 3.4.1.3、创建软链接

```shell
shell># ln -s /usr/lib64/libstdc++.so.6.0.18 /usr/lib64/libstdc++.so.6
```

##### 3.4.1.4、查看效果

```shell
shell># strings /usr/lib64/libstdc++.so.6 | grep GLIBC
```

处理前：

![1545618624940](1545618624940.png)

处理后：

![1545620378915](1545620378915.png)

### 3.5、SDK 目录结构

```
$prefix(SDK目录)
|- doc/                    离线 HTML 版相关文档
|- app/                    用于存放搜索项目的 ini 文件
|- lib/XS.php              入口文件，所有搜索功能必须且只需包含此文件    
\- util/                   辅助工具程序目录
    |- RequireCheck.php    用于检测您的 PHP 环境是否符合运行条件
    |- IniWizzaard.php     用于帮助您编写 xunsearch 项目配置文件
    |- Quest.php           搜索测试工具
    \- Indexer.php         索引管理工具
```

## 4、体验 DEMO 项目

### 1、查看项目配置文件

```shell
shell># cat /usr/local/xunsearch/sdk/php/app/demo.ini
```

![1544345119859](1544345119859.png)

> 参数说明：
>
> [pid]	中括号中的 pid 代表字段名称
>
> type = id		type 设置字段类型，id 为主键

#### 1.1、type 字段类型

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| id      | 主键型，确保每条数据具备唯一值，是索引更新和删除的凭据，每个搜索项目必须有且仅有一个 id 字段，该字段的值不区分大小定 |
| string  | 字符型，适用多数情况，也是默认值                             |
| numeric | 数值型，包含整型和浮点数，仅当字段需用于以排序或区间检索时才设为该类型，否则请使用 string即可 |
| date    | 日期型，形式为 YYYYmmdd 这样固定的 8 字节，如果没有区间检索或排序需求不建议使用 |
| title   | 标题型，标题或名称字段，至多有一个该类型的字段               |
| body    | 内容型，主内容字段，即本搜索项目中内容最长的字段，至多只有一个该类型字段，本字段不支持字段检索 |

### 2、填充索引数据

出于测试方便，我们采用 csv 格式来写入索引数据，请先按以下方式操：

```shell
shell># /usr/local/xunsearch/sdk/php/util/Indexer.php --source=csv --clean demo
```

![1544346129635](1544346129635.png)

> 注意：有可能会报错误：/usr/bin/env: php: 没有那个文件或目录
>
> ![1550977083365](1550977083365.png)
>
> 解决方法：
>
> 将PHP设置系统环境变量中
>
> shell># ln -s /usr/local/php/bin/php /usr/local/bin/php

```
# 测试数据
1,关于 xunsearch 的 DEMO 项目测试,项目测试是一个很有意思的行为！,1314336158
2,测试第二篇,这里是第二篇文章的内容,1314336160
3,项目测试第三篇,俗话说，无三不成礼，所以就有了第三篇,1314336168
```

![1544346191261](1544346191261.png)

> 说明：
>
> ​	最后一条数据打完后必须敲入回车，然后按 Ctrl-D 结束操作。
>
> ​	在 Windows 的命令行下运行请使用 Ctrl-Z 来表示结束。

### 3、测试搜索

首先，我们体验一下正常的搜索，分别以关键词 项目、测试、项目测试、俗话说、莫须有 进行检索：

```shell
shell># /usr/local/xunsearch/sdk/php/util/Quest.php demo 项目
shell># /usr/local/xunsearch/sdk/php/util/Quest.php demo 测试
shell># /usr/local/xunsearch/sdk/php/util/Quest.php demo 项目测试
shell># /usr/local/xunsearch/sdk/php/util/Quest.php demo 俗话说
shell># /usr/local/xunsearch/sdk/php/util/Quest.php demo 莫须有
```

![1544346389051](1544346389051.png)

## 5、PHP 操作迅搜

### 5.1、搜索概述

代码参考：code/demo1.php

![1545621962291](1545621962291.png)

```php
  1	<?php
  2         //引入核心文件
  3         require_once '/usr/local/xunsearch/sdk/php/lib/XS.php';
  4         //建议 xs 对象，项目(索引)名称为：demo
  5         $xs = new XS('demo');
  6         //获取搜索的对象
  7         $search = $xs->search;
  8         //搜索内容为：测试
  9         $query = '项目测试';
 10         $search->setQuery( $query );
 11         $data = $search->search();
 12         var_dump( $data );
 13         $count = $search->count();
 14         var_dump( $count );

```

效果如下图所示：

![1544362630037](1544362630037.png)

### 作业

​	搜索内容让用户自己输入

### 5.2、使用 MySQL 数据生成索引

本示例以sh_goods.sql做数据

#### 5.2.1、创建索引文件

```
shell># cd /usr/local/xunsearch/sdk/php/app
shell># cp demo.ini goods.ini
```

![1544366102527](1544366102527.png)

#### 5.2.2、修改 goods.ini 文件

```
shell># vim goods.ini
```

![1544366203218](1544366203218.png)

内容如下图所示：

![1550979354128](1550979354128.png)

#### 5.2.3、把 sh_goods.sql 文件上传到服务器

通过FTP软件上传

![1544366669403](1544366669403.png)

#### 5.2.4、导入数据到 test 数据库中

```
mysql> create database text charset=utf8;
shell># mysql -uroot -p123456 test < sh_goods.sql
```

![1550979699487](1550979699487.png)

#### 5.2.5、生成索引

```
shell># /usr/local/xunsearch/sdk/php/util/Indexer.php --project=goods --source=mysql://root:123456@localhost/text/sh_goods --rebuild
```

![1544367670120](1544367670120.png)

#### 5.2.6、测试

```
shell># /usr/local/xunsearch/sdk/php/util/Quest.php goods 小米
```

![1544367740551](1544367740551.png)

## 6、案例

示例代码：code/goods.php

### 6.1、代码如下

![1544369972063](1544369972063.png)

```php
<?php
	if($_POST){
		require_once '/usr/local/xunsearch/sdk/php/lib/XS.php';
		// 建立 XS 对象，项目名称为：goods
		$xs = new XS('goods');
		// 获取搜索对象
		$search = $xs->search;
		$goods = isset($_POST['goods'])?$_POST['goods']:'';
		$data = $search->search($goods);
	}
?>
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<form action="" method="post">
		搜索：<input type="text" name="goods">　
		<input type="submit" value="搜索">
	</form>
	<?php
		if(isset($data) && $data){
			foreach ($data as $v){
				echo $v->goods_name.'<hr/>';
			}
		}
	?>
</body>
</html>
```

效果：

![1544370035311](1544370035311.png)

### 6.2、回显示关键字

代码如下：

![1544407692712](1544407692712.png)

效果如下：

![1544407714181](1544407714181.png)

### 6.3、为关键字设置高亮

代码如下：

![1544408444819](1544408444819.png)

```php
<?php
	if($_POST){
		require_once '/usr/local/xunsearch/sdk/php/lib/XS.php';
		// 建立 XS 对象，项目名称为：goods
		$xs = new XS('goods');
		// 获取搜索对象
		$search = $xs->search;
		$goods = isset($_POST['goods'])?$_POST['goods']:'';
		$data = $search->search($goods);
	}
?>
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<form action="" method="post">
		搜索：<input type="text" name="goods" value="<?php echo $goods; ?>">　
		<input type="submit" value="搜索">
	</form>
	<style type="text/css">
		em{
			color: red;
		}
	</style>
	<?php
		if(isset($data) && $data){
			foreach ($data as $v){
				echo $v->rank().'&nbsp;&nbsp;'.$search->highlight($v->goods_name).'&nbsp;[ '. $v->percent() .' %]<hr/>';
			}
		}
	?>
</body>
</html>
```

效果如下：

![1544408507847](1544408507847.png)

### 作业

​	使用ajax实现搜索功能 

# 四、MySql的查询缓存机制

## 1、查询缓存机制的概述

​	在mysql所有数据查询都是实时的过程，不管一次查询数据是否有找到，那么数据库的编译器都必须在硬盘中做一次数据的查找响应，原理如下所示：

![1](1.png)

MySql提供了一个机制，就是开启一个叫Qcache的空间缓存，这个缓存空间存在MySql编译器内存当中，有一个用户发起了一次请求编译器会判断当前是否已经发起过，如果发起过，编译器将不会继续在硬盘当中做出查找的响应，而是把Qcache当中的结果进行返回，如果用户发起的请求是第一次的发起，那么mysql依然会从硬盘当中查找数据，但是同时也会把查找结果保存到Qcache空间当中，准备下一次查询响应，其原理如下所示：

![2](2.png)

## 2、开启MySql的查询缓存

开启MySql查询缓存就是开启编译器中的Qcache空间，这个空间默认的情况下以字节的大小来计算空间的大小，所以我们需要使用字节换算来开启Qcache空间的大小

1k = 1024字节

1M = 1024k =1024*1024字节

如果希望表示一个64M的字节大小，那么我们可以换算成为以下形式公式：

64M = 64 * 1024 * 1024字节

开启MySql查询缓存的语法格式如下：

set global query_cache_size = 字节大小

需求1：开启64M的查询缓存空间(Qcache)，代码如下：

set global query_cache_size = 64 * 1024 * 1024

如果没有开启查询缓存时，使用select语句查询tbl_no的username="user999999"的信息记录如下所示：

第1次查询如下,用时：3.47秒

![1535457477689](1535457477689.png)

第2次查询如下,用时3.29秒,虽然有所提升,但是并不稳定,也不够理想

![1535457548155](1535457548155.png)

这时我们使用查询缓存机制,开启一个64M的Qcache空间,如下所示：

![1535457621553](1535457621553.png)

开启成功后，我们重新对tbl_no表username="user999999"进行查询

第1次查询：由于第1次查询Qcache并没有任何的记录，所以mysql会从硬盘中查找
耗时：

![1535457716621](1535457716621.png)

第2次查询：由于第2次查询时Qcache已经记录当前的查询结果，因此mysql不会去硬盘中查找数据，直接从Qcache当中返回数据,因此耗时接近0秒甚至达到0秒

![1535457748074](1535457748074.png)

假设这时我们换一个查询条件，查询username="user888888"的信息如下：
第1次查询：发觉这时又没有缓存的结果，因为这是第1次查询，耗时3.26秒

![1535457837382](1535457837382.png)

第2次查询，发觉耗时接近0秒甚至达到0秒，查询缓存的开启就会让第1次查询很慢之后都会很快，所以达到一定的优化过程

![1535457871532](1535457871532.png)

==查询缓存的缺点是==：如果查询缓存的累积空间达到64M的上限，那么mysqld就会自动释放Qcache空间，使得原来缓存消失，并且开启过多的Qcache空间会导致mysql编译器内存空间被占用过多，因此后来有人使用Memcache和redis进行取代，但是如果你的数据不算非常多，那么使用查询缓存是非常值得，因为查询缓存的开启方式和使用方式非常的简单

## 3、关闭mysql的查询缓存

关闭mysql的查询缓存有两种方法：

### 3.1、重启mysql服务

### 3.2、使用命令关闭

set global query_cache_size = 0;（建议使用该方法）

![1535458057101](1535458057101.png)

关闭之后，查询查询username='user999999'的信息如下：

![1535458125766](1535458125766.png)

发现查询缓存消失，数据查询重新开始变慢

## 4、查看查询缓存是否已经开启

有时候我们需要知道这个查询缓存是否已经开启，我们可以使用以下命令进行查看：

show variables like '%query_cache_size%';

![1535458260816](1535458260816.png)

# 五、MySql的慢查询日志

## 1、慢查询日志的概述

在开发当中，有时候我们希望能够捕捉到慢操作的相关具体命令，我们就需要使用慢查询日志(query_slow_logs)，然而慢查询日志不只是记录select语句的慢操作，其他的语句如：insert、update、delete如果慢的话也会被日志所记录。

慢查询日志有以下重要的函数(命令)：

#表示当前MySql认为慢操作的时间

show variables like 'long_query_time';

![1535467271359](1535467271359.png)

MySql默认情况下认为达到10秒钟的操作才算慢，这就与8秒原则相矛盾了，因此我们需要调整这个long_query_time的时间，根据自己喜欢认为怎么样才算慢去调整，一般调整为2秒钟。

#显示当前MySql的慢查询日志相关选项

show variables like '%slow%';

![1535467503853](1535467503853.png)

默认是没有开启慢查询日志，因此我们无法捕捉实时慢操作记录，slow_query_log_file为默认存放慢日志文件路径

#显示当前MySql的慢操作总数

show status like 'Slow_queries';

![1535467664399](1535467664399.png)

这个变量用于记录超过10秒的记录数，如果没有超过10秒则不记录。

## 2、开启慢查询日志

如果希望开启慢查询日志，我们就必须要在MySql配置文件中打开对应的配置选项，MySql的配置文件存放在C:\phpStudy\PHPTutorial\MySQL\my.ini当中，打开该文件在[mysqld]选项中添加以下内容

![1535522177741](1535522177741.png)

```mysql
#是否开启慢查询日志
slow_query_log=1
#mysql认为慢的时间
long-query-time=2
#日志存放的路径
slow_query_log_file=C:\phpStudy\PHPTutorial\MySQL\myslow.txt
```

如果重启没有报错，就代表配置选项设置成功，退出mysql登录重新登录，重看相关选项

![1535522250817](1535522250817.png)

```mysql
show variables like 'long_query_time';
show variables like '%slow%';
```

然后我们检查配置无误后，我们到C:\phpStudy\PHPTutorial\MySQL目录下查看，发觉有以下文件：

![1535522327042](1535522327042.png)

然后我们使用一个超过2秒钟的慢查询语句，如下所示

![1535522564036](1535522564036.png)

这时我们打开C:\phpStudy\PHPTutorial\MySQL\myslow.txt文件查看

![1535522646430](1535522646430.png)

# 六、总结

全文索引

​	like

设置全文索引

​	fulltext

迅搜

​	创建索引，索引文件后缀 .ini

​	直接导入数据库数据？可以

​	/usr/local/xs/sdk/php/util/Iindex.php --help  //查看帮助信息

​	/usr/local/xs/sdk/php/util/Query.php 索引名称 需要查找的内容  //测试？

php操作迅搜

$xs = new xs('索引文件名称');	//注意：不需要带后缀名

$search = $xs->search;

$search->search('关键字');

![1545641528051](1545641528051.png)

慢查询：

​	show variables like 'long_query_time'; // 查看慢查询时间 10秒

​	slow_query_log 开启慢查询日志功能

​	slow_query_log_file //存放慢查询日志文件路径

SQL语句才认为是同一条SQL，严格区分大小写

show status like '%qcache%'

