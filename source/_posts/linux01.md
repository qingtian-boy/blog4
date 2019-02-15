---
title: linux01
date: 2019-02-13 18:31:10
tags: linux
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识回顾

能够实现VM虚拟机安装CentOS操作系统

​	首先创建一个虚拟主机

​	然后载入光盘( .iso镜像文件)

​	再然后开机

能够运用vim编辑器

​	有几种模式？	3种，分别为：命令行模式、编辑模式、末行模式

vim 文件 --- 直接进到命令行模式

​	复制一行：yy

​	复制多行：ny n代表多少行 例如：复制3行，3y

​	粘贴：p

​	想去文件末尾：G

​	想去文件首先：gg或 :1

​	删除光标所在当前行：dd

​	删除多行：

​		nd+回车键，例如：删除6行，默认会删除7行

​		ndd，例如：删除6行，默认会删除6行

​	撤销：u

​	想替换当前光标位置：r

​	删除光标当前位置的内容：x

进入编辑模式： i a o I A O

退出编辑模式：esc

怎么进入末行模式：首先必须到命令行模式，输入冒号( : )

:q 退出

:q! 强制退出

:w 保存

:w! 强制保存

:wq 保存并退出

:wq! 强制保存并退出

把3行复制到4行： 3copy4

把第3行移到第4行：3m4
<!-- more -->
# 二、Linux内核分析

## 1、/etc/grub.conf文件

### 1.1、passwd命令

​	Linux以安全性和稳定性在世界上自居，在Linux发明之初就在安全领域做了很多手段，其中最简单就是提供了密码的登录和密码修改的功能，在Linux系统当中无论什么用户都必须具有密码才能登录Linux操作系统。

**命令格式： passwd [用户名]**

**命令作用：更新或者设置用户登录的密码**

![1541233855181](1541233855181.png)

> 注意
>
> ​	如果你是root用户，你可以随意修改任何一个普通用户的密码，主要是编译root管理其他的用户而生的一个安全手段。

## 2、/etc/inittab文件

​	其实在linux存在多种操作界面，常见的有图形操作界面也有字符操作界面(黑窗口)，而我们的虚拟机在启动的时候选择是字符操作界面而不是图形操作界面呢？
原因在于Linux有一个文件专门控制用户的登录操作界面，这个文件叫/etc/inittab文件，我们在业界当中专业的说法叫inittab文件为==用户选择登录操作界面控制文件==

分析/etc/inittab文件，使用vim打开文件，内容如下图所示：

```shell
shell># vim /etc/inittab
```

![1](1.png)

第18行： 0 代表Linux的关机模式,该模式如果一旦设置，那么Linux无法启动(因为开机就会关机)，所以我们不能设置该模式为Linux的模式

第19行：1代表Linux单用户模式，如果设置为1，那么Linux启动就马上进入单用户模式状态，一般用户运维工程师的维护系统工作当中，php程序员一般不会使用

第20行：2表示字符操作界面的多用户模式，这个模式没有NFS上网的功能，所以这个模式用于60年代没有互联的时候 比较多，今天在互联网的时代基本没有人使用

==第21行：3代表功能齐全的多用户字符操作界面(俗称黑窗口界面)，真正的服务器一般都是用该模式，所以我们使用这个模式的几率在真正的开发中是100%==

第22行：4代表自定义模式,这个模式一般是C和C++程序员用于开发==嵌入式==的时候使用得非常多，使用该模式需要有非常强的Linux源码分析能力和C语言能力，一般开发者不是这个专业的很少接触

第23行：5代表图形操作界面

第24行：6代表Linux重启模式，这个模式一旦设置(Linux开启就重启)，无法正常的启动，因此我们不能设置这个模式

![3](3.png)

​	以上代码选择为3，代表功能齐全的字符操作界面,所以Linux启动的时候，就会进入黑窗口界面让用户进行登录，比如阿里云就会设置该模式

==为什么0和6这个两个模式会存在呢?(拓展知识)==

