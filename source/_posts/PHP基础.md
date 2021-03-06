---
title: PHP基础1
date: 2019-02-20 16:59:45
tags: PHP
---
# 一、昨日回顾

## 1. 知识回顾

1. 能够说出静态网站和动态网站的区别

   静态网站：数据是写死的，页面是能够被浏览器解析的代码的内容；

   动态网站：数据不是写死的，需要依托于程序语言（PHP）操作数据库得到数据的代码内容；

2. 什么是服务器？什么是IP？什么是域名？什么是DNS？什么是端口？

   服务器：就是“电脑”

   IP：IP正规情况下指的是IP协议，但是我们口头表述的IP指的是IP地址，IP地址是在互联网当中能够标识计算机位置的地址编号。
<!-- more -->
   域名：IP的面具

   DNS：将域名解析成IP地址的一个过程

   端口：我们平时口头所说的端口指的是虚拟端口，虚拟端口是计算机中软件的编号

3. 什么是虚拟主机？

   虚拟主机：指的是从计算机中划分出来的一段磁盘空间。




# 二、知识路径

- PHP语法初步

  ​	PHP代码标记

  ​	PHP中的注释

  ​	PHP中的语句分隔符（结束符）

  ==目标：能够使用PHP代码标记，注释和语句分隔符==

- 变量

  ​	变量名的命名规则

  ​	变量的基本使用

  ​	预定义变量

  ​	可变变量

  ​	变量的传值

  ==目标：能够懂得如何定义变量、如何设置变量、如何查看变量的值==

- 常量

  ​	常量的概念

  ​	常量的定义语法

  ​	数组常量

  ​	常量的使用

  ​	系统常量

  ​	魔术常量

  ==目标：能够懂得定义常量和调用常量，并且能够掌握魔术常量的特点==

- 数据类型

  ​	数据类型的分类

  ​	整型（int）

  ​	浮点型（float）

  ​	布尔型（bool）

  ​	字符串型（string）

  ==目标：能够理解整型、浮点型、布尔型、字符串型数据分别是什么==



# 三、今日课程内容：PHP基础

## 1. PHP语法初步

### PHP代码标记

#### 最常用的PHP标记

开始标记语法：==<?php==

结束标记语法：==?>==



**==演示案例==**：

![1537579090512](3)



#### 短标记short_open_tag型

开始标记：==<?==

结束标记：==?>==

==使用条件==：必须在php.ini中开启短标记语法的配置项short_open_tag



**==演示案例==**：

1. 首先打开PHP的配置文件php.ini，将short_open_tag配置项修改为On值，

   ![1537579348842](4)

2. 然后再重启apache，构建测试代码进行访问，

   ![1537579383083](5)

   创建code2.php，测试代码为：

   ![1537579427017](6)

   访问code2.php，效果为：

   ![1537579440298](7)



#### PHP标记的简便使用方式

如果程序脚本中最后一行后面已经没有任何程序代码了（包括html代码），则结束标记可以==省略==不写。

**==演示案例==**：

![1537579583493](8)



### PHP中的注释

==作用==：被注释的内容将不会被解析执行，是方便程序人员在程序中记录信息说明的一种方式。

PHP中的注释主要包括==两类==：1）行注释(两种)；2）块注释；



#### 行注释

第一种行注释语法：==//==

第二种行注释语法：==#==



**==演示案例==**：

![1537579866408](9)



#### 块注释

开始块注释标记：==/*==

结束块注释标记：==*/==



**==演示案例==**：

![1537580043676](10)



### PHP中的语句分隔符（结束符）

符号： **==;==**   (英文半角分号)

意义：用来表示一句PHP语句的结束，通常没有指定将会报错。



**==演示案例==**：

![1537580268761](11)

![1537580423274](12)



**==小结==**：虽然，特殊情况下，比如上面这个例子中，最后一句没有写语句结束符，也能正常执行，但是为了严谨起见，以后无论能不能成功，最好都在每一句PHP语句结束后，加上语句结束符。



## 2. 变量

**概念**： 变量就是==临时==保存数据的载体。

