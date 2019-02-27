---
title: http协议与文件编程
date: 2019-02-20 17:00:32
tags: PHP
---
# 一、昨日回顾

## 1. 知识回顾

1. 连库基本操作涉及的函数？

   1) mysqli_connect函数     负责连接数据库和选择数据库操作

   2) mysqli_set_charset函数      负责设置字符集操作

2. 如何执行设置（增删改）数据操作？

   mysqli_query函数      执行增删改SQL语句的，成功返回true，失败返回false
<!-- more -->
3. 如果执行查询操作？

   1) 构建查询数据的SQL语句；(select * from xxx where 1)

   2) 通过mysqli_query函数执行查询SQL语句，执行失败返回false，执行成功返回对象结果集；

   3) 解析对象结果集；

   ​        a. 通过mysqli_fetch_assoc函数解析得到一行数据，得到的结果是一个一维关联数组；

   ​	b. 通过mysqli_fetch_all函数一次性解析得到所有数据，得到的结果是一个二维数组，默认情况下是索引类型的，如果指定第二个参数为MYSQLI_ASSOC则得到的结果是一个关联类型的二维数组；


# 二、知识路径

- HTTP协议的相关概念

  ​	为什么学习HTTP协议

  ​	什么是HTTP协议

- HTTP协议的分类

  ​	HTTP请求

  ​	HTTP响应

- HTTP协议的特点

  ==目标：能够说出HTTP请求和HTTP响应的组成部分 和 HTTP协议的特点==

- HTTP协议的应用

  ​	跳转

  ​	刷新跳转

  ​	SSL安全访问协议

  ​		https简介

  ​		配置SSL协议

  ==目标：实现跳转和刷新跳转==

- 文件编程相关概念

  ​	为什么使用文件编程

  ​	什么是文件编程

- 文件编程的分类

  ​	对目录的操作

  ==目标：实现对目录内容的增删改查==

  ​	对文件的操作

  ​		PHP4相关文件操作

  ​		PHP5相关文件操作

  案例：文件下载

  ==目标：实现文件下载功能==



# 三、今日课程内容（上）：HTTP协议

## 1. HTTP协议的相关概念

### 为什么学习HTTP协议

浏览器访问网站页面时，走的都是==HTTP协议==，如以下截图红框所示部分，标明的就是HTTP协议。

![1539825750609](2)

作为一个将要从事web项目开发工作的开发者，对HTTP协议需要有一个基本的了解。



### 什么是HTTP协议

**HTTP**：Hyper Text Transfer Protocol  中文翻译为 "超文本传输协议"。

 

Mr ZHANG是英国人，只会说英语；宫本是日本人，只会说日语；两人想要相互沟通交流，如果都各自使用本国语言，那么对方都将听不懂。

为了能够让两个人无障碍相互沟通交流，让Mr ZHANG和宫本都学会中文，那么，两人就都能够使用中文进行交流了。而中文在这里就变成了两人之间沟通的一个协议。这个协议规定了两方向对方说话，按照某种语法来表达一个具体的意思。

```sequence
Mr ZHANG(英国人)-->宫本（日本人）: 使用中文说："早上好！" 问候宫本
note right of 宫本（日本人）:宫本按照中文的语法\n来理解"早上好！"的意思
宫本（日本人）-->Mr ZHANG(英国人): 使用中文说："早上好！" 回复Mr ZHANG
note left of Mr ZHANG(英国人):Mr ZHANG按照中文的语法\n来理解"早上好！"的意思
```

而在web项目中，是浏览器和服务器之间进行交互的一个过程。也就意味着浏览器与服务器两方要通话。

而浏览器和服务器本身并不是同类，双方之间要想互相说话让对方听的明白，可以让双方都支持一个第三方的规定，通过这个规定来进行沟通交流。那么，HTTP协议，就是浏览器和服务器之间沟通的这个规定。

```sequence
浏览器-->服务器: 使用HTTP协议规定的格式\n发出请求
note right of 服务器:服务器按照HTTP协议规定的格式\n解析并处理请求内容
服务器-->浏览器: 使用HTTP协议规定的格式\n返回响应
note left of 浏览器:浏览器按照HTTP协议规定的格式\n解析并处理响应内容
```

 

**概念**：HTTP协议，就是一种==规定==，它规定了浏览器请求服务器时，需要用什么样的格式发送请求数据；也规定了服务器响应浏览器时，需要用什么样的格式回应响应数据；这些对**数据格式的规定**就是HTTP协议。



