---
title: redis
date: 2019-02-18 20:17:33
tags: redis
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识点回顾

能够运用工具连接 memcached服务器

```shell
# telnet ip  port
shell># telnet 127.0.0.1 11211
```

能够运用set、get、add、delete、incr、decr命令

set 命令

​	作用：修改，如果key存在修改，否则新增

```shell
# 添加一个 key = php33，value = php33
> set php33 0 0 5
php33
# 说明
php33 代表key
第1个0代表标识符
第2个0代表过期时间
5代表着字节大小
```

get 命令

​	作用：获取一个存在key

```shell
# 获取一个 key = php33
> get php33
```

delete 命令

​	作用：删除一个key

```shell
# 删除一个 key = php33
> delete php33
```

incr 自增

​	作用：自增

​	如果key不存在？会报错，不会自动新增，key存在它类型必须 int

```shell
# 自增 1
> set age 0 0 2
12
> incr age 1  #代表着 age 自增1
```

decr 自减

​	作用：自减

​	如果key不存在？会报错，不会自动新增，key存在它类型必须 int

```shell
# 自减
> set age 0 0 2
12
>decr age 1 #代表着 age 自减1
```

flush_all 清空所有缓存

不建议直接操作该命令

```shell
> flush_all #清空所数据
```

能够实现Linux与windows系统上PHP的 memcached 扩展安装

windows （PHP）

​	1、首先看我php是几点几(版本)

​	2、看属性x86(32位)、x86_64(64位)

​	3、把 php_memcache.dll 文件复制到我php扩展目录(ext)

​	4、修改php.ini文件

​	5、重启apache

Linux 

​	1、执行 phpize 生成一个 configure 文件 /usr/local/php/bin/phpize

​	2、./configure --with-php-config=/usr/local/php/bin/php-cofnig

​	3、make

​	4、make install

​	5、修改 php.ini  

​	6、重启php/apache

能够掌握Linux与windows系统上扩展文件的区别

​	后缀名一样？

​	不一样

​	windows 下memcache扩展文件的后缀名 .dll

​	Linux 下 memcache 扩展文件的后缀名 .so

能够实现 php 操作 memcached 服务器

```php
<?php
    # 1、实例化 memcached 对象
    $m = new Memcached();
	# 2、连接 memcached
	#$m->addServer('ip', 'port');
	$m->addServer('127.0.0.1', 11211);

	# 新增一个key = php33 value = php33
	# $m->set( key, value[, 过期时间]);
	$m->set( 'php33', 'php33' ); # 过期时间默认为 0，代表永久不过期
	
	# 获取一个 key = php33 
	# $m->get( key );
	$data = $m->get( 'php33' );

	# 删除一个 key = php33
	# $m->delete( key );
	$m->delete( 'php33' );
```

memcached 分布集群

```php
<?php
    # 1、实例化 memcached 对象
    $m = new Memcached();
	# 2、连接
	$m->addServers( array(
    	# array( ip, pot, 权重 ),
        array('192.168.82.46', 11211, 50),
        array('192.168.82.244', 11211, 50),
        # ...
    ) );
	
	# 首先添加 1000条数据
    for($i = 1; $i<=1000; $i++){
        $m->set('i'.$i, $i);
    }
```
<!-- more -->
# 二、NoSql之Redis

## 1、Redis简介

中文网站：http://www.redis.cn

​	Redis 是 Remote Dictionary Server（远程数据服务）的缩写，它是NoSql中一款非常出色的产品，发明者是意大利人 antirez（Salvators Sanfilippo）。该软件使用 ==C语言==编写，它的存储格式也是key-value然而它支持丰富的数据结构，具体一共有==5种数据类型==，还有1种服务器命令类型(key服务器指令，不支持windows)

String（字符串类型）

list（链表）

hash（哈希表类型）

set（无序的集合）

sorted set（有序集合，缩写为zset）

而且 Redis 通过简单的配置把数据从内存保存到硬盘当中进行持久保存。

==Redis 为高并发而生，是NoSql中的佼佼者==

==Redis读的速度是 110000次/秒，写的速度是81000次/秒。==

## 2、Redis 和 Memcache 的区别