变量的**特点**：在PHP程序执行结束后，程序中的所有变量将会被自动销毁。

定义语法：==$变量名=变量值;==



**==演示案例==**：

![1537580688856](13)



### 变量名的命名规则

**规则**： 只能是由字母、数字、下划线组成的字符，并且不能以数字开头。



**==演示案例==**：

![1537580849124](14)



### 变量的基本使用

#### 设置操作（增删改）

**==演示案例==**：

![1537581417158](15)



#### 查询操作

**==演示案例==**：

![1537582849576](16)



#### 赋值的概念

在PHP中，通过等号“=====”将一个值给到另外一个变量的过程，被称为“赋值”。

**==演示案例==**：

![1537583109330](17)



### PHP代码嵌入HTML中的注释问题

**==需求1==**：实现将PHP代码嵌入HTML代码中，查看效果。

**==解答1==**：

![1537583523311](18)



**==需求2==**：实现将PHP代码嵌入HTML代码中的注释中，查看效果。

**==解答2==**：

![1537583691098](19)



**==小结==**：通过以上两个例子，我们了解到：

1. PHP的代码必须被包裹在PHP的标记当中才能被正常解析执行；
2. 即使PHP的代码被包裹在HTML的注释当中，也一样会被正常解析执行，也就是说HTML的注释是无法控制PHP的代码的。

![1537584258464](20)



### 预定义变量

PHP中的预定义变量包括：1）==九大超全局预定义数组变量==；2）其他预定义变量 



#### 九大超全局预定义数组变量

包括以下九个成员：

> $_ENV              获得当前程序所在环境相关信息的变量
>
> $_SERVER        获得当前服务器相关参数信息的变量
>
> ​			以下三个将会在表单传值知识点进行详细的学习
>
> $_GET
>
> $_POST
>
> $_REQUEST
>
> ​			以下两个将会在会话技术知识点进行详细的学习
>
> $_COOKIE
>
> $_SESSION
>
> ​			下面这个将会在文件上传知识点进行学习
>
> $_FILES
>
> ​			下面这个将会在作用域知识点进行学习
>
> $GLOBALS



##### $_ENV

作用：获得程序所部署的环境相关信息。

==直接输出==该变量==将无法获取任何信息==，需要先配置php.ini中的variables_order选项才能查看。

**==演示案例==**：

1. 打开php的配置文件php.ini，在variables_order选项中加上E，开启$_ENV的值，

   ![1537585928431](21)

2. 访问输出$_ENV的程序文件，查看效果：

   ![1537586113157](22)



##### $_SERVER

作用：获得与服务器交互的相关信息。

**==演示案例==**：

![1537586267808](23)



#### 其他预定义变量

**$argv**和**$argc**变量

**==注意==**：这两个变量也是PHP事先预定义好了的，但是如果通过浏览器直接访问程序，将无法获得任何有效信息的。

![1537586475117](24)



**==演示案例==**：

![1537587085290](25)



**==小结==**：通过上面这个演示案例，我们观察到：

1. 通过浏览器访问程序的方式，将无法获得$argv和$argc的信息；
2. 只有通过命令行执行程序的方式，$argv和$argc才能够获得相关信息；$argv将会获得传递给被执行程序文件的参数信息，并且第一条数据永远都是被执行程序文件的全路径地址；$argc将会获得$argv当中总共有几条数据的记录条数；



### 可变变量

**概念**：某个==变量的变量名==被==另外一个变量的变量值==所==代替==，这个变量就叫可变变量。 



**==演示案例==**：

![1537587717574](26)



### 变量的传值

变量的传值包括==两个部分==： 1）变量的==值传递==；2）变量的==引用传递==；

 

#### 变量的值传递

**==演示案例==**：

![1537588238064](27)



==变量值传递的原理==：

![1537588501128](28)



**==小结==**：修改其中一个变量的值，不影响另外一个变量的值，这个效果就是变量的值传递；



#### 变量的引用传递

**==演示案例==**：

![1537588633265](29)



==变量引用传递的原理==：

![1537588802918](30)



