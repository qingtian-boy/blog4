---
title: Ajax无刷新分页
date: 2019-02-20 14:54:25
tags: Ajax JS
---
# Ajax从入门到精通



 

# 一、Ajax无刷新分页

## 1、效果图

![img](wpsC8D.tmp.jpg) 
<!-- more -->
## 2、编写无刷新分页

demo02_youku.html 示例代码：

![img](wpsC8E.tmp.jpg) 

demo02.php 示例代码：（接收数据 + 数据分页 + 返回字符串结果）

![img](wpsC8F.tmp.jpg) 

![img](wpsC90.tmp.jpg) 

![img](wpsC91.tmp.jpg) 

## 3、给分页设置样式

![img](wpsC92.tmp.jpg) 

## 4、读取时间格式pubdate

![img](wpsCA3.tmp.jpg) 

## 5、添加页码

![img](wpsCA4.tmp.jpg) 

## 6、设置分页样式

![img](wpsCA5.tmp.jpg) 

## 7、无刷新分页的核心

![img](wpsCA6.tmp.jpg) 

## 8、扩展：优酷播放器

① 上传视频到优酷

![img](wpsCA7.tmp.jpg) 

② 进入播放页面，找到分享菜单，如下图所示：

![img](wpsCA8.tmp.jpg) 

③ 把复制的代码放入我们的HTML页面中

![img](wpsCA9.tmp.jpg) 

运行结果：

![img](wpsCAA.tmp.jpg) 

# 二、JSONP技术

## 1、Ajax跨域请求

① <http://localhost/20181205/>  本机域名

② <http://www.oa.com/>  远程域名

![img](wpsCAB.tmp.jpg) 

记住：设置完成后，一定要重启Apache服务器，否则配置文件无法生效。

demo03_kuayu.html 示例代码：

![img](wpsCAC.tmp.jpg) 

<http://www.oa.com/demo.php> 示例代码：

![img](wpsCAD.tmp.jpg) 

运行结果：

![img](wpsCAE.tmp.jpg) 

由以上运行结果可知，浏览器禁止我们从一个域上通过Ajax请求另外一个域上的资源。我们把这种情况，就称之为“Ajax跨域请求或Ajax跨源请求”。

## 2、Ajax跨域请求原理图

![img](wpsCAF.tmp.jpg) 

## 3、Ajax跨域请求应用场景

① 内部系统数据共享

官网：<http://www.itcast.cn/>

oa系统：<http://oa.itcast.cn/>

ems系统：<http://ems.itcast.cn/>

oa系统想与ems系统实现数据交互与共享，就会发生Ajax跨域请求。

 

② 手机APP与数据后台

手机APP（IOS、Android、混合式APP开发），设计APP的界面。

数据都有一个后台，专门为APP提供数据，比如很多平台都是通过PHP+MySQL开发。

APP（3G、4G、Wifi=>IP地址） -> 网站后台获取数据（<http://www.news.com>）

192.168.84.65 => <http://www.news.com/>发送请求，就会发生Ajax跨域请求。

## 4、解决Ajax跨域请求

早期的解决方案：通过script标签来解决Ajax跨域请求。

demo04_kuayu.html 示例代码：

![img](wpsCB0.tmp.jpg) 

<http://www.oa.com/demo.php> 示例代码：

![img](wpsCB1.tmp.jpg) 

运行结果：页面一加载完毕，就立即返回结果

![img](wpsCB2.tmp.jpg) 

## 5、JSONP技术

在前台通过动态添加script标签及src属性，表面看上去与ajax极为相似，但是，这和ajax并没有任何关系；

为了便于使用及交流，逐渐形成了一种“非正式传输协议”，人们把它称作 JSONP技术。

 

问题：请说出JSON与JSONP的区别？

JSON是一种轻量级的数据传输格式，主要用于数据的传输与存储。

JSONP是一种非官网的协议，主要用于实现解决Ajax的跨域问题。

 

demo05_jsonp.html 示例代码：

![img](wpsCB3.tmp.jpg) 

 

<http://www.oa.com/demo.php> 示例代码：

![img](wpsCB4.tmp.jpg) 

## 6、使用JSONP+JSON实现跨域请求

demo06_jsonp.html示例代码：

![img](wpsCB5.tmp.jpg) 

<http://www.oa.com/demo.php> 示例代码：

![img](wpsCC5.tmp.jpg) 

demo06_jsonp.html示例代码：遍历返回的JSON数据

![img](wpsCC6.tmp.jpg) 

## 7、跨域的另外一种解决方案（简单）

要求：要求客户端代码与服务器端代码都必须拥有控制权。

![img](wpsCC7.tmp.jpg) 

<http://www.oa.com/demo.php> 示例代码：

![img](wpsCC8.tmp.jpg) 

运行结果：

![img](wpsCC9.tmp.jpg) 

由以上运行结果可知，以上代码缺少一个CORS头信息。

解决方案：在服务器端添加一个CORS的头信息，如下图所示：

![img](wpsCCA.tmp.jpg) 