Redis 拥有安全认证的密码机制，Memcache 没有

Redis 是内存 + 硬盘的存储机制存储大小远超 memcached 的  64M

Redis 的 1 个 key 最大可以存储 1G，而 memcached是 1M

# 三、Redis 的安装和启动

## 1、在 Linux 下安装 Redis

https://www.cnblogs.com/qianxiaoruofeng/p/8046570.html

安装命令：

```shell
shell># yum -y install redis
```



Reids 的命令行工具是非常人性化，所以我们可以使用 xshell 工具来操作 Redis，安装的步骤如下图所示：

![1543036732777](1543036732777.png)

安装成功后如下图所示：

![1543036752775](1543036752775.png)

有可以报以下错误：

我们当前这个yum没有这个包，可以先安装：yum -y install epel-release

![1545099094392](1545099094392.png)

解决方法：

```shell
shell># yum -y install epel-release 
```

当安装完成之后，我们可以使用rpm -ql redis查看redis所有的安装目录和文件的存放位置，效果如下图所示：

![1543036824324](1543036824324.png)

## 2、启动并登录 Redis 客户端

### 2.1、启动 redis 服务器

```shell
shell># service redis start		# 启动
shell># service redis restart	# 重启
shell># service redis stop		# 停止
shell># service redis status	# 查看状态
```

![1543036853918](1543036853918.png)

> ==注意：==
>
> ​	使用 service redis status 会发现 Redis 服务已停，效果如下图所示：
>
> ![1543036943621](1543036943621.png)

![1545099450733](1545099450733.png)

### 2.2、链接 Redis

```
命令：redis-cli
```

![1545099595807](1545099595807.png)

> 注意：Redis 默认端口为：6379

### 2.3、停止 redis 服务

```
命令： service redis stop
```

![1543037114668](1543037114668.png)

> 注意：如果使用 service redis stop 无法停止 redis 服务，需要使用 pkill -9 redis 命令强制杀死 redis 进程

# 四、Redis 的数据类型和管理命令

## 1、String 数据类型

string 是 Redis 最基本的类型

### 1.1、get 命令

该命令用于获取 Redis 中的 key 对应的值，如果 key 不存在返回 nil

命令语法：get 键名

![1545099858857](1545099858857.png)

#### 1.1.1、如果一个键名不存在，获取该键名会返回 nil ( 就是英语的 null )

![1545099906069](1545099906069.png)

#### 1.1.2、获取一个已经存在的键名，可以得到该键名的值

![1545099934436](1545099934436.png)

### 1.2、set 命令

该命令用于设置 或者修改Redis 中的键值对

命令语法：set 键名(key)	值(value)

案例1：添加一个 key = name，value = 张大锤 的键值对。

![1545100078606](1545100078606.png)

案例2：把 name 的 value 修改为 qiudachui

![1545100245070](1545100245070.png)

==注意： Redis的过期时间 expire 命令(设置过期时间)和删除键的命令 del 是属于 key 服务器的管理指令==

在默认的情况下，Redis 会以一种叫做快照的模式（把数据存储在硬盘中）操作数据，所以无论是重启还是杀死 Redis的进程，都不会导致 Redis的数据丢失，因此 Redis 是一个持久化的 NoSql产品。

## 2、hash（哈希表）数据类型

### 2.1、hset 命令( hash set )

命令功能：在哈希表中设置一个字段(key)和一个字段的值(value)，用一张表来表示一些特定的信息，如下所示：

| id   | username  | age  | job  |
| ---- | --------- | ---- | ---- |
| 1    | qiudachui | 18   | IT   |
| 2    | lidachui  | 17   | IT   |

命令格式：hset 哈希表名的名称  字段(key) 字段值(value)

```
ip:6379> hset userInfo:qiudachui username qiudachui
ip:6379> hset userInfo:qiudachui id 1
ip:6379> hset userInfo:qiudachui job IT
ip:6379> hset userInfo:qiudachui age 22
```

![1545100817979](1545100817979.png)

### 2.2、hget 命令

命令功能：在一张指定的哈希表中获取字段的值，如果字段不存在将返回 nil

