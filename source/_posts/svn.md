---
title: svn
date: 2019-02-20 16:50:16
tags: svn
---


# 一、svn代码版本管理控制工具

## 1、为什么需要svn



### 传统团队开发项目



![img](/wps63DD.tmp.jpg)

上面传统团队开发的流程会产生哪些问题？

- 代码冲突问题。因为多个组员可能修改同一个文件，造成同一个文件相同行代码进行整合的时候会产生冲突。

- 代码丢失问题。某个组员误删了文件或某段代码，找不回来了。

- 代码==共享==问题。代码传来传去太麻烦。

 <!-- more -->


采用svn代码版本控制工具就可以轻松解决上面产生的问题。

### 现在企业团队开发项目





![现在团队开发](/wps4FF7.tmp.jpg) 



可见，现在都是把项目代码统一放在svn服务器中，张三需要上传代码直接提交即可，李四要获取代码直接从svn服务器更新即可。



##  2、SVN介绍

SVN全名Subversion，即版本控制系统（主要可以追溯以前的代码版本）,Subversion是一个通用的系统,可用来管理任何类型的文件,其中最主要文件是==程序源码==,因为源码文件经常变动，有利于代码版本的追溯（回退）。

SVN将源码存放在一个中央仓库(repository)中,这个仓库有个特点，它会**记住文件的每一次变动**,利用这种特性，我们可以回退到之前的任何一个代码版本。

![1535632679899](/1535632679899.png)





除开主流的svn当然还有其他的版本控制软件：

- CSV: 全称-Concurrent Versions System,与SVN是同一个厂家，不过是svn的前身,现已被svn替代。

- VSS: 全称-Visual Source Safe,此工具是Microsoft公司提供的。

