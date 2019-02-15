---
title: 源码编译安装lamp环境
date: 2019-02-15 18:57:18
tags: linux
---
# 目录

[TOC]

# 一、查看系统信息

## 1、主机名

查看：

```shell
shell># hostname
```

![1542503658201](1542503658201.png)

<!-- more -->
永久修改：

==centos 6修改如下：==

```
shell>#vim /etc/sysconfig/network
```

![1542503766524](1542503766524.png)

```
shell># vim /etc/hosts
```

![1542503845155](1542503845155.png)

```
shell>#hostname Youlongit
```

==centos 7 修改如下：==

```
shell># hostnamectl set-hostname <hostname>
```

重新链接我们Linux服务器后可发现主机名已更改

![1542504091053](1542504091053.png)

## 2、服务器时间

查看：shell># date

如果时间不正确，可以这样修改

```shell
shell># vim /etc/sysconfig/clock
```

内容为：

```shell
ZONE="Asia/Shanghai"
UTC=false
ARC=false
```

![1542504377270](1542504377270.png)

然后再执行：

```shell
shell># date -s "YYYYMMDD HH:mm:ss"
```

例如：date -s "20190215 10:39:11"

==注意： -s 的s是小写的==

![1542504506099](1542504506099.png)

最后同步BIOS时钟，强制把系统时间写入CMOS

```shell
shell># clock -w
```

==注意：-w的w是小写==

![1542504625983](1542504625983.png)

## 3、系统内核及位数

查看内核：

```shell
shell># cat /etc/redhat-release
```

![1542504751734](1542504751734.png)

查看位数：

```shell
shell># uname -a
```

![1542504792476](1542504792476.png)

有x86_64这个关键字表明系统是64位的

## 4、虚拟内存

​	如果服务器的物理内存小于2G，而且又没有设置虚拟内存的话，在编译安装MySQL的时候，就很容易发生以下报错：

![1542504914761](1542504914761.png)

解决方法：==增加内存==或者==增加虚拟内存==

先用free指令查看内存情况

```shell
shell># free
```

![1542505972541](1542505972541.png)

可以看到系统还没有设置虚拟内存，添加虚拟内存的方法如下：

**第一步：创建swap文件**

```shell
shell># cd /var
shell># dd if=/dev/zero of=swapfile bs=1024 count=2048000
shell># mkswap swapfile
```

**第二步：激活**

```shell
shell># swapon swapfile
```

**第三步：开机自动创建**

```shell
shell># echo "/var/swapfile swap swap defaults 0 0" >> /etc/fstab
```

# 二、WEB环境搭建

## 1、底层软件库

运行以下命令确保底层软件库都已安装

```shell
shell># yum -y install gcc gcc-c++ autoconf automake zlib libxml2 libxml2-devel ncurses-devel libmcrypt libtool cmake bison make pcre pcre-devel libevent openssl openssl-devel gd-devel bzip2 bzip2-devel libcurl curl-devel python python-devel mysql-devel expat-devel
```

## 2、源码编译安装MySQL

​	`MySQL从5.5`版本开始，通过`./configure`进行编译配置方式已经被取消，取而代之的是`cmake`工具。需要下载`CMake`编译器、`Boost`库、`ncurses`库和`GNU`分析器生成器`bison`这4种工具，可以用`yum install`的方式安装 `cmake`、`bison`、`boost`、`ncurses`

MySQL二进制文件可以 http://dev.mysql.com/downloads/mysql 或者 http://mirrors.sohu.com/mysql 去下载

注意要下载与操作系统对应的版本及位数（32位或64位），编译安装需要下载源码版(Source Code)，此处以mysql-5.7.24tar.gz为例

**以下 安 装中 涉 及的 几点 需 要提 前 说明 的问 题 ：**

- 所有 下 载的 文件 将 保存 在 /root 目 录下
- mysql 将以 mysql 用户 运 行， 而 且将 加入 service 开 机 自动 运 行
- mysql 将被 安 装在 /usr/local/mysql/ 目录 下
- mysql 默认 安 装使 用 utf8 字符集
- mysql 的数 据 和日 志文 件 保存 在 /usr/local/mysql/data/ 目录 下
- mysql 的配 置 文件 保存 于 /usr/local/mysql/etc/my.cnf
- tmp
- ==注意 从 MySQL 5.7.5 开 始 Boost 库是 必需 的==

