---
title: MySQL优化-sphinx
date: 2019-02-25 18:40:48
tags: MySQL
---
# 目录

[TOC]

# 二、全文索引(Sphinx)

## 1、什么是 Sphinx

​	Sphinx 是由俄罗斯人 Andrew Aksyonoff 开发的一个全文检索引擎。它是基于 C 语言开发出来的。中文翻译为斯芬克司(作者定义这个索引的名字为 Sphinx 据说是为了纪念他的爱情)，Sphinx 最好的应用操作系统是 Linux，在国内我们使用 Sphinx 一般使用其词库分支 Coreseek(这个好比我们使用 Linux 一般使用分支 Centos 一样)。

## 2、Sphinx 必须的编译工具 gcc

​	由于安装 sphinx 必须要使用到 gcc 等编译工具，因此无论你是否已经有安装，为了保证准确性，都应该执行以下安装

```shell
shell># yum -y install make gcc g++ gcc-c++ libtool autoconf automake imake
```
<!-- more -->
**Sphinx 分词技术：是一种 C 语言的算法技术**

**广州黑马程序   广州   黑马   广州黑马   广州程序员    广州黑马程序员**

**黑马   黑马程序员   程序员**

# 三、Sphinx 中文分词库 CoreSeek

## 1、CoreSeek 的简介

​	Sphinx 支持众多的词库分支，在国内的开发中，我们一般选择 ==CoreSeek==，即中文词库，其词库提供者叫：高春辉

## 2、安装 CoreSeek

### 2.1、安装包说明

![1544165565635](1544165565635.png)

### 2.2、上传到 Linux 服务器上

![1544165801365](1544165801365.png)

### 2.3、在 xshell 中切换到 /root 目录，修改 ==install_coreseek.sh== 文件为 777

![1544166043333](1544166043333.png)

### 2.4、执行 ==./install_coreseek.sh==进行安装

安装如下图所示：

![1544166272652](1544166272652.png)

成功如下图所示：

![1544166257043](1544166257043.png)

> 注意：
>
> ​	如果出现警告你可以忽略
>
> ​	如果出现错误就必须重新编译，有些过程好像死机了，但是编译的时间过长所造成，不用理会
>
> ​	必须在 /root 目录下进行

可能报以下错误：

![1551001566435](1551001566435.png)

![1551001661195](1551001661195.png)

![1551001679128](1551001679128.png)

![1551001708783](1551001708783.png)

## 3、CoreSeek 的重要文件说明

### 3.1、CoreSeek 的配置文件

```shell
/usr/local/coreseek/etc/csft.conf
```

### 3.2、CoreSeek 的命令行工具

```shell
/usr/local/coreseek/bin
```

## 4、配置 CoreSeek 的数据源

​	配置数据源，首先我们需要先创建自己喜欢数据表(本例用code目录下 example.sql 为例子)

### 4.1、创建 test 数据库

```mysql
create database test charset=utf8
```

![1544166884313](1544166884313.png)

### 4.2、把 example.sql 文件上传

![1544166931681](1544166931681.png)

### 4.3、导入 test 数据库当中

```shell
shell># mysql -uroot -p123456 test < example.sql
```

![1544167058615](1544167058615.png)

执行成功后结果如下图所示：

![1544167108376](1544167108376.png)

![1544167143906](1544167143906.png)

### 4.4、修改 csft.conf 配置文件

```shell
shell># vim /usr/local/coreseek/etc/csft.conf
```

![1544169959131](1544169959131.png)

保存并退出(:x),coreseek就生效了无需重启

==假设你需要配置多张表的数据为sphinx全文索引:可以做如下定义数据源和索引信息==

![1544167483694](1544167483694.png)

![1544167509802](1544167509802.png)

==注意：在 Sphinx 配置的数据源越多，服务器的 CPU 消耗越多==

## 5、CoreSeek 的命令行基本使用

==/usr/local/coreseek/bin是存放sphinx的命令工具，所有的命令都必须在该目录中操作==

### 5.1、indexer 命令

```shell
shell># ./indexer [--all] [--rotate]
```

作用：创建 Sphinx 的全文索引

选项：

--all 生成所有的全文索引文档(如果有多张表就生成多张表索引)

![1544169844047](1544169844047.png)

--rotate 生成增量索引(与php进行交互时生成增量索引api)

![1544170006800](1544170006800.png)

>  注意：
>
> ​	有可能报以下错误：./indexer: error while loading shared libraries: libmysqlclient.so.20: cannot open shared object file: No such file or directory
>
> ​	解决方法：shell># ln -s /usr/local/mysql/lib/libmysqlclient.so.20 /usr/lib64/

### 5.2、search  命令

作用：搜索关键字(包括中文和英文的关键字)

```shell
shell># ./search 关键字
```

比如搜索广州,可以写成./search 广州

# 四、在PHP中开启sphinx扩展

