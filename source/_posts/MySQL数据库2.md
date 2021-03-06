---
title: MySQL数据库2
date: 2019-02-20 17:00:10
tags: MySQL
---
# 一、昨日回顾

## 1. 知识回顾

1. 数据库的操作

   显示所有的数据库：show databases;

   创建数据库操作：create database 库名;

   删除数据库操作：drop database 库名;

   选择数据库：use 库名;

2. 数据表的操作

   显示所有数据表：

   1） show tables;         显示当前所在的数据库中的所有表

   2） show tables in 库名;        显示指定的数据库中的所有表
<!-- more -->
   创建数据表：

   ```mysql
   create table 表名(
   字段1 字段1数据类型,
   字段2 字段2数据类型,
   字段n 字段n数据类型
   );
   ```

   删除数据表：drop table 表名;

   显示表结构：desc 表名;

   显示建表语句：show create table 表名;

   设置表属性：

   ```mysql
   create table 表名(
   字段1 字段1数据类型,
   字段2 字段2数据类型,
   字段n 字段n数据类型
   )engine=表引擎 charset=字符集;
   ```

   修改表属性：alter table 表名 属性名=属性新值;

   查看表属性：show table status like '表名'\G

   修改表名：

   1） rename table 旧表名 to 新表名;          修改当前所在的数据库中某个表的表名

   2）rename table 库名.旧表名 to 库名.新表名;           将某个表转移到新的数据库中，并且可以重命名

3. 数据的操作

   增：

   1)  insert into 表名 (字段1, 字段2, ....字段n)  values (字段1值, 字段2值, ... 字段n值);

   2)  insert into 表名  values (字段1值, 字段2值, ... 字段n值);      这种方式需要为表中的所有字段一一的去指定对应的值

   删：delete from 表名 where 条件语句;

   改：update 表名 set 字段1名=字段1新值, 字段2名=字段2新值, .... 字段n名=字段n新值 where 条件语句;

   查：select 字段列表 from 表名 [where 条件语句];


# 二、知识路径

- MYSQL基础设置

  ​	字符集设置

  ​	列（字段）类型设置

  ​	列属性设置

  ​	修改表结构

  ==目标：能够设置字符集操作，能够掌握MYSQL中列类型设置和列属性设置，能够修改表结构==





# 三、今日课程内容：MYSQL数据库

## 1. MYSQL基础设置

### 字符集设置

#### 查看MYSQL中的字符集

查看MYSQL支持的所有字符集：==show charset;== 

**==操作演示==**：

![1534210112053](46)



#### 字符集的概念

TIPS：字符集顾名思义就是==字符的集合==。 

字符集包括：1）字符的集合 2）字符的编码



> 世界上有非常多种类的文字，而不同的文字又有不同的字符所组成，比如英文有26个英文字母（字符），英文单词就是由这26个英文字母（字符）所组成。人们为了能够在电脑中展示出英文文字，就固定用一些值来表示这些英文字母（字符），打个比方，就好比我用0000 0001表示a，0000 0002表示b，将这些英文字母编排成一个字符的集合，取名为ASSIC码字符集。
>
>  
>
> 但是英文字母（字符）又分大小写，并且英文中除了单词还有一些符号，比如英文的逗号，冒号，分号等，于是又将这些字符全部编排进ASSIC码字符集，我们现在所使用的ASSIC码其实就是这些字符的集合。
>
>  
>
> 而最终，英文单词就是由这些字符组合而成！



#### 常用的字符集介绍

ASCII码  ==》 ASCII码扩展表  ==》 latin1 (包括ASCII码原表和ASCII码扩展表)

GB2312（不到8000）占2个字节  ==》gbk  （2个字节） ==》GB18030

unicode编码（占4个字节）==》 UTF-32 (占4个字节) ==》UTF-16(占2个字节) ==》UTF-8 (变长字节)



#### MYSQL中的字符集设置

在MYSQL中，我们重点需要掌握三个部分的字符集设置（这三个字符集属性可以单独进行设置）

查看语句：==show variables like 'character_set_%';==

