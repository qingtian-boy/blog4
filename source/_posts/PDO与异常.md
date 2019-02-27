---
title: PDO与异常
date: 2019-02-20 17:01:03
tags: PHP
---
# 一、昨日回顾

## 1. 知识回顾

1. 能够使用serialize()和unserialize()对对象进行序列化和反序列化

   1)对象的序列化：直接使用serialize函数将某个对象转换成具有固定格式的字符串；

   2)对象的反序列化：使用unserialize函数将 序列化后的字符串 进行还原成原来的对象；在反序列化的过程中，必须包含原始类的定义；
<!-- more -->
2. 能够知道\_\_sleep()和\_\_wakeup()的作用和调用时机

   1） __sleep：PHP不负责定义，只负责调用，调用的时机是当使用serialize函数去序列化对象时，将会触发PHP自动调用该魔术方法执行1次；

   2）__wakeup：PHP不负责定义，只负责调用，调用的时机是当使用unserialize函数去反序列化 一个序列话后的字符串时，将会触发PHP自动调用该魔术方法执行1次；

3. 能够理解命名空间的含义和作用

   命名空间： 

   ```php
   namespace 空间名;
   ```

   作用： 可以使得同一个程序脚本中同时存在多个同名的函数、类和（老版本）常量；

4. 能够使用use关键字引入命名空间

   空间类：use \xx\xxx\类名;

   空间函数：use function \xx\xxx\函数名;


# 二、知识路径

- PDO的基本概念

  ​	为什么使用PDO

  ​	什么是PDO

- PDO的基本操作

  ​	开启PDO扩展

  ​	使用PDO实现连库基本操作

  ​	使用PDO实现设置操作（增删改）

  ​	使用PDO实现查询操作

  PDO中的事务

  ​	回顾MYSQL客户端中的事务

  ​	使用PDO实现事务	

- PDO中的预处理技术

  ​	为什么使用预处理技术

  ​	MYSQL客户端中实现预处理技术

  ​	PDO中实现预处理技术

- PDO中的异常处理

  ​	PDO中实现异常

- 案例：封装PDO操作类

  ==目标：能够使用PDO完成对数据的增删改查操作==

  

# 三、今日课程内容：PDO与异常

## 1. PDO的基本概念	

### 为什么使用PDO

在WEB开发中，能够使用的数据库不仅仅只有MYSQL，还可能使用其他的数据库，比如：ORACLE、FIREBIRD、MSSQL等。



在PHP程序中，我们执行的是相同的操作，但是所写的操作函数却不一样，以连接数据库操作为例：

MySQLi：mysqli_connect

MSSQL：mssql_connect

ORACLE：oci_connect



所以，当我们要切换数据库时，就必须更换为相应扩展的操作函数

![1530715856586](./2)



这无疑将提高程序人员操作的门槛，如果需要在项目中更换数据库或者增加不同的数据库，那么一方面我们需要学习不同的数据库操作函数，非常麻烦；另一方面操作功能多了维护起来也会增加负担。

 

然而PDO却给我们提供了一个方便的操作方式，我们不再需要学习市面上各种各样的数据库操作，只需要学会使用PDO，那么就可以通过PDO来达到使用相同的操作方法，改变个别参数值，就能达到操作不同类型数据库的目的。

![1530716189186](./3)

### 什么是PDO

PDO：PHP Data Object  即 PHP数据对象

**概念**：PDO即PHP中专门用来操作数据库的一套扩展。



PDO在手册中的位置：

![1530716434955](./4)



## 2. PDO的基本操作

### 开启PDO扩展

第一步，打开PHP配置文件php.ini，确认extension_dir配置项，

![1535766119290](8)

第二步，继续在php.ini中配置PDO的extension配置项，

![1535766192670](9)

第三步，确认php_pdo_mysql.dll文件存在，

![1535766271022](10)

第四步，重启apache，测试PDO是否开启成功，

![1535766342103](11)



### 使用PDO实现连库基本操作

引言：我们可以通过实例化PDO类的对象，来达到实现连库基本操作的目标。



一旦实例化PDO类的对象，则PDO类中的构造方法将会被PHP自动调用执行，所以我们需要先了解一样下PDO中的构造方法：

> **__construct**(基本参数， 数据库帐号， 数据库密码)      负责初始化连库基本操作

参数详解：

