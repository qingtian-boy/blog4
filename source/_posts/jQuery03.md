---
title: jQuery03
date: 2019-02-20 14:56:45
tags: JQ JS
---
# jQuery从入门到精通



# 一、jQuery中的文档操作

## 1、删除操作

empty() ：清空元素的内容，但是元素本身保留

remove() ：清空元素的内容，同时删除元素本身

<!-- more -->

demo01_del.html 示例代码：

![img](wpsE39.tmp.jpg) 

## 2、复制（克隆）操作

clone([bool]) ：克隆页面中的某个元素

参数说明：boolean类型，可选参数。

bool ：不写，默认为false，代表只复制元素本身，但是不复制元素本身的事件

​	   true，代表不仅要复制元素本身，还需要复制元素本身的事件

① 只克隆元素，不克隆元素本身的事件

![img](wpsE3A.tmp.jpg) 

② 克隆元素而且克隆元素本身的事件

![img](wpsE3B.tmp.jpg) 

## 3、替换操作

① html()方法 ：替换元素本身的内容，但是不替换标签

② replaceWith()方法 ：不仅替换元素的内容，还要替换标签本身

要新掌握的知识点：创建jQuery对象

基本语法：

var  jQuery对象 = $(dom);

demo03_tihuan.html 示例代码：

![img](wpsE3C.tmp.jpg) 

## 4、包裹操作

<div><div>div1</div></div>

<div><div>div2</div></div>

<div>

<div>div1</div><div>div2</div>

</div>

<div><strong>div1</strong></div>

<div><strong>div2</strong></div>

 

wrap()方法 ：把所有匹配的元素用其他元素的结构化标记包裹起来，如wrap(‘<div></div>’);

wrapAll()方法：将所有匹配的元素用单个元素包裹起来，一次包裹，如wrapAll(‘<div></div>’);

wrapInner()方法： 将每一个匹配的元素的子内容(包括文本节点)用一个HTML结构包裹起来。如wrapInner(‘<strong></strong>’);

unwrap()方法：移出选中元素的父元素，但是元素本身不移除

demo04_wrap.html 示例代码：

![img](wpsE4D.tmp.jpg) 

## 5、查询操作（重点）

eq(index) ：根据元素的index索引来查找元素，默认索引从0开始

filter(expr) ：筛选操作，$(‘div’).filter(‘.cls1’);

not(expr) ：匹配除指定选择器以外的其他元素

children([expr]) ：获取当前元素下的所有子元素（父子关系）

find(expr) ：获取当前元素下的所有后代元素（祖先与后代元素关系）

next([expr]) ：获取当前元素下紧邻的下一个元素

prev([expr]) ：获取当前元素上紧邻的上一个元素

parent([expr]) ：获取当前元素的父元素（子元素上一级的父元素）

siblings([expre]) ：获取当前元素的所有同级兄弟元素（所有同级兄弟节点，无论上下）

 

demo05_chazhao.html 示例代码：

![img](wpsE4E.tmp.jpg) 

![img](wpsE4F.tmp.jpg) 

## 6、案例：后台折叠菜单

效果图：

![img](wpsE50.tmp.jpg) 

demo06_menu.html 示例代码：

![img](wpsE51.tmp.jpg) 

设置CSS样式：

![img](wpsE52.tmp.jpg) 

引入jQuery源代码：

![img](wpsE53.tmp.jpg) 

编写jQuery代码，实现菜单折叠效果：

![img](wpsE54.tmp.jpg) 

# 二、jQuery中的插件

## 1、为什么要使用插件

在jQuery中，虽然其提供很多好用的方法，但是有些时候其并不能满足我们的项目需求，怎么办？（全选、全不选、反选功能）

答：可以使用jQuery中的插件方法，人为扩展函数来实现自定义功能。

## 2、基本语法

$.fn.extend(JSON格式对象);

或

jQuery.fn.extend(JSON格式对象);

## 3、插件机制中的this