命令格式：hget 哈希表的名称 字段名称(key name)

案例：在userInfo:qiudachui 的哈希表中获取姓名(username)和年龄(age)，如下所示：

```
ip:6379> hget userInfo:qiudachui username
ip:6379> hget userInfo:qiudachui age
ip:6379> hget userInfo:qiudachui job
```

![1545100876507](1545100876507.png)

以上写法相当于：

```
select username from userInfo where username='qiudachui'
```

### 2.3、hmset 命令(hash multiple set)

命令功能：在哈然表中设置多个字段(field)和多个字段的值(value)

命令格式：hmset 哈希表的名称 字段(key) 字段值(value).....

```
ip:6379> hmset userInfo:lidachui id 2 username lidachui age 17 job IT
```

![1545101757743](1545101757743.png)

### 2.4、hgetall 命令

命令功能：在哈希表获取所有的字段和值

命令格式：hgetall 哈希表的名称

```
ip:6379> hgetall userInfo:lidachui
```

![1545102865506](1545102865506.png)

相当于编写

```
select * from userInfo:lidachui
```

案例：把userInfo:lidachui的年龄(age)修改为16

修改前：

```
ip:6379> hgetall userInfo:lidachui
```

![1545103016524](1545103016524.png)

修改：

```
hset userInfo:lidachui age 16
```

![1545103103388](1545103103388.png)

修改后：

```
ip:6379> hgetall userInfo:lidachui
```

![1545103131605](1545103131605.png)

## 3、list 链表数据类型

链表数据结构中的栈：先进后出的特点，其原理图如下：

![1542987736596](1542987736596.png)

链表数据结构中的队列：先进先出，其原理图如下：

![1542987785965](1542987785965.png)

### 3.1、lpush 命令(跟栈相关)

命令功能：在链表的栈中由头部压入一条数据

命令格式：lpush 链表的名称(栈名称) 值

案例：在一个名为 list1 栈中压入数据：one、two、three

```
ip:6379> lpush qiudachui one
ip:6379> lpush qiudachui two
ip:6379> lpush qiudachui three
```

![1545104172959](1545104172959.png)

以上分别在栈中头部压入的顺序为：one、two、three，如下图所示：

![1542988008884](1542988008884.png)

### 3.2、rpush 命令(跟队列相关)

命令的功能：在链表的队列中由尾部压入一条数据

命令格式：rpush 链表的名称(队列) 值

案例：在一个名为 list2 的队列中插入数据：one、two、three

```
ip:6379> rpush list2 one
ip:6379> rpush list2 two
ip:6379> rpush list2 three
```

![1545104294772](1545104294772.png)

以上分别在队列中尾部压入的顺序为：one、two、three，如下图所示：

![1542988755645](1542988755645.png)

### 3.3、lrange 命令(跟队列和栈都相关)用于查询

命令功能：在链表中获取一个范围(索引的范围)的数据

命令格式：lrange  链表的名称  索引开始位置  索引结束位置(-1 代表获取到全部)

案例1：获取一个栈的所有数据，例如：lidachui

```shell
ip:6379> lrange qiudachui 0 -1
```

![1550458963750](1550458963750.png)

如下图所示：

![1542988958787](1542988958787.png)

案例2：获取一个队列的所有数据，例如：list2

```
ip:6739> lrange list2 0 -1
```

![1550459004954](1550459004954.png)

如下图所示：

![1542989072953](1542989072953.png)

案例3：在 qiudachui栈中，从位置为1的地方开始获取到最后

```
ip:6379> lrange qiudachui 1 -1
```

![1550458774632](1550458774632.png)

案例4：在 list2 队列中，从位置为1的地方开始获取到最后

```
ip:6379> lrange list2 1 -1
```

![1550458805465](1550458805465.png)

### 3.4、lpop 命令(与栈和队列相关)

命令的功能：弹出队列中的头部元素或者弹出栈中的头部元素，并且返回其值

命令格式：lpop 链表名称

案例1：弹出 list2 队列中的头部元素并且删除

```
ip:6379> lpop list2
ip:6379> lrange list2 0 -1
```

![1550459619443](1550459619443.png)

