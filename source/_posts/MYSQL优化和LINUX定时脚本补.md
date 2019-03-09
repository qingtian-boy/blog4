---
title: MYSQL优化和LINUX定时脚本补
date: 2019-03-04 17:10:37
tags: MySql
---




# 	  MYSQL优化和LINUX定时脚本补

## 一、PHP批量入库优化

### 批量插入优化

==准备一张测试的空表==:

```
测试环境：phpstudy 
PHP 7.2.10 + mysql+apache 2.4
我的表结构如下：
create database insert_test;
use insert_test;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `userindex` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```
<!-- more -->
#### 1、==传统for循环插入==(需要设置对应php版本的最大可执行时间：max_execution_time 默认为30，可根据实际项目测试改参数值，我们下面的案例设置：max_execution_time = 60)，重启服务器 

```
$servername = "localhost";
$username = "root";
$password = "root";
$dbname  = 'insert_test';
// 创建连接

$conn = new mysqli($servername,$username,$password,$dbname);
$start = time();
// 传统插入
for ($x=1; $x<=100000; $x++) {
        $sql = 'insert into user (userindex) values ('.$x.')';
        $conn->query($sql);
}
$end = time();
echo '传统插入耗时：'.($end-$start);
//关闭连接
$conn->close();
```

#### 2、使用==分段事务提交==

批量插入数据库(每隔2W条提交下) 

```
$servername = "localhost";
$username = "root";
$password = "root";
$dbname  = 'insert_test';
// 创建连接
$conn = new mysqli($servername,$username,$password,$dbname);
$start = time();

$conn->query('BEGIN');
for($i=1;$i<100000;$i++){ 
        $sql = 'insert into user (userindex) values ('.$i.')';
        $conn->query($sql);
        if($i%20000===0){
                $conn->query('COMMIT');
                $conn->query('BEGIN');
        }
}

$end = time();
echo '分段事务插入耗时：'.($end-$start);
//关闭连接
$conn->close();
```

#### 3、==拼接sql语句==

(注意：可执行的sql语句的长度是有限制的,可自行修改mysql配置文件my.cnf/my.ini的参数max_allowed_packet  = x的值修改限制,查看默认设置的sql长度限制命令: ==show VARIABLES WHERE Variable_name LIKE 'max_allowed_packet';== )[查看，修改设置，查看]

```
$servername = "localhost";
$username = "root";
$password = "root";
$dbname  = 'insert_test';
// 创建连接
$conn = new mysqli($servername,$username,$password,$dbname);
$start = time();

$sql= "insert into user (userindex) values";
for($i=0;$i<100000;$i++){ 
        $sql.="('$i'),";
}
// 去除最后一个逗号
$sql = substr($sql,0,strlen($sql)-1);

$conn->query($sql);

$end = time();
echo 'sql语句拼接插入耗时：'.($end-$start);
//关闭连接
$conn->close();


```

==因为数据库插入时。须要维护索引数据，无序的记录会增大维护索引的成本。所以我们的表是有主键id的== 

==执行效率高的主要原因是合并后日志量(MySQL的binlog和innodb的事务让日志)减少了，降低日志刷盘的数据量和频率，从而提高效率。通过合并SQL语句，同时也能减少SQL语句解析的次数，减少网络传输的IO== 

## 二、查询优化

### 1、**SELECT语句务必指明字段名称**

SELECT*增加很多不必要的消耗（CPU、IO、内存、网络带宽）；增加了使用覆盖索引的可能性；当表结构发生改变时，前断也需要更新。所以要求直接在select后面接上字段名。

### 2、**当只需要一条数据的时候，使用limit 1**

这是为了使EXPLAIN中type列达到const类型

### 3、**如果排序字段没有用到索引，就尽量少排序** 

### 4、**如果限制条件中其他字段没有索引，尽量少用or**

or两边的字段中，如果有一个不是索引字段，而其他条件也不是索引字段，会造成该查询不走索引的情况。很多时候使用union all或者是union（必要的时候）的方式来代替“or”会得到更好的效果

### 5、**尽量用union all代替union**

union和union all的差异主要是前者需要将结果集合并后再进行唯一性过滤操作，这就会涉及到排序，增加大量的CPU运算，加大资源消耗及延迟。当然，union all的前提条件是两个结果集没有重复数据

### 6、**不使用ORDER BY RAND()**

select id from `dynamic` order by rand() limit 1000;

上面的SQL语句，可优化为：

select id from `dynamic` t1 join (select rand() * (select max(id) from `dynamic`) as nid) t2 on t1.id > t2.nidlimit 1000;