​	==原因是因为Linux是由Unix发展而来，而Unix最早是用于军事机密当中，产生上个世界的60年代中期(美苏冷战时期)，美国官方把Unix操作系统用于星球大战计划当中(导弹远程系统，其实这个导弹系统就会有一个自我保护和自动爆炸的功能。当中0这个模式就代表自动毁灭，6就代表自动爆炸功能，因此0和6是历史的遗留问题，最早是用于导弹系统当中。(出自于鸟哥Linux私房菜当中的介绍)，0和6其实今天被海豹特战队用于定时炸弹C4当中==

# 三、Linux子安全系统selinux

## 1、什么是selinux

![4](4.png)

​	selinux是为安全而生，但是由于selinux操作系统太安全所以只用于航天行空或者科技发射领域当中，对于php开发来说selinux是一个==bug==，原因selinux会阻碍ftp、composer；如果开启selinux使用，php操作sphinx、redis、memcache、mongdb会导致获取数据失败，因此我们需要把selinux关闭掉

## 2、分析selinux的配置文件

​	任何一台服务器如果系统进行php或者java的开发，就必须关闭centos的selinux，首先如果希望知道selinux如何关闭,其实我们可以看看阿里云的配置跟我们有什么不一样，于是我们可以使用vim打开selinux的配置文件(/etc/selinux/config)如下所示：

```
shell># vim /etc/selinux/config
```

![1541400699990](1541400699990.png)

## 3、关闭Selinux

### 3.1、使用vim修改配置

![1544751664474](1544751664474.png)

把SELINUX修改为disabled,表示关闭,保存并退出(:x)

> ==注意：==
>
> ​	==修改selinux必须重启,否则无法生效==

### 3.2、重启操作系统

![1541400889574](1541400889574.png)

# 四、用户组

## 1、用户组是什么

​	在Linux操作系统当中，Linux是一个多用户，多任务的操作系统，Linux为了方便的管理用户，所以Linux把对应的很多用户都进行归组，任何一个用户在Linux当中都必须存在于一个用户组当中，否则会导致很多软件无法正常的启动，比如mysql软件如果要启动，就必须要有一个mysql的用户和用户组，用户和用户组其实就很像一个公司的架构，如下所示：

![5](5.png)

​	在Linux操作系统当中，Linux把一切的东西都当成目录和文件来对待，那么用户和用户组有没有相关的文件呢？这个答案是肯定的，用户和用户组在Linux当中也被当成文件来对待。

## 2、与用户组相关的文件

用户组文件：/etc/group文件
用户组密码文件：/etc/gshadow文件（在Centos当中被红帽子公司忽略，因为对于开发来说这个文件没用，所以该文件了解就可以了）

## 3、分析用户组文件(/etc/group文件)

使用vim打开/etc/group文件，如下：

```
shell># vim /etc/group
```

![1541401794250](1541401794250.png)

打开后，内容如下所示：

![1541401830591](1541401830591.png)

用冒号(：)进行分隔，可以把上图分为4列：

第1列：组名称

第2列：x表示组密码（x是一个占位符，真实的密码存放在/etc/gshadow当中，但是在centos中无需关注，被弃用了）

==第3列：表示组的id(组id是唯一，冲突就发生错误)==

​	==如果组id=0表示该组的名称为root，为超级管理组，这个组不能随便改名，也不能修改id的值，否则linux无法正常启动，0-499表示Linux的默认系统创建的组，称为系统用户组，500或这500以上（>=500）的组id，我们称为自定义用户组，不过自定义用户组有可能是我们手动创建，也有可能是我们安装软件的时候，软件的开发者帮我们创建的==

第4列：组内用户（表示附属组成员，在centos中无需理解，因为使用不上）

## 4、用户组相关的指令

在Linux当中我们可以通过命令对用户组进行管理，主要可以管理用户组的增删查该功能

### 4.1、添加用户组

命令格式：groupadd [-选项] 组名称

命令作用：添加用户组，一般使用该命令添加的用户组id大于或者等于500

**案例1：添加一个名为php34的用户组**

![1544754013811](1544754013811.png)

**案例2：添加一个名为php32的用户组,id=520**

这时如果希望自定义用户组的id，那么其命令规则如下：

```
groupadd -g [id][组名]
```

![1544754168924](1544754168924.png)

运行后的结果如下：

![1544754316130](1544754316130.png)

没有出现错误，就代表自定义用户组成功，使用vim查看/etc/group效果如下：

![1541402407607](1541402407607.png)

### 4.2、搜索或者定位

命令格式：grep 关键字 [文件的名称或者路径]

命令作用：搜索或者定位指定的关键字内容

**案例1：查找/etc/group文件中id=1314的组信息**

![1544754547221](1544754547221.png)

**案例2：查找/etc/group文件中php520的组信息**

![1544754602585](1544754602585.png)

### 4.3、删除用户组

命令格式：groupdel 组名称

命令作用：删除一个用户组

**案例：删除一个名为itcast的用户组**

![1544754755022](1544754755022.png)

![1544754865721](1544754865721.png)

> 注意：
>
> ​	其实这个命令，只会在红帽子认证中考试的时候会使用，但是在现实开发中很少有人会删除用户组，因为删除用户组会导致用户失去所在组的依靠，导致某些软件无法正常启动，所以删除组需谨慎操作

### 4.4、修改用户组

命令格式：groupmod [-选项] 组名称

命令作用：修改用户组的信息

**案例1：修改php34为php33**

如果需要修改组的名称，我们的命令格式如下：

```
groupmod -n [新的名字][旧的名字]
```

![1544755291118](1544755291118.png)

**案例2：修改itcast的组Id=502**

如果需要修改组的id，我们的命令格式如下：

```
groupmod -g [id][组的名称]
```

![1544755524913](1544755524913.png)

> 注意：
>
> 如果修改的组id已经存在，那么linux就会在修改时报错，如下所示：
>
> ![1544755586986](1544755586986.png)

# 五、Linux的用户

## 1、与用户相关的文件

用户文件：/etc/passwd

用户的密码文件：/etc/shadow

## 2、分析/etc/passwd用户文件

使用vim打开/etc/passwd文件，如下所示：

![1541405172124](1541405172124.png)

打开后的内容如下：

![1541405205459](1541405205459.png)

以冒号( : )进行分隔，以上一共可以分为7列

第1列：用户名称

第2列：用户密码（使用x占位符，真实的密码存放在/etc/shadow文件当中）

第3列：用户id

第4列：用户所在的组id

第5列：用户备注名称（类似于微信的中备注名称，在Linux当中尽量不要修改备注名）

第6列：用户的宿主目录

==第7列：表示一个用户是否可以登录Linux操作系统==

==如果/sbin/nologin表示用户没有登录Linux权限==

==如果/bin/bash表示用户可以登录Linux操作系统==

`其实还有一个选项叫/sbin/halt或者/sbin/shutdown，这个其实是军事的遗留问题，一般开发用不上`

## 3、分析/etc/shadow文件（用户密码文件）

用户密码文件我们只需记住其中2列可以了，用vim打开/etc/shadow文件如下所示：

![1541405811575](1541405811575.png)

打开后的内容如下：

![1541405828383](1541405828383.png)

用冒号( : )进行分隔，记住第1和第2列就足够了

第1列：用户名称

第2列：用户密码（是使用军事上的md5散列算法，因为linux以安全著称，所以这个Md5的加密不同于php的md5加密，一般很难被破解）

​	==在密码的列中有时会出现!!的情况，这时表示当前用户没有密码，如果一个用户在linux中没有密码则无法登录Linux操作系统，但是可以被root用su命令进行随意的切换==

## 4、与用户相关的指令

### 4.1、显示用户的相关信息

命令格式：id 【存在的linux用户】

命令作用：显示用户的相关信息

**案例：显示root的相关信息**

![1544757672541](1544757672541.png)

### 4.2、显示用户的信息

命令格式：groups 【存在的linux用户】

命令作用：以键值对【用户名=>组名】来显示用户的信息

**案例：显示root的相关信息用键值的方式进行显示**

![1544757758035](1544757758035.png)

### 4.3、添加用户

命令格式：useradd [-选项] 用户名称

命令作用：自定义添加一个用户（普通用户）

例子1:添加一个用户叫：lisi

![1544757977142](1544757977142.png)

如果添加一个用户，默认会以用户名创建一个宿主目录在/home下，同时也会创建一个与用户名相同的组名称，如果下图所示：

![1544758056726](1544758056726.png)

例子2:添加一个用户osullivan，把osullivan的宿主目录设置/home/oshaliwen

如果希望在添加用户的时候为用户指定一个宿主目录,那么我们就需要使用以下命令格式
useradd -d [/home/宿主目录名称] 用户名

![1550028467777](1550028467777.png)

但是我们需要注意一个事项，当前 lisi 的这个用户没有密码，所以无法正常的登录Linux操作系统，所以添加一个用户，通常还要为该用户设置一个密码才能使得用户进入linux

![1544758331339](1544758331339.png)

这时切换用户，可以看到以下宿主目录信息

![1544758456693](1544758456693.png)

例子3：添加一个用户 long，把long归组到 lisi 当中

如果希望在添加用户的时候希望指定用户所在的用户组，那么我们就需要使用以下命令格式
useradd -g [存在的组名称] 用户名

![1544758693047](1544758693047.png)

![1544758828299](1544758828299.png)

### 4.4、删除用户

命令格式：userdel [-选项] 用户名

命令作用：删除一个用户

例子1：删除 long 这个用户

![1544758970905](1544758970905.png)

![1544759046399](1544759046399.png)

例子2:删除 osullivan 这个用户，同时删除 osullivan 的宿主目录

如果希望删除用户的宿主目录，就需要使用以下格式：

userdel -r 用户名

![1544759138635](1544759138635.png)

### 4.5、修改用户

```
命令格式:usermod[-选项][用户名称]
命令作用:修改用户的相关信息
```

例子：把用户 itcast 的修改为 dz,使用选项 -l，该选项用于修改用户的名称，格式如下:

usermod -l [新的名称] 用户名

![1544759427180](1544759427180.png)

> ==注意：==
>
> ​	==如果进行了用户名的修改，其实新的用户不会被修改所在的用户组，同时也会运用原有宿主目录，用户Id也不会发生改变，这个修改用户很可能会成为一道社会中的面试题出现，所以要重点注意这个问题==
>
> shell># groupmod -n dz itcast

## 5、禁止用户登录操作系统

​	有时我们在开发当中，我们需要维护计算机的硬盘或者修改配置文件的的时候，我们不希望某些用户登录到系统当中，那么我们就需要禁止用户登录，禁止用户登录Linux有以下两种模式：（1）禁止单个用户登录（2）禁止所有的普通用户登录。

### 5.1、禁止单个用户登录

​	不删除用户，也不修改用户的密码，目的是保证公司对一个用户的信任和保证一个用户的工作文件得以继续运行和保存，这时我们就需要单方面禁止用户的登录，比如说禁止用lh520登录操作

第1步：添加一个叫做lh520的用户,并且确保lh520可以正常的登录

![1542248217487](1542248217487.png)

第2步：通过修改/etc/passwd文件禁止lh520登录

使用vim打开/etc/passwd文件

![1542248434366](1542248434366.png)

使用 :/lh520 定位到 lh520 用户信息

![1544760075602](1544760075602.png)

发现 lh520 的第 7 列为 /bin/bash 表示可以登录操作系统,因此我们需要把 /bin/bash 修改为 /sbin/nologin，如下图所示：

![1544759858230](1544759858230.png)

禁止后使用xshell登陆提示以下内容：

![1544759980178](1544759980178.png)

保存后，切换到 lh520 用户时提示以下信息：

![1542248855637](1542248855637.png)

### 5.2、禁止多个用户登录

​	如果我们希望只有root用户可以登录Linux操作系统进行系统维护，其他普通用户都不可以进入Linux操作系统,这种情况我们就不可以一个一个用户进行修改，我们就需要批量禁止普通用户登录操作系统，那么这时我们需要怎么做呢?

​	这时我们的方法很简单,只需要在/etc目录底下创建一个叫nologin文件就可以禁止除了root以外所有的普通用户进入Linux操作系统,操作如下所示：

![1542250212839](1542250212839.png)

创建完成,注销root用户,看看普通用户是否都不能登录

```
shell># su lisi
```

![1542250320028](1542250320028.png)

​	发觉 /etc/nologin 文件生效了，所有用户都不能登录，除了 root 以外。当我们维护计算完成，那么就需要让所有普通用户都可以重新正常的登录Linux操作系统，我们就应该为所有普通用户进行解冻操作，那么这时又应该如何进行普通用户解冻操作?其实很简单，我们只需删除 /etc/nologin 文件就可以了

![1542250421593](1542250421593.png)

​	删除后，我们重新切换用户，看看普通用户是不是都恢复了正常的登录状态？发觉只要删除/etc/nologin文件，所有的普通用户被解冻了

# 六、文件操作的相关命令

## 1、删除文件或者目录

```
命令格式：rm [-选项][文件名称或者文件路径]
命令作用：删除文件或者目录
```

例子1：删除/root/php33下的a.php文件

![1550040832066](1550040832066.png)

例子2：删除/root/php33下的b.php文件,但无需提示直接删除

那么如果为了完成这个例子，我们就需要使用强制删除的手段加上-f选项，-f（force）
如下图所示:

![1550040917180](1550040917180.png)

例子3：删除/root/php32下所有的文件，但不删除目录

​	那么如果为了完成这个例子，我们就需要使用强制删除的手段加上-f选项，-f（force）
同时在-f后面加上*，rm -f *表示删除一个目录下所有的子文件，这是一个固定的linux命令组合，我们需要永远牢记

![1550041008811](1550041008811.png)

例子3：删除/root/php32下所有的文件，包括子目录和文件，以及子目录中子文件和子目录

这时如果为了达到目的，我们就需要使用递归删除的方法，使用 -rf 选项和*

所以格式如下:

rm -rf *

这也是一个固定的组合

![1550041279257](1550041279257.png)

## 2、删除的灾难性和Linux删除原理

rm -rf * 假设这条命令在根目录（/）执行，那么就是一个毁灭性的操作，所以这条语句等同于rm -rf /，这种删除是绝对不能执行了，为什么不能执行，因为Linux的删除是粉碎性删除（删除后无法进行数据恢复，因为linux是安全著称的系统，删除是很彻底），其原理如下 所示：

```
会删除文件的同时删除节点，因此没有节点，则无法恢复数据
```

> ==注意：==
>
> ​	==所以在公司当中切记不要随便在根目录下或者重要的目录下使用rm -rf *或者使用rm -rf /
> Linux删除是粉碎性的，但是它也 可以保证我们的重要机密文件不能被恢复。==

## 3、复制命令

```
命令格式：cp [-选项][源文件或目录][目标位置]
命令作用：复制文件或者目录
```

例子1：复制/root/php32下的a.php文件复制到cp的目录当中

![1550041718812](1550041718812.png)

例子2：复制/root/php32下的1.php文件复制到test的目录当中，那么会产生什么样的现象呢?

![1544770011010](1544770011010.png)

例子3：复制/root/php32下的1.php文件复制到cp的目录当中，并且把1.php文件修改为phpinfo.php?

![1544770111293](1544770111293.png)

例子4：复制/root/php32下test目录到itcast目录当中，包括test目录的子文件和子目录

如果需要复制的是一个目录，我们就需要使用-r进行递归复制,格式如下:

cp -r 目录 目标位置

![1544770370450](1544770370450.png)

## 4、移动/剪切文件

```
命令格式:mv [-选项][源文件或目录][目标位置]
命令作用:移动文件或者目录
```

例子1：移动php32/1.php到/root/itcast/long/当中

![1544770619650](1544770619650.png)

例子2：a.php移动/root/itcast/long/ldc.php

![1544770755963](1544770755963.png)

例子3：把/root/php32/test移动到/root/php32/itcast/long

![1544771013968](1544771013968.png)

# 七、Linux的网络配置

## 1、IP段的查找方式和ip地址选择

ip段其实是一个ip地址的前3位就是一个ip的段，同一个ip段的机器称为局域网机器。

IP最后那位可用只有：2~254

网关：192.168.82.1

比如现在有一个ip段位192.168.82.*，如果我们都处于这个ip段，我们俗称为62ip段，在这个段中的计算机都可以互相通信，同时也导致ip冲突的可能性发生，所以每一个计算机它的ip地址都是唯一的，所以如果你希望确定一个ip地址，首先你就需要确定ip段的同时，用以下方式去确定一个ip是否没有被别人使用：

### 1.1、在windows当中使用cmd打开命令运行窗口如下所示

### 1.2、在窗口中输入ipconfig

![1550043504853](1550043504853.png)

​	如果这ip地址是62那么就代表192.168.82.62这个ip地址是已经被您的本地所使用了，因此linux是不能使用该地址的，同时我们怎么知道其他地址是否可以使用呢？比如说，我们怎么知道ip地址为192.168.82.197是否可以使用呢？

### 1.3、使用ping命令，ping 192.168.82.197

结果如下所示

![1550043570074](1550043570074.png)

​	如果无法访问目标主机，就代表当前ip段中，没有一个使用197ip地址的计算机，因此这个地址可以作为我们的linux服务器ip地址的静态地址。如果一个ip地址可以被ping通就会出现一下的界面，同时也代表这个ip地址不可用：

![1550043612864](1550043612864.png)

## 2、在Linux当中使用静态Ip地址

### 2.1、查看IP地址

```
命令格式:ifconfig
命令作用:显示Linux当中的ip地址和网卡信息
```

![1550043669470](1550043669470.png)

eth0代表Linux的第1块网卡的名字

inet addr:192.168.82.197

inet6 addr:

表示Linux的ip地址,当前没有出现任何的ip代表当前Linux没有配置过任何的网络

### 2.2、Linux网卡配置信息文件分析

Linux网卡文件地址：/etc/sysconfig/network-scripts/ifcfg-eth0

使用vim打开该地址,如下所示：

```
shell># vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

![1544772426219](1544772426219.png)

![1544772197485](1544772197485.png)

内容如下所示：

![1544772213698](1544772213698.png)

第1行：网卡的名字

第2行：Linux的物理地址

第3行：Linux网络类型(代表使用因特网进行网络配置)，一般不做任何的修改，如果当前是飞机上的网络跟地面通讯，这个选项其实是SocketsEthernet

第4行：Linux的网卡文件唯一Id

==第5行：表示Linux启动的时候是否启动网卡配置信息，如果是no表示不启动，如果是yes表示启动，一般设置为yes==

第6行：表示Linux的网卡是否受到NFS的安全控制，一般都是设置为yes，表示当前网络处于NFS网络安全控制当中

==第7行：表示网卡ip地址的类型，dhcp表示当前使用的Ip地址是自动获取的，static表示使用当前的ip地址是静态,一般设置static作为静态地址方便xshell或者putty作为Linux连接管理==

​	但问题来了，以上这7行信息，不足以让Linux的网卡配置起效，如果希望网卡起效，我们还需要配置三个重要的选项：

- IPADDR表示网卡的IP地址  IPADDR=192.168.82.197
- NETMASK表示Linux子掩码地址  NETMASK=255.255.255.0
- NETWORK表示网关  NETWORK=192.168.82.1

因此我们可以把网卡信息的配置设置如下所示：

![1550044327642](1550044327642.png)

```
DEVICE=eth0
HWADDR=00:0C:29:95:91:11
TYPE=Ethernet
UUID=bd01a1b9-bea8-4479-8730-7036b160011f
ONBOOT=yes #将no修改成yes。Linux操作系统网卡默认是不启动
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.82.197	#IP地址
NETMASK=255.255.255.0	#子掩码
NETWORK=192.168.82.1	#网关
```

​	保存并退出(:wq)，这时其实我们修改网卡的配置信息，这时其实我们应该重启，但是不是重启Linux服务器，而是重启网卡的服务，在Linux当中网卡的服务器使用service network来表示，重启网卡使用命令：service network restart，运行结果如下所示：

![1550044376829](1550044376829.png)

出现以上界面，代表我们的网卡配置没有错误，代表ip地址成功设置为linux的静态地址
如果使用ifconfig查看Linux网卡信息,结果如下所示：

![1550044442706](1550044442706.png)

### 2.3、网卡的服务管理命令

命令格式如下：

```
shell># service network restart	 #重启网卡
shell># service network stop	 #停止网卡服务
shell># service network star	 #开启网卡服务
shell># service network status	 #查看网卡当前的状态
```

## 3、使用ssh远程工具管理Linux

### 3.1、什么是ssh

​	其实ssh是一种网络的管理协议，专门用于连接管理Linux服务器，它是众多网络协议的其中一种，网络协议有：http，https，ftp，telnet，socket。在ssh协议当中有一个重要的概念，ssh协议是一种==加密传输协议==，非常的安全，因此linux就采用了这种协议，这种协议的默认的端口为22。在世界上有很多专门用于管理Linux的ssh工具,这种工具主流有以下两种：putty与xshell

### 3.2、使用putty连接Linux服务器

![1550044920700](1550044920700.png)

![1550044950958](1550044950958.png)

连接成功后，如下图所示：

![1550044992417](1550044992417.png)

![1550045029364](1550045029364.png)

### 3.3、使用xshell来连接Linux服务器

​	xshell实际是功能非常齐全的并且对于个人和学生是免费的一款产品，安装时我们就是下一步下一步就可以完成安装，但是在安装的时候要注意选择家庭和学生免费版。安装完成,我们可以通过以下方式连接Linux

第1步：通过ssh协议进行连接,连接登录方式是==用户名@ip地址==

![1550045227840](1550045227840.png)

第2步：在弹出的界面中选择接受并保存，如果是免费版其实密码无法保存的

登陆成功，如下图所示：

![1550045311496](1550045311496.png)

> ==注意事项：==
>
> ​	如果我们使用版本有一些老公司，有可能使用centos5.5的，这时在centos5.5当中，其实Linux在默认的情况下是没有ssh协议的，这时如果我们遇到了这种情况，我们就需要启动ssh的协议服务，ssh服务的命令是固定，格式如下：service sshd
>
> service sshd start : 开启ssh协议
>
> service sshd restart : 重启ssh协议
>
> service sshd stop:停止(centos6默认开启,而centos5.5默认关闭)
>
> service sshd status:查看ssh的状态

如果ssh服务器没有开启，那么是不可能连接linux服务器的，如下图xshell所示：

![1550045562569](1550045562569.png)

# 八、Linux的防火墙机制

​	Linux默认的情况下是开启防火墙，那么如果开启了防火墙那么Linux会处于一个非常稳定和安全的状态当中，但是防火墙的默认开启会导致php无法链接数据库，也导致ftp无法链接服务器，那么这时我们就需要关闭防火墙并且把防火墙加入启动关闭脚本当中。

防火墙在Linux中也是一种服务，所以固定形式为service iptables [start | restart | stop | status]，如果想知道防火墙有没有开启，那么我们可以查看： service iptables status如下所示：

防火墙开启状态如下：

![1544778413249](1544778413249.png)

防火墙关闭的状态如下：

![1544778507862](1544778507862.png)

​	我们需要关闭防火墙，真正的服务器其实也是关闭了防火墙，但是这时我们的疑问是这样的；
关闭了防火墙那么岂不是Linux服务器很危险？在真正的服务器中（阿里云），其实有安全组的防火墙规则（免费使用的），因此我们可以在真正的服务器中，关闭了防火墙，安全组如下所示：

![1542268415999](1542268415999.png)

​	只要有安全组，就可以关闭iptables，但是我们学习的时候不可能存在安全组，因此我们直接关闭 vmware 中的 linux 的防火墙就可以了，因为我们学习的环境不存在所谓的黑客攻击行为，因此设置防火墙的关闭如下所示：

![1544778738147](1544778738147.png)

但是这时问题来了：如果现在我们重启Linux，防火墙又会重新的自动开启，如下图所示：

![1544778788808](1544778788808.png)

这样是不是很麻烦，因为每一次开机我们就需要手动关闭。

问题：我们可以把防火墙加入开机关闭脚本当中吗？

答：可以使用chkconfig命令。

命令作用：把服务设置开机关闭脚本或者启动脚本

命令格式：chkconfig 服务器名称 [on|off]

on：加入开机启动脚本

off：加入开机关闭脚本

如果希望把iptables服务加入开机关闭脚本那么步骤如下：

![1544778880125](1544778880125.png)

重启计算机，查看脚本结果是否生效,如果加入开机关闭脚本成功，那么iptables不会重新启动了

![1544778978871](1544778978871.png)

CentOS7使用firewalld打开关闭防火墙与端口

​	firewalld的基本使用
​	启动： systemctl start firewalld
​	关闭： systemctl stop firewalld
​	查看状态： systemctl status firewalld 
​	开机禁用  ： systemctl disable firewalld
​	开机启用  ： systemctl enable firewalld

# 九、Linux的文件和目录权限

## 1、Linux的文件类型

​	在Linux系统中其实林纳斯·托瓦兹最早只定义了5种类型，但在CentOS6.8中Red Hat公司把文件类型定义为6种，比原作者定义多了sock类型的文件：

![1542270496060](1542270496060.png)

其中红色标记的3种是开发当中最常见的三种文件类型

## 2、Linux中的权限简介

​	Linux是一个多用户多任务的操作系统而且它把一切都看成了目录和文件来对待，为了安全性Linux把这些文件和目录分配了权限。在现实开发当中，我们常常会听到运维工程师说一种这样的数字组合：777，666，755，644
这些数字不是密码，而是==Linux的权限==

## 3、Linux中的权限分类

r (read),可读权限，权重值4

w(write),可写权限，权重值2

x(execute),可执行权限，权重值1

==注意：没有权限用（-）横杠来表示，可以把没有权限的权重值当作0来对待==

## 4、Linux的权限表达式换算方法

原则：Linux权限换算的顺序必须遵循读写执行(rwx)的顺序进行换算，不遵循该顺序表达都是无效的权限换算表达式。这个概念其实初学时非常抽象，所以日常需要通过更多的例子进行换算才能熟练和理解。

例子1：666 的权限表达是怎么理解的呢？

6 = 4 + 2 + 0 = rw-

6 = 4 + 2 + 0 = rw-

6 = 4 + 2 + 0 = rw-

666 = rw-rw-rw-

例子2：777 的权限表达是怎么理解的呢？

7 = 4 + 2 + 1 = rwx

7 = 4 + 2 + 1 = rwx

7 = 4 + 2 + 1 = rwx

777 = rwxrwxrwx

例子3：755 的权限表达是怎么理解的呢？

7 = 4 + 2 + 1 = rwx

5 = 4 + 0 + 1 = r-x

5 =4+0+1 = r-x

755 = rwxr-xr-x

例子4：644 的权限表达是怎么理解的呢？

6 = 4+2+0=rw-

4 = 4+0+0=r--

4 = 4+0+0=r--

644 = rw-r--r--

例子4：r-xrwx-wx的权限表达是怎么理解的呢？

r-x = 4 + 0 + 1 = 5

rwx = 4+2+1 = 7

-wx = 0+2+1 = 3

例子5：rwwrxxwwx 的权限表达是怎么理解的呢？

==这时错误的权限表达==

例子6：684 的权限表达是怎么理解的呢？

==这时错误的权限表达==

例子7：520 的权限表达是怎么理解的呢？

520 = r-x-w----

520权限虽然是正确但是运用极少

## 5、Linux中的权限表达公式

Linux权限表达的公式一定是10个字符组成,格式为：==文件类型 + 权限表达式==

例子1：如果我们希望在linux中表达a.php的权限为777

分析a.php是一个什么文件的类型，a.php是一个普通文件，所以用-

777可以换算为：rwxrwxrwx

所有a.php表示 -rwxrwxrwx

例子2：如果我们希望在linux中表达/home/zhangsan/test的权限为644

/home/zhangsan/test是一个目录，所以其类型类型为d

644可以换算为：rw-r--r--

/home/zhangsan/test = drw-r--r--

例子3：如果我们希望在linux中表达/etc/grub.conf的权限为755

/etc/grub.conf是一个链接文件，其类型l

755可以换算为：rwxr-xr-x

/etc/grub.conf = lrwxr-xr-x

## 6、在Linux当中查看文件的权限类型

在Linux当中如果我们输入ls -lh就可以以人性化的方式查看文件或者目录的相关信息，如下所示：

![1542331672751](1542331672751.png)

按照空隔来进行分隔，那么以上可以分为7列

![1542331749532](1542331749532.png)

==第1列（drwxr-xr-x）：表示Linux文件或者目录类型的权限，共有10位组成功，后面的.不算是一个字符，在此案例中表示文件的类型是目录权限为755==

==该列有1个特点，它一定是由10个字符组成==

==第1个字符：表示文件的类型==

==第2-4个字符：表示拥有者的权限是什么，本案例表示rwx=7，表示root用户可读可写可执行==

==第5-7个字符：表示所属组的权限是什么，本案例表示r-x=5,表示在root用户组当中的用户拥有可读可执行权限，但没有可写的权限==

==第8-10个字符：表示其他的人的权限是什么，本案例中表示r-x=5,表示既不是拥有者，也不是所属组中组内用户那么就是其他人，他们权限就是可读可执行，比如如果lisi属于php32这个用户组，那么lisi就是其他人，其权限就是可读可执行，不可写==

第2列(4)：表示Linux中的硬链接数（调用次数），不过centos当中这个数字是不准确，所以一般人不关心

第3列（root）：表示文件或者目录的默认拥有者是谁，在此案例中表示图片目录的拥有者是root，在linux中默认谁创建了一个文件或者一个目录，谁就是这个目录或者文件的第1个拥有者。不过拥有者是可以改变的

第4列（root）：表示当前文件或者目录所在的默认用户组是哪个一个，在此案例中图片目录处于root用户组当中。用户组也是可以改变的。

第5列（4.0K）：表示文件或者目录的大小，不过在Linux中这个大小不一定是非常准确的，所以只有一定的参考价值，不能过分的迷信它的大小字节

第6列(11月  15 19:34)：文件或者目录的创建时间或者修改时间

第7列（test）：表示文件或者目录的名称，本案例表示目录的名称是图片

# 十、总结

防火墙
	service	iptables status	查看防火墙
	service iptables stop 停用防火墙
	service iptables start 启用防火墙
	service iptables restart 重启防火墙
	加入开机自动关闭防火墙
	chkconfig iptables off

复制文件
	cp 文件名 目标路径
	例如：把 /root/php33/a.php 文件复制到 /root
	cp /root/php33/a.php /root
	例如：把 /root/php33/目录下所有文件复制到 /root/php
	cp -r /root/php33/* /root/php

删除文件
	rm 文件名称
	例如：删除 /root/php33/a.php
	rm /root/php33/a.php 提示用户是否确认删除

	例如：删除 /root/php33/b.php，不需要提示确认信息
	rm -f /root/php33/b.php
	
	例如：删除 /root/php33目录下所有文件，不需要提示确认信息
	rm -rf /root/php33

移动文件/剪切
	mv 文件名 目标路径
	例如：把 /root/php33目录下的c.php移动到，/root/php32
	mv /root/php33/c.php /root/php32


用户组
	添加用户组：groupadd 组名
	删除用户组：groupdel 组名
	修改用户组：groupmod -n 新组名 原先组名

用户
	添加用户：useradd 用户名
	删除用户：userdel 用户名
	修改用户：usermod -l 新的用户名 原先用户名

查看当前用户信息
	id [用户名]

网络配置
	查看ip：ifconfig
	vim /etc/sysconfig/network-script/ifcfg-eth0

管道指令
	例如：查找一个用户名叫做 php34
	grep 需要搜索内容 文件
	grep php34 /etc/passwd

	例如：查找php34的用户组是否存在
	grep php34 /etc/group