在插件的处理机制中，也存在一个特殊的关键字this，但是一定要注意：

这个this指向当前正在操作的jQuery对象。

## 4、案例：向jQuery中添加一个func方法

![img](wpsE55.tmp.jpg) 

## 5、案例：向jQuery定义三个方法qx()、qbx()、fx()

实现功能：单击全选按钮，调用qx方法，选中所有复选框

​		  单击全不选按钮，调用qbx方法，取消选中所有复选框

​		  单击反选按钮，调用fx方法，选中的复选框取消，未选中的选中

demo08_chajian.html 示例代码：

![img](wpsE56.tmp.jpg) 

![img](wpsE57.tmp.jpg) 

![img](wpsE58.tmp.jpg) 

![img](wpsE59.tmp.jpg) 

特别注意：扩展插件中，其函数内部的this指向当前正在操作的jQuery对象，谨记！！！

# 三、jQuery中的each方法

## 1、each方法功能

jQuery对象的本质就是一个类数组的特殊对象。jQuery对象又比较特殊，其内部的每个元素又都是一个DOM对象。在实际项目开发中，我们除了可以使用for循环遍历jQuery对象以外，还可以使用each方法来遍历jQuery对象。

简单来说：each方法就是用来遍历jQuery对象

## 2、基本语法：

jQuery对象.each(function(i,item){

​	//每次遍历时，系统会将jQuery对象的索引下标放入变量i中

​	//每次遍历时，系统会将jQuery遍历的结果dom对象放入item中

});

## 3、案例1：使用each方法遍历div对象

![img](wpsE5A.tmp.jpg) 

## 4、案例2：使用each方法遍历img对象

![img](wpsE6A.tmp.jpg) 

总结：each方法的核心，就是实现对jQuery对象的遍历。

​	  其方法特别简单，只有一个参数，要求是一个function(i,item) {}

​	  i ：jQuery对象的索引下标

​	  item ：每次遍历时，得到的dom对象

# 四、jQuery中的Ajax（底层）

## 1、jQuery中的Ajax

两大类：① 底层实现 ② 高级实现

## 2、Ajax底层实现

基本语法：

$.ajax(options)

参数说明：$.ajax非常简单，只有一个参数，要求是一个JSON对象

{

​	“type”:代表发送的请求类型，get或post，默认get

​	“url”:代表请求的url地址

​	“async”：是否异步，默认为true，代表异步。false为同步

​	“cache”：是否缓存，get请求在IE浏览器下才有缓存，true，设置false代表没有缓存

​	“data”：代表请求的数据，一般是一个字符串

​	“dataType”： “text|xml|json”，代表期待的返回值类型

​	“success”：要求是一个匿名函数，如function(msg){}，msg代表Ajax请求成功后的返回结果

}

demo11_ajax.html 示例代码：

![img](wpsE6B.tmp.jpg) 

demo11.php 示例代码：

![img](wpsE6C.tmp.jpg) 

## 3、使用Ajax传输数据

demo12_username.html 示例代码：

![img](wpsE6D.tmp.jpg) 

demo12.php 示例代码：

![img](wpsE6E.tmp.jpg) 

## 4、Ajax的缓存问题

Ajax中的get请求在IE内核的浏览器下运行，其都会产生缓存。

![img](wpsE6F.tmp.jpg) 

在Ajax底层实现中，解决缓存so easy，只需要添加一个cache属性即可。

![img](wpsE70.tmp.jpg) 

由以上代码运行结果可知，Ajax底层实现是使用时间戳方式，解决的缓存问题。

![img](wpsE71.tmp.jpg) 

## 5、期待的返回值类型

Ajax执行完毕后，其可能会返回多种类型的数据。一般为:

① text文本类型

② xml类型

③ json类型

在$.ajax中，我们可以通过dataType来设置期待的返回值类型，默认返回text文本类型。

demo13_dataType.html 示例代码：

![img](wpsE72.tmp.jpg) 

![img](wpsE73.tmp.jpg) 