**==目标展示==**：

![1534214044022](47)



**==操作案例1==**：character_set_client测试

![1539053334735](2)

为什么上面的操作包含中文就报错了呢?

我们先查看一下character_set_client的值，

![1539053370093](3)

我们发现character_set_client值是utf8，表示告诉mysql服务器，客户端在以utf8编码发送数据，我们再查看一下客户端实际采用的编码，

![1539053432728](4)

我们发现，客户端实际在以gbk编码发送数据，说明发送时所使用的编码和接收时所使用的编码不一致，最终mysql服务器会以utf8编码去解析gbk编码发送的数据，所以肯定不能正常解析成功，故报错。



解决办法：1）要么改变客户端使用的编码为utf8；2）要么改变character_set_client的值为gbk；（通常采用这种）

设置的语句语法：==**set character_set_client=gbk;**==

![1539053603441](5)

再次执行新增包含中文数据的操作：

![1539053658224](6)



**==操作案例2==**：character_set_results测试

![1539053781879](7)

为什么查询中的数据会乱码呢？

我们先查看character_set_results的值，

![1539053951343](8)

发现值为utf8，再次查看黑窗口客户端采用的编码，

![1539054008483](9)

是GBK编码，所以mysql客户端会以gbk编码解析utf8编码发送过来数据，所以不能解析成功，故导致了显示中文乱码的现象。



解决方法：1）要么改变客户端使用的编码为utf8；2）要么改变character_set_results的值为gbk；（通常采用这种）

设置方法：**==set character_set_results=gbk;==**

![1539054096918](10)

![1539054114985](11)

改变之后，再次查看包含中文的数据，

![1539054137990](12)

效果正常了。



**==小结1==**：

1）如果操作的数据包含中文，则必须使得客户端和MYSQL服务器端的配置项的编码值一致，否则将会操作失败；

2）英文永远不会出现乱码；





一次性设置三个字符集：==set names 目标字符集;==

**==操作演示==**：

![1539055173199](13)



**==小结2==**：set names操作指定什么样的编码，完全取决于客户端使用的是什么编码；



### 列（字段）类型设置

在MYSQL中也存在数据类型，分别为：1）整型类型；2）小数类型；3）时间日期型；4）字符串类型； 



#### 整型类型

| 类型               | 大小    | 有符号（最小值/最大值）                   | 无符号（最小值/最大值） |
| ------------------ | ------- | ----------------------------------------- | ----------------------- |
| ==**tinyint(m)**== | 1个字节 | -128/127                                  | 0/255                   |
| smallint(m)        | 2个字节 | -32768/32767                              | 0/65535                 |
| mediumint(m)       | 3个字节 | -8388608/8388607                          | 0/16777215              |
| **==int(m)==**     | 4个字节 | -2147483648/2147483647                    | 0/4294967295            |
| bigint(m)          | 8个字节 | -9223372036854775808/ 9223372036854775807 | 0/18446744073709551615  |



**==演示案例==**：

创建一个测试表，其结构如下：

```
字段：a、b、c、d、e
类型：tinyint、smallint、mediumint、int、bigint
```

==测试细节1==：字段保存的整型值不能超过整型极值边界。 

![1539055822711](48)

![1539056196556](14)



**==小结==**：整型数据在构建时唯一需要注意的就是不要越界；



==测试细节2==：通过关键字==unsigned==来设置整型字段为无符号属性。

![1539056489009](15)



==测试细节3==：整型中的m参数详解

再创建一个测试表，结构如下：

```
字段：a、b
类型：int(5)、int(7)
属性：unsigned、unsigned zerofill
```

TIPS：零填充；零填充的实现方式：增加zerofill属性来设置。 

m参数：表示该数据的==最大显示宽度==；**==并不是表示该数据所占的字节数==**。  

![1539057030569](16)



**==小结==**：显示宽度和实际字段所占的字节数没有任何的关系。



#### 小数类型