案例2：弹出 qiudachui 栈中的头部元素并且删除

```
ip:6379> lpop qiudachui
ip:6379> lrange qiudachui 0 -1
```

![1545115579367](1545115579367.png)

lpop 对于队列和栈的操作都是相同，都是弹出头部元素且删除该元素，在现实开发当中

lpop 大部分时间用于操作队列（称为：消息队列技术）

### 3.5、rpop命令

命令的功能：移除并获取列表最后一个元素

命令格式：rpop key

例1：弹出qiudachui最后一个元素

```
ip:6379> rpop qiudachui
ip:6379> lrange qiudachui 0 -1
```

![1550459803165](1550459803165.png)

### 3.6、ltrim 命令(一般用于队列操作比较多)

命令的功能：让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除

命令格式：ltrim 链表名称 开始的位置  结束的位置

现在有一个队列叫 list3，其元素如下所示：

```
ip:6379> rpush list3 one two three four five
```

![1550459860099](1550459860099.png)

查询结果如下：

```
ip:6379> lrange list3 0 -1
```

![1550459885661](1550459885661.png)

如果我们希望保存 two、three、four 区间的值,其他都删除,那么我应该怎么做?我们就需要使用 ltrim  命令，如下所示：

```
ip:6379> ltrim list3 1 3
ip:6379> lrange list3 0 -1
```

![1550460034919](1550460034919.png)

在已经拥有的 list3 数据状况如下，我们的需求变成保留 two 和 four，如下图所示：

```
ip:6379> ltrim list3 0 -1
ip:6379> lrange list3 0 -1
```

![1550460123276](1550460123276.png)

> ==注意：==
>
> ​	==以上要求是无法实现，因为 ltrim 保存区间必须具有连续性，而不是能是断开连续的==

> ==总结：==
>
> ​	==list 数据类型中，作为PHP开发者使用队列的次数和频率高于栈，一般用于实现一种叫做：消息队列的功能==

## 4、set 集合数据类型(无序 )

在现实开发当中集合一般用于社交网站或者社交软件的朋友圈功能(例如：新浪微博好友圈)

![1542991120404](1542991120404.png)

在现实的社交网站开发当中存在着一个好友推荐的功能，在 Redis 当中这种功能可以使用集合中求差集的方法进行实现

差集的定义：一个集合存在某一个元素而在另外一个集合中不存在，该元素就属于两个集合的差集

![1542991425476](1542991425476.png)

交集的定义：一个集合和另外一个集合共同的元素，称为交集，在现实开发当中就是社交网站的共同好友功能

![1542991567292](1542991567292.png)

并集的定义：一个集合和另外一个集合进行合并然后去除重复的元素后所得到的结果就是并集，在实际中我们一般运用群聊的功能

![1542991774357](1542991774357.png)

### 4.1、sadd 命令(set add)

命令的功能：在无序集合当中添加一个元素，该元素如果存在该元素不会被重复添加

命令格式：sadd 集合的名称   集合的元素

案例1：创建一个名为 long的无序集合，含有元素a1、b1、c1、d1

```
ip:6379> sadd long a1
ip:6379> sadd long b1
ip:6379> sadd long c1
ip:6379> sadd long d1
```

![1545119262178](1545119262178.png)

案例2： 创建一个名为 youlong的无序集合，含有元素a1、b1、c2、d2、e2

```
ip:6379> sadd youlong a1 b1 c2 d2 e2
```

![1545119453099](1545119453099.png)

### 4.2、smembers 命令

命令功能：查看一个无序集合中的所有元素

命令格式：smembers 无序集合的名称

 案例：分别查看 zsf 和 lsf 的元素

```
ip:6379> smembers long
ip:6379> smembers youlong
```

![1545119564828](1545119564828.png)

### 4.3、sdiff 命令(好友推荐功能)

命令的功能：以一个集合作为标准去求另外一个集合不存在的元素，我们称为差集(可以参考集合概念中差集的图片辅助理解)

命令格式：sdiff 作为标准的集合名称  求差集的集合名称

案例1：以logn作为标准求youlong的差集（第1个参数就作为标准的参数）