### 2.1、安装依赖库

安装 cmake 和 bison 与 ncurses

```shell
shell># yum -y install -y cmake bison ncurses
```

下载 Boost

shell># wget http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz

解压

```shell
shell># tar -zxvf boost_1_59_0.tar.gz
```

![1542525971527](1542525971527.png)

![1544860866590](1544860866590.png)

移动

```shell
shell># mv boost_1_59_0 /usr/local/boost
```

![1542525911758](1542525911758.png)

### 2.2、安装MySQL

#### 2.2.1、建立mysql安装目录和用户

在/usr/local/mysql建立【data】、【tmp】、【log】、【etc】4个目录

```shell
shell># mkdir -pv /usr/local/mysql
```

![1542526004136](1542526004136.png)

```shell
shell># cd /usr/localmysql
```

![1542526027705](1542526027705.png)

```shell
shell># mkdir data tmp log etc
```

![1542526068917](1542526068917.png)

```shell
shell># groupadd mysql
shell># useradd -g mysql -s /usr/sbin/nologin mysql
```

![1544865695700](1544865695700.png)

#### 2.2.2、下载mysql

```shell
shell># cd ~
shell># wget http://mirrors.sohu.com/mysql/MySQL-5.7/mysql-5.7.24.tar.gz
```

#### 2.2.3、解压并进入目录

```shell
shell># tar -zxvf mysql-5.7.24.tar.gz
shell># cd mysql-5.7.24
```

![1542526192979](1542526192979.png)

#### 2.2.4、进行软件配置和环境检测

```shell
shell># cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/tmp/mysql.sock \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DSYSCONFDIR=/usr/local/mysql/etc \
-DMYSQL_USER=mysql \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS:STRING=utf8,gbk \
-DWITH_DEBUG=0 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_EMBEDDED_SERVER=1 \
-DMYSQL_USER=mysql \
-DWITH_BOOST=/usr/local/boost \
-DDOWNLOAD_BOOST=1
```

以上内容的解释：

> shell># cmake
>
> ​	#设置安装目录
>
> ​	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql
>
> ​	#设置sock目录
>
> ​	-DMYSQL_UNIX_ADDR=/usr/local/mysql/tmp/mysql.sock
>
> ​	#设置数据库存放目录
>
> ​	-DMYSQL_DATADIR=/usr/local/mysql/data
>
> ​	#设置配置文件my.cnf存放目录
>
> ​	-DSYSCONFDIR=/usr/local/mysql/etc
>
> ​	#设置mysql的默认使用用户
>
> ​	-DMYSQL_USER=mysql
>
> ​	#设置默认字符集
>
> ​	-DDEFAULT_CHARSET=utf8
>
> ​	#默认校对规则
>
> ​	-DDEFAULT_COLLATION=utf8_general_ci
>
> ​	#指明安装额外字符集的支持
>
> ​	-DWITH_EXTRA_CHARSETS:STRING=utf8,gbk
>
> ​	#去除debug模式
>
> ​	-DWITH_DEBUG=0
>
> ​	#以下几条开启常用的引擎
>
> ​	-DWITH_MYISAM_STORAGE_ENGINE=1
>
> ​	-DWITH_INNOBASE_STORAGE_ENGINE=1
>
> ​	-DWITH_ARCHIVE_STORAGE_ENGINE=1
>
> ​	-DWITH_PARTITION_STORAGE_ENGINE=1
>
> ​	#Enable LOAD DATA LOCAL INFILE（default：disabled）
>
> ​	-DENABLED_LOCAL_INFILE=1
>
> ​	#使用一些字符函数的汇编版本
>
> ​	-DWITH_EMBEDDED_SERVER=1
>
> ​	-DMYSQL_USER=mysql
>
> ​	#5.7编译需要boost类库，指明boost的路径
>
> ​	-DWITH_BOOST=/usr/local/boost
>
> ​	#在以上路径如果没查找到boost会自动下载并解压(如事先已下载好，此句可略)
>
> ​	-DDOWNLOAD_BOOST=1

#### 2.2.5、编译软件并且进行安装

```shell
shell># make && make install
```

> ==注意：==
>
> ​	接下来可能需要耗较长时间，请耐心等待。另外==如果过程中出现报错而中断，需要删除CMakeCache.txt文件后再执行 2.2.4步骤内容（没出错请不要执行以下两行指令）：==
>
> shell># make clean
>
> shell># rm -rf CMakeCache.txt或者 shell># unlink CMakeCache.txt
>