| 类型              | 名称     | 大小                       | 备注                                         |
| ----------------- | -------- | -------------------------- | -------------------------------------------- |
| float(M, D)       | 单精度数 | 4个字节                    | 默认精度位数为6到7位左右（取决于操作系统）   |
| double(M, D)      | 双精度数 | 8个字节                    | 默认精度位数为16到17位左右（取决于操作系统） |
| ==decimal(M, D)== | 定点数   | 变长，大致是每9个数4个字节 | M最大为65默认为10；D最大为30默认为0；        |

创建一个测试表，结构如下：

```
字段：a、b
类型：float、double
```

**==演示案例==**：查看测试表的a字段和b字段数据类型

![1539057244535](17)



==细节测试1==：float和double类型的有效位数问题。

![1539057501758](49)





创建一个测试表，结构如下：

```
字段：a
类型：decimal(5, 2)
```

==细节测试2==：M表示==整个数的位数==；D表示==小数允许的最大位数==。

1） 如果整数位数超出了计算后 整数允许的最大位数，则无法新增数据成功；

![1539067091636](18)

2）如果小数位数超出了限制的位数，则MYSQL会自动四舍五入进行截取。 

![1539067188265](19)



**==小结==**：

1）M表示所有数字的总位数，D表示小数的最大位数；

2）新增的数据最好整数位数和小数位数都在限定范围内；



#### 时间日期型

| 类型      | 显示格式            | 取值                                             | 大小    |
| --------- | ------------------- | ------------------------------------------------ | ------- |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00到9999-12-31 23:59:59         | 8个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:00到2038-01-19 03:14:07 （UTC） | 4个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01到9999-12-31                           | 3个字节 |
| TIME      | HH:MM:SS            | -838:59:59到838:59:59                            | 3个字节 |
| YEAR      | YYYY                | 1901到2155                                       | 1个字节 |

创建一个测试表，结构如下：

```
字段：a、b、c、d、e
类型：datetime、timestamp、date、time、year
```

**==演示案例==**：

1）查看测试表的表结构；

![1539067849523](20)

2）按照字段类型允许的格式，新增一条数据，查看新增的数据；

![1539068047504](21)



==测试细节1==：timestamp的默认值为当前时间的时间戳。 

![1539068313496](22)



==细节测试2==：timestamp类型的极值需要注意时区。

![1539068700924](23)



==细节测试3==：time类型的数据还可以表示一个时间范围。 

![1539069008440](24)



**==小结==**：在以PHP结合的项目中，我们一般保存时间数据使用int类型。



#### 字符串类型

| 类型                                                 | 最大长度                                                     | 备注                                            |
| ---------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| ==char==(M) 定长                                     | 定长字符类型：255个==字符==                                  | Char(M)，M表示字符数                            |
| ==varchar==(M) 变长                                  | 变长字符类型：65535字节，但需要1-2个保存信息，同时由于记录的限制，实际最大值为65532 | 编码不同字符数不同：   Gbk<=32767   Utf8<=21845 |
| tinyText            ==text==   mediumText   longText | (2^8)    --》256字节                                                                                     (2^16)-1  --》65535字节，约64KB                                                                          (2^24)-1 --》16777215字节，约16M                                                                     (2^32)-1字节 --》4294967295字节，约4G | 定义时无需指定长度，将会自动计算                |
| enum                                                 | 枚举：数字65535                                              | 内部存储是整型；字段只能是某一个值              |
| set                                                  | 集合：最多占8个字节，即64个状态值                            |                                                 |

**==注意==**：char表示定长，无论我实际的字符有多少个，都会以char定义时指定的数值大小来保存数据；varchar表示变长，保存数据的空间大小取决于实际数据的大小。 

**==演示案例1==**：char和varchar

![1539069885548](25)



==测试细节1==：char和varchar类型括号中的最大数值表示的是字符的个数，而不是字节数。

创建一个测试表，结构如下：

```
字段：a
类型：char(255)
```

![1539071024932](26)

我们首先假设255是字节数，那么我们当前这个表采用的是utf8编码，1个字符占3个字节，那么255个字节最多能保存85个字符；我们先就构建90个中文字符，执行新增指令来查看究竟这90中文字符能否新增成功，如果新增成功，则证明我们的假设是不成立的，说明255是字符数而不是字节数；如果新增失败，才证明我们的假设是成立的，255是字节数。
![1539071163916](27)