## 2. HTTP协议的分类

HTTP协议的分类包含两种：

- [x] HTTP请求
- [x] HTTP响应



### HTTP请求

TIPS：我们可以理解成就是浏览器和服务器说的话。

包含四个组成部分：1）请求行；2）请求头；3）空白行；4）请求数据 

**==图例==**：

![1539827285408](3)



#### 请求行

包含基本的请求信息部分。

**==图例==**：

![1539827546864](4)



#### 请求头

由一个一个的==请求协议项==组成。

**==图例==**：

![1539829815818](5)



**常见的请求头协议项**：

> **host**：当前url中所要请求的服务器的主机名（域名）
> **accept-encoding**：是浏览器发给服务器,声明浏览器支持的压缩编码类型  比如gzip
> **accept_charset**：表示浏览器支持的字符集
> **==referer==**：表示，此次请求来自哪个网址
> **accept-language**：可以接收的语言类型，cn，en等
> **cookie**：如果之前当前请求的服务器在浏览器端设置了数据（cookie），那么当前浏览器再次请求该服务器的时候，就会把对应的数据带过去
> **user-agent**：用户代理，当前发起请求的浏览器的内核信息
> **accept**：表示浏览器可以接收的数据类型
> **content-length**（==post==）：只有post提交的时候才会有的请求头，显示的是当前要提交的数据的长度（字节）
> **if-modified-since**（==get==）：表示，在客户端向服务器请求某个资源文件时，询问此资源文件是否被修改过
> ==**content-type**==（==post==）：用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件



#### 空白行

专门用于分隔请求头和请求数据的结构。

**==图例==**：

![1539830299654](6)



#### 请求数据

只有当==post方式==提交数据时才会存在。

**==图例==**：

![1539830334212](7)



#### 小结

1. HTTP请求包含四个组成部分：

   1）请求行

   2）请求头

   3）空白行

   4）请求数据




### HTTP响应

包含四个组成部分：1）状态行；2）响应头；3）空白行；4）响应数据 

**==图例==**：

![1539830564966](8)



#### 状态行

包含基本的响应信息部分。

**==图例==**：

![1539830678029](9)



##### 响应状态码

1xx：表示请求尚未完成；

2xx：表示请求和响应都没有问题；

3xx：表示重定向；

4xx：表示请求出现问题，响应失败；

5xx：表示服务器出现问题，响应失败；



==**常见的响应状态码**==：

200  表示请求和响应完全没有问题

301和302：表示永久重定向和临时重定向

404：表示请求的文件丢失或者找不到

500：表示服务器环境出现了问题



#### 响应头

由一个一个的响应协议项组成。

**==图例==**：

![1539832878217](10)



**常见的响应头协议项**：

> **server**：服务器主机信息
>
> **date**：响应时间
>
> **last-modified**：文件最后修改时间（对应请求中：if-modified-since）
>
> **content-length**：响应主体的长度（字节）
>
> **content-type**：响应内容的数据类型：text/html，image/png等
>
> ==**location**==：重定向，浏览器遇到这个选项，就立马跳转（不会解析后面的内容）
>
> ==**refresh**==：重定向（刷新），浏览器遇到这个选项就会准备跳转，刷新一般有时间限制，时间到了才跳转，浏览器会继续向下解析
>
> **content-encodeing**：文件编码格式
>
> **cache-control**：缓存控制，no-cached不要缓存



#### 空白行

专门用于分隔响应头和响应数据的结构。

**==图例==**：

![1539832982993](11)



#### 响应数据

包含完整的返回给浏览器的内容。

**==图例==**：

![1539833009701](12)



#### 小结

1. HTTP响应包含四个组成部分：

   1）状态行；

   2）响应头；

   3）空白行；

   4）响应数据；




## 3. HTTP协议的特点

1) 不仅支持B/S模式，还支持C/S模式。

2) 灵活，支持任意类型的数据。

3) ==无连接特性==，指的是每次完整的请求之后，本次连接要断开。

4) ==无状态特性==，HTTP协议对会话过程中产生的数据不具有记忆能力。



## 4. HTTP协议的应用

### 跳转

**==需求==**：实现访问A页面就直接跳转到B页面的效果。

**==解答==**：构建一个名为code1.php的程序文件，代码如下：

```php
<?php

echo '这是code1.php文件';

//直接跳转
header('Location:http://www.home.com/class/day13/code/code2.php');
```