### 2.3、配置

#### 2.3.1、使用递归，把mysql目录所有者设置为mysql这个用户

```shell
shell># chown -R mysql:mysql /usr/local/mysql
```

![1544865725279](1544865725279.png)

如果 /etc/my.cnf 存在的话，请删除

```shell
shell># unlink /etc/my.cnf
```

![1544865771181](1544865771181.png)

#### 2.3.2、进入MySQL安装目录下

```shell
shell># cd /usr/local/mysql
```

![1544865811567](1544865811567.png)

重建 my.cnf文件

#### 2.3.3、根据实际情况优化mysql配置

```shell
shell># vim etc/my.cnf
```

![1542530887034](1542530887034.png)

大概内容如下：

```shell
# my.cnf 的示例配置
[client]
default-character-set = utf8
port = 3306
socket = /usr/local/mysql/tmp/mysql.sock

[mysqld]
datadir =/usr/local/mysql/data
port = 3306
socket = /usr/local/mysql/tmp/mysql.sock
user = mysql
symbolic-links = 0
pid-file = /usr/local/mysql/tmp/mysql.pid

explicit_defaults_for_timestamp = true
sql_mode = ERROR_FOR_DIVISION_BY_ZERO,NO_ZERO_DATE,NO_ZERO_IN_DATE,NO_AUTO_CREATE_USER

slow_query_log = on
slow_query_log_file = /usr/local/mysql/log/slow.log
long_query_time = 2

log_error = /usr/local/mysql/log/mysql.err
```

#### 2.3.4、初始化和启动

##### 2.3.4.1、初始化mysql的基本表

==5.7及以上的做法：==

--initialize会生成一个随机密码(~/.mysql_secret)，而--initialize-insecure不会生成密码

```shell
shell># /usr/local/mysql/bin/mysqld \
--initialize-insecure \
--basedir=/usr/local/mysql \
--datadir=/usr/local/mysql/data \
--user=mysql
```

![1544865931414](1544865931414.png)