### 7、**区分in和exists、not in和not exists**

select * from 表A where id in (select id from 表B)

上面SQL语句相当于

select * from 表A where exists(select * from 表B where 表B.id=表A.id)

区分in和exists主要是造成了驱动顺序的改变（这是性能变化的关键），如果是exists，那么以外层表为驱动表，先被访问，如果是IN，那么先执行子查询。所以IN适合于外表大而内表小的情况；EXISTS适合于外表小而内表大的情况。

关于not in和not exists，推荐使用not exists，不仅仅是效率问题，not in可能存在逻辑问题。如何高效的写出一个替代not exists的SQL语句？

原SQL语句：

select colname … from A表 where a.id not in (select b.id from B表)

高效的SQL语句：

select colname … from A表 Left join B表 on where a.id = b.id where b.id is null

### 8、**使用合理的分页方式以提高分页的效率**

select id,name from product limit 866613, 20

使用上述SQL语句做分页的时候，可能有人会发现，随着表数据量的增加，直接使用limit分页查询会越来越慢。

优化的方法如下：可以取前一页的最大行数的id，然后根据这个最大的id来限制下一页的起点。比如此列中，上一页最大的id是866612。SQL可以采用如下的写法：

select id,name from product where id> 866612 limit 20

### 9、**避免在where子句中对字段进行null值判断**

对于null的判断会导致引擎放弃使用索引而进行全表扫描。

### 10、**不建议使用%前缀模糊查询**

例如LIKE“%name”或者LIKE“%name%”，这种查询会导致索引失效而进行全表扫描。但是可以使用LIKE “name%”。

那如何查询%name%？

**如何解决这个问题呢，答案：使用全文索引。** 

### 11、**避免在where子句中对字段进行表达式操作**

比如：

select user_id,user_project from user_base where age*2=36;

中对字段就行了算术运算，这会造成引擎放弃使用索引，建议改成：

select user_id,user_project from user_base where age=36/2;

### 12、**避免隐式类型转换**

where子句中出现column字段的类型和传入的参数类型不一致的时候发生的类型转换，建议先确定where中的参数类型

### 13、**对于联合索引来说，要遵守最左前缀法则**

举列来说索引含有字段id、name、school，可以直接用id字段，也可以id、name这样的顺序，但是name;school都无法使用这个索引。所以在创建联合索引的时候一定要注意索引字段顺序，常用的查询字段放在最前面。

### 14、**注意范围查询语句**

对于联合索引来说，如果存在范围查询，比如between、>、<等条件时，会造成后面的索引字段失效。

### 15、**关于JOIN优化** 

**1）MySQL中没有full join，可以用以下方式来解决：**

select * from A left join B on B.name = A.name where B.name is null union all select * from B;

**2）尽量使用inner join，避免left join：**

参与联合查询的表至少为2张表，一般都存在大小之分。如果连接方式是inner join，在没有其他过滤条件的情况下MySQL会自动选择小表作为驱动表，但是left join在驱动表的选择上遵循的是左边驱动右边的原则，即left join左边的表名为驱动表。

**3）合理利用索引：**

被驱动表的索引字段作为on的限制字段。

**4）利用小表去驱动大表：**

