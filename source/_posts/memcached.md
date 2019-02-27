---
title: memcached
date: 2019-02-16 18:57:13
tags: memcache
---
# 目录?

[TOC]

# 一、昨日回顾

## 1、知识点回顾

能够运用 chmod、chown 命令

​	chmod 修改文件权限

​		chmod -R 权限值(777) 文件/目录

​	chown 修改文件拥有者，同时也可以修改所属组

​		chown -R 拥有者(用户名):所属组(组名) 文件/目录

能够运用 sudo、whereis 命令

​	whereis 查找文件

​		whereis my.cnf

​	sudo 提权

​		sudo yum -y install 需要安装的包名

​		在其它的Linux下 apt-get install vim

​			sudo apt-get install 包名

能够实现FTP客户端连接FTP服务端

​	FTP分为两个端：客户端、服务端(vsftpd)

​	客户端：1、把IP地址填写上，2、输入用户名与密码，3、输入端口

​	服务端：1、安装，2、启动

​	wget http://vsftpd.*.rpm

​		报 wget 的指令不存在

​			yum -y install wget

​	rpm -ivh vsftpd.*.rpm

​	启动：service vsftpd start

​	查看状态：service vsftpd status

​	停止：service vsftpd stop

​	重启：service vsftpd restart

能够运用 tar 命令

​	作用：解压 .tar  .gz .tar.gz

​	httpd-2.24.tar.gz

​	tar -zxhv httpd-2.24.tar.gz

能够说同源码安装的步骤

分几步走？ 3步走

下载

解压

进入解压后的目录中

1、./configure 检查环境，并生成一个makefile文件

2、make 读取 makefile文件，并生成二进制文件

3、make install 读取二进制文件，并安装到指定目录

<!-- more -->
# 二、NoSql的含义

​	NoSQL（Not Only SQL），指非关系型数据库，它是由一次叫“反Sql运动”的社区讨论而诞生的体系。这个运动的发起最早源自于社区网站 LiveJournal 的开发团队，它们的初衷是为了用于==减少数据库连接数，减轻数据库的工作压力。==

NoSql优点在于速度快，简单易学，缺点在没有官方支持，安全性较差。

# 三、Memcache的概述

## 1、Memcache的优点

- 纯内内存的存储机制，不占据硬盘空间，速度快
- 内置分布式算法，使得开发不需要自己去实现算法，只需要克隆就可以形成分布式集群服务器

## 2、Memcache的缺点

- 安全性很差，因为它是用telnet协议
- 容量小，只有 64M，1个 key 只能存储 1M
- 由于 Memcache 把数据置于内存中，重启就会丢失，数据无法持久保存
- 数据零散，无法遍历

==注意：今天我们还在学习 Memcache，是因为还有一定使用量，随着时间的推移，该软件会逐渐被抛弃掉，redis 能够实现 memcache 的所有功能，且比 memcache 强大很多。==

# 四、Memcache的安装

安装的命令如下：

```shell
shell># yum -y install memcached telnet telnet-server
```

![1545013637073](1545013637073.png)

![1542765288189](1542765288189.png)

安装成功后如下图所示：

![1542765336000](1542765336000.png)

# 五、启动 Memcache 的服务器

需要启动 Memcache 和 telnet 的服务，才能正常使用 Memcached

## 1、启动 Memcache

```shell
shell># service memcached start  	# 启动
shell># service memcached status 	# 查看状态
shell># service memcached stop		# 停止
shell># service memcached restart	# 重启
```

![1542776997646](1542776997646.png)

![1545013818919](1545013818919.png)

> 注意：
>
> ​	如果需要开启自启动 memcache服务，使用 shell># chkconfig memcached on

## 2、启动Telnet服务

```shell
shell># service xinetd start	# 启动
shell># service xinetd status	# 查看状态
shell># service xinetd stop		# 停止
shell># service xinetd restart	# 重启
```

![1542777297540](1542777297540.png)

# 六、使用 telnet 连接 Memcache

```
命令格式：telnet [memcache 的服务器地址 IP] [端口]
```

![1545014069242](1545014069242.png)

==注意：刚打开 telnet 客户端敲入回车键会出现 ERROR，并不是代表连接失败而是因为 memcache 服务器在等待你输入正确的指令==