> 基本参数： 包含数据库类型，数据库IP地址，端口号，字符集和默认的数据库名
>
> 基本参数的格式：'数据库类型：==host\===IP地址==；port\===端口号==；charset\===字符集==；dbname\===数据库名'



演示案例：实例化PDO类的对象实现连库基本操作

code1.php

```php
<?php

$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=db3';
$pdo = new PDO($dsn, 'root', '123abc');//操作成功则返回一个基于PDO类的对象，操作失败则直接报致命的错误

var_dump( $pdo ); 
```

访问code1.php，效果为

![1540691885358](5)



### 使用PDO实现设置操作（增删改）

涉及的方法：

> **exec**(SQL语句)      执行增删改操作SQL语句
>
> **lastInsertId**()      获得最近一次新增数据的id



**==需求==**：完成以下操作

1. 使用PDO，向test数据库中的cz_user表实现新增一条数据；
2. 使用PDO，向test数据库中的cz_user表实现修改id=8的数据name值为'小红帽'；
3. 使用PDO，删除test数据库中的cz_user表id为1的数据；

**==解答==**：构建名为code2.php的文件，代码如下：

```php
<?php

$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');//操作成功则返回一个基于PDO类的对象，操作失败则直接报致命的错误

#1. 新增操作
//构建SQL语句
$sql = 'insert into cz_user values (null, "小王罡", "002", "wg@admin.com")';
//执行SQL语句   执行失败返回false，执行成功返回一个整型值，表示多少条数据受影响
$re = $pdo->exec($sql);
var_dump( $re ); echo '<br/>';

$re = $pdo->lastInsertId();//获取最近一次新增数据的id值
var_dump( $re ); echo '<hr/>';

#2. 修改操作
$sql = 'update cz_user set name="小红帽" where id=8';
$re = $pdo->exec($sql);
var_dump( $re ); 

#3.删除操作
$sql = 'delete from cz_user where id=1';
$re = $pdo->exec($sql);
var_dump( $re ); 
```



**==小结==**：在PDO中，增删改操作的实现步骤：

1）构建增、删、改 操作的SQL语句；

2）使用PDO类的对象调用exec方法执行SQL语句即可；成功返回受影响的行数，失败返回false；



### 使用PDO实现查询操作

涉及的方法：

【PDO类中】

> **query**(SQL语句)      执行查询操作SQL语句

【PDOStatement类中】

> **fetch**(解析类型)      一次解析一行数据
>
> **fetchAll**()       一次性解析所有数据
>
> **rowCount**()        获得查询出来的数据的总行数
>
> **columnCount**()         获得查询出来的数据的总列数（字段总个数） 
>
> **fetchColumn**(列索引号)        一次解析一列数据中的一个



**==需求1==**：完成以下操作

1. 使用PDO，查询test数据库中的cz_user表id=3的数据；

**==解答1==**：构建名为code3.php的文件，代码如下：

```php
#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#查询操作
$sql = 'select * from cz_user where id=3';
$pdostatement = $pdo->query($sql);//执行成功返回一个对象结果集，执行失败返回false

//解析对象结果集
$row = $pdostatement->fetch(PDO::FETCH_ASSOC);
print_r($row); echo '<hr/>';
$row = $pdostatement->fetch(PDO::FETCH_ASSOC);
var_dump($row); echo '<hr/>';
$row = $pdostatement->fetch(PDO::FETCH_ASSOC);
var_dump($row); echo '<hr/>';

/*
fetch方法的特点：
1）一次执行解析得到一行结果；
2）当获得完所有的数据之后，再次执行，则只能获得false；
fetch方法支持的参数：
PDO::FETCH_ASSOC：获取的数据是一个关联类型的数组；（推荐使用）
PDO::FETCH_NUM：获取的数据是一个索引类型的数组；
PDO::FETCH_BOTH：获取的数据是一个即包含索引又包含关联类型的数组；
PDO::FETCH_OBJ：获取的数据是一个对象类型的数据；
*/
```

访问code3.php的效果：

![1540695375307](12)

**==小结1==**：实现查询一条数据的方式：

1. 首先构建查询数据的SQL语句查询一条数据；
2. 然后使用PDO类创建的对象调用query方法执行查询SQL语句，得到基于PDOStatement类创建的对象结果集；
3. 最后使用PDOStatement类创建的对象调用fetch方法解析得到一行数据；






