---
title: Ajax01
date: 2019-02-20 14:47:25
tags: Ajax JS
---
# Ajax从入门到精通（二）


# 一、Ajax与JSON

## 1、什么是JSON

JSON 指的是 JavaScript 对象表示法（JavaScript Object Notation）

JSON 是轻量级的文本数据交换格式

JSON 独立于语言 

JSON 具有自我描述性，更易理解

为什么要使用JSON不使用XML呢？答：JSON无论在生成还是解析相对于XML都简单一些。

## 2、定义JSON
<!-- more -->
在JS中，我们可以通过一对花括号来定义JSON数据传输格式。

{

​	“键名”:值,

​	“键名”:值

}

在JS中，我们可以把JS对象，通过JSON.stringify(JS对象)转换为JSON格式的数据。

## 3、PHP与JSON（重点）

① 把数组或对象生成JSON数据：json_encode(数组或对象)

② 把JSON数据转化为数组或对象：json_decode(JSON数据,bool)

如果bool为true，代表把JSON转换为数组

如果bool为false，代表把JSON转换为对象，默认为false

 

demo01_encode.php 示例代码：

![img](wpsDB08.tmp.jpg) 

demo02_decode.php 示例代码：

![img](wpsDB09.tmp.jpg) 

## 4、Ajax与JSON（超过2条）

![img](wpsDB0A.tmp.jpg) 

## 5、案例：点击按钮，返回数据（重点）

demo03_json.html 示例代码：

![img](wpsDB0B.tmp.jpg) 

![img](wpsDB0C.tmp.jpg) 

demo03.php 示例代码：

![img](wpsDB0D.tmp.jpg) 

所以由以上代码运行可知，JSON数据格式无论在生成还是解析相对于XML而言，都更加简单。所以，很多人建议把Ajax改成Ajaj。

# 二、Ajax框架封装（重点）

## 1、为什么要封装Ajax框架

简单来说，就是简化Ajax操作。以前都需要通过Ajax五步走或六步走来事件Ajax技术。但是略微复杂，而且代码冗余性过高。

## 2、封装Ajax框架

第一步：封装一个自调用匿名函数（自己调用自己）

基本语法：

(function(){

​	//自调用匿名函数

})();

ajax.js 示例代码：

![img](wpsDB0E.tmp.jpg) 

为什么要使用自调用匿名函数？就是为了解决框架与框架之间的命名冲突问题。

第二步：定义一个函数$，用于获取指定id的dom对象

![img](wpsDB0F.tmp.jpg) 

第三步：让局部变量$全局化，可以进行全局访问

提示：在JS中，我们在全局作用域中定义的变量都是以属性的形式添加到window对象中的。

![img](wpsDB1F.tmp.jpg) 

第四步：封装Ajax对象

在JS中，一切都是对象，所以ajax变量也是对象，所以我们可以为其赋予属性。

![img](wpsDB20.tmp.jpg) 

第五步：封装Ajax中的get请求（五步走）

![img](wpsDB21.tmp.jpg) 

第六步：调用$.get方法，测试成功与否

![img](wpsDB22.tmp.jpg) 

第七步：让Ajax框架根据不同的参数返回不同的数据类型，添加type参数

![img](wpsDB23.tmp.jpg) 

第八步：测试type参数是否可用，使用Ajax返回两个数的四则运算结果

demo05_size.html示例代码：

![img](wpsDB24.tmp.jpg) 

demo05.php 示例代码：

![img](wpsDB25.tmp.jpg) 

在demo05_size.html 页面中进行JSON解析：

![img](wpsDB26.tmp.jpg) 

## 3、Ajax中post请求的封装

第一步：创建Ajax对象

第二步：设置回调函数

第三步：初始化Ajax对象

第四步：设置请求头

第五步：发送Ajax请求

第六步：判断与执行

![img](wpsDB27.tmp.jpg) 

![img](wpsDB28.tmp.jpg) 

测试$.post方法是否可用：

![img](wpsDB29.tmp.jpg) 

demo06.php 示例代码：

![img](wpsDB2A.tmp.jpg) 

# 三、案例：百度下拉搜索

## 1、效果图

![img](wpsDB2B.tmp.jpg) 

## 2、构建HTML页面

![img](wpsDB2C.tmp.jpg) 

## 3、编写Ajax

![img](wpsDB2D.tmp.jpg) 

![img](wpsDB2E.tmp.jpg) 

## 4、编写服务器端代码

![img](wpsDB2F.tmp.jpg) 
