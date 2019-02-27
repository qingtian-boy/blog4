---
title: Ajax00
date: 2019-02-20 14:44:20
tags: Ajax JS
# Ajax从入门到精通（一）

# 学习目标

Ajax概述

Ajax应用场景

Ajax中的get请求（五步走）

Ajax中的post请求（六步走）

Ajax缓存问题的解决

Ajax中XML与JSON

# 一、Ajax概述
<!-- more -->
## 1、什么是Ajax

1998年，Microsoft微软公司Outlook Web Access，允许客户端在无刷新的前提下，向服务器端发送HTTP请求，并随后集成在IE4.0中应用（XMLHTTP）

2005年，Google谷歌公司发现XMLHTTP技术不仅应用于软件，还可以应用于Web系统中，所以其在自家的多款产品中，引入XMLHTTP技术，更名为Ajax技术。Google Suggest、Gmail邮箱、Google地图。

 

Ajax = Asynchronous [ə'sɪŋkrənəs; eɪ-] 异步 + JavaScript + And + XML

## 2、Ajax应用场景

数据验证（用户名是否唯一验证）

无刷新分页

短信验证法发送

无刷新图片上传

Ajax技术的核心：在无刷新的前提下，从客户端通过HTTP向服务器发送请求

## 3、快速入门

创建demo01_rumen.html 示例代码：

![img](wpsC283.tmp.jpg) 

# 二、Ajax对象（重点）

## 1、为什么需要Ajax对象

使用Ajax技术必须有一个前提，首先要创建一个Ajax对象。

## 2、Ajax对象的创建

① 基于IE内核的浏览器

var xhr = new ActiveXObject(‘Microsoft.XMLHTTP’);

② 基于W3C内核的浏览器

var xhr = new XMLHttpRequest();

demo02_xhr.html 示例代码：

![img](wpsC284.tmp.jpg) 

## 3、解决Ajax对象的兼容性问题（封装，重点）

① 创建一个public.js文件，用于实现代码的封装

② 首先创建一个$函数，用于获取指定id的dom对象

![img](wpsC285.tmp.jpg) 

③ 创建一个createXhr函数，用于创建Ajax对象（解决Ajax对象的兼容性问题）

![img](wpsC286.tmp.jpg) 

④ 调用createXhr函数，获取Ajax对象

![img](wpsC287.tmp.jpg) 

## 4、Ajax对象下属性和方法

属性：

第一步：创建Ajax对象，var xhr = createXhr()

第二步：设置回调函数，xhr.onreadystatechange = function() {}

疑问：当状态改变时触发onreadystatechange，什么状态呢？

答：使用readyState状态码记录状态的改变

0：表示对象已建立，但未初始化，只是 new 成功获取了对象，但是未调用open方法 

1：表示对象已初始化，但未发送，调用了open方法，但是未调用send方法 

2：已调用send方法进行请求 

3：正在接收数据（接收到一部分），客户端已经接收到了一部分返回的数据 

4：接收完成，客户端已经接收到了所有数据

status ：响应状态码，成功返回200，未找到页面，返回404

responseText ：如果PHP服务器端页面返回的是字符串，则使用xhr.responseText接收

responseXML ：如果PHP服务器端页面返回的是XML，则使用xhr.responseXML接收

 

方法：

open(method,url,[aycs])方法 ：初始化Ajax对象（有目标但是未执行）

参数说明：

method ：get请求或post请求

url ：请求页面的url地址

[aycs] ：是否异步，默认为true，代表异步。我们也可以把其设置为false，代表同步

setRequestHeader(header,value)方法 ：设置请求头信息

参数说明：

header ：请求头名称

value ：请求头的值信息

特别注意：setRequestHeader方法只能出现在open方法之后，一定要谨记！！！

send(data)方法 ：发送Ajax请求，如果是get请求，data=null。

​							  如果是post请求，data=请求的参数。

# 三、Ajax中get请求五步走

## 1、Ajax五步走

第一步：创建Ajax对象（前提）

第二步：设置回调函数onreadystatechange

第三步：初始化对象open

第四步：发送Ajax请求send

第五步：判断与执行xhr.readyState == 4

字符串：xhr.responseText

XML ：xhr.responseXML

## 2、使用Ajax向服务器端发送请求

demo03_get.html 示例代码：

![img](wpsC288.tmp.jpg) 

demo03.php 示例代码：

![img](wpsC289.tmp.jpg) 

## 3、几个遗漏的小问题

① 在服务器端，我们都是通过echo进行返回数据的，我们可不可以将其更改为return？

答：不行，Ajax属于客户端语言，所以要求PHP服务器端页面的数据必须返回到客户端浏览器，但是return只能返回数据到PHP服务器端，不能返回到客户端，所以如果使用return作为返回，则客户端接收不到任何数据。

② 当我们发送Ajax请求时，如果服务器端页面不存在，其可以返回数据么？

答：如下图所示，即使页面不存在，Ajax也会返回数据。404

![img](wpsC28A.tmp.jpg) 

解决页面不存在的问题：

![img](wpsC28B.tmp.jpg) 

以上代码还可以进一步简写：

![img](wpsC28C.tmp.jpg) 

③ 什么是同步什么是异步？

定时器、事件绑定、Ajax都是异步的，那到底什么是异步，什么是同步！

![img](wpsC28D.tmp.jpg) 

## 4、案例：使用Ajax验证用户名是否唯一

① 设计数据库

![img](wpsC29E.tmp.jpg) 

插入一条数据：

![img](wpsC29F.tmp.jpg) 

 

② 编写HTML页面demo05_user.html 示例代码

![img](wpsC2A0.tmp.jpg) 

demo05.php 示例代码：

![img](wpsC2A1.tmp.jpg) 

运行结果：

![img](wpsC2A2.tmp.jpg) 

## 5、扩展loading图片的加载

![img](wpsC2A3.tmp.jpg) 

# 四、Ajax中get请求在IE下的缓存

## 1、什么是IE缓存问题

问题：在tb_user表中添加一个itcast用户，W3C浏览器中，验证itcast，发现用户名已被注册。但是如果之前在IE浏览器中验证过itcast这个用户，当再次访问时，系统会自动调用系统缓存，如下图所示：

![img](wpsC2A4.tmp.jpg) 

记住：只要在IE内核的浏览器中使用Ajax中的get请求，系统会自定生成缓存，当我们第二次调用同一个url地址时，系统会优先走缓存。

![img](wpsC2A5.tmp.jpg) 

虽然IE浏览器出于好心，想加快数据的访问。但是在Ajax中，其可能会导致服务器端的数据无法实时的加载！

## 2、解决Ajax中get请求在IE下的缓存问题

① 在客户端解决Ajax的缓存问题（重点）

在url尾部添加一个无意义的参数（比如下划线_），其值为new Date().getTime()毫秒时间戳。

demo05_user.html 示例代码：

![img](wpsC2A6.tmp.jpg) 

运行结果：

![img](wpsC2A7.tmp.jpg) 

② 在服务器端解决Ajax的缓存问题

基本语法：

PHP页面顶端添加header语句如下：

header("Cache-Control: no-cache");

功能：告诉客户端浏览器不要缓存当前页面

![img](wpsC2A8.tmp.jpg) 

# 五、Ajax中post请求六步走

## 1、get请求与post请求有和区别呢？

① 传参方式不同

get在请求的url地址尾部追加参数

post请求在数据行位置追加参数，如下图所示：

![img](wpsC2A9.tmp.jpg) 

② 请求头信息不同

post 多了一行请求头 

xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');  

![img](wpsC2AA.tmp.jpg) 

get 不需要设置

③ 传递数据的类型不同

参数值类型：  

get 传参值只能是数字、字符串  

post 可以传递数字、字符串以外，还可以传递二进制数据（图片、文件）

## 2、post请求六步走

第一步：创建Ajax对象 var xhr = createXhr()

第二步：设置回调函数 xhr.onreadystatechange = function(){}

第三步：初始化Ajax对象 xhr.open(‘post’,’demo.php’)
第四步：设置请求头信息 xhr.setRequestHeader()

第五步：发送Ajax请求 xhr.send(data)，send方法在数据行位置

第六步：判断与执行 xhr.readyState == 4 && xhr.status == 200

## 3、案例：无刷新录入功能

demo07_luru.html 示例代码：

![img](wpsC2AB.tmp.jpg) 

![img](wpsC2AC.tmp.jpg) 

demo07.php 示例代码：

![img](wpsC2AD.tmp.jpg) 

## 4、扩展HTML5+FormData+Ajax上传图片

demo08_upload.html 示例代码：

![img](wpsC2AE.tmp.jpg) 

在HTML5为我们提供了 FormData 对象：

通过FormData对象可以组装一组用 XMLHttpRequest发送请求的键/值对。它可以更灵活方便的发送表单数据，因为可以独立于表单使用。

FormData对象使用方法一共分为两步：

var fm = $('fm');//1、FormData第一行先获取表单

var fmdata = new FormData(fm);//2、实例化FormData对象，表单作为参数

demo08_upload.html 示例代码：Ajax + FormData发送数据

![img](wpsC2BF.tmp.jpg) 

![img](wpsC2C0.tmp.jpg) 

demo08.php 示例代码：

![img](wpsC2C1.tmp.jpg) 

# 六、Ajax中的XML（了解）

Ajax合成词：A异步的，j（JavaScript），a（And），x（XML）

## 1、什么是XML

XML和JSON一样，也是一种数据的传输格式。相对于JSON而言，略微复杂。

## 2、XML的基本语法

① XML语法结构与HTML基本类似，因为XML最初设计的初衷就是为了代替HTML

② XML语法略微严格一些，一个XML文档只能仅只能拥有一个根元素

<root>

  <child>

​    <subchild>.....</subchild>

  </child>

</root>

③ 可选参数，XML文档声明

<?xml version="1.0" encoding="utf-8"?>

④ 所有标签必须是闭合的，有开始就必须有结尾

<user>

​    <name>刘能</name>

​    <!-- 错误的,无结束标签 -->

​    <age>46

​	<!-- 错误的,无开始标签 -->

​    男</sex>

</user>

⑤ 严格区分大小写

<?xml version="1.0" encoding="UTF-8"?>

<user>

​    <name>刘能</name>

​    <!-- 错误，标签大小写不一致 -->

​    <age>46</Age>

​    <sex>男</sex>

</user>

⑥ XML标签不允许有交叉嵌套，XML标签名不建议使用特殊字符，尽量用数字字母下划线

<?xml version="1.0" encoding="UTF-8"?>

<user>

​    <name>刘能</name>

​    <age>46</age>

​    <!-- 不建议使用 -->

​    <s-ex>男</s-ex>

​    <s.ex>男</s.ex>

</user>

demo09_doc.xml 示例代码：

![img](wpsC2C2.tmp.jpg) 

## 3、Ajax与XML

在实际项目开发中，如果我们从服务器端获取的数据超过2条以上，就可以把数据封装在XML结构中。这样，利于数据的传输。

特别注意：如果服务器端返回的数据是XML结构，则使用Ajax接收时，也需要使用xhr.responseXML进行接收。

## 4、案例：点击按钮，获取数据列表

demo10_xml.html 示例代码：

![img](wpsC2C3.tmp.jpg) 

demo10.php 示例代码：拼接XML字符串

![img](wpsC2C4.tmp.jpg) 

## 5、获取XML中的数据

![img](wpsC2C5.tmp.jpg) 

观察以上图解发现，以上XML结构产生的DOM树与HTML中的DOM树是完全一致的。所以想获取里面的元素，我们也可以使用以前学习过的方法。dom对象.getElementsByTagName

![img](wpsC2C6.tmp.jpg) 