**==需求2==**：完成以下操作

1. 使用PDO，查询test数据库中的cz_user表id<7的所有数据；

**==解答2==**：构建名为code4.php的文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#查询多条数据
//构建SQL语句
$sql = 'select * from cz_user where id<7';
//执行SQL语句
$pdostatement = $pdo->query($sql);
//解析对象结果集得到所有数据
$rows = $pdostatement->fetchAll();

echo '<pre>';
print_r($rows);
```



**==小结2==**：实现查询一条数据的方式：

1. 首先构建查询数据的SQL语句查询多条数据；
2. 然后使用PDO类创建的对象调用query方法执行查询SQL语句，得到基于PDOStatement类创建的对象结果集；
3. 最后使用PDOStatement类创建的对象调用fetchAll方法解析得到全部数据，数据是一个二维数组；



**==需求3==**：完成以下操作

1. 使用PDO，查询test数据库中的cz_user表id<7的数据一共有几行；

**==解答3==**：构建名为code5.php的文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#查询多条数据
//构建SQL语句
$sql = 'select * from cz_user where id<7';
//执行SQL语句
$pdostatement = $pdo->query($sql);
//解析对象结果集得到数据的总行数
$rowNum = $pdostatement->rowCount();

var_dump( $rowNum ); 
```

访问code5.php的效果：

![1540697023371](13)

**==小结3==**：rowCount可以获取查询结果中的总行数。





**==需求4==**：完成以下操作

1. 使用PDO，查询test数据库中的cz_user表id<7的数据一共有几列；

**==解答4==**：构建名为code6.php的文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#查询多条数据
//构建SQL语句
$sql = 'select * from cz_user where id<7';
//执行SQL语句
$pdostatement = $pdo->query($sql);
//解析对象结果集得到数据的总列数
$colNum = $pdostatement->columnCount();

var_dump( $colNum ); 
```

访问code6.php的效果：

![1540697141266](14)

**==小结4==**：columnCount能够获得查询结果中总共的列数。





**==需求5==**：完成以下操作

1. 使用PDO中的fetchColumn方法，查询test数据库中的cz_user表id<4的所有数据，要求从查询得到的数据中将name列的每一个数据取得并打印，查看打印效果；

**==解答5==**：构建名为code7.php的文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#查询多条数据
//构建SQL语句
$sql = 'select * from cz_user where id<4';
//执行SQL语句
$pdostatement = $pdo->query($sql);

//逐个解析指定某一列的每个值
$colVal = $pdostatement->fetchColumn(1);
var_dump( $colVal ); echo '<hr/>';
$colVal = $pdostatement->fetchColumn(1);
var_dump( $colVal ); echo '<hr/>';
$colVal = $pdostatement->fetchColumn(1);
var_dump( $colVal ); echo '<hr/>';
```

访问code7.php的效果：

![1540697595030](15)

**==小结5==**：fetchColumn方法可以获取指定某一列的每个值，当获得完所有值之后再次执行，则将只会获得false。





## 3. PDO中的事务

### 回顾MYSQL客户端中的事务

我们以zhangsan向lisi借钱为例子，如果要想在数据库实现zhangsan向lisi借100元钱，则过程应该是：

首先lisi的money需要减少100，必须要执行成功如下的SQL语句：

update user set money=money-100 where name='lisi';

 

其次,zhangsan的money需要增加100，必须要执行成功如下SQL语句：

update user set money=money+100 where name='zhangsan';

 

这个过程，必须是两条SQL语句同时都执行成功，才算是借钱成功，只要中途有任何一个执行不成功，都不能算借钱成功。



**提前准备**：

```mysql
#user表
create table user(
id int unsigned primary key auto_increment,
name varchar(30) not null default '',
money decimal(10, 2) not null default 0
)engine=Innodb charset=utf8;

#user表的两条数据
insert into user values (null, 'zhangsan', 1000);
insert into user values (null, 'lisi', 1000);
```



**==需求==**：使用黑窗口实现成功借钱的例子；

**==步骤==**：

1. 开启事务，开启事务的语句：

   ```mysql
   start transaction;
   ```

   ![1540698464590](16)

2. 然后执行第一个子过程，让lisi的账户减少100块，

   ![1540698585942](17)

3. 然后再执行第二个子过程，让zhangsan的账户增加100块，

   ![1540698658340](18)