```
ip:6379> sdiff logn youlong
ip:6379> smembers logn
ip:6379> smembers youlong
```

![1550460903846](1550460903846.png)

案例2：以youlong 作为标准求logn的差集

ip:6379> sdiff youlong logn

![1550460953189](1550460953189.png)

### 4.4、sinter 命令

命令的功能：一个集合和另外一个集合共同的元素，我们称为交集(可以参考集合概念中交集的图片辅助理解)

命令格式：sinter 集合名称1  集合名称2

案例：求出logn和youlong的共同好友

```
ip:6379> sinter logn youlong
ip:6379> sinter youlong logn
```

![1550461066016](1550461066016.png)

假设logn 与 youlong 同时加了 brucelee为好友，如下图所示：

```
ip:6379> sadd logn brucelee
ip:6379> sadd youlong brucelee
```

![1550461130734](1550461130734.png)

再次求交集，结果如下所示：

```
ip:6379> sinter long youlong
ip:6379> sinter youlong long
```

![1545120211534](1545120211534.png)

### 4.5、sunion 命令

命令的功能：求出两集合合并后所有的元素并去掉重复的元素的结果称为并集(可以参考集合概念中并集的图片辅助理解)

命令格式：sunion  集合名称1  集合名称2 ...

案例：例如把logn的朋友和youlong的朋友合并在一起进行群聊功能就可以使用以下命令：

```
ip:6379> sunion logn youlong
ip:6379> sunion youlong logn
```

![1550461296082](1550461296082.png)

### 4.6、scard 命令

命令功能：统计集合中的元素个数，并返回总数的整型值，该命令一般在社交网络中用于统计粉丝数

命令格式：scard 集合名称

案例：例如希望知道logn或youlong有多少个好友

```
ip:6379> scard logn
ip:6379> scard youlong
```

![1545120306179](1545120306179.png)

### 4.7、srem 命令

命令功能：用于删除无序集合中的元素，在社交网络开发中用于黑名单功能

命令格式：srem  集合名称  元素名称

案例：例如删除logn 集合中为  c1  的元素，执行命令如下：

```
ip:6379> srem logn c1
ip:6379> smembers logn
```

![1550461534747](1550461534747.png)

## 5、zset 集合数据类型(有序集合)

​	sorted set 是 set 的一个升级版本，在 set 有基础上增加了一个顺序属性，这一属性在添加修改元素的时候可以指定，每次指定后，zset 会自动重新按新的值调整顺序。==有序列表值完成的是集合元素排序的功能，一般很少用于其他方向。==

### 5.1、zadd 命令

命令功能：向有序集合中添加元素。如果该元素存在，则更新其顺序。

在 zset 当中序号是顺序，索引号是下标，注意区分

命令格式：zadd 集合名   序号 元素

案例：例如有一个明星社区网站， 需要前3名的明星进行排名，需求是人气最高的明星排在第1位，以此类推；假设有：杨幂--人气最高，鹿晗--人气第2，游龙--人气第3

```
ip:6379> zadd stars 1 liminxian
ip:6379> zadd stars 2 quidachui
ip:6379> zadd stars 3 youlong
```

![1545120791704](1545120791704.png)

>  注意：1，2，3叫序号，不叫索引，liminxian的序号是1，而索引号是0

### 5.2、zrange 命令

命令功能：按序号升序(由小到大)获取有集合中的内容

命令格式：zrange 集合名称  开始位置(索引)  结束位置(索引)(-1 获取全部)

案例：获取 stars 当中所有有序集合中的元素，用升序进行排列

```
ip:6379> zrange stars 0 -1
```

![1545120976644](1545120976644.png)

![1545121019692](1545121019692.png)

只想获取liminxian

![1545121056283](1545121056283.png)

### 5.3、zrevrange 命令

命令功能：按序号降序(由大到小)获取有序集合中的内容

命令格式：zrevrange  集合名称  开始位置(索引)  结束位置(索引)(-1 获取全部)

案例：获取 stars 当中所有有序集合中的元素，用降序进行排列

```
ip:6379> zrevrange stars 0 -1
```

![1545121154268](1545121154268.png)

> 总结：无序集合为了功能而生,但zset集合为了排序而生