![1542783360911](1542783360911.png)

# 七、Memcache 的常用指令

## 1、add命令

```
命令作用：创建一个存在内存中的 key => value的键值对
命令格式：add [key 的名称][标识符][过期的时间][字节的大小]
标识符：标识符表示内存中的节点，一般设置为0，对于PHP开发没有太大的意义
过期时间：在 memcached 当中如果设置过期时间为0，表示永久保存，不会过期，如果设置为30表示30秒后过期，过期时间以秒为单位
字节的大小：一般 memcached命令行(黑窗口)当中字节大小是需要我们自己设定，比较麻烦，用byte来作为单位，对于php开发没有太大的意义
```

设置key = name，value = vuejs，如下所示：

![1545014477138](1545014477138.png)

现实开发中使用 `add` 的频率比较低，多数用 `set`

==注意事项1：如果字节超出会报错==

![1542784257258](1542784257258.png)

==注意事项2：如果字节小于设定的大小情况如下==

![1545014639668](1545014639668.png)

## 2、get命令

```
命令作用：获取一个 memcache 的 key 中 value
命令格式：get [key 的名称]
```

获取一个 key 为 name 的 value，如下所示：

![1545014790838](1545014790838.png)

## 3、set命令

```
命令作用：修改或者添加一个键值对，键存在则是修改，不修改则添加
命令格式：set [key 的名称][标识符][过期的时间][字节的大小]
```

修改一个已经存在的键值对

![1545014988935](1545014988935.png)

![1545015071364](1545015071364.png)

## 4、delete命令

```
命令作用：主动删除一个键值对
命令格式：delete [key 名称]
```

删除一个键值对

![1545015187374](1545015187374.png)

删除不存在：

![1545015246959](1545015246959.png)

## 5、incr 命令

```
命令作用：自增n
命令格式：incr key value
```

![1545016720777](1545016720777.png)
![1545016844965](1545016844965.png)

> 注意：
>
> ​	使用incr指令时，如果age这个key会导致失败
>
> ![1545016937309](1545016937309.png)

## 6、decr 命令

```
命令作用：自减n
命令格式：decr key value
```

![1545017105787](1545017105787.png)

> 注意：
>
> ​	使用decr指令时，如果age这个key会导致失败
>
> ![1545017186101](1545017186101.png)



## 7、过期时间

由于开发中使用 set 比较多，所以我们可以使用 set 命令操作 memcache 的过期时间

设置一个键值对在 10秒后过期

![1545017418643](1545017418643.png)

## 8、xshell 操作 memcache 的注意事项

回退(退格删除)快捷键：

ctrl + backspace

退出 memcache 命令行：

- 按下 ctrl + ]

- 在telent>输入 quit

  ![1545017578275](1545017578275.png)

## 9、清空 memecache 中所有数据

清空 memcached 中的内存数据，有两种方法：

第1种方法：重启 memcached，使用：service memcached restart

![1545018011255](1545018011255.png)

第2种方法：使用`flush_all`命令来清空

![1545017875644](1545017875644.png)

注意事项：重启telnet服务并不会清除 memcached 当中数据

![1542788284360](1542788284360.png)

telnet只要启动过一次，将来发生启动或停止，但 memcached 没有停止的情况下，memcached 并不会丢失数据，除非 memcached 自己发生重启或停止，所有链接都会被终止与所有数据都丢失。

# 八、在win中开启Memcache的扩展

## 1、查看我们PHP版本号与线程

![1545028984446](1545028984446.png)

## 2、下载扩展文件

https://github.com/nono303/PHP7-memcache-dll

![1545029293266](1545029293266.png)

## 3、复制到php扩展文件目录下

![1545029416843](1545029416843.png)

## 4、修改我们php.ini文件

![1545029501045](1545029501045.png)

保存并重启apache服务

![1545029556141](1545029556141.png)

# 九、在win安装memcache服务

## 1、下载memcache服务软件

官网地址：http://memcached.org/

![1545030285270](1545030285270.png)

解压文件：

![1545030350980](1545030350980.png)

## 2、启动memcache服务

以D:\gzphp33\memcache\资料\软件\memcached目录为例

```cmd
D:\gzphp33\memcache\资料\软件\memcached> memcached.exe
```