4. 提交事务，提交事务的语句：

   ```mysql
   commit
   ```

   ![1540698733405](19)



**==需求==**：使用黑窗口实现失败借钱的例子；

**==步骤==**：

1. 开启事务，开启事务的语句：

   ![1540698820734](20)

2. 执行第一个子过程，让lisi的账户减少100块，

   ![1540698866962](21)

   这个时候lisi的媳妇杀出来了，阻止lisi借钱，所以我们需要让整个事务失效，

3. 回滚事务，回滚事务的语法：

   ```mysql
   rollback;
   ```

   ![1540699062710](22)

   



==**小结**==：要想在MYSQL中实现事务，则：

1. 必须先开启事务： start transaction;
2. 执行子过程中的各种SQL语句；
3. 如果所有的子过程都没有问题，则提交事务：commit;
4. 如果子过程中有任何一个出现问题，则回滚事务：rollback;



### 使用PDO实现事务	

**==需求==**：使用PDO实现借钱的例子；

**==步骤==**：构建code8.php程序文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#开启事务
$pdo->beginTransaction();

#执行事务当中的子过程
$flag = true;

//让lisi的存款减少100
$sql = 'update user set money=money-100 where name="lisi"';

$re = $pdo->exec($sql);
if( !$re ){//执行失败
    $flag = false;
}

//让zhangsan的存款增加100
$sql = 'update user set money=money+100 where name="zhangsan"';

$re = $pdo->exec($sql);
if( !$re ){//执行失败
    $flag = false;
}

#根据执行的$flag表示判断是否提交还是回滚
if( $flag ){//如果子过程全部都成功
    $pdo->commit();
    echo '执行事务成功的情况!'; 
}else{//如果有任何一个子过程执行失败
    $pdo->rollBack();
    echo '执行事务失败的情况!'; 
}
```



==**小结**==：在程序中，最终执行提交还是回滚取决于$flag表示最终是true还是false;

==**注意**==：如果要使用事务，则数据表必须Innodb的引擎。



## 4. PDO中的预处理技术

### 为什么使用预处理技术

在MYSQL中，其标准的执行SQL语句的流程是：

1. 构建SQL语句；
2. 通过黑窗口客户端发送SQL语句到MYSQL服务器；
3. MYSQL服务器接收SQL语句；
4. 解析SQL语句；
5. 执行SQL语句；
6. 将执行的结果返回；



在通常的情况下，无论我们在MYSQL中执行的是不是相同的操作，MYSQL都会每次去重新解析完整的SQL语句，那么，如果当我们的操作，每次执行的都是相同的，只不过数据不同，如下面新增数据的SQL语句：

```mysql
insert into cz_user values (null, 'zhangsan', 'aa',"hh@admin.com");
insert into cz_user values (null, 'lisi', 'bb',"hh@admin.com");
insert into cz_user values (null, 'wangwu', 'cc',"hh@admin.com");
insert into cz_user values (null, 'zhaoliu', 'dd',"hh@admin.com");
```

都是向cz_user表执行新增数据的动作，只不过每次新增的数据不同而已。但是MYSQL依然会每次都完整的去解析这些SQL语句。

 

这样就造成每次都重复的解析了相同的操作，浪费了一定的时间。所以，在MYSQL中，提供了一种名为预处理的技术，这个技术就是专门用来优化上述问题的技术。



### MYSQL客户端中实现预处理技术

#### 不带参数的预处理

涉及的语句语法：

> 构建预处理语句语法：==**prepare**  预处理语句名  **from**  “SQL语句”;== 
>
> 执行预处理语句语法：==**execute**  预处理语句名;==
>
> 删除预处理语句语法：==**drop  prepare**  预处理语句名;== 



**==需求==**：使用预处理技术执行"select * from cz_user"SQL语句；

**==步骤==**：

1. 构建预处理语句

   ![1540711990995](23)

2. 执行预处理语句

   ![1540712005657](24)

3. 删除预处理语句

   ![1540712017654](25)



#### 携带参数的预处理

涉及的语句语法：

> 构建预处理语句语法：==**prepare**  预处理语句名  **from**  “SQL语句”;== 
>
> 新增参数数据语句语法：==**set  @**变量名=变量值;==
>
> 执行预处理语句语法：==**execute**  预处理语句名  **using**  @变量1, @变量2，….,@变量n;==
>
> 删除预处理语句语法：==**drop  prepare**  预处理语句名;== 



**==需求==**：使用预处理技术实现往cz_user表中添加数据；

**==步骤==**：

1. 构建预处理SQL语句

   ![1540713080777](26)

2. 准备数据

   ![1540713223679](27)

3. 携带参数执行预处理语句

   使用变量填充数据的位置取决于其自然排列的顺序，比如第一个变量是@name，则第一个？占位符将会使用这个变量的值来填充，后面的依次类推。

   ![1540713371503](28)

4. 删除预处理语句

   ![1540713504910](29)



### PDO中实现预处理技术

涉及的方法：

【PDO类中】

> **prepare**(SQL语句)      生成预处理语句

【PDOStatement类中】

> **bindParam**(参数序号， ==&==参数值)      绑定参数
>
> **execute**([参数集合])       执行预处理语句



**==需求==**：使用PDO实现用预处理技术往cz_user表中添加数据，要求

1. 使用序号绑定参数的方式实现一次；
2. 使用"：字段名"绑定参数的方式实现一次；
3. 使用==数组绑定参数的方式==实现一次；

**==解答1==**：构建code9.php程序文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#准备预处理语句
$sql = 'insert into cz_user values (null, ?, ?, ?)';
$pdostatement = $pdo->prepare($sql);

#准备参数数据并且绑定参数
$name = '大罡罡';
$pwd = '0438';
$email = 'dgg@admin.com';

$pdostatement->bindParam(1, $name);
$pdostatement->bindParam(2, $pwd);
$pdostatement->bindParam(3, $email);

#执行预处理语句
$re = $pdostatement->execute();
var_dump( $re ); 
```