构建一个名为code2.php的程序文件，代码如下：

```php
<?php

echo '这是code2.php文件';
```

访问code1.php，浏览器展示的效果为：

![1539833950790](13)

我们并没有观察到code1.php中的输出，就直接跳转到code2.php中了。



**==小结==**：

1. header('Location:http://www.xxxx.com/xx.php')这种方式可以实现直接跳转。

   ![1539834546924](14)



### 刷新跳转

**==需求==**：实现访问A页面，在A页面停留3秒钟展示提示信息"别眨眼，3秒后我就要跳转了！"，3秒后再跳转到B页面。

**==解答==**：构建一个名为code3.php的程序文件，代码如下：

```php
<?php

echo '别眨眼，3秒后我就要跳转了！';

//刷新跳转
header('Refresh:3; url=http://www.home.com/class/day13/code/code4.php');
```

构建名为code4.php的程序文件，代码如下：

```php
<?php

echo '这是code4.php';
```



### SSL安全访问协议

#### https简介

HTTPS  即  Secure Hypertext Transfer Protocol  翻译为中文：==安全的超文本传输协议==



> 它是由Netscape开发并**内置于其浏览器中**，用于**对数据进行压缩和解压操作**，**并用于网络传输**的一种技术。
>
> HTTPS实际上应用了Netscape的完全套接字层（SSL）作为HTTP应用层的子层。（==HTTPS使用端口443==，而不是象HTTP那样使用端口80来和TCP/IP进行通信。）SSL使用40 位关键字作为RC4流加密算法，这对于商业信息的加密是合适的。HTTPS和SSL支持使用==X.509==数字认证，如果需要的话用户可以确认发送者是谁。
>
> https是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，https的安全基础是SSL。
>
> 它是一个URI scheme(抽象标识符体系)，句法类同http:体系。用于安全的HTTP数据传输。https:URL表明它使用了HTTP，但HTTPS存在不同于HTTP的默认端口及一个加密/身份验证层（在HTTP与TCP之间）。这个系统的最初研发由网景公司进行，提供了身份验证与加密通讯方法，现在它被广泛用于万维网上安全敏感的通讯，例如交易支付方面。



#### 配置SSL协议

**==目标==**：使用  https://www.t1.com/t.php  访问 day13/code目录中  的t.php文件。

**==步骤==**：

第一步，将day13/source目录下的key文件夹拷贝到apache安装目录下的==conf目录==中

复制如下图所示的key目录，

![1539846057097](15)

粘贴到apache安装根目录下的conf目录中，

![1539846117333](16)



第二步，打开apache的配置文件httpd.conf，开启mod_socache_shmcb 和mod_ssl模块，同时开启引入ssl配置文件，

去掉httpd.conf中如下所示配置项的注释符，

![1539846328069](178)

同时去掉httpd.conf中引入httpd-ssl.conf配置文件前的注释符，

![1539846493371](17)

第三步，调整apache安装目录下的extra/httpd-ssl.conf配置文件SSLSessionCache配置项的路径，同时注释VirtualHost示例，

调整httpd-ssl.conf中92行引入文件的路径，修改为实际apache的安装根目录，

![1539846672181](18)

然后注释掉121～290行，

![1539846787748](19)



第四步，打开虚拟主机配置文件，增加如下虚拟主机配置项：

```mysql
<VirtualHost *:443>
	SSLEngine on
	SSLProtocol all -SSLv2 -SSLv3
#SSL加密计算的方式
	SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5
#认证证书文件
	SSLCertificateFile "F:/usr/apache24/conf/key/server.crt"
#证书私钥
	SSLCertificateKeyFile "F:/usr/apache24/conf/key/server.key"
#根证书
	SSLCertificateChainFile "F:/usr/apache24/conf/key/ca.crt"
	DocumentRoot "F:/home/class/day13/code"
	ServerName www.t1.com
	<Directory />
	Options +Indexes +FollowSymLinks +ExecCGI
	AllowOverride All
	Order allow,deny
	Allow from all
	Require all granted
	</Directory>
</VirtualHost>
```



第五步，重启apache，向hosts文件中新增如下配置，

![1534923186999](20)

在hosts文件中增加如下配置，(C:/Windows/system32/drivers/etc/hosts)

```
127.0.0.1  www.t1.com
```



第六步，在day13/code目录中创建名为t.php的程序文件，代码如下