**==小结==**：修改一个变量的值，也将会影响另外一个变量的值，这个效果就是变量的引用传递。



## 3. 常量

**概念**：常量也是程序中保存数据的载体，但是在**整个程序脚本执行过程中==只能定义一次==**。 



### 常量的定义语法

包括两种：1）通过==const==关键字定义；2）通过==define==函数进行定义；



**==演示案例==**：

![1537598606273](31)



**==需求1==**：在程序中同时定义两个同名的常量，查看效果。

**==解答1==**：

![1537598747112](32)



**==需求2==**：在程序中定义一个常量，然后再次通过赋值的方式为常量赋值，查看效果。

**==解答2==**：

![1537598818893](33)



**==小结==**：常量，在程序中定义一次后，不可改变。



### 数组常量

在php7之后，常量支持以数组的形式来进行定义。 



**==注意==**：该知识点有点超前，目前大家只需了解PHP7之后常量支持以数组的形式来进行定义即可。



**==演示案例==**：

![1537599149592](34)



### 常量的使用

分别完成如下需求操作：

**==需求1==**：在程序中定义一个常量，并且输出该常量的值，查看效果。

**==解答1==**：

![1537599291989](35)



**==需求2==**：在程序中定义一个包含特殊字符的常量，并且尝试输出该常量的值，查看效果。

**==解答2==**：

![1537599585025](36)



**==小结==**：

1. 我们直接可以像输出变量一样，去输出常量来使用常量；
2. 特殊的，如果常量名包含特殊字符，则还需要使用PHP的内置函数constant来进行输出；



#### 判断一个常量是否存在

操作方法：通过defined==函数==来进行判断常量是否存在。



**==演示案例==**：

![1537599901615](37)



#### 获得程序中定义出来的所有常量操作

操作方法：需要通过get_defined_constants==函数==获得当前程序脚本中所有的常量。



**==演示案例==**：

![1537600217601](38)



### 系统常量（预定义常量）

所谓的系统常量，其实就是PHP事先帮我们定义好的一些常量。

手册中的位置： 

![1533422838063](2)



**==演示案例==**：

![1537600543742](39)



**==小结==**：预定义常量无需我们手动定义，是PHP已经定义好的。



### 魔术常量

魔术常量本质上==不能算==是严格意义上的==常量==，常量在脚本代码中是不允许被修改的，也就是值是固定的，但是魔术常量会随着在脚本代码中位置的变换，其值也会发生改变。



手册中的位置： 

![1533422838063](2.png)



**==演示案例==**：演示__FILE\_\_魔术常量

![1537600941948](40)



**==小结==**：从效果上看，\_\_FILE\_\_虽然是上图所说的意思，但是，实际上，\_\_FILE\_\_表示当前这个魔术常量所  ==写在的文件==  他的绝对路径地址。



## 4. 数据类型

### 数据类型的分类

PHP中数据类型主要分为==三个大类==： 1）标量数据类型；2）复合数据类型；3）特殊数据类型；

标量数据类型：a）整型（int）； b）浮点型（float）；c）字符串型（string）；d）布尔型（bool）；

复合数据类型：a）数组（array）；b）对象（object）；

特殊数据类型：a）资源类型（resource）；b）NULL类型；



### 整型（int）

含义：指的就是数学中的整数。英文全名：intval 

**==演示案例==**：

![1537602019858](41)



我们通常习惯使用的都是十进制的数值，但是在计算机中，也经常会使用其他进制的整型数值。

#### 八进制整型数字和十六进制整型数字

定义八进制整型数字方法：以0（整数零）开头的数字。

**==演示案例==**：

![1537602267145](42)



定义十六进制整型数字方法：以0x（零+英文x）开头的数字。 

**==演示案例==**：

![1537602393131](43)



**==小结==**：在PHP程序中，八进制数值以整型零开头；十六进制数值以整型零+字母x开头；



#### 整型的最大值和最小值的问题

在PHP中，整型固定是以==4个字节==保存。