memcached基本参数设置
　　　　-p 监听的端口
　　　　-l 连接的IP地址, 默认是本机
　　　　==-d start 启动memcached服务==
　　　　==-d restart 重起memcached服务==
　　　　==-d stop|shutdown 关闭正在运行的memcached服务==

　　　　-d install 安装memcached服务
　　　　-d uninstall 卸载memcached服务
　　　　-u 以的身份运行 (仅在以root运行的时候有效)
　　　　-m 最大内存使用，单位MB。默认64MB
　　　　-M 内存耗尽时返回错误，而不是删除项
　　　　-c 最大同时连接数，默认是1024
　　　　-f 块大小增长因子，默认是1.25
　　　　-n 最小分配空间，key+value+flags默认是48
　　　　-h 显示帮助

![1545030801713](1545030801713.png)

## 3、添加win系统服务中

![1545031059777](1545031059777.png)

![1545031287030](1545031287030.png)

说明：

==-d start 启动memcached服务==
==-d restart 重起memcached服务==
==-d stop|shutdown 关闭正在运行的memcached服务==	

这3条必须是memcached服务添加到系统服务中才使用

## 4、删除win系统服务

![1545031401104](1545031401104.png)

# 十、在PHP中安装 Memcache 的扩展

## 1、安装

### 1.1、下载

https://github.com/websupport-sk/pecl-memcache/archive/php7.zip

### 1.2、上传到Linux服务器上

使用rz命令 上传至linux虚拟机上，前提得先安装该指令

```shell
shell># yum install -y lrzsz
```

![1542790491837](1542790491837.png)

安装成功后如下图所示：

![1542790511923](1542790511923.png)

上传pecl-memcache-php7.zip到Linux服务器上

![1542790657390](1542790657390.png)

### 1.3、解压并进入目录

```shell
shell># unzip pecl-memcache-php7.zip
```

有可能报以下图所示的错误

![1542790996717](1542790996717.png)

==注意：主要是没有安装 unzip 指令==

解决方法：使用 yum -y install unzip 指令安装即可

```shell
shell># cd pecl-memcache-php7
```

### 1.4、生成编译文件

```shell
shell># /usr/local/php/bin/phpize
```

![1545032891497](1545032891497.png)

### 1.5、进行软件配置和环境检测

```shell
shell># ./configure --with-php-config=/usr/local/php/bin/php-config
```

![1542791229206](1542791229206.png)

### 1.6、编译软件并且进行安装

```shell
shell># make && make install
```

![1542791290372](1542791290372.png)

成功如下图所示：

![1542791315952](1542791315952.png)

![1545033051691](1545033051691.png)

## 2、配置

### 2.1、修改php.ini 加载Memcache组件

查找到 extension_dir = "ext"

```shell
shell># vim /usr/local/php/etc/php.ini
```

![1542791478824](1542791478824.png)

### 2.2、结束PHP进程

```shell
shell># pkill -9 php-fpm
```

### 2.3、重新启动PHP进程

```shell
shell># service php-fpm start
```

![1542791610021](1542791610021.png)

### 2.4、检查是否添加成功

第1种方法：在站点目录下创建一个phpinfo.php文件，输入以下内容

```php
<?php
    phpinfo();
```

效果如下：

![1542791790904](1542791790904.png)

第2种方法：使用php -m查看

```shell
shell># /usr/local/php/bin/php -m
```

![1542791893009](1542791893009.png)

# 十一、在PHP中安装 Memcached 的扩展

## 1、安装依赖库安装

### 1.1、下载

```shell
shell># cd ~
shell># wget http://pecl.php.net/get/igbinary-2.0.8.tgz
shell># wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz
```

### 1.2、安装igbinary依赖库

#### 1.2.1、解压文件

```shell
shell># tar -zxvf igbinary-2.0.8.tgz
```

### 1.2.2、进入目录

```shell
shell># cd igbinary-2.0.8
```

#### 1.2.3、生成编译文件

```shell
shell># /usr/local/php/bin/phpize
```

![1544939439395](1544939439395.png)

#### 1.2.4、进行软件配置和环境检测

```shell
shell># ./configure --prefix=/usr/local/igbinary --with-php-config=/usr/local/php/bin/php-config
```