```php
<?php

echo '哈哈哈，需要通过https来访问我哟～';
```



第七步，在浏览器中使用  https://www.t1.com/t.php  访问t.php，测试使用效果

![1539847792959](21)



# 四、今日课程内容（下）：文件编程

## 1. 文件编程相关概念

### 为什么使用文件编程

在web项目中，文件编程的应用相当广泛，比如：文件下载（软件，电影，照片....），生成报表文件并支持下载功能等都需要使用到文件编程技术。



### 什么是文件编程

所谓的文件编程技术，指的就是对==文件==或==目录==的==增删改查==操作！



## 2. 文件编程的分类

- [x] 对目录的操作
- [x] 对文件的操作



### 对目录的操作

**设置操作**（增删改）

> 涉及的函数：
>
> **mkdir**(新目录名[,  目录权限[,  是否递归创建]])      创建一个目录（make directory）
>
> **rmdir**(目录全路径)      删除一个目录（remove directory）
>
> **rename**(旧名字,  新名字)     给目录改名或转移目录



**==操作需求1==**：在code目录中执行创建目录操作，要求：

1. 创建名为dir1和dir2两个目录；
2. 递归创建dir3/dir3_1/dir3_1_1目录；

**==解答==**：构建一个名为code5.php的程序文件，代码如下：

```php
<?php

//创建目录
$re = mkdir('./dir1');
var_dump( $re ); echo '<hr/>';

$re = mkdir('./dir2');
var_dump( $re ); 

//如果需要递归创建目录，则需要指定第三个参数为true，否则无法递归创建目录
$re = mkdir('./dir3/dir3_1/dir3_1_1', 0777, true);
var_dump( $re ); 
```

**==操作需求1小结==**

1. 将mkdir函数的第三个参数指定为true,则可以递归创建目录。



**==操作需求2==**：在code目录中执行修改目录操作，要求：

1. 将dir1的目录名改为dir100；
2. 将dir2目录转移到dir3目录中，并且重命名为dir200；

**==解答==**：构建一个名为code6.php的程序文件，代码如下：

```php
<?php

//将dir1的目录名改为dir100
$re = rename('./dir1', './dir100');
var_dump( $re ); 

//将dir2目录转移到dir3目录中，并且重命名为dir200
$re = rename('./dir2', './dir3/dir200');
var_dump( $re ); 
```

**==操作需求2小结==**

1. rename不仅能够修改目录的名字，还能够将目录转移到指定的另一个目录中。



**==操作需求3==**：在code目录中执行删除目录操作，要求：

1. 将dir100删除；
2. 尝试将dir3目录删除；

**==解答==**：构建一个名为code7.php的程序文件，代码如下：

```php
<?php

//将dir100删除
$re = rmdir('./dir100');
var_dump( $re ); 

//尝试将dir3目录删除
$re = rmdir('./dir3');//因为dir3不是一个空目录，所以没有办法删除掉
var_dump( $re ); 
```



**==操作需求3小结==**

1. 删除操作的前提是该目录一定是一个空目录。



**查询操作**

> 涉及的函数：
>
> **opendir**(目录全路径)      打开一个目录
>
> **readdir**(打开的目录资源)      读取目录中的内容
>
> **closedir**(打开的目录资源)     关闭一个打开的目录



**==操作需求==**：查询code下的demo目录，要求：

1. 读取并输出该目录下的所有文件的文件名；

**==解答==**：构建一个名为code8.php的程序文件，代码如下：

```php
<?php

//打开一个目录
$op = opendir('./demo');
var_dump( $op ); echo '<hr/>';

//读取打开的目录
$dir = readdir($op);       //  #1
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #2
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #3
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #4
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #5
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #6
echo $dir; echo '<br/>';
$dir = readdir($op);       //  #7
var_dump($dir); echo '<br/>';
$dir = readdir($op);       //  #8
var_dump($dir); echo '<br/>';

//关闭打开的目录
$re = closedir($op);
var_dump( $re ); echo '<br/>';
var_dump( $op ); 
```

访问code8.php输出的内容：

```mysql
resource(3) of type (stream)    //$op的输出
.       #1的输出
..       #2的输出
article.txt       #3的输出
dir1       #4的输出
dir2       #5的输出
dir3       #6的输出
bool(false)        #7的输出
bool(false)        #8的输出

NULL //输出closedir的返回值，返回值为NULL说明没有返回值
resource(3) of type (Unknown)   //关闭打开的目录之后输出的$op
```