我们发现结果新增成功，说明==255是字符数==，而不是字节数！



==测试细节2==：如果varchar指定的字节长度数超过了255个字节，MYSQL会额外使用该指定长度中的==2个字节==保存这个字符数据的总长度。 

创建一个测试表，结构如下：

```
字段：a
类型：varchar(65535)
字符集：latin1
```

![1539071549233](28)

上面执行之所以不成功，是因为当指定的取值超过了255个字节时，MYSQL将会使用额外的2个字节保存长度数据，所以，我们的最大取值应该在65535的基础上减2.

![1539071642375](29)

执行结果还是失败，这是为什么呢？我们再继续看下面这个细节。



==测试细节3==：MYSQL在构建varchar类型时，当超过255个字节时，除了要使用额外的2个字节保存实际数据的长度以外，还需要用1个字节保存null值。 

![1539071782022](30)



**==演示案例==**：text

创建一个测试表，结构如下：

```
字段：a
类型：text
```

向测试表中新增一条数据：

![1539072030685](31)



**==小结==**：text类型一般用于保存篇幅较大的文章字符串数据。



**==演示案例==**：enum和set

TIPS：枚举其实就相当于html中的单选按钮；集合就相当于html中的多选按钮；其值都必须在指定的范围内选择；

创建一个测试表，结构如下：

```
字段：sex、hobbies
类型：enum('male', 'female', 'secret')、set('乒乓', '羽毛球', '足球')
```

向测试表中新增一条数据：

![1539072411022](32)



**==小结==**：枚举相当于当选，集合相当于多选。



### 列属性设置

#### null属性

字段NULL属性的默认情况为==可以使用NULL==值，建表时可以指定==not null==设置为不使用NULL值。



**==操作演示==**：

创建一个测试表，结构如下：

```
字段：a、b
类型：varchar(30)、varchar(30) not null
```

查看测试表表结构：

![1539073120742](34)



==细节==：如果默认值是NULL，又对该字段设置了不能使用NULL值，那么，在插入NULL数据时，则无法插入成功！ 

![1539073107007](33)



==**小结**==：加上了not null属性，则该字段将不允许使用null值。



#### 默认值

设置方法：通过==default==关键字进行指定。

**==操作演示==**：

创建一个测试表，结构如下：

```
字段：a、b
类型：varchar(30)、varchar(30) not null default 'abc'
```

![1539073480450](35)



**==小结==**：以后我们在建表的时候，最好都为每个字段设置一个默认值。



#### 主键

设置方法：指定==primary key==属性的形式让某个字段变成主键。

**==操作演示==**：

创建一个测试表，结构如下：

```
字段：id、name、age
类型：int primary key、varchar(30)、tinyint unsigned
```

查看测试表表结构：

![1539074658551](37)



==细节1==：一个表只能有唯一的一个主键。

![1539074752798](38)



==细节2==：从数据的角度来看，主键也是唯一的。

![1539074913867](39)



==细节3==：建表时设置主键的另外一种方式。

![1539075002976](40)



**==小结==**：

1) 我们可以通过primary key关键字设置主键；

2) 从表结果的角度而言，主键是唯一的；

3) 从数据的角度而言，主键依然是唯一的；



==TIPS==：从查询速度的角度来讲，将一个整型字段设置主键是常用一种方式，这样做，能够使得以后指定主键为查询条件时，让查询速度会得到提升。 



#### 自动增长

设置方法：通过**==auto_increment==**属性设置自动增长。

**==注意==**：当前如果需要指定某个字段为自动增长属性的话，则这个字段需要是==主键字段==。

**==操作演示==**：

创建一个测试表，结构如下：

```
字段：id、name、age
类型：int primary key、varchar(30)、tinyint unsigned
```

失败的情况：

![1539075284246](41)

成功的情况：

![1539075336435](42)