# 五、与Key 相关的命令(管理命令)

## 1、keys * 命令

命令功能：返回当前数据库里面的键(key)

```
ip:6379> keys * 
```

![1545121295373](1545121295373.png)

![1545121393699](1545121393699.png)

## 2、exists 命令

命令功能：判断一个键(key)是否存在

命令格式：exists 键名

案例：判断 stars 是否存在 Redis 中

```
ip:6379> exists stars
```

![1545121517676](1545121517676.png)

> 注意：如果返回 0 代表该键名不存在，如果返回 1 代表键名存在

## 3、del 命令

命令功能：删除指定的键(key)，如果返回 1 代表删除键名成功，返回 0 代表删除失败

命令格式：del  键名

```
ip:6379> del qiudachui
ip:6379> get qiudachui
```

![1550472080434](1550472080434.png)

## 4、type 命令

命令功能：返回一个键的数据类型

命令格式：type  键名

```
ip:6379> type long
ip:6379> type qiudachui
```

![1545121858126](1545121858126.png)

## 5、expire 命令

命令功能：设置键的有效期，如果不调用该命令设置键名，默认的情况下键名本身就是永远不过期

命令格式：expire  键名  有效期(秒数)

案例：设置一个 key 在 20 秒后过期

```
ip:6379> expire name 20
```

![1545122113681](1545122113681.png)

## 6、ttl 命令

命令作用：查看一个 key 的过期剩余时间

命令格式：ttl  键名

```
ip:6379> ttl name
```

![1545122182610](1545122182610.png)

> 注意：如果是负数代表完全过期，并且已经被删除

# 六、Redis 安全认证

在默认情况下 Redis 不需要任何密码就可以登陆，任何客户端只要连接6379端口就可以调用Redis命令，安全性肯定不够，如果希望提高 Redis 的安全性，我们可以为 Redis 设置一个密码。Redis 把设置 密码的过程称为安全认证功能(Redis Auth)

密码存放在 Redis 的配置文件中(例如：/etc/redis.conf)

设置安全认证的步骤如下：

第1步：使用 vim 打开 /etc/redis.conf 文件，查找到 foobared，如下图所示：

```shell
shell># vim  /etc/redis.conf
```

![1543076673926](1543076673926.png)

第2步：把 requirepass 前面的#(井号)去除，将后面的 foobared 修改为：123456(Redis认证密码)

![1545123137639](1545123137639.png)

设置完成后保存并退出

> ==注意：内容必须顶格，requirepass前面不能有任何空格等==

第3步：重启 Redis 服务

```shell
shell># service redis restart
```

![1543076751493](1543076751493.png)

第4步：使用 redis-cli 命令登陆，发觉可以进入客户端界面但是无法操作

```
ip:6379> hgetall userInfo:qiudachui
ip:6379> set php33 room-1
```

![1550472861188](1550472861188.png)

第5步：使用安全认证密码进行登陆

安全认证登陆命令：redis-cli -a 安全认证密码

```shell
shell># redis-cli -a 123456
```

![1550472809272](1550472809272.png)

# 七、Redis 的持久化 AOF 设置

在 Redis 当中，Redis 有两种持久化模式，默认为快照样式(用于保存数据到硬盘的模式)

## 1、Redis 的快照模式

默认安装完成就会自动开启的持久化模式

快照文件默认存储以下位置：/var/lib/redis/dump.redb

```shell
shell># cd /var/lib/redis
shell># ls -lh
```

![1543076995396](1543076995396.png)

默认情况下快照模式==没有利用计算机的内存==，只是单方面反数据置于硬盘当中，如果希望 Redis 把硬盘+内存的存储方式利用起来，则要调整为 aof 模式

## 2、Redis 中的 Aof 模式

如果开启硬盘+内存方式存储数据，Redis 默认情况下会先把数据存放到内存中，然后每隔 1 秒钟不把内存的数据同步到硬盘中

首先我们需要观察 /var/lib/redis 下有没有存在一个 .aof 的文件，查看结果如下图所示：

```shell
shell># /var/lib/redis
shell># ls -lh
```

![1543077035462](1543077035462.png)