**==操作需求小结==**

1. 如果需要读取一个目录中的内容，则第一步必须使用opendir打开这个文件，然后再使用readdir去读取打开文件中的内容（文件名）;

2. readdir读取文件的方式：

   ​      1)  一次执行只能读取到一个文件的名字，只要是非根盘符目录，则第一次和第二次读取到的一定是"."和".."的目录；

   ​      2) 如果读取完之后，再次执行，则只能得到false值，表名已经读完了打开目录中的内容；



**查询辅助操作**

> 涉及的函数：
>
> ==**realpath**==(路径)      将给定的路径转换为绝对路径地址
>
> **==basename==**(路径)      返回当前给定路径的基础文件（或文件夹）名部分
>
> ==**dirname**==(路径)      返回当前给定路径的目录部分
>
> **is_dir**(全路径)      判断一个给定文件是否是一个目录



**==操作需求==**：在code目录下对相对路径"./"进行操作，要求：

1. 将相对路径"./"转换为绝对路径，并将转换后的结果输出；
2. 获得"1"中转换的绝对路径中的基础文件名并将其输出；
3. 获得"1"中转换的绝对路径中的目录部分并将其输出；
4. 判定"./code1.php"是否是一个目录；
5. 判定"./dir3"是否是一个目录；

**==解答==**：构建一个名为code9.php的程序文件，代码如下：

```php
<?php

#1. 将相对路径"./"转换为绝对路径，并将转换后的结果输出
$path = './';
$nowPath = realpath($path);
echo $nowPath . '<hr/>';

#2. 获得"1"中转换的绝对路径中的基础文件名并将其输出
$re = basename($nowPath);
echo $re . '<hr/>';

#3. 获得"1"中转换的绝对路径中的目录部分并将其输出
$re = dirname($nowPath);
echo $re . '<hr/>';

#4. 判定"./code1.php"是否是一个目录
$re = is_dir('./code1.php');
var_dump( $re ); echo '<hr/>';

#5. 判定"./dir3"是否是一个目录
$re = is_dir('./dir3');
var_dump( $re ); 
```

访问code9.php输出的结果：

```mysql
F:\home\class\day13\code     #操作1的输出
code     #操作2的输出
F:\home\class\day13     #操作3的输出
bool(false)     #操作4的输出
bool(true)     #操作5的输出
```

**==操作需求小结==**

1. realpath虽然也能处理绝对路径，但是给绝对路径参数是没有意义的。



### 对文件的操作

因php版本更替原因，在php中对文件的操作分为两类：

- [x] php4相关文件操作
- [x] php5相关文件操作



#### PHP4相关文件操作

涉及的函数：

> **fopen**(文件全路径,  打开模式)    打开一个文件
>
> **fread**(打开的文件资源,  读取内容的长度)    读取文件中的内容
>
> **fwrite**(打开的文件资源,  写入的内容)    向文件中写入内容
>
> **fclose**(打开的文件资源)    关闭打开的文件
>
> **filesize**(文件全路径)  获取文件的大小



**==操作需求==**：操作code目录下的article.txt文件，要求：

1. 将文件中的所有内容读取并输出；
2. 向文件中最末尾写入"这是一篇好文章"；
3. 关闭打开的文件；

**==解答==**：构建一个名为code10.php的程序文件，代码如下：

```php
<?php

#1. 将文件中的所有内容读取并输出
//$fp = fopen('./article.txt', 'r');//r为只读模式
$fp = fopen('./article.txt', 'a+');//a+为读写模式，并且文件指针指向了文件的最末尾
var_dump( $fp ); echo '<hr/>';

//读取打开文件的所有内容
$size = filesize('./article.txt');//获取文件的大小
var_dump( $size ); echo '<br/>';

$content = fread($fp, $size);
echo $content . '<hr/>';

#2. 向文件中最末尾写入"这是一篇好文章"
$str = '这是一篇好文章';
$re = fwrite($fp, $str);
var_dump( $re ); echo '<hr/>';

#3. 关闭打开的文件
$re = fclose($fp);
var_dump( $re ); 
```



**==小结==**：PHP4中能够对文件执行什么样的操作，取决于fopen时采用了什么样的模式；



#### PHP5相关文件操作

涉及的函数：

> **file_put_contents**(文件全路径,  写入的内容[,  写入方式])      向文件中写入内容	
>
> ​													写入方式可指定为FILE_APPEND
>
> **file_get_contents**(文件全路径)      获得文件中的内容
>