**![img](https://mmbiz.qpic.cn/mmbiz_png/OvnShhIvBxS8uz9hCiaMnFEAicKajeE5nQrPEIIsa6V8fK9QRDYkQw1UoSzMMllLcaEjkF5bOxIObIocOXFqyrFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**

从原理图能够直观的看出如果能够减少驱动表的话，减少嵌套循环中的循环次数，以减少 IO总量及CPU运算的次数

5

## 三、linux定时任务

#### 1、使用场景

在linux服务器里面，一般来说我们都要定期的去做某些操作，例如数据库的备份【1. 全量备份，但是一般这个需要消耗很大的系统资源，一般周期长，一般选择在凌晨3-5点左右完成，不能在人多的时候完成,这个时候就需要使用linux的系统定时任务来进行操作。例如让定时任务定期执行某个PHP脚本，来做一些操作，例如每周五下午五点统计该网站注册的的用户总人数发送给相关人员。

#### 2、使用格式

在LINUX中你应该先输入crontab -e，然后就会有个vi编辑界面，再输入0 3 * * 1 /clearigame2内容到里面 :wq 保存退出。 

在LINUX中，周期执行的任务一般由cron这个守护进程来处理[ps -ef|grep cron]。cron读取一个或多个配置文件，这些配置文件中包含了命令行及其调用时间。

cron的配置文件称为“crontab”，是“cron table”的简写。

如何编写定时任务呢

第一步： crontab -e 进入vi编辑定时器脚本

\* * * * * 执行程序，一般来说是一个shell脚本【shell编程】

格式例如：* * * * * sleep 1; /bin/date >>/tmp/date.txt

其中这个五个星分别代表：

| 项目    | 含义                 | 范围                    |
| ------- | -------------------- | ----------------------- |
| 第一个* | 一小时当中的第几分钟 | 1-59                    |
| 第二个* | 一天中的第几个小时   | 0-23                    |
| 第三个* | 一个月中的第几个天   | 1-31                    |
| 第四个* | 一年中的第几个月     | 1-12                    |
| 第五个* | 一个星期中的星期几   | 0-7（0和7都代表星期天） |

| 项目 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| *    | 代表任何时间，比如第一个*，代表一个小时里面的每分钟都执行一次命令 |
| ,    | 代表不连续的时间，比如  0     9,12              * * * ，代表每天的9点0分，12点0分执行一次命令 |
| -    | 代表连续的范围，比如  0   10   * *     1-6 ，代表周一到周六的每天上午10点执行一次命令 |
| */n  | 代表每隔多久执行一次，比如 */10 * * * *，代表每隔10分钟执行一次命令 |

#### 3、服务安装与案例

如果没有crontab指令的同学需要安装一下，一般的linux系统都会有的

##### **安装crontab:**

yum -y install crontabs

说明：
/sbin/service crond start //启动服务
/sbin/service crond stop //关闭服务
/sbin/service crond restart //重启服务
/sbin/service crond reload //重新载入配置

查看crontab服务状态：service crond status

手动启动crontab服务：service crond start

查看crontab服务是否已设置为开机启动，执行命令：ntsysv

加入开机自动启动:
chkconfig –level 35 crond on

注意：crontab -e 本质其实就是向 /etc/crontab 文件里面写入上面的定时任务信息。

```
#编辑
# vim /etc/crontab
查看
# crontab -l
# 清除
# crontab -r
```

在linux下建立文件夹/var/test专门用来学习crontab测试用

```
1: cd /var

2: mkdir test
```

##### 1) 案例1：调用系统date指令，每一分钟在 /var/date.log文件里面记录下当前时间

```
# crontab -e
* * * * * date >> /var/test/date.log
```

赋予date.log文件可执行权限

chmod -R 777 /var/test/date.log

重新启动 crond

```
service crond restart
```

##### 2) 案例二：通过定时任务执行我们的自定义shell脚本

增加一个普通的定时器脚本`dateTime.sh`，放于`/`根目录 

1:  新增自定义shell脚本： vim /dateTime.sh ，输入一下内容，保存  

```
 #!/bin/bash
    date
```

2: 增加可执行权限 

```
chmod 777 /dateTime.sh
```

3: 编写定时任务 

```
crontab -e
```

4: 编写脚本内容

```
*/1 * * * * /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 5 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 10 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 15 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 20 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 25 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 30 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 35 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 40 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 45 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 50 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 55 && /dateTime.sh >> /var/test/dateTime.log
*/1 * * * * sleep 60 && /dateTime.sh >> /var/test/dateTime.log
```

重新启动 crond

```
service crond restart
```

2分钟后查看效果

==// linux定时任务执行某个php脚本,可以通过指定目录下的php文件或则支持curl调用的http完整路径==

##### 3) 案例3：定时任务执行php文件

1、在网站根目录(我是yum安装方式，每个人的服务器网站根目录不一样，根据自己的实际情况新建文件)新建一个test.txt文件，赋予可以操作权限chmod 777 test.txt

```
vim /mydata/wwwroot/php33/www/test.txt
```

```
chmod 777 /mydata/wwwroot/php33/www/test.txt
```

2、网站根目录下新建cron.php文件

```
<?php
$filename="./test.txt";
$handle=fopen($filename,"a+");
$str=fwrite($handle,'当前时间是:'.date('Y-m-d H:i:s')."\n");
fclose($handle);
```

3、编写定时任务 (运行httpd：/usr/local/httpd/bin/apachectl start　)

`crontab -e`

下面的例子是使用CURL访问URL来每1分执行PHP脚本。Curl默认在标准输出显示输出。使用”curl -o”选项，你也可以把脚本的输出转储到临时文件。 

```
*/1 * * * * /usr/bin/curl -o /mydata/wwwroot/php33/www/test.txt 192.168.82.45/cron.php
```

4、重新启动crond服务

```
service crond restart
```

5、查看效果(打开对应目录下的txt文件内容)

```
vim  /mydata/wwwroot/php33/www/test.txt
```



 