==**解答2**==：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#准备预处理语句   使用":字段名"的方式绑定参数
$sql = 'insert into cz_user values (null, :name, :pwd, :email)';
$pdostatement = $pdo->prepare($sql);

#准备参数数据并且绑定参数
$name = '大娃';
$pwd = '9432';
$email = 'dw@admin.com';

$pdostatement->bindParam(':name', $name);
$pdostatement->bindParam(':pwd', $pwd);
$pdostatement->bindParam(':email', $email);

#执行预处理语句
$re = $pdostatement->execute();
var_dump( $re ); 
```



==**解答3**==：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#准备预处理语句   使用":字段名"的方式绑定参数
$sql = 'insert into cz_user values (null, :name, :pwd, :email)';
$pdostatement = $pdo->prepare($sql);

#准备参数数据并且绑定参数
$data = [':name'=>'二娃', ':pwd'=>'9527', ':email'=>'ew@admin.com'];

#执行预处理语句
$re = $pdostatement->execute($data);
var_dump( $re ); 
```



==**小结**==：推荐使用第三种数组的方式来实现预处理技术。



## 5. PDO中的异常处理

"异常"两个字的意思，我们可以理解为错误，它可以是执行时出现的错误，也可以是逻辑错误。



### PDO中实现异常

在PDO中，也封装了一个类用于实现对PDO中的错误进行异常处理方式的处理，这个类名字PDOException，它的本质就是继承了PHP的系统类Exception，所以PHP系统类Exception具有的操作，也都被PDOException所拥有。

![1530745661759](6)



涉及的方法：

【PDO类】

> **setAttribute**(属性名， 属性值)      设置PDO的模式属性

【PDOException类】

> **getMessage**()     获取错误信息



**==需求==**：使用PDO异常处理方式实现执行SQL语句出错的案例。

**==解答==**：构建code14.php程序文件，代码如下：

```php
<?php

#连库基本操作
$dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
$pdo = new PDO($dsn, 'root', '123abc');

#将PDO的错误处理模式调整为  异常模式
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

$sql = 'insert into cz_userddddddd values (......';

try{//监听可能出错的语句
    $pdostatement = $pdo->exec($sql);//执行查询SQL语句，因为执行语句的过程中可能存在执行错误的情况，所以把它监听起来
    // $obj = new PDOException;
    // $err = $obj;
}catch(PDOException $err){//捕获出现的异常（错误）
    echo $err->getMessage() . '<br/>';//输出错误的信息
    echo $err->getCode() . '<br/>'; 
    echo $err->getLine() . '<br/>'; 
    echo $err->getFile() . '<br/>'; 
}

echo '程序执行完毕！';
```