#### 1.2.5、生成二进制 

```shell
shell># make
```

#### 1.2.6、编译安装

```shell
shell># make install
```

#### 1.2.7、修改php.ini配置文件

查找到：extension_dir

```shell
shell># vim /usr/local/php/etc/php.ini
```

### 1.3、安装libmemcached依赖库

#### 1.3.1、解压文件

```shell
shell># cd ~
shell># tar -zxvf libmemcached-1.0.18.tar.gz
```

### 1.3.2、进入目录

```shell
shell># cd libmemcached-1.0.18
```

#### 1.3.3、进行软件配置和环境检测

```shell
shell># ./configure --prefix=/usr/local/libmemcached --with-memcached
```

#### 1.3.4、生成二进制 

```shell
shell># make
```

#### 1.3.5、编译安装

```shell
shell># make install
```

## 2、Memcached扩展

### 2.1、下载

```shell
shell># cd ~
shell># wget http://pecl.php.net/get/memcached-3.0.4.tgz
```

### 2.2、解压

```shell
shell># tar -zxvf memcached-3.0.4.tgz
```

### 2.3、进入目录

```shell
shell># cd memcached-3.0.4
```

### 2.4、生成编译文件

```shell
shell># /usr/local/php/bin/phpize
```

### 2.5、进行软件配置和环境检测

```shell
shell># ./configure --with-php-config=/usr/local/php/bin/php-config --enable-memcached --enable-memcached-json --enable-memcached-igbinary --with-libmemcached-dir=/usr/local/libmemcached --disable-memcached-sasl
```

### 2.7、生成二进制文件

```shell
shell># make
```

### 2.8、编译安装

```shell
shell># make install
```

### 2.9、修改php.ini配置文件

```shell
shell># vim /usr/local/php/etc/php.ini
```

![1550289459069](1550289459069.png)

重启 php

```shell
shell># pkill -9 php-fpm
shell># service php-fpm start
```

# 十二、Memcached 类的常用方法说明

常用的类方法如下所示：

addServer(host, port)：host 就是 Linux 服务器地址，port默认是 11211（memcached端口），用于连接 memcached 服务器

==addServers(servers)：用于连接分布式 memcached 服务器，主要分布式==

set(key, value, expire)：用于修改或添加内存的 memcached 数据，该方法有3个参数

参数说明：

key 为键名

value 是键值

flag 使用MEMCACHE_COMPRESSED指定对值进行压缩(使用zlib)

exipre 是过期时间，expire 默认为0表示永远不过期，如果设置为秒，最大只能设置为30天的秒数(30 * 86400秒)

get(key)：获取对于键值对

delete(key)：删除 memcached 当中键值对

## 1、使用 addServer 方法连接 memcache 服务器

详细代码参考code/addServer.php,上传代码到/mydata/wwwroot/php33/www中进行测试

![1545036917392](1545036917392.png)

```php

```

效果如下：

![1545036943573](1545036943573.png)

## 2、set 方法和 get 方法操作各种数据类型

详细代码参考code/sg.php,上传代码到/mydata/wwwroot/php33/www中进行测试

### 2.1、操作整型

代码如下：

![1545037203343](1545037203343.png)

效果如下：

![1545037238866](1545037238866.png)

### 2.2、操作浮点类型

代码如下所示：

![1545037502842](1545037502842.png)

效果如下所示：

![1545037480057](1545037480057.png)

### 2.3、操作字符串类型

代码如下所示：

![1545037667177](1545037667177.png)

效果如下所示：

![1545037702437](1545037702437.png)

### 2.4、操作布尔类型

代码如下所示：

![1545037819366](1545037819366.png)

效果如下所示：

![1545037854315](1545037854315.png)

### 2.5、操作复合类型

代码如下所示：

![1545037964586](1545037964586.png)

效果如下所示：

![1545037991357](1545037991357.png)

### 2.6、操作序列化数据

代码如下所示：

![1545038120224](1545038120224.png)

效果如下所示：

![1545038158853](1545038158853.png)

memcache 不会修改php的数据类型，给它什么它就返回什么。

黑窗口获取变量的结果如下，发现中文占据utf-8的3个字节为1个字符长度，标量的节点为0，也就是标识符为0

![1542856976693](1542856976693.png)