==细节1==：设置为自增长的字段，在插入数据时，如果不指定数据，则MYSQL会自动从上一次最大的索引id值开始+1计算。 

![1539075591684](43)



==细节2==：如果之前删除了某条记录，再新增数据，自增长的主键id值依然会从该表的最大索引值+1开始计算。

![1539075807404](44)



==细节3==：当我们必须给自增长字段指定值时，我们可以指定为null，则MYSQL依然会为这个字段从索引列表的最大值+1开始计算。

![1539076068858](45)



==细节4==：只要自增长的主键id值不重复，指定任意的数值都可以。

1） 指定一个曾经被删除过的主键值；

![1539076196143](50)



2）指定任意不重复的主键值；

![1539076319606](51)



**==小结==**：

1）在属性结构中指定auto_increment关键字即可设置自增长属性；

2）自增长属性需要跟随主键属性；



#### 唯一键

设置方法：通过==unique==属性设置唯一键。 

**==操作演示==**：

创建一个测试表，结构如下：

```
字段：id、acc
类型：int unsigned primary key auto_increment、varchar(30) unique
```

查看测试表表结构：

![1539076512688](52)



==细节==：从数据的角度来讲，唯一键的数据是不能重复的。

![1539076621184](53)



==细节==：从定义结构角度来讲，一个表可以设置多个唯一键。

![1539076729067](54)



**==小结==**：

1）唯一键需要使用unique关键字来进行设定；

2）唯一键从数据的角度而言，不能重复；

3）唯一键从结构的角度而言，可以设置多个；



#### 列描述

设置方法：通过==comment==关键字来指定列描述，列描述其实就是备注内容。

**==操作演示==**：

![1539076985349](55)



**==小结==**：

建表时养成良好的习惯，写备注信息。



### 修改表结构

#### 向已有的一个表中添加字段

语句语法：==**alter table** 表名 **add** 新字段名 字段类型 [字段属性列表];== 

**==操作演示==**：





#### 删除已有表中的某个字段

语句语法：==**alter table** 表名 **drop** 字段名;== 

**==操作演示==**：





#### 改变已有表中某个字段的名字

语句语法：==**alter table** 表名 **change** 旧字段名 新字段名 新字段类型 [字段属性];== 

**==操作演示==**：





==细节==：如果只想单纯改名，则必须在改名的同时维持原属性不变写一次。 





#### 修改表字段的类型或属性

语句语法：==**alter table** 表名 **modify** 字段名 字段新类型 [字段新属性列表];== 

**==操作演示==**：





## 4. 全天总结

1. 能够理解mysql中字符集、校对规则概念

   字符集：字符的集合，包括两个组成部分：1）字符；2）字符的编码；

2. 能够设置正确的字符集

   设置字符集：set names 字符集;

   字符集的设定取决于客户端使用了什么样的字符集。

3. 能够理解不同整数类型的区别

   整型数据类型的区别是取值范围不一样。

   重点掌握：tinyint、int

4. 能够理解不同小数类型的区别

   小数类型的区别是精度不同。

   重点掌握：decimal(M, D)     M表示总的位数，D表示小数位数

5. 能够理解不同时间类型的区别

   时间类型的区别是格式不同。

6. 能够理解不同字符串类型的区别

   字符串类型的区别是保存数据时占据的空间分配方式不同。

   重点掌握：char、varchar、text

7. 能够理解字段属性的空值，默认值含义

   空值即NULL值，如果要设置为不能使用NULL值，需要指定not null属性；默认值需要通过default属性关键字来进行设置

8. 能够理解字段属性的主键，自增长，唯一键含义

   主键：通过primary key关键字进行设置

   1)主键从结构的角度来看，是唯一的；

   2)主键从数据的角度来看也是唯一的；

   自增长：通过auto_increment关键字进行设置

   1)需要跟随主键一起使用；

   2)设置成了自增长的字段，将会在原来最大索引的基础上加1进行计算填充值；

   唯一键：通过unique关键字进行设置

   1)从结构上来看，唯一键可以设置多个；

   2)从数据的角度来看，唯一键数据不能重复；

   