| 有符号（最大值/最小值）           | 无符号（最大值/最小值） |
| --------------------------------- | ----------------------- |
| \- 2147483648 / 2147483647        | 0 / 4294967295          |
| 计算方法：-2^(32-1)  / 2^(32-1)-1 | 计算方法：0 / 2^(32)-1  |



##### 进制转换相关函数

==bin==：binary二进制    ==dec==：decimal十进制   ==oct==：octal八进制   ==hex==：hexdecimal十六进制

> abs函数    取一个数值的绝对值
>
> bindec函数    二进制转十进制
>
> octdec函数    八进制转十进制
>
> hexdec函数    十六进制转十进制
>
> decbin函数    十进制转二进制
>
> decoct函数     十进制转八进制
>
> dechex函数    十进制转十六进制
>
> base_convert函数    n进制转m进制（取值范围是2到36，包括2和36）



**==演示案例==**：

![1537604170958](44)

### 浮点型

含义：相当于数学当中的==小数==，PHP语言默认浮点型就是双精度浮点型，占==8个字节==。



**==演示案例==**：

![1537604355094](45)



#### 浮点型数字比较问题

TIPS：由于浮点型数据在计算机中转换为二进制进行存储时，可能会存在==精度丢失==的问题，故浮点型数据的比较时常会出现不准确的情况。



**==演示案例==**：

![1537605750599](46)



**==小结==**：我们通过演示案例，发现浮点型数据在比较的过程中不稳定，所以以后我们在项目中，尽量规避使用浮点型数据做比较。



#### 科学计数法来表示浮点型

TIPS：需要使用字母e的结构来进行构建，e后面是负数，代表总共有多少位小数。



**==演示案例==**：

![1537605991257](47)



### 布尔型

TIPS：布尔值只有两个值，==true代表真==，==false代表假==。多用于比较之后得到的结果。



**==演示案例==**：

![1537606139055](48)



### 字符串型

TIPS：在PHP中，被引号（单引号或双引号都可以）所包裹的内容就是字符串。



**==演示案例==**：

![1537606390041](49)



#### 单引号与双引号包裹的区别

TIPS：在PHP中，由单引号与双引号定义的字符串是有区别的。



**==演示案例==**：

![1537606719144](50)



**==小结==**：单引号和双引号都可以定义出字符串，但是只有双引号定义的字符串中可以解析变量；



#### 更加良好的书写习惯

TIPS：通常，如果双引号中包含了变量，我们会使用"=={}=="包裹变量。



**==演示案例==**：

![1537607019929](51)



**==小结==**：

1. 单引号或双引号均可以定义出字符串；但是双引号定义的字符串中变量可以被解析；
2. 以后在构建程序的过程中，一旦定义的字符串里需要使用变量，则最好用"=={}=="包裹一下变量，以便增强可读性同时使得程序能够识别出从哪到哪是一个完整的变量；





## 5. 全天总结

1. 能够理解PHP的基本语法如：PHP标记、注释等

   PHP的开始标记：<?php

   PHP的结束标记：?>

   注释分为行注释和块注释，

   行注释：1)==//==  2) ==#==

   块注释：/\*.....\*/

2. 能够理解变量的4项基本操作

   对变量的4个基本操作有：增、删、改、查

   增：直接定义一个变量

   改：名不变，值改变

   删：使用unset函数直接删除某个变量，注意一次只能删一个

   查：1) ==echo==的方式输出；2)print_r的方式输出；3)==var_dump==的方式输出；

3. 能够理解并使用可变变量

   可变变量：

   ```php
   $var1 = 'name';
   $$var1 = 'zhangsan';
   #当前$name变量的变量名被$var1变量的值所替代了，则当前这个$name（等同于$$var1）就是可变变量
   ```

   

4. 能够理解变量间的传值方式

   变量的传值：1）变量的值传递；2）变量的引用传递；

   变量的值传递：修改一个变量的值，不影响另外一个变量的值；

   变量的引用传递：修改一个变量的值，将会影响另外一个变量的值；

5. 能够理解并使用常量

   定义常量：1）使用const关键字定义；2）使用define函数定义；

   常量的使用：1）常量定义后，不能进行修改操作；2）常量只能进行查看操作；











