---
title: Nginx
date: 2019-02-21 14:25:47
tags: Nginx
---
# 目录

[TOC]

# 一、昨日回顾

## 1、知识点回顾

能够实现库、集合的查看

​	show databases #查看数据库

​	db #查看集合(表)

能够实现文档(数据)的写入操作

​	MongoDB> use php33

​	MongoDB> db.集合名.insert( { key: value, ....} )

能够实现文档(数据)的更新操作
<!-- more -->
​	MongoDB> db.集合名.update( 修改条件, 需要修改字段, 是否开启批量修改(默认为：true，关闭批量修改), 是否开启批量修改(默认值：false，默认关闭) )

​	MongoDB> db.集合名.update( { name: 'php33' }, { '$set': { lesson: 'tp5' } }, false, true )

能够实现文档(数据)查询操作

​	查询所有数据：db.集合名.find()

​	查询 _id 等于或小于 9：db.集合名.find( { _id:{ '$lte' : 9} } )

​	查询 _id 等于或大于 9：db.集合名.find( { _id:{ '$gte' : 9} } )

能够运用 mongodb的帮助文档

​	查看数据库帮助文档：db.help()

​	查看集合帮助文档：db.集合名.help()

​	查看find帮助文档：db.集合名.find().help()

## 3、案例4(作业)

http://php.net/manual/zh/book.mongodb.php

删除 _id 为 11,33,55 的数据

代码参看：code/delete.php，上传到 /mydata/wwwroot/php33/www/mongodb 下进行测试

删除之前：

![1545355616070](1545355616070.png)

```
MongoDB> db.class3.remove({_id:{'$in':[11,33,55]}})
```

代码如下图所示：

![1545355695718](1545355695718.png)

删除后如下图所示：

```mongodb
MongoDB> db.class3.find({_id:{'$in':[11,33,55]}})
```

![1545355673332](1545355673332.png)

# 二、Nginx是什么

http://www.nginx.cn/doc/

http://tengine.taobao.org/

​	Nginx("engine x") 是一个高性能的 HTTP 、HTTPS和反向代理服务器，也是一个 IMAP/POP3/SMTP 邮箱服务器。Nginx 是由==塞索耶夫(lgor Sysoev)==为俄罗斯访问量第二的 Rambler.ru 站点开发的。

web服务器：https、http请求

邮箱服务器：支持收发邮件

cdn服务器/文档服务器：加速、主要存储哪些？静态资源文件 ：图片、视频、音乐、

数据库服务器：存储数据

高可用服务器：专门监听主机、备份数据、后备服务器随时替代其它服务器

## 1、Nginx 服务器的特点

- 热部署
- 可以高并 连接
- 内存消耗低
- 处理响应请求很快
- 具有很高的可靠性

## 2、国内使用 Nginx 作为服务器的著名网站

百度、京东、新浪、网易、腾讯、淘宝、唯品会、三七互娱等

## 3、查看一个网站是否使用 nginx

使用 lua 命令：curl -I -H "Accept-Encoding: gzip, deflate" "网址"

案例：获取 37wan 这个网站的相关信息

```
shell># curl -I -H "Accept-Encoding: gzip, deflate" "http://www.37.com"
```

![1543305884821](1543305884821.png)

其实淘宝也是使用 Nginx，只不过淘宝的工程师把 Nginx 的源代码进行了修改，他们把修改后的 Nginx 服务叫做 Tengine，是一个进阶版 Nginx

![1543306098134](1543306098134.png)

# 三、安装PHP与MySQL

​	请参考Linux第3天---<==源码编译安装.md==>文档

# 四、源码编译安装 Nginx

​	安装 nginx 之前，首先要检查安装 pcre（正则表达式库）、pcre-devel 和 libevent（提高应用程序的性能）、make 以及 openssl、openssl-devel

```
shell># rpm -qa | grep pcre*
shell># rpm -qa | grep libevent*
shell># rpm -qa | grep openssl*
```

如果没有安装，可以使用 `yum -y install`命令进行安装。

Nginx 可以到 http://nginx.org/en/download.html 下去，此处以 nginx-1.15.6.tar.gz 为例

**以下安装中涉及的几点需要提前说明的问题：**

所有下载文件将保存在 /root 目录