> ==注意：如果只有快照文件，而没有 aof 的文件，说明 redis 没有开启 aof 持久化模式===

我们需要通过编辑 /etc/redis.conf 文件去开启 redis 的 aof 持久化模式，设置 aof 持久化模式的步骤如下：

### 2.1、使用 vim 打开 /etc/redis.conf 文件，搜索appendonly

```shell
shell># vim /etc/redis.conf
```

![1543077116048](1543077116048.png)

> 注意：如果 appendonly 为 no 表示当前是快照模式，不是 aof 模式

### 2.2、把 appendonly 的选项由 no 改为 yes

代表开启 aof 持久化模式，如下图所示：

![1543077135121](1543077135121.png)

### 2.3、重启 redis 服务

```shell
shell># service redis restart
```

![1543077163020](1543077163020.png)

### 2.4、查看是否可启成功

进入 /var/lib/redis 目录下去查看是否有 aof 文件

```shell
shell># cd /var/lib/redis
shell># ls -lh
```

![1543077192536](1543077192536.png)

> ==注意：如果我们添加数据，会改变 appendonly.aof 文件的大小==

# 八、PHP 中开启 Redis 扩展

## 1、安装

### 1.1、下载

下载地址：https://codeload.github.com/phpredis/phpredis/zip/develop

### 1.2、上传到服务器

可以使用 rz 上传或 ftp 上传

> 注意：必须得安装了 vsftpd  或  lrzsz，我们这里以 rz 为例

### 1.3、解压与进入目录

```shell
shell># cd ~
shell># unzip phpredis-develop.zip
```

![1543048105571](1543048105571.png)

```shell
shell># cd phpredis-develop
```

![1543048162367](1543048162367.png)

### 1.4、生成编译文件

```shell
shell># /usr/local/php/bin/phpize
```

![1543048205748](1543048205748.png)

### 1.5、进行软件配置和环境检测

```shell
shell># ./configure --with-php-config=/usr/local/php/bin/php-config
```

![1543048398270](1543048398270.png)

### 1.6、编译软件并且进行安装

```shell
shell># make && make install
```

![1543048489883](1543048489883.png)

成功如下图所示：

![1543048534222](1543048534222.png)

## 2、配置

### 2.1、修改 php.ini 加载 Redis 组件

```shell
shell># vim /usr/local/php/etc/php.ini
```

![1550473915785](1550473915785.png)

### 2.2、结束PHP进程

```shell
shell># pkill  -9  php-fpm
```

![1543048763406](1543048763406.png)

### 2.3、重新启动PHP进程

```shell
shell># service php-fpm start
```

![1543048790392](1543048790392.png)

### 2.4、检查是否开启成功

### 在站点目录下创建一个phpinfo.php文件，输入以下内容

![1543048899532](1543048899532.png)

在默认的情况下，redis的作用设置了一个本地访问机制,只允许黑窗口进行连接,这个机制把php的访问也阻止了，因此我们需要把本地访问机制关闭，操作如下所示：

第1步：使用vim打开/etc/redis.conf文件，找打bind 127.0.0.1这个选项如下所示：

```shell
shell># vim /etc/redis.conf
```

![1543049189985](1543049189985.png)

第2步：在bind前面加上#号进行注解

![1543049245983](1543049245983.png)

第3步：重启redis服务器

```shell
shell># service redis restart
```

![1543049421411](1543049421411.png)

# 九、PHP 操作 Redis

​	Redis 在 php 的官方里面并没有被认可，所以 php 的官方没有 Redis 的 php 开发文档，因此对我们使用 php 操作 Redis 就带来了问题，所以我们需要备用一些离线的手册作为日后开发只用。可以使用以下这个文档，也可以自己去下载自己喜欢的文档。

## 1、使用php连接Redis

参考代码:code/redis.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550474610975](1550474610975.png)

效果如下：

![1550475249829](1550475249829.png)

## 2、PHP对Redis的各种数据类型的操作

### 2.1、操作 string 类型

参考代码:code/string.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550475766095](1550475766095.png)

效果如下：

![1550475732470](1550475732470.png)

### 2.2、操作 hash 类型