黑窗口运行变量的结果如下，发觉中文占据utf-8的3个字节为1个字符长度，数组的节点为1，也就是标识符为1，数组存在内存中是用memcached的序列化结构来存储，不等同于php的序列化

![1542857382269](1542857382269.png)

==注意事项：如果我们在黑窗口中设置一个key的标识为1，也就是把这个键值对放在内存中节点1当中，这时php是无法读取的,如下图所示：==

![1542857658795](1542857658795.png)

如果标识为1,则无法读取该键值对

![1542857857282](1542857857282.png)

如果我们修改表示为0,则可以正常获取

![1542858139975](1542858139975.png)

![1542858127204](1542858127204.png)

## 3、设置过期时间

详细代码参考code/expire.php,上传代码到/mydata/wwwroot/php33/www中进行测试

代码如下所示：

![1545039529387](1545039529387.png)

效果如下所示：

![1545039561618](1545039561618.png)

## 4、使用delete方法

详细代码参考code/delete.php,上传代码到/mydata/wwwroot/php33/www中进行测试

代码如下所示：

![1545039691076](1545039691076.png)

效果如下所示：

![1545039666290](1545039666290.png)

![1545039729519](1545039729519.png)

![1545039751672](1545039751672.png)

# 十三、PHP操作 Memcache分布式服务器

![1545039978431](1545039978431.png)

memcache内置自动分布算法的原理：

![1545040877822](1545040877822.png)

第1步：使用分布式的链接服务器，插入1000条数据分布到不同的服务器中

代码如下所示(code/randAdd.php)

![1545040386130](1545040386130.png)

第2步：使用get方法获取分布式服务器中的数据，代码如下(code/ms/getMS.php)

![1545040939352](1545040939352.png)

# 十四、修改 session 存储到 memcache 中

## 1、修改 php.ini 文件

```shell
shell>#vim /usr/local/php/etc/php.ini
```

![1550306367462](1550306367462.png)

```shell
# 创建 tmp 目录
shell># mkdir -pv /usr/local/php/tmp
```

## 2、重启服务

```shell
# 杀死 php 所有进程
shell># pkill -9 php-fpm
# 启动 php 服务
shell># service php-fpm start
```

## 3、设置session

![1550306759559](1550306759559.png)

有可能报以下错误：

![1550306664254](1550306664254.png)

以上错误代表用户没有权限去操作 /usr/local/php/tmp

设置拥有都与所属组：

```shell
shell># chown -R www:www /usr/local/php/tmp
```

## 4、设置session到memcache服务器

![1550308713319](1550308713319.png)

==注意：==

​	==修改之后，能正常存储与获取，但是使用 telnet 连接 memcache 无法获取数据，通过 session_id 无法获取；可能是 memcache 或 php 版本导致的。==

# 十五、TP5中应用 memcache (扩展)

## 1、创建虚拟主机

```shell
shell># vim /usr/local/httpd/conf/extra/httpd-vhosts.conf
```

![1550308958386](1550308958386.png)

## 2、上传代码

把项目压缩包上传

## 3、创建一个tp5目录

![1550309243341](1550309243341.png)

把项目的代码移到我们的tp5目录

```shell
shell># mv thinkphp_5.0.24_with_extend.zip
```

![1550309310941](1550309310941.png)

### 4、进入 tp5 目录，并解压项目

```shell
shell># cd /mydata/wwwroot/tp5
shell># unzip thinkphp_5.0.24_with_extend.zip
```

![1550309400405](1550309400405.png)

## 5、修改 windows 下hosts 文件

![1550309521671](1550309521671.png)

## 6、重启 Linux 下的 apache

![1550309642612](1550309642612.png)

## 7、修改 session 配置文件

![1550310215857](1550310215857.png)

# 十六、总结

memcache常用方法

add 添加，如果key存在，添加失败

set 修改/添加，如果key存在修改，不存在添加

get 获取数据

delete 删除指定key

decr 自减n

incr 自增n

flush_all 清空所有数据

php扩展文件安装

win得注意什么？

先查看版本号、php线程、位数

Linux

先查看版本号、php线程、位数

扩展文件memcache与memcached有什么区别？

memcached可实现分布式

修改配置文件后需要重启什么？

重启我们apache/php