nginx 将以 ==www== 用户运行，而且将加入 service 开机自动运行(nginx与apache共存，当前环境不需要添加开机自动运行)

nginx 将被安装在 ==/usr/local/nginx== 目录下

nginx 的每个网站将存放在 /mydata/wwwroot 目录下，默认网站为 php33/www

nginx 的配置文件保存于 /usr/local/nginx/conf/nginx.cnf

nginx 的临时目录都保存于 /usr/local/nginx/tmp/ 目录下

## 1、安装 nginx

### 1.1、创建用户和组

```shell
# 如果已经添加了 www 用户组与用户名的时候，就不需要操作以下指令
shell># groupadd www
shell># useradd -g www -s /usr/sbin/nologin www
```

![1543308965931](1543308965931.png)

### 1.2、创建目录

```shell
shell># mkdir -pv /usr/local/nginx/tmp
```

![1545358646406](1545358646406.png)

### 1.3、下载  nginx

```shell
shell># cd ~
shell># wget http://nginx.org/download/nginx-1.15.6.tar.gz
```

![1543308942793](1543308942793.png)

### 1.3、解压并进入目录

```shell
shell># tar -zxvf nginx-1.15.6.tar.gz
shell># cd nginx-1.15.6
```

![1543309058993](1543309058993.png)

### 1.4、将 configure 参数及详情解析另存为一个文件，以供学习参考 用

```
shell># ./configure --help > nginx_configure.txt
```

### 1.5、进行软件配置和环境检测

```shell
./configure \
--prefix=/usr/local/nginx \
--user=www \
--group=www \
--with-http_ssl_module \
--with-http_flv_module \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-http_realip_module \
--with-http_image_filter_module \
--with-pcre \
--http-client-body-temp-path=/usr/local/nginx/tmp/client_body_temp \
--http-fastcgi-temp-path=/usr/local/nginx/tmp/fastcgi_temp \
--http-proxy-temp-path=/usr/local/nginx/tmp/proxy_temp \
--http-uwsgi-temp-path=/usr/local/nginx/tmp/uwsgi_temp \
--http-scgi-temp-path=/usr/local/nginx/tmp/scgi_temp
```

参数说明：

```
./configure \
--prefix=/usr/local/nginx \		# 指明安装目录
--user=www \					# 默认的 nginx 使用的用户
--group=www \					# 默认组
--with-http_ssl_module \
--with-http_flv_module \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-http_realip_module \
--with-http_image_filter_module \	# 这个还需要安装gd库 yum -y install gd-devel
--with-pcre \
--http-client-body-temp-path=/usr/local/nginx/tmp/client_body_temp \
--http-fastcgi-temp-path=/usr/local/nginx/tmp/fastcgi_temp \
--http-proxy-temp-path=/usr/local/nginx/tmp/proxy_temp \
--http-uwsgi-temp-path=/usr/local/nginx/tmp/uwsgi_temp \
--http-scgi-temp-path=/usr/local/nginx/tmp/scgi_temp
```

### 1.6、编译安装

```shell
shell># make && make install
```

![1543309642837](1543309642837.png)

## 2、配置

### 2.1、简单配置

#### 2.1.1、创建网站根目录

```shell
shell># mkdir /mydata/wwwroot			# 暂时可以不执行
shell># chown www:www /mydata/wwwroot
shell># cd /usr/local/nginx/conf
```

#### 2.1.2、开始配置

shell># vim /usr/local/nginx/conf/nginx.conf

- 在 `http{}` 代码段里添加 `client_max_body_size 200m;` 以支持 php 上传大文件。(请根据自己项目需求来定值)

![1545360099775](1545360099775.png)

- 将第一个 `server{}` 代码段里的 `location /{}` 里面的 root 值改为 `/mydata/wwwroot/php33/www`。并在 `index ` 的值 后面追加 `index.php`。然后将 `root` 和 `index` 都转移到 `location` 外面(同一个 `server` 代码段内可能需要多个 `location`，但每个 `location` 里面都要写 `root` 指令和 `index` 指令，就冗余了。所以将它们转移到 `location` 外面就可以消除此冗余)，然后同一 `server` 代码段里面的其他 `root` 指令注释掉

![1550714672861](1550714672861.png)

### 2.2、支持 PHP