==**小结**==：

1. 必须将PDO的错误处理模式设置为异常模式；
2. 使用try结构监听执行SQL语句的代码；
3. 使用catch结构捕获异常，获取错误的错误信息；



## 6. 案例：封装PDO操作类

### 功能分析

1. 连库基本操作；
2. 增删改操作；
3. 查一条数据；
4. 查多条数据；
5. 使用PDO中的异常处理模式获取执行的错误信息；



### 代码实现

构建名为PDOTool.class.php的程序，代码如下，

```php
<?php

class PDOTool{

    protected $pdo;

    //构造方法
    public function __construct(){ 
        
        //连库基本操作
        $dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
        $this->pdo = new PDO($dsn, 'root', '123abc');

        //开启PDO异常错误处理模式
        $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    }

    //设置操作（增删改）
    public function setData($sql){ 
        
        try{
            $this->pdo->exec($sql);//执行SQL语句
        }catch(PDOException $err){
            $this->record($err);
            return false;
        }
        return true;
    }

    //查询单条数据的操作
    public function getRow($sql){ 
        
        try{//监听执行SQL语句的代码
            $pdostatement = $this->pdo->query($sql);//执行查询SQL语句
        }catch(PDOException $err){//捕获出现的错误（异常）
            $this->record($err);//记录出错的信息到日志文件中
            return false;//返回false值
        }

        return $pdostatement->fetch(PDO::FETCH_ASSOC);//解析得到一条数据，并将其返回
    }

    //查询多条数据的操作
    public function getRows($sql){ 
        
        try{
            $pdostatement = $this->pdo->query($sql);//执行查询SQL语句
        }catch(PDOException $err){
            $this->record($err);
            return false;
        }

        return $pdostatement->fetchAll(PDO::FETCH_ASSOC);//解析得到所有数据，并将其返回
    }

    //记录错误信息的方法
    protected function record($err){ 
        $str = '';
            $str .= "begin==========".date('Y-m-d H:i:s')."==========\r\n";
            $str .= "错误的编码是：" . $err->getCode() . "\r\n";
            $str .= "出错的行号：" . $err->getLine() . "行\r\n";
            $str .= "出错的文件路径：" . $err->getFile() ."\r\n";
            $str .= "错误的信息是：" . $err->getMessage() . "\r\n";
            $str .= "end===================================\r\n";
            file_put_contents('./err.log', $str, FILE_APPEND);
    }
}

$obj = new PDOTool;

//$sql = 'insert into cz_user (name) values ("葫芦小金刚")';
//$sql = 'update cz_user set pwd="123123" where id=17';
//$sql = 'delete from cz_user where id=13';
//$re = $obj->setData($sql);
//var_dump( $re ); 

//$sql = 'select * from cz_useeeeeer where id=8';
$sql = 'select * from cz_user where id<8';
$rows = $obj->getRows($sql);
echo '<pre>';
print_r( $rows ); 
```



## 7. 全天总结

1. 理解PDO的原理和使用PDO的好处

2. 能够正确创建PDO对象

   ```php
   $dsn = 'mysql:host=localhost;port=3306;charset=utf8;dbname=test';
   $pdo = new PDO($dsn, 'root', '123abc');
   ```

   

3. 能够使用query方法和exec方法

   exec方法：执行增、删、改SQL语句的方法；

   query方法：执行查询语句的方法；

4. 能够使用lastInsertId方法获取最新id值

   这个方法需要在刚新增完数据的情况下，然后调用，才能获得最近一次新增数据的id值。

5. 能够理解PDO预处理的原理和好处

   优化MYSQL的执行效率，提升MYSQL执行速度。场景是固定：执行的操作相同，操作的目标也是一样，只不过数据不断在变化。

6. 能够使用fetch()/fetchAll()/fetchColumn()取得数据

   fetch方法：解析一行数据

   fetchAll方法：一次性解析得到所有数据

   fetchColumn方法：一次执行得到某一列中的一个数据

7. 能够使用rowCount()获取上一条SQL的受影响行数

   rowCount方法：获取查询得到的数据中一共有多少行

8. 能够使用异常方式处理PDO的错误

   1）首先必须先开启异常错误处理模式；

   2）然后使用try结构监听执行SQL语句的代码；

   3）最后使用catch结构捕获出现的异常错误信息；
