**==演示案例==**：读取article.txt文件中的内容，并且向article.txt文件中写入"好酒哇！"内容

实现：构建名为code11.php的程序文件，代码如下：

```php
<?php

#1. 读取article.txt文件中的内容
$content = file_get_contents('./article.txt');
echo $content . '<hr/>';

#写入"好酒哇！"内容
$str = '好酒哇！';
//$re = file_put_contents('./article.txt', $str);//不指定第三个参数，则默认为覆盖写入
$re = file_put_contents('./article.txt', $str, FILE_APPEND);//指定第三个参数为FILE_APPEND常量，则写入的方式为追加写入
var_dump( $re ); 
```



**==小结==**：file_put_contents写入内容时，如果不指定第三个参数默认为覆盖写入，如果指定第三个参数为FILE_APPEND，则表示追加写入内容。



## 3. 案例：文件下载

**==操作需求==**：实现文件下载功能，要求：

1. 通过下载页面可以下载code/dw目录中的txt文件和zip文件；
2. 下载txt文件指定默认的新名字为a.txt；zip文件指定默认的新名字为b.zip；

**==步骤==**：

第一步，构建名为code12.php的程序页面，代码如下：

```html
<!DOCTYPE html>
<HTML>
<head>
    <meta charset="UTF-8">
    <title>文件下载</title>
</head>
<body>
    <a href="http://www.home.com/class/day13/code/code13.php?type=1">点击下载txt好文章</a>
    <a href="http://www.home.com/class/day13/code/code13.php?type=2">点击下载zip好种子</a>
</body>
</HTML>
```

第二步，构建名为code13的程序处理页面，代码如下：

```php
<?php

if( $_GET['type']==1 ){//下载txt文件

    $newName = 'a.txt';//新的名字
    $path = './dw/article.txt';//txt文件的原文件路径
}elseif( $_GET['type']==2 ){//下载zip文件

    $newName = 'b.zip';//新的名字
    $path = './dw/fscp.zip';//zip文件的原文件路径
}

//服务器告诉浏览器 响应数据的内容类型是文件流格式的类型
header('Content-type:application/octet-stream');

//服务器告诉浏览器  响应数据的处理方式应该以attachment即附件的形式来处理，并且指定一个新的名字即filename参数后面所指定的名字
header("Content-disposition:attachment; filename={$newName}");

//将需要下载的文件内容全部作为响应内容响应给浏览器
echo file_get_contents($path);
```

第三步，测试使用效果：

访问code13.php，点击下载txt文件，

![1539854904182](22)

点击之后，弹出上图所示下载框，新名字为a.txt，

然后选择下载路径，直接下载，

![1539854955433](23)



## 4. 全天总结

1. 能够解释http协议的概念和特点

   http协议：超文本传输协议；

   特点：1) 不仅支持B/S还支持C/S模式；2)灵活支持任意类型的数据；3)==无连接==特性；4)==无状态==特性;

2. 能够理解http请求的组成部分

   1) 请求行；2)请求头； 3) 空白行; 4)请求数据；

3. 能够理解http响应的组成部分

   1) 状态行； 2）响应头；3）空白行；4）响应数据；

4. 能够理解常见响应状态码的含义

   200：请求和响应都成功；

   301和302：永久重定向和临时重定向

   404：请求页面不存在或者找不到

   500：服务器环境出现错误

5. 能够使用header函数来设置常见响应信息

   header('Location:http://www.xxxx.com');    立即跳转

   header('Refresh:3; url=http://www.xxx.com/code1.php');    刷新跳转，3秒之后跳到指定的链接地址去

6. 能够创建、打开、删除一个目录

   创建： mkdir函数

   打开：opendir函数

   删除： rmdir函数

7. 能够使用文件基本读写函数

   读：file_get_contents函数

   写：file_put_contents函数    不指定第三个参数，则默认为覆盖写入；如果指定第三个参数为FILE_APPEND则为追加写入。

8. 能够使用fopen()函数打开或创建一个文件

   ```
   fopen('./article.txt', 模式);//模式为r表示只读模式；模式为a+表示读写模式，并且文件指针指向文件末尾，如果文件不存在则会自动创建
   ```

   

### 课后练习

1. 实现跳转和刷新跳转到指定的页面；
2. 实现对目录的增删改查操作；
3. 实现文件下载功能；