在 `server{}`代码段里新增以下代码就可以支持 `php` 的访问了

```shell
location ~ \.php {
    fastcgi_pass	127.0.0.1:9000;
    fastcgi_index	index.php;
    include			fastcgi.conf;
}
```

![1545360250057](1545360250057.png)

### 2.3、优化虚拟主机配置

​	`nginx.conf` 中的每个 `server{}`代码段都是一个独立的虚拟主机配置。当配置多个虚拟主机时，需要在`nginx.conf`里定多个`server{}`代码段，那么主配置文件就会变得重复、臃肿。==所以建议，将 server{} 代码段都写在另一个文件(例如：vhost.conf 或 server.conf)或多个文件(例如：按客户或项目分组)，然后再用include 指令包含进来。==

本文档以 `vhost.conf`举例说明：

`nginx.conf`的 `server{}`代码段都移走，换成一个`include`指令

![1543594096293](1543594096293.png)

> ==注意：在/usr/local/nginx/conf/目录创建vhost.conf文件；需要在/mydata/wwwroot目录创建itcast/www目录与phpshop.com目录==
>
> 1、创建 itcast/www
>
> ![1543594551779](1543594551779.png)
>
> 2、创建 phpshop.com 目录
>
> ![1543594594772](1543594594772.png)

```
shell># vim /usr/local/nginx/conf/vhost.conf
```

![1543594227331](1543594227331.png)

`vhost.conf` 的示例代码如下：

```
# 默认网站
server {
    listen	80;
    server_name	localhost;
    root	/mydata/wwwroot/itcast/www;
    index index.htm index.html index.php;
    
    location ~ \.php {
        fastcgi_pass	127.0.0.1:9000;
        fastcgi_index	index.php;
        include			fastcgi.conf;
    }
}

# 电商
server {
    listen	80;
    server_name	phpshop.com;
    root	/mydata/wwwroot/phpshop.com;
    index	index.htm index.html index.php;
    
    location ~ \.php {
        fastcgi_pass	127.0.0.1:9000;
        fastcgi_index	index.php;
        include			fastcgi.conf;
    }
}
```

​	==有没有发现每个虚拟主机都需要配置 php 的代理转发，所以还可以再这样优化，将转发的指令移动到 fastcgi.conf里面==

`fastcgi.conf` 的示例代码：

```
# add by youlong from vhost.conf
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
```

shell># vim /usr/local/nginx/conf/fastcgi.conf

![1543594790546](1543594790546.png)

`vhost.conf`的示例代码：

shell># vim /usr/local/nginx/conf/vhost.conf

![1543594898110](1543594898110.png)

```
# 默认网站
server {
    listen	80;
    server_name	localhost;
    root	/mydata/wwwroot/itcast/www;
    index index.htm index.html index.php;
    
    location ~ \.php {
        include			fastcgi.conf;
    }
}

# 电商
server {
    listen	80;
    server_name	phpshop.com;
    root	/mydata/wwwroot/phpshop.com;
    index	index.htm index.html index.php;
    
    location ~ \.php {
        include			fastcgi.conf;
    }
}
```

## 3、支持 url rewrite

`nginx` 的 `rewirte` 规则可以直接写在 `location`代码段里，也可以写在外部文件（例如 `.htaccess`，`.htnginx`之类的，名字随便取）然后再用 `include`包含进来就可以了。==rewrite的语法请参考官方手册。==

本案例以 /mydata/wwwroot/phpshop.com 虚拟主机为例：

shell># vim /usr/local/nginx/conf/vhost.conf

![1543595252865](1543595252865.png)

```
# 为了安全，先将 .ht 文件禁止访问。
location ~ /\.ht {
    deny all;
}
location / {
    include /mydata/wwwroot/phpshop.com/.htaccess;
}
```

## 4、nginx 管理

### 4.1、启动

```shell
shell># /usr/local/nginx/sbin/nginx
```

### 4.2、重载配置文件

```shell
shell># /usr/local/nginx/sbin/nginx -s reload
```

### 4.3、停止

```shell
shell># /usr/local/nginx/sbin/nginx -s stop
```

### 4.4、通知nginx关闭服务

```shell
shell># /usr/local/nginx/sbin/nginx -s quit
```

### 4.4、安装为服务