参考代码:code/hash.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550476347456](1550476347456.png)

效果如下：

![1550476358353](1550476358353.png)

参考代码:code/hashAll.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

![1550476675166](1550476675166.png)

效果如下图所示：

![1550476653215](1550476653215.png)

### 2.3、操作set无序集合

参考代码:code/sadd.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550477106113](1550477106113.png)

效果如下：

![1550477053897](1550477053897.png)

> 注意：一般无序集合在开发当中使用比较少

### 2.4、操作zset有序集合

参考代码:code/zadd.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550477840011](1550477840011.png)

![1550477860143](1550477860143.png)

效果如下：

![1550477788646](1550477788646.png)

> ==注意：==
>
> ==Zrange 与 zRevRange 有4个参数，第4个参数默认为：false，如果想键值调换就把第4个参数设置为：true==

# 十、使用 Redis 实现消息队列

在银行办理业务的过程就是使用 Redis 的消息队列完成，过程分以下两个步骤：

- 取号(把取号人的数数据压入 list 队列中)
- 叫号(把 list 队列中的头部元素弹出队列)

## 1、代码实现取号过程

参考代码:code/setList.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550479361053](1550479361053.png)

效果如下：

![1550479254753](1550479254753.png)

![1550479343497](1550479343497.png)

## 2、代码实现叫号过程

参考代码:code/getList.php，上传到 /mydata/wwwroot/php33/www/redis 下进行测试

代码如下：

![1550479641504](1550479641504.png)

效果如下：

![1550479619235](1550479619235.png)

# 十一、总结

1、redis有几种数据类型

答：5种

分别：string、hash、list、set(无序集合)、sorted set（有序集合，缩写为zset）

2、管理命令有哪些？

答：keys、exists判断key是否存在、del、type、expire、ttl

3、redis默认是否开启安全认证？

答：无

修改redis配置文件，在配置文件中查找requirepass，把他前面#删除，后面跟是他密码。必须顶格

4、redis默认是否允许使用服务器ip访问？

答：不，默认只允许localhost或127.0.0.1

修改redis配置文件，在配置文件中查找到 bind，在他前面加#

5、redis默认是否开记aof模式(硬盘+内存存储方式)？

答：无

修改redis配置文件，在配置文件中查找到 appendonly 把他后面 no 修改为 yes

6、在PHP中开启 redis 扩展文件

phpize 生成一个configure文件

./configure 生成编译文件 Makefile

make 生成一个二进制文件

make install 编译并安装

# 十二、win开启php的redis扩展(扩展)

## 1、查看PHP版本

![1550481330406](1550481330406.png)

## 2、复制扩展文件到 php的ext目录下

![1550481380420](1550481380420.png)

## 3、修改 php.ini 文件

![1550481418748](1550481418748.png)

保存后，重启 apache

效果如下图所示：

![1550481448977](1550481448977.png)

# 十三、win安装redis服务器(扩展)

## 1、下载(windwos)

https://github.com/MicrosoftArchive/redis/releases

官方网站：https://redis.io/

中文网站：http://redis.cn/

## 2、启动

#> redis-server.exe redis.windows.conf

![1550481685226](1550481685226.png)

![1550481786241](1550481786241.png)

## 3、添加系统服务中

```cmd
D:\gzphp33\Redis\资料\软件\Redis-x64-3.2.100> redis-server --service-install redis.windows-service.conf --loglevel verbose
```

![1550482122812](1550482122812.png)

## 4、从系统服务中删除 redis 服务

```cmd
D:\gzphp33\Redis\资料\软件\Redis-x64-3.2.100>redis-server --service-uninstall
```

![1550482190797](1550482190797.png)

## 5、常用操作

```cmd
redis-server --service-install --service-name redisService1 --port 10001 指定端口
redis-server --service-start --service-name redisService1	启动服务，指定服务名称
redis-server --service-install --service-name redisService2 --port 10002 指定端口
redis-server --service-start --service-name redisService2 启动服务，指定服务名称
redis-server --service-install --service-name redisService3 --port 10003 指定端口
redis-server --service-start --service-name redisService3 启动服务，指定服务名称
```