demo13.php 示例代码：

![img](wpsE74.tmp.jpg) 

# 五、jQuery中的Ajax（高级）

## 1、Ajax中高级实现的两种类型

$.get基本语法：

$.get(url,[data],[callback],[type]);

参数说明：

url ：请求的url地址

data ：请求时的数据，可以是字符串也可以是JSON格式

callback ：Ajax执行成功后，所执行的回调函数，function(msg) {}

type ：期待的返回值类型，”text|xml|json”

特别注意：如果$.get方法，没有传输的数据，第二个参数可以省略不写，直接第二个参数的位置写callback。

$.post基本语法：

$.post(url,[data],[callback],[type]);

参数说明：

url ：请求的url地址

data ：请求时的数据，可以是字符串也可以是JSON格式

callback ：Ajax执行成功后，所执行的回调函数，function(msg) {}

type ：期待的返回值类型，”text|xml|json”

特别注意：如果$.post方法，没有传输的数据，第二个参数可以省略不写，直接第二个参数的位置写callback。

## 2、$.get案例发送Ajax请求

![img](wpsE75.tmp.jpg) 

## 3、Ajax中的缓存问题

![img](wpsE76.tmp.jpg) 

在高级实现中，我们可以通过_=时间戳的形式来解决缓存问题。

![img](wpsE77.tmp.jpg) 

运行结果：

![img](wpsE78.tmp.jpg) 

## 4、Ajax+JSON实现二级联动菜单

![img](wpsE89.tmp.jpg) 

① 设计数据库

![img](wpsE8A.tmp.jpg) 

② 插入测试数据

![img](wpsE8B.tmp.jpg) 

③ 设计HTML页面

![img](wpsE8C.tmp.jpg) 

④ 编写jQuery代码

![img](wpsE8D.tmp.jpg) 

![img](wpsE8E.tmp.jpg) 

运行结果：

![img](wpsE8F.tmp.jpg) 

# 六、Ajax跨域请求（JSONP）

## 1、什么是跨域请求

所谓的跨域请求，就是从一个域名向另外一个域名发送Ajax请求。

本机域名：<http://localhost>

远程域名：<http://www.oa.com>

当我们在localhost中，想[www.oa.com](http://www.oa.com)中发送请求时，系统会自动禁止这种行为。

demo16_kuayu.html 示例代码：

![img](wpsE90.tmp.jpg) 

[www.oa.com/demo.php](http://www.oa.com/demo.php) 示例代码：

![img](wpsE91.tmp.jpg) 

由于受到浏览器（同源策略）的限制，系统不允许从一个域名向另外一个域名发送Ajax请求。

## 2、解决Ajax跨域问题

① 解决方案1：$.ajax中的get请求解决

![img](wpsE92.tmp.jpg) 

 

② 解决方案2：$.get方法解决

![img](wpsE93.tmp.jpg) 

 

③ 解决方案3：$.getJSON方法解决

![img](wpsE94.tmp.jpg) 

# 七、jQuery插件库

## 1、推荐网站

懒人图库：<http://www.lanrentuku.com/>

jQuery插件库：<http://www.jq22.com/>

## 2、推荐旋转插件

<http://www.jqueryrotate.com/> 官方网站

![img](wpsE95.tmp.jpg) 

## 3、百度UEditor/Echarts（重点）

UEditor ：

<https://ueditor.baidu.com/website/onlinedemo.html>

 

Echarts：

① 下载源代码 <http://www.echartsjs.com/download.html>

② 放入项目中，使用script标签引入

③ 把示例代码copy到项目页面中，更改数据

![img](wpsE96.tmp.jpg) 

④ 设计数据库

![img](wpsE97.tmp.jpg) 

⑤ 使用Ajax获取数据（同步模式）

![img](wpsEA7.tmp.jpg) 

⑥ 定义php后端页面

![img](wpsEA8.tmp.jpg) 

运行结果：

![img](wpsEA9.tmp.jpg) 