```shell
shell># vim /etc/rc.d/init.d/nginx
```

内容如下（特别是 `chkconfig` 和 `description`那两行代码，一定要写，否则无法加入服务）：

```powershell
#!/bin/sh
# chkconfig: - 85 15
# description: nginx is a World Wide Web server. It is used to serve
start() {
    echo 'Starting Nginx ...'
    /usr/local/nginx/sbin/nginx > /dev/null 2>&1 &
}

stop() {
    echo 'Stoping Nginx ...'
    /usr/local/nginx/sbin/nginx -s stop > /dev/null 2>&1 &
}

reload() {
    echo 'Reloading Nginx ...'
    /usr/local/nginx/sbin/nginx -s reload
}

if [ $# -ne 1 ]
	then
		echo 'please input one params like start|restart|stop|reload'
	exit 1
fi

case "$1" in
	'start')
		start
	;;
	
	'stop')
		stop
	;;
	
	'restart')
		stop
		sleep 2
		start
	;;
	
	'reload')
		reload
	;;
	
	'*')
		echo 'please input one params like start|restart|stop|reload'
	;;
esac
```

#### 4.4.1、配置权限并增加到开机启动

```shell
shell># chmod a+x /etc/rc.d/init.d/nginx
shell># chkconfig --add nginx	(暂时不需要操作)
shell># chkconfig --level 345 nginx on (暂时不需要操作)
```

![1543595435290](1543595435290.png)

**以后就可以直接用以下指令来完成日常管理了：**

```shell
shell># service nginx start		#启动
shell># service nginx reload	#重载
shell># service nginx stop		# 停止
```

## 5、与 ThinkPHP 的整合

==如果希望 nginx 支持 ThinkPHP 的运行，还需要将 nginx 配置成支持 pathinfo 模式。==

​	fastcgi 模块自带了一个 fastcgi_split_path_info 指令专门用来解决此类问题的，该指令会根据给定的正则表达式来分隔 URL，从而提取出脚本名和 pathinfo 信息，使用这个指令可以避免使用 if 语句，配置更简单。

另外判断文件是否存在也有更简单的方法，使用 try_files 指令即可。

本案例以 /mydata/wwwroot/phpshop.com 虚拟主机为例：

shell># vim /usr/local/nginx/conf/vhost.conf

![1550720756427](1550720756427.png)

```
server {
    ......
    location / {
        # 访问路径的文件不存在则重写 URL 转交给 ThinkPHP 处理
        try_files $uri /index.php$uri;
    }
    
    location ~ .+\.php($|/) {
        # 设置 PATH_INFO，注意 fastcgi_split_path_info 已经自动改写了 fastcgi_script_name 变量
        # 后面不需要再改写 SCRIPT_FILENAME，SCRIPT_NAME 环境变量，所以必须在加载 fastcgi.conf 之前设置
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        
        # 加载 Nginx 默认"服务器环境变量"配置
        include fastcgi.conf;
    }
}
```

==可将第二个 location 的内容剪切追加到 fastcgi.conf 文件的内容前面，以实现代码重用。==

本案例以 /mydata/wwwroot/phpshop.com 虚拟主机为例：

shell># vim /usr/local/nginx/conf/vhost.conf

![1543596898371](1543596898371.png)

```
server {
    listen 80;
    server_name phpshop.com
    root /mydata/wwwroot/phpshop.com/public;
    index index.htm index.html index.php;
    location ~ /\.ht {
        deny all;
    }
    
    location / {
        # 访问路径的文件不存在则重写 URL 转交给 ThinkPHP 处理
        try_files $uri /index.php$uri;
    }
    
    location ~ .+\.php($|/) {
        # 加载 Nginx 默认"服务器环境变量"配置
        include fastcgi.conf;
    }
}
```

shell># vim /usr/local/nginx/conf/fastcgi.conf

```
fastcgi_split_path_info ^(.+.php)(/.*)$;
fastcgi_param PATH_INFO $fastcgi_path_info;
```

![1543596995958](1543596995958.png)

> ==注意：每次更改了配置内容后，记得执行重载命令让配置生效。==

shell># service nginx reload

## 6、开放端口

将 80 端口加入防火墙