- ==git==: 全世界最有名的代码版本工具，与其相关的网站是[github](https://github.com/)，发明者是Liunx之父李纳斯，最开始主要是用于管理linux内核代码而开发。


也有些第三方知名的代码托管网站：

- 国外大名鼎鼎的[github](https://github.com)网站，基本上所有的开源项目源码在这里都可以找到，全世界开发者的源码也都被此网站管理，同时github也被开玩笑称为最大的同性交友网站

- 国内有[码云](https://coding.net/),还有[码市](https://coding.net/)  

- 学完linux之后再看：搭建属于自己的git服务器： https://www.jianshu.com/p/e79ea05d9b61 

  

# 二、SVN软件的安装

svn是c/s（client/server）架构的软件，有服务端和客户端。

服务端下载地址:  <https://www.visualsvn.com/visualsvn/download/>  

客户端下载地址： <https://tortoisesvn.net/downloads.html>  





## 1、安装svn服务端

服务端提供两个版本：

- **Setup-Subversion-1.6.5.msi** 
- **VisualSVN-Server-2.7.7.msi**

相应软件已经在课件资料中，推荐安装**VisualSVN-Server-2.7.7.msi**,如果安装失败或老出错，可以选择安装**Setup-Subversion-1.6.5.msi**版本的。



开始安装，安装**VisualSVN-Server-2.7.7.msi**服务端步骤如下：

1. 双击安装程序，点击next下一步

![img](/wpsE28F.tmp.jpg) 

2. 接受软件的相关协议，点击next。

![img](/wpsE290.tmp.jpg) 

3. 选择勾选最后一项，配置svn服务端的相关的命令到系统的环境变量，点击next下一步

![img](/wpsE291.tmp.jpg) 

4. 选择标准版Standard Edition。

![img](/wpsE292.tmp.jpg) 

5. 选择一个安装目录，这里以安装到D:\svn\server为例

![img](/wpsE293.tmp.jpg) 

注：安装若出现以下错误，则重新安装选择上面非443端口即可，端口号输入1024-65535之间都可以，不要设置和系统端口冲突即可，如mysql的3306。

![img](/wpsE294.tmp.jpg) 

 

6. 点击finish完成

![img](/wpsE2A5.tmp.jpg) 

 

7. 查看安装的svn服务端环境变量是否生效，直接在cmd命令窗口中出输入svn命令，若出现以下提示说明环境生效。 

![img](/wpsE2A6.tmp.jpg) 

若提示**不是内部或外部命令则**未生效。



如果安装的时候勾选了环境变量，但依然没有出现以上正确提示，有以下两种解决办法：

- 重启电脑
- (推荐)找到环境变量path选项，直接点击确定即可，不需要修改，这样可以==立即生效==

![img](/wpsE2A7.tmp.jpg) 

重新打开新的cmd窗口，再次输入svn命令进行测试即可。



## 2、安装svn客户端

安装客户端的时候，先检查自己电脑系统的位数，选择对应系统位数安装即可。

查看系统位数：我的电脑->右键属性



客户端有两个文件：

- TortoiseSVN-1.8.7.25475-x64-svn-1.8.9.msi  ==主文件==
- LanguagePack_1.8.7.25475-x64-zh_CN.msi  ==汉化包==



先安装主文件TortoiseSVN-1.8.7.25475-x64-svn-1.8.9.msi，步骤如下，

1. 双击安装程序，点击next下一步

![img](/wps48C8.tmp.jpg) 

2. 接受软件的相关协议，点击next。

![img](/wps48D8.tmp.jpg) 

3. 选择选项command line client tools-Will be installed on local hard driver

![img](/wps48D9.tmp.jpg) 

4. 点击第一个选项TortoiseSVN，选择安装的目录，目录可自定义，这里以安装到D:\svn\client\中为例，点击next。

![img](/wps48DA.tmp.jpg) 

注：路径尽量不要包含特殊字符和中文。

 

5. 安装完成，点击finish.。

![img](/wps48DB.tmp.jpg) 

6. 测试是否安装成功，在桌面鼠标右键，多出以下两个选项说明安装成功：

![img](/wps48DC.tmp.jpg) 

 

7. 如需要汉化可安装系统对应位数的汉化包即可：

![img](/wps48DD.tmp.jpg) 

安装一路next即可，会自动找到上面svn客户端的安装位置：

切换中文步骤：TortoiseSVN-->Settings-->General-->Language

![img](/wps48DE.tmp.jpg) 

![img](/wps48EF.tmp.jpg) 

再次查看变成中文效果：

![img](/wps48F0.tmp.jpg) 

# 三、部署svn单仓库

注意:在svn服务器中，我们把存放代码的地方称之为 ==仓库==。而不是称为项目文件夹。

## 1、部署单仓部的步骤

在svn中创建单仓库的步骤总共有三步：

**步骤1**：建立项目文件夹 。如我这里建立在D:/app/blog 

![img](/wps3B0E.tmp.jpg) 

**步骤2**：把上面项目目录变为仓库。 

命令为： ==svnadmin create  D:/app/blog==（项目目录绝对路径）

![img](/wps3B0F.tmp.jpg) 

 ![1544185567671](/1544185567671.png)

**步骤3**：开启监管仓库的服务。

命令为：==svnserve  -d  -r  D:/app/blog== 

选项说明

-d：作为后台服务运行。

-r：监管的目录。

![1535635030048](/1535635030048.png)



> ==注==：上面黑窗口正在监管仓库服务，不要关闭它，否则导致后面连接不上。



## 2、svn中三大指令的使用

- checkout ：检出指令，用于==首次==与svn服务器建立连接，获取代码到本地目录（第二次以后直接使用update更新指令获取代码。）
- commit：提交本地代码到svn仓库
- update：把svn仓库上最新代码更新到本地

 

三大指令使用图解：

 

![img](/wpsC806.tmp.jpg) 

 

> 注意：
>
> 以后去公司上班，公司基本早就已经部署好svn服务器了，我们开发者只需要在自己的电脑中安装svn客户端即可，公司的相关人员会告诉我们svn仓库地址，且会给我们分配一个账号和密码进行连接svn服务器，然后checkout检出代码在本地进行开发。写完代码后使用commit进行提交。第二天上班直接使用update指令获取别人提交的最新代码下来进行开发。





## 3、连接svn服务器

**步骤1**：如在我们桌面中，建立一个项目检出目录myblog，用于存放检出的代码：

![img](/wps8946.tmp.jpg) 

检出和版本库浏览器的区别：

检出checkout：是把仓库所有的代码都检出到本地目录。

版本库浏览器：它可以浏览仓库中有哪些文件，有选择性的检出指定文件。

 

**步骤2**：输入我们svn服务器的地址，指定一个检出代码的本地目录

![img](/wps8947.tmp.jpg) 

检出成功之后，会在本地检出目录多出一个.svn的隐藏文件夹，同时版本为0， 说明是一个新建立的仓库，之前没有任何的commit提交。

![img](/wps8948.tmp.jpg) 

## 4、上传代码到svn仓库中

**步骤1**：在检出的目录中，建立一些测试文件或目录，鼠标右键选择commit指令进行提交

![img](/wps84A0.tmp.jpg) 

**步骤2**：【重要】填写提交的备注，主要是用于后面代码的版本回退（代码找回）

![img](/wps84A1.tmp.jpg) 

点击确定之后，提示无权限提交，如下：

![img](/wps84A2.tmp.jpg) 

因为默认新创建的仓库，只能更新或检出，但是不能commit提交,后面需要修改此仓库的配置文件，进行权限配置。





## 5、单仓库的权限配置

权限可以细分两种：

- 匿名用户提交【了解】（不需要用户名和密码也可以提交，但是不安全，个人测试使用较多）
- 【==重点掌握==】允许授权用户提交（需要用户名和密码，安全一点，企业中使用都是需要密码账号的）

### （1）匿名用户提交

每个仓库的conf目录中，都会有以下三个配置文件：

- `svnserve.conf`：当前仓库的核心配置文件，可以开启某些文件的功能   
- `passwd`：给当前仓库的增加用户名和密码
- `authz`：给当前仓库的用户设置一些权限

> 权限w：write可更新可提交 ,
>
> 权限r：read 可更新不可提交
>
> 提示：后面学的linux中还有一种权限为x,代表可执行

 

设置匿名用户访问，仅需修改`conf/svnserve.conf`配置文件即可，把其中`anno-access = read`前面

的注释`#`号给去掉，把read改为write，如下：

```powershell
anon-access = write
```

>  注：最前面要顶格写，即不要留空格。只要保存配置文件会立刻生效，不需要重新监管仓库服务。



保存后在`检出目录`进行commit提交测试：成功会提示`版本1`，说明此仓库成功提交过1次。

若修改文件再次提交就会产生`版本2`，以此类推。

即svn仓库中，保留了2个代码版本。

 

### （2）[**重点**]授权用户的权限配置

授权即需要用户名和密码才可以进行代码的操作

 

需要修改仓库conf目录中的三个配置文件：svnserve.conf、passwd、authz

**步骤1**：修改`svnserve.conf`配置文件，修改以下4处配置

```powershell
anon-access = none	 	#设置为none,禁止匿名用户访问
auth-access = write  	#设置为write,允许授权用户可更新也可提交
password-db = passwd 	#代表开启passwd配置文件
authz-db = authz 	    #代表开启authz配置文件
```

 

**步骤**2：修改`passwd`文件，给当前仓库增加一些用户名和密码

```powershell
[users]
dashen = dashen123
cainiao = cainiao123
admin = admin123
```

格式： `用户名 = 密码（明文）`

> 提醒：以后在Linux中修改含有敏感的词汇，如密码，需要给这类文件设置严格的访问权限。这样更加安全。



**步骤3**：修改`authz`文件，设置组内用户权限，及给当前仓库的用户名分配一些访问权限（rw）

设置组及组内用户：

```powershell
[groups]
# 组名 = 组成员（多个用逗号隔开）
php_group = dashen 
cainiao_group = cainiao
```

分配权限：

```powershell
 
# 权限语法格式：
# [/目录/子目录/子孙目录/....]
# @组名 = 权限rw
# 用户名 = 权限rw
# * = （其他人没有权限访问）

# 对此单仓库的所有文件进行权限控制
[/] 
@php_group = rw
@cainiao_group = rw
admin = rw
* =

# 对此单仓库的core目录下面的文件进行权限控制
[/core]
@php_group = rw 
@cainiao_group = rw
admin = rw
* =

```

==注意==：上面目录core后面不可以加斜杠/。

再次进行commit提交，若出现一个输入密码的弹窗，说明配置授权用户访问成功：

![img](/wps61DE.tmp.jpg) 

==注意==：

- 如果没有w权限，提交的时候会提示==Access Denies==权限拒绝。 
- 权限有r但未有w,只能检出代码但不能提交。  



 

 

# 四、**svn常见文件状态图标说明**

## 1、【了解】图标异常(没有出现)的解决办法

- win7解决办法：

底部启动任务管理器-->进程-->找到进程名explorer.exe--==>鼠标右键结束==，然后再找到文件选项新建刚才结束的进程explorer.exe,如下图所示：

![img](/wpsB102.tmp.jpg) 

 

- win8，win10解决办法：

修改如下注册表：按win+r,输入regedit

![img](/wpsB103.tmp.jpg) 

需要修改注册表中的某些选项，参考下面的图片：

![img](/wpsB104.tmp.jpg) 

找到对应的注册表的值（Tortoise字符打头的就是svn相关图标），==在前面加多空格，空格越多优先级越高==：

![img](/wpsB105.tmp.jpg) 

最后在底部任务栏管理器选中window资源管理器-->鼠标右键重新启动即可,再去看检出目录就可以看到图标了。



>  ==注意==：这个svn图标不是特别重要，没有出现也没有关系，因为操作系统中的某些程序图标优先级更大，导致svn图标显示不出，svn图标仅为了可以看到文件的状态，因为提commit交代码的时候，在文件变更列表中也同样是可以看到的。

 

## **2、常见图标的认识**

- 常规(同步)图标：

![img](/wpsB106.tmp.jpg) 

出现此图标，说明此文件中的内容与svn服务器中对应文件内容一样。

 

- 修改图标：

![img](/wpsB107.tmp.jpg) 

当我们对本地的某个文件进行修改，导致与svn服务器上面的对应的文件的内容不一致就会出现此图标。

 

- 无版本控制图标

![img](/wpsB117.tmp.jpg) 

说明此文件没有与svn服务器建立关联，说明没有被仓库管理过

 

- 忽略图标

![img](/wpsB118.tmp.jpg) 

当我们有些文件不想提交到svn服务器或者没有必要提交，我们可以把这些文件==设置为忽略==即可，那么提交的时候这些文件就不会出现在提交的文件变更列表中，从而也不会提交到svn服务器中

忽略一般有两种：

- 忽略一个具体的文件：1.jpg
- 忽略某一类的文件：*.jpg

 

具体操作：选中要忽略的文件鼠标右键-->TortoiseSVN-->增加到忽略列表。如下：

![img](/wpsB119.tmp.jpg) 

 

其中带recursively是递归的意思，多用于忽略目录中及其子目录中的所有的文件。

 

- 冲突的图标

![img](/wpsB11A.tmp.jpg) 

尤其是团队开发的时候，多个开发人员对同一个文件的==相同行代码都进行了修改==，那么后者提交的会覆盖前者提交的，但是svn不会进行覆盖，会提示我们解决冲突，把具有冲突的文件更新下来就是上面的黄色图标。

 

# 五、【重点】svn代码冲突的解决

>  当团队开发的时候，多个开发人员对同一个文件的相同行代码都进行了修改，那么后者提交的会失败，svn会会提示我们解决冲突。

 

下面就来演示造成冲突的场景以及怎么解决

## 1、造成冲突的场景

这里以dashen用户和cainiao用户对blog仓库的index.php的文件进行操作。

1. 首先，刚开始两者都要把代码给checkout进行检出下来，此时，两者的index.php文件的代码应该是完全一致的。

```php
<?php 
echo 'hello world';
```

2. 先是cainiao用户对index.php文件进行修改：增加3-5行`cainiao write`代码,并成功commit提交，如下：

```php
<?php 
echo 'hello world';
echo 'cainiao write';
echo 'cainiao write';
echo 'cainiao write';
```

上面修改成功提交之后，即svn仓库中index.php的文件的最新内容是cainiao用户提交的。

3. 随后dashen用户对index.php也进行了操作，增加3-7行`dashen write`代码，如下 :

```php
<?php 
echo 'hello world';
echo 'dashen write';
echo 'dashen write';
echo 'dashen write';
echo 'dashen write';
echo 'dashen write';
```

现在dashen用户准备commit提交代码会出现如下的提示：

![1535642971074](/1535642971074.png)



出现上面的提示，说明提交的index.php文件的内容与svn仓库中的index.php文件内容有`冲突部分`。




产生冲突的原因：

cainiao用户：修改3-5行    

dashen用户：修改3-7行 

其中3-5都被两者修改过，导致仓库无法知道使用哪部分代码，所以后者提交失败，报冲突。

## 2、解决冲突

**步骤1**：鼠标选中有冲突的文件，鼠标右键选择更新，

![img](/wpsB12F.tmp.jpg) 

![img](/wpsB130.tmp.jpg) 

 

可见，更新下来会多出`三个辅助文件`：

- 文件名(黄色标识)：把服务器上面最新文件内容与即将提交的冲突内容进行一个融合。
- 文件名.mine: 当前用户即将要提交的文件内容。
- 文件名.r(前版本号)：所有用户提交之前，即文件冲突之前，仓库上最新的文件内容。
- 文件名.r(后版本号)：仓库上最新的文件内容。

 

==解决冲突办法==：

把三个辅助文件都给删除，修改含有黄色感叹号的文件，进行代码整合修改，再次进行提交即可。


> 注：整合冲突代码的时候，千万不要覆盖人家代码，合并之前需要程序员之间彼此商量一下。

 

# 六、[重点]svn中的版本回退(后悔药)

>  使用到版本回退的场景：
>
> 一般由于不小心误删文件或某段代码，或者想回到之前的功能版本代码，这时候就可以通过svn提供的更新至版本功能来找回。



**步骤1**：选中要回退的文件，更新至版本

![img](/wpsB131.tmp.jpg) 



在通过之前提交的日志来找回：

![img](/wpsB132.tmp.jpg) 


**步骤2** ：找到之前提交的日志信息，点击确定，即可回到之前的代码版本。

![img](/wpsB133.tmp.jpg) 

 

> 注意事项
>
> 1、在检出目录中，鼠标右键点击更新，是更新最新的版本，如果需要回退到指定的版本，需要通过更新至版本来实现。
>
> 2、如要要查看某个文件的版本，右键选中此文件更新至版本即可，可以查看当前文件所有的版本变化。 
>
> 3、若点击空白处，可以查看当前项目所有代码的版本



# 七、svn中部署多仓库

> 引入

因为一个公司会有多个项目同时进行，比如有java项目，android项目、php项目。其实也就对应着多个仓库，那么不同的开发人员提交的仓库也就不一样。所以我们有必要为不同的项目分别创建一个仓库，便于项目的后期管理，也利于具体开发人员的权限分配。



>  实现步骤：

**步骤1**：如在app目录中建立多个项目文件夹（android、java）

![img](/wpsB143.tmp.jpg) 

**步骤2**：把上面的android和java目录变为项目仓库。

![img](/wpsB144.tmp.jpg) 

注意：blog之前已经是仓库了，不要重复创建。

 

**步骤**3：监管所有仓库的==父目录==（D:/app）,这样才可以访问到其中的某个仓库项目代码

![img](/wpsB145.tmp.jpg) 



> 问题？怎么访问多仓库中的某个仓库代码？

答： svn://ip/仓库名/目录/子孙目录/....

如访问blog仓库，地址为：svn://127.0.0.1/blog/

如访问java仓库，地址为： svn://127.0.0.1/java/

如访问android仓库，地址为： svn://127.0.0.1/android/

 

如访问其中blog仓库项目：

![img](/wpsB146.tmp.jpg) 

 

> ==注==： svn://127.0.0.1/目录/子孙目录/... 这种url访问形式只针对单仓库有效，多仓库的访问ip后面需加仓库名。

# 八、多仓库的权限设置

由于这里有三个仓库，这里以其中一个仓库blog为例，进行权限设置。

还是修改三个文件：svnserve.conf 、passwd、authz 

svnserve.conf配置和passwd配置和之前单仓库配置一样，不用变。  


仅需修改authz配置文件权限部分即可：

```powershell
# 格式：
# [/目录/子目录/子子孙孙目录/....]
# @组名 = 权限rw
# 用户名 = 权限rw
# * = （其他人没有权限访问）

# 对此多仓库的blog仓库的所有文件进行权限控制
[blog:/] 
@php_group = rw
@cainiao_group = rw
admin = rw
* =
# 对此多仓库的blog仓库的core目录下面的文件进行权限控制
[blog:/core]
@php_group = rw 
@cainiao_group = rw
admin = rw
* =
```



单仓库和多仓库的authz权限文件权限配置区别：

- 单仓库：[/目录/子目录/子孙目录/.....] 
- 多仓库：[当前仓库名:/目录/子目录/子孙目录/.....]

# 九、SVN其他功能

## 1、清除用户名和密码

做法：鼠标右键TortoiseSVN-->Settings-->已保存数据-->清除全部-->确定

![img](/wpsB148.tmp.jpg) 

## 2、export导出指令

export指令：相当于拷贝项目,导出的代码不会含有隐藏文件.svn，即不受svn版本控制

![img](/wpsB159.tmp.jpg) 

导出来如下所示：

![img](/wpsB15A.tmp.jpg) 

## 3、更改svn服务器地址

步骤：TortoiseSVN-->重新定位-->输入新的仓库地址即可

![img](/wpsB15B.tmp.jpg) 

重新输入新的仓库地址即可：

![img](/wpsB15C.tmp.jpg) 

# 十、svn监管服务注册成window系统服务

## 1、创建SVN监管仓库服务

快捷键win+r，输入cmd,以管理员的方式执行以下命令:  

```powershell
sc create SVNService binpath= "D:\svn\server\bin\svnserve.exe --service -r D:\app" start= auto
```



> 特别注意:

==binpath=后面有个空格 start=后面有个空格（只能有一个空格）==，其中SVNService 是服务的名称，此服务名称可以自己自定义，只要不和系统其它服务名重名即可。

![img](/wpsB15D.tmp.jpg) 

![img](/wpsB15E.tmp.jpg) 

 

注册成功之后会在系统服务面板中出现对应的服务。

底部任务栏-->任务管理器-->服务-->找到所注册的服务


![img](/wpsB15F.tmp.jpg) 



![img](/wpsB160.tmp.jpg) 

==设置为服务自动启动==：选择服务名鼠标右键属性选择自动即可


![img](/wpsB171.tmp.jpg) 

 

>  ==注==：如果是以之前cmd窗口的形式监管仓库服务，就不可以再使用window服务的形式进行监管，两者只能有一个在监管仓库服务。（一山不容二虎）

## 2、服务相关控制指令

关闭、开启、重启服务:

```powershell
net   stop|start|restart   服务名 
```

如开启svn服务：

```powershell
net  start  SVNService
```



![img](/wpsB172.tmp.jpg) 

删除服务:

```powershell
sc  delete  服务名	
```

如删除svn服务：

```powershell
sc  delete  SVNService
```

![img](/wpsB173.tmp.jpg) 

> 注：删除服务成功之后，若再创建同名的服务会提示此服务已标记删除，换一个服务名重新创建就可。

## 3、cmd命令的批处理

我们可以把之前在cmd中写的命令直接写在==后缀名为bat==的文件中，然后以管理员方式运行bat文件就相当于在cmd命令行中运行命令是一样的，这样操作起来更加方便。 



![img](/wpsB174.tmp.jpg) 

如其中关闭svn服务的批处理文件stop_svn.bat的内容如下：

```powershell
net stop svnService
```

开启svn服务的start_svn.bat内容如下：

```powershell
net start svnService
```





# 十一、svn中的钩子程序

## 1、钩子介绍

抽象介绍(鸟语)：所谓钩子就是与一些版本库事件触发的程序，例如新修订版本的创建，或是未版本化属性的修改。每个钩子都会被告知足够多的信息，包括那是什么事件，所操作的对象，和触发事件的用户名。通过钩子的输出或返回状态，钩子程序能让工作继续、停止或是以某种方式挂起。 

  


说的简单点，我们可以利用钩子在提交前或者是提交后做一些操作。如:

- 利用**提交前的钩子**让用户在提交代码前强制用户必须填写备注信息(了解)。

- 利用**提交后的钩子**把svn仓库代码实时同步部署到网站web目录（==重点掌握，开发中使用较多==）

 

前钩子和后钩子触发的顺序图解：

![img](/wpsB177.tmp.jpg) 

钩子的种类：


每个仓库目录中都会有个hooks目录，其中包含了所有的钩子模板代码。 

其中使用最多的有两个钩子：提交前的钩子、提交后的钩子（重点，开发中使用较多）

![img](/wpsB187.tmp.jpg) 



## 2、钩子实战

### （1）【**重点**】后钩子实时同步仓库代码到web站点

 利用**提交后的钩子post-commit**把svn仓库代码实时同步到网站web目录



blog仓库的检出目录：含有.svn关联文件即可。



检出目录：C:\Users\Administrator\Desktop\blog

web站点目录：F:\local.com\myblog。




**步骤1**：先确保web站点目录与相应的仓库要建立关联，只要含有==其仓库关联的.svn隐藏文件夹==即可。在web站点目录checkout检出就有了



**步骤2**：把blog仓库中的hooks的post-commit.tmpl复制一份在当前目录，并改名为post-commit.bat


![img](/wpsB188.tmp.jpg) 

post-commit.bat内容为：

```bash
SET SVN="D:\svn\sever\bin\svn.exe"
SET DIR="D:\local.com\blog"
SVN update %DIR% --username  dashen --password dashen123
```

> 注意：要确保上面设置的用户有写入的权限。



实时同步效果：

![img](/wpsB189.tmp.jpg) 





### （2）[了解]前钩子强制用户填写提交时备注信息

利用**提交前的钩子**让用户在提交代码前强制用户必须填写备注信息。

**步骤1**：打开blog仓库的hooks目录，把pre-commit.tmpl文件复制一份在当前目录，改名为pre-commit.bat,

![img](/wpsB18A.tmp.jpg) 

pre-commit.bat内容如下：

![img](/wpsB18B.tmp.jpg) 

>  ==注==：
>
>  如需要中文提示，为防止乱码需要把pre-commit.bat文件的编码改为ANSI。英文提示则不会乱码
>
>  使用echo将提示信息返回给客户端，在window平台下，必须使用 1>&2 作为结尾
>
>  exit 0 表示结果正确,  exit 1 或者是非零数值，表示结果错误，svn只将错误信息返回给客户端





作业：组长安装svn服务端，给组员设置访问权限，进行代码冲突和版本回退的演示操作。（需要关闭防火墙）

 

 

# 小结（用xmind总结）

- 创建单仓库（3个步骤）：

  1. 创建项目目录

     如：`D:/app/blog`

  2. 把项目目录变为仓库 

  ```powershell
  svnadmin create D:/app/blog
  ```

  3. 监管仓库服务

  ```powershell
  svnserve -d -r D:/app/blog
  ```

- 访问单仓库项目：

  ```powershell
  svn://ip/目录/子孙目录...
  ```

- 单仓库的权限配置

  1. 修改svnserve.conf

  ```powershell
  anon-access = none	#禁止匿名用户访问
  auth-access = write #允许授权用户访问（可更新可提交）
  password-db = passwd 	#代表开启passwd配置文件
  authz-db = authz 	    #代表开启authz配置文件
  
  ```

  2. 修改passwd文件，添加用户名和密码

  ```powershell
  [users]
  # 用户名 = 密码
  dashen = dashen123
  cainiao = cainiao123
  admin = admin123
  ```

  3. 修改authz文件，分配权限

  ```powershell
  [/]	#对单仓库内的所有的文件进行权限控制
  @php_group = rw
  @cainiao_group = rw
  admin = rw
  * =
  
  [/core] #对单仓库内core目录中的所有的文件进行权限控制
  @php_group = rw
  @cainiao_group = rw
  admin = rw
  * =
  ```

  

- 创建多仓库（3个步骤）：

  1. 创建多个项目目录

  如： `D:/app/android   D:/app/java`

  2. 把项目目录变为仓库 

     ```powershell
     svnadmin create D:/app/blog
     svnadmin create D:/app/java
     ```

  3. 监管仓库的==父目录==

     ```powershell
     svnserve -d -r D:/app
     ```
     访问：

     svn://ip/仓库名/目录/子孙目录

- 多仓库的权限配置（authz）：

  svnserve.conf和passwd配置一样，只需修改authz权限文件即可，

```powershell
[blog:/]
@php_group = rw
@cainiao_group = rw
admin = rw
* =

[blog:/core]
@php_group = rw
@cainiao_group = rw
admin = rw
* =
```

- 版本回退：

鼠标右键->tortoiseSvn->更新至版本-->点击显示日志->通过对应的日志信息，回退到指定的版本

- 解决冲突

1. 先把具有冲突的文件update更新下来
2. 把三个辅助文件删掉，修改含有黄色感叹号的文件，进行代码整合（需要程序员之间商量）。再次进行提交

 





# mysql可视化工具navicat的基本使用

参考地址： https://www.jianshu.com/p/6b3740ab112e 