> ==注意：==
>
> ​	如果初始化出错，则需要用 rm -rf /usr/local/mysql/data/* 将 mysql 的 data 目录下的文件和目录都删除，然后再重新运行以上语句

##### 2.3.4.2、启动mysql

```shell
shell># /usr/local/mysql/bin/mysqld_safe > /dev/null 2>&1 &
```

![1544866021108](1544866021108.png)

##### 2.3.4.3、修改mysql的root密码

```shell
shell># /usr/local/mysql/bin/mysqladmin -u root password 123456
```

![1544866215949](1544866215949.png)

（其中 ==123456== 为您希望使用的密码）

##### 2.3.4.4、增加到开机启动

先将mysqld设置为服务

```shell
shell># cp /usr/local/mysql/support-files/mysql.server /etc/rc.d/init.d/mysqld
```

![1544866307623](1544866307623.png)

然后将mysqld服务加入启动项：

```shell
shell># chkconfig --add mysqld
```

![1544866344744](1544866344744.png)

设置为自启动：

```shell
shell># chkconfig --level 345 mysqld on
```

![1544866389307](1544866389307.png)

--level<等级代号> 　指定读系统服务要在哪一个执行等级中开启或关毕。
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动

##### 2.3.4.5、将mysql命令加入到环境变量里

```shell
shell># PATH=$PATH:/usr/local/mysql/bin
```

![1544866455158](1544866455158.png)

为了重启后仍能有效：

```shell
shell># echo 'PATH=$PATH:/usr/local/mysql/bin' >> /root/.bashrc
```

![1544866479258](1544866479258.png)

##### 2.3.4.6、如果需要对外开放 3306 端口

```shell
shell># iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
```

如果 mysqld 服务正常运行中，但是执行 mysql 指令时报出以下错误：

![1550214182031](1550214182031.png)

解决方案就是创建一个软链接

```shell
shell># mkdir -pv /var/lib/mysql
shell># ln -s /usr/local/mysql/tmp/mysql.sock /var/lib/mysql/mysql.sock
```

![1544866541226](1544866541226.png)

## 3、源码编译安装PHP

安装PHP之前，首先要检查安装libxml2、libxml2-devvel、gd-devel

```shell
shell># rpm -qa | grep libxml2*
shell># rpm -qa | grep gd-devel*
```

如果没有安装，可以使用`yum -y install`命令进行安装。

php可以到 http://php.net/downloads.php 或者 http://mirrors.sohu.com/php/ 去下 载,此处 以php-7.2.2.tar.gz为例 

**以下安装中涉及的几点需要提前说明的问题：**

- 所有下载的文件将保存在 /root 目录下

- php 将以 FastCGI模式运行，监听 9000 端口(默认端口)

- php 将被安装在 /usr/local/php 目录下

- php 的配置文件保存于 /usr/local/php/etc/php.ini

- php 的扩展库文件，如果可以的话，尽量放在 /usr/local/php/lib/php/extensions/ 目录


### 3.1、安装PHP

#### 3.1.1、下载

```shell
shell># cd ~
shell># wget http://am1.php.net/distributions/php-7.2.2.tar.gz
```

#### 3.1.2、解压并进入目录

```shell
shell># tar -zxvf php-7.2.2.tar.gz
shell># cd php-7.2.2
```

#### 3.1.3、将configure参数及详情解析另存为一个文件，以供学习参考用：

```shell
shell># ./configure --help > php_configure.txt
```

> 注意：
>
> ​	这步可以忽略

#### 3.1.4、进行软件的配置和环境检测

```shell
shell># ./configure \
--prefix=/usr/local/php \
--with-config-file-path=/usr/local/php/etc \
--enable-fpm \
--enable-opcache \
--with-zlib-dir \
--with-bz2 \
--with-libxml-dir=/usr \
--with-gd \
--with-freetype-dir \
--with-jpeg-dir \
--with-png-dir \
--enable-mbstring \
--with-mysql=/usr/local/mysql \
--with-mysqli=/usr/local/mysql/bin/mysql_config \
--with-pdo-mysql=/usr/local/mysql \
--with-iconv \
--disable-ipv6 \
--enable-static \
--enable-inline-optimization \
--enable-sockets \
--enable-soap \
--with-openssl \
--with-curl
```

以上内容的解释如下：

> ./configure \										# 检测环境，并生成makefile文件
>
> --prefix=/usr/local/php \							# 指定php安装目录
>
> --with-config-file-path=/usr/local/php/etc \			# 设置php.ini配置文件存放的路径
>
> --enable-fpm \									# 开启PHP-FPM，用于控制PHP-CGI的FastCGI进程
>
> --enable-opcache \								# PHP新增opcache功能
>
> --with-zlib-dir \									# 加载zlib库
>
> --with-bz2 \										# 加载bz2的压缩库
>
> --with-libxml-dir=/usr \							# 加载xml库
>
> --with-gd \										# 加载gd库
>
> --with-freetype-dir \								# 加载字体库
>
> --with-jpeg-dir \									# 加载jpeg库
>
> --with-png-dir \									# 加载png库
>
> --enable-mbstring \								# 开启mbstring加密库
>
> --with-mysql=/usr/local/mysql \					# 加载mysql，并指定mysql基本路径
>
> --with-mysqli=/usr/local/mysql/bin/mysql_config \	# 加载mysqli库，并指定mysql配置工具
>
> --with-pdo-mysql=/usr/local/mysql \				# 加载pdo对mysql的支持
>
> --with-iconv \										# 开启转换字符集
>
> --disable-ipv6 \									# 关闭ipv6支持
>
> --enable-static \									# 以静态方式编辑php库
>
> --enable-inline-optimization \						# 开启优化线程
>
> --enable-sockets \								# 开启sockets支持
>
> --enable-soap \									#开启soap对web service的支持
>
> --with-openssl \									# 开启ssl支持
>
> --with-curl										# 模拟浏览器

#### 3.1.5、编译软件并且进行安装

```shell
shell># make && make install
```

### 3.2、配置

#### 3.2.1、复制配置文件

```shell
shell># cp php.ini-production /usr/local/php/etc/php.ini
```

![1542538692743](1542538692743.png)

```shell
shell># cp sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm
```

![1542538501466](1542538501466.png)

赋予其可执行权限

```shell
shell># chmod +x /etc/rc.d/init.d/php-fpm
```

![1542538529733](1542538529733.png)

拷贝产生 php-fpm 的配置文件

```shell
shell># cd /usr/local/php/etc
shell># cp php-fpm.conf.default php-fpm.conf
```

![1542538570410](1542538570410.png)

#### 3.2.2、配置php.ini

```shell
shell># vim php.ini
```

- 找到 ;date.timezone = 修改为 date.timezone = Asia/Shanghai

  ![1542538756501](1542538756501.png)

- 根据自己的需求调整以下选项的值

error_reporting = E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED

​	![1550217004054](1550217004054.png)

display_errors = On

![1550217096993](1550217096993.png)

max_execution_time = 60

![1550217178184](1550217178184.png)

max_input_time = 60

![1550217222694](1550217222694.png)

memory_limit = 256M

![1550217267541](1550217267541.png)

post_max_size = 256M

![1550217309855](1550217309855.png)

upload_max_filesize = 256M

![1550217349602](1550217349602.png)

#### 3.2.3、配置 php-fpm.conf

```shell
shell># cd /usr/local/php/etc/php-fpm.d
shell># cp www.conf.default www.conf
```

![1544872431124](1544872431124.png)

```shell
shell># vim www.conf
```

- 找到 user = nobody 和 group = nobody，将 nobody 改成 www

  ![1544872562086](1544872562086.png)

- 找到 listen.owner=nobody 和 listen.group= nobody，将 nobody 改成 www

  ![1544872613288](1544872613288.png)

> ==注意：==
>
> ​	==必须得先新增www用户，否则无法启动php==
>
> shell># groupadd www
>
> shell># useradd -g www -s /usr/sbin/nologin www

#### 3.2.4、将 php-fpm 加入服务并自动启动

```shell
shell># service php-fpm start
shell># chkconfig --add php-fpm
shell># chkconfig --level 345 php-fpm on
```

--level<等级代号> 　指定读系统服务要在哪一个执行等级中开启或关毕。
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动

## 4、源码编译安装Apache

安装apache之前，首先要先确保已安装 APR 和 APR-util

```shell
shell># yum -y install apr*
```

apache 可以到 http://mirrors.sohu.com/apache/ 下载，此处理以 httpd-2.4.37.tar.gz 为例

**以下安装中涉及的几点需要提前说明的问题：**

- 所有下载的文件将保存在 /root 目录下
- apache 将以 www用户运行，而且将加入 service 开机自动运行
- apache 将被安装在 /usr/local/httpd 目录下
- apache 的每个网站将存放在 /mydata/wwwroot 目录下，默认网站为php33
- apache 的配置文件保存于 /usr/local/httpd/conf/httpd.cnf

### 4.1、安装依赖库

安装 apr 和 apr-util

```shell
shell># cd ~
shell># wget http://archive.apache.org/dist/apr/apr-1.4.8.tar.gz
shell># mkdir /usr/local/apr
```

![1544872970716](1544872970716.png)

```shell
shell># tar -zxf apr-1.4.8.tar.gz
shell># cd apr-1.4.8
shell># ./configure --prefix=/usr/local/apr
shell># make && make install
```

```shell
shell># cd ~
shell># wget http://archive.apache.org/dist/apr/apr-util-1.5.2.tar.gz
shell># mkdir /usr/local/apr-util
shell># tar -zxf apr-util-1.5.2.tar.gz
shell># cd apr-util-1.5.2
shell># ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr/bin/apr-1-config
shell># make && make install
```

### 4.2、安装apache

#### 4.2.1、下载apache

```shell
shell># cd ~
shell># wget http://mirrors.sohu.com/apache/httpd-2.4.37.tar.gz
```

#### 4.2.2、解压并进入目录

```shell
shell># tar -zxvf httpd-2.4.37.tar.gz
shell># cd httpd-2.4.37
```

#### 4.2.3、将configure参数及详情解析另存为一个文件以供学习参考用

```shell
shell># ./configure --help > httpd_configure.txt
```

> 注意：
>
> ​	这步可以忽略

#### 4.2.4、进行软件的配置和环境检测

```shell
shell># ./configure \
--prefix=/usr/local/httpd \
--enable-deflate \
--enable-expires \
--enable-headers \
--enable-so \
--enable-modules=most \
--with-mpm=worker \
--enable-rewrite \
--enable-static-ab \
--enable-ssl \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util
```

以上内容解释如下：

![1542591854322](1542591854322.png)

#### 4.2.5、编译软件并且进行安装

```shell
shell># make && make install
```

### 4.3、配置

#### 4.3.1、简单配置

创建网站根目录

```shell
shell># mkdir -pv /mydata/wwwroot
```

![1544874079411](1544874079411.png)

```shell
shell># chown www:www /mydata/wwwroot
```

![1544874103421](1544874103421.png)

```shell
shell># cd /usr/local/httpd/conf
```

开始配置

```shell
shell># vim httpd.conf
```

内容修改如下：

把

User daemon
Group daemon

修改以下

User www

Group www

![1542612075783](1542612075783.png)

把ServerName www.example.com:80 前面#去除，修改成：ServerName localhost:80

![1542612127128](1542612127128.png)

将 DocumentRoot 以及整个 Directory 标签代码段的内容都注释掉

![1542612248495](1542612248495.png)

![1542612281096](1542612281096.png)

DirectoryIndex index.htm index.html index.php

![1542612328037](1542612328037.png)

modules/mod_rewrite.so	去除前面的 # (井号)

![1542612372383](1542612372383.png)

Include conf/extra/httpd-vhosts.conf	去除前面的 # (井号)

![1544874374696](1544874374696.png)

保存并退出

#### 4.3.2、优化虚拟主机配置

​	httpd.conf中的每个 VirtualHost 代码段都是一个独立的虚拟主机配置。当配置多个虚拟主机时，需要在 httpd.conf 里写多个 VirtualHost 代码段，那么主配置文件就会变得重复、臃肿。==所以建议，将 VirtualHost 代码段都写在另一个文件（例如：conf/extra/httpd-vhosts.conf）或多个文件（例如按客户或项目分组），然后再用 Include 指令包含进来。==

![1550220207258](1550220207258.png)

本文档以 httpd-vhosts.conf 举例说明：

```shell
shell># vim /usr/local/httpd/conf/extra/httpd-vhosts.conf
```

```shell
# 默 认 的 网 站 根 目 录
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot "/mydata/wwwroot/php33/www/"
    <Directory />
        # 允 许 访 问 该 目 录
        Require all granted
        # 允许执行 .htaccess 文件中的指令。
        AllowOverride All
        # 不 允 许 浏 览 目 录
        Options -Indexes
    </Directory>
</VirtualHost>
```



> 注意：
>
> ​	把httpd-vhosts.conf文件的内容全部注释掉或删除

保存

#### 4.3.3、设为服务并开机自运行

编辑 apachectl

```shell
shell># vim /usr/local/httpd/bin/apachectl
```

加入以下注释内容

```shell
# add by php33 2019-02-15
# chkconfig: 2345 85 15
# description: Activates/Deactivates Apache Web Server
```

![1550220481692](1550220481692.png)

找到 $HTTPD -k $ARGV

改成 $HTTPD -k $ARGV -f /usr/local/httpd/conf/httpd.conf

![1542613024062](1542613024062.png)

保存并启动 apache

```shell
shell># /usr/local/httpd/bin/apachectl start
```

将 httpd 设置为服务

```shell
shell># cp /usr/local/httpd/bin/apachectl /etc/rc.d/init.d/httpd
```

![1542613546384](1542613546384.png)

加入启动项：

```shell
shell># chkconfig --add httpd
shell># chkconfig --level 345 httpd on
```

--level<等级代号> 　指定读系统服务要在哪一个执行等级中开启或关毕。
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动

![1542613572183](1542613572183.png)

### 4.4、整合PHP

#### 4.4.1、修改 apache 的配置文件

```shell
shell># vim /usr/local/httpd/conf/httpd.conf
```

去掉以下两项的注释

```shell
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
```

![1542613682596](1542613682596.png)

在底部加上

```shell
<FilesMatch \.php$>
	SetHandler "proxy:fcgi://127.0.0.1:9000"
</FilesMatch>
```

![1542613758094](1542613758094.png)

保存

#### 4.4.2、重启 apache

```shell
shell># service httpd restart
```

### 4.5、开放端口

将 80 端口加入防火墙

```shell
shell># iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

添加至自启动文件

```shell
shell># echo 'iptables -I INPUT -p tcp --dport 80 -j ACCEPT' >> /etc/rc.d/rc.local
```

# 三、总结

Lamp源码安装

源码编译安装分几步走？
答： 3步

第1步： ./configure 检查系统环境，并生成 makefile 文件
第2步： make 读取 makefile文件，编译生成寸进制文件
第3步： make install 读取二进制文件，并安装系统指定目录

这三步顺序能调换吗？
答：不能