```
shell># iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

添加至自启动文件

```
shell># echo 'iptables -I INPUT -p tcp --dport 80 -j ACCEPT >> /etc/rc.d/rc.local
```

# 五、网站的静态资源和 gzip 压缩

一般静态页面指没有和服务器互相交互的 html 页面。

我们平常所说的 html、css、image、js、xml、json、视频、音乐等都属于静态资源。

静态资源越大越多网页加载的速度就会越慢，因此有必要对静态资源进行压缩优化，这就可以为网站提速不少。

然而有一些大型网站他们除了对静态资源压缩之外他们还会使用 CDN 静态资源服务器做支撑，但 CDN 服务器是需要一定的资金成本的。

查看一个网站是否有启用静态资源压缩：

```
shell># curl -I -H "Accept-Encoding: gzip, deflate" "http://www.sohu.com"
```

![1543639503896](1543639503896.png)

在 Nginx 服务器中，我们可以通过开启 gzip 来对网站的静态资源进行压缩

## 1、配置如下

shell># vim /usr/local/nginx/conf/nginx.conf

![1545375459573](1545375459573.png)

```
http {
    ......
    #是否启动gzip压缩,on代表启动,off代表开启
    gzip  on;
    #需要压缩的常见静态资源
    gzip_types text/plain application/javascript   application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png video/mp4 video/mp3;
    #由于nginx的压缩发生在浏览器端而微软的ie6很坑爹,会导致压缩后图片看不见所以该选项是禁止ie6发生压缩
    gzip_disable "MSIE [1-6]\.";
    #如果文件大于1k就启动压缩
    gzip_min_length 1k;
    #以16k为单位,按照原始数据的大小以4倍的方式申请内存空间,一般此项不要修改
    gzip_buffers 4 16k;
    #压缩的等级,数字选择范围是1-9,数字越小压缩的速度越快,消耗cpu就越大
    gzip_comp_level 2; 
}
```

## 2、重新加载

```
shell># service nginx reload
```

注意：默认情况下 gzip 压缩必须是静态文件大于 1k 才会启动，如果文件小于 1k 则无法启动压缩，如下图所示：

![1545375509468](1545375509468.png)

如果无法启动压缩，则 gzip 选项在 curl 命令中无法显示，如果成功如下图所示：

![1545375547564](1545375547564.png)

# 六、使用缓存功能把静态资源缓存到客户端

## 1、启动缓存功能的配置

shell># vim /usr/local/nginx/conf/vhost.conf

```
# 设置需要压缩的文件类型
location ~ .*\.(jpg|jpeg|gif|css|js|png|ico|mp3|mp4|swf|flv){
     expires 10;  # 缓存10天
}
```

如果希望对静态文件缓存 10 天，设置为如下：

![1545375585224](1545375585224.png)

保存并退出，然后重启 nginx 服务

```
shell># service nginx reload
```

![1543640996633](1543640996633.png)

使用 lua 的 curl 测试如下：

```
curl -I -H "Accept-Encoding: gzip, deflate" "http://10.10.10.11/it.mp4"
```

![1545375685236](1545375685236.png)

> ==注意：如果 Linux 时间不对要修改 Linux 中的系统时间和硬件的时间，否则缓存有问题==

# 七、服务器集群(了解)

服务器(电脑主机)，安装许多软件，例如：php/apache/mysql/redis/memcache 等等

这些软件是网站运行需要的

网站运行经营的比较好的，每天就会有许多人员进行访问 pv (point view)：1000万

1 天 3600 * 4 = 14400 秒，平均并发访问量：700 次，峰值：3000/4000/5000或以上并发量：服务器每秒被访问的次数

现在服务器每天负载很高（工作量非常多）

并发访问量高的时候，服务器要做优化（否则影响用户使用）：

可以购买多个服务器来支撑网站运行，这个多服务器的组合就是"集群"

集群就是成本很，维护很困难，使用云计算的服务器空间，云计算在世界上最专业最贴近运维技术的就是亚马逊云(aws)

# 八、负载均衡的分发实验

## 1、正向代理和反向代理的原理图

![1543570290551](1543570290551.png)

![1543570318660](1543570318660.png)

## 2、负载均衡的原理和作用

**应用场景**

正向代理：是为了请求访问不到的网站发起了代理，因此正向代理是多数应用是为了提供上网服务，我们说的翻墙就是一种正向代理，VPN就是正向代理的一种服务器软件

反向代理：是为了实现负载均衡解决网站大访问量所应用的技术手段， ==Nginx== 就是一种能实现反向代理的服务器，因此 ==Nginx== 成为反向代理服务器，由于反向代理服务器能实现负载均衡，所以也有人把反向代理服务器叫做==负载均衡器==

## 3、使用 nginx 部署一个负载均衡的实验

![1545377251034](1545377251034.png)

### 3.1、负载均衡必须要有至少 3 台服务器

Proxy(反向代理服务器)：192.168.230.99

Lamp1(Linux 的 php 环境)：106.13.14.113

Lamp2(Linux 的 php 环境)：185.225.139.195:88

必须确保所有的服务器都处于开启状态

### 3.2、修改 Proxy 服务器中 nginx.conf 文件内容

shell># vim /usr/local/nginx/conf/nginx.conf

![1545378993582](1545378993582.png)

```
# 负载均衡的反向代理分发选项
upstream web {
    # 配置服务器地址
    server ip地址 weight=1 max_fails=3 fail_timeout=20s;
    server ip地址 weight=1 max_fails=3 fail_timeout=20s;
}
```

参数说明：

weight=1：权重，如果分配的值越大，权重超高

max_fails=3：最多连接失败次数为 3 次

fail_timeout=20s：每次连接失败的时间

### 3.3、修改 Proxy 服务器中 vhost.conf 文件内容

shell># vim /usr/local/nginx/conf/vhost.conf

![1545379196642](1545379196642.png)

```
location / {
    proxy_pass http://web/;
}
```

![1545379765642](1545379765642.png)

### 3.4、重新加载 nginx

```
shell># service nginx reload
```

#  九、Lamp 和Lnmp 的区别

## 1、Lamp 架构概述

​	LAMP（Linux+Apche+MySQL+PHP）网站架构是目前国际流行的 Web 框架，这个框架是经典且古老的框架，应该是拥有 PHP 开始这组合就是开始了，Lamp 最大特点是其面对解析程序执行的时候速度很快，因为 ==Apache 为算法而生==，而其缺点在于使用模块加载显得有些==臃肿==，第一个扩展的修改 Apache 就需要重新加载一次，所以 Apache 在实际开发中其实不是为了提高访问量而生，而是为了提高自算法而生的。

## 2、Lnmp 框架概述

​	Lnmp 是 Linux + Nginx + MySQL + PHP 的组合服务器架构，PHP修改配置文件可以无需重启 Nginx 服务，它管理 PHP 是依赖 PHP 的内存管理工具 ==php-fpm==，所以当 PHP 的配置发生修改，Nginx 服务器是不需要重新启动去加载 PHP 的配置，只需要单纯重新启动 php-fpm 本身就可以完成 PHP 的配置。而 Nginx 服务器是一个反向代理的服务器软件，是为访问量而生的服务器软件。

## 3、Lamp 和 Lnmp 的区别是什么

​	Lamp 为算法而生，所以如果一个网站的逻辑非常复杂，例如：股票交易、金融等适合采用 Lamp 架构，Lnmp 为访问量而生，如果访问量比较大适合采用 Lnmp，目前的流行的 Lamp  + Lnmp 两者结合使用。

在现实开发中，如果希望 nginx 服务器和 Lamp 一起工作，那么我们就采用负载均衡，这也是工作用得最多的，工作中使用单独 Lnmp 环境其实并不是非常多，多数是 Lanmp 环境。

# 十、总结

服务器？

​	web服务器

​		可以提供http与https

​	cdn（资源）服务器

​		存放静态资源

​	数据库的服务器

​		存放数据

​	高可用服务器

​		备份数据、监听其它服务器、有可能随时替换其它服务器

​	邮箱服务器



Nginx

​	web服务器？http与https协议

​	反向代理

​	负载均衡

​	邮件服务器

启动nginx

​	/usr/local/nginx/sbin/nginx	#启动  nginx

​	/usr/local/nginx/sbin/nginx -s quit  #通知nginx关闭服务

​	/usr/local/nginx/sbin/nginx -s reload	#重新加载 、热加载

​	/usr/local/nginx/sbin/nginx -s stop # 停止nginx

session ?

![1545382195663](1545382195663.png)