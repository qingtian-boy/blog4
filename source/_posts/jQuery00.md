---
title: jQuery00
date: 2019-02-20 14:49:15
tags: JQ JS
---
# jQuery从入门到精通

# 一、jQuery概述

## 1、什么是jQuery

jQuery 就是一款免费且开放源代码的JavaScript代码库。

提供了HTML文档操作，节点查找，事件处理，动画设计，Ajax交互等丰富的功能；

并且兼容各种主流浏览器；

jQuery口号：Write Less,Do More
<!-- more -->
## 2、下载jQuery

![img](wps8D41.tmp.jpg) 

## 3、使用jQuery

![img](wps8D42.tmp.jpg) 

# 二、jQuery选择器

## 1、什么是选择器

在jQuery中，一共提供了50多种方法来实现对页面中的元素进行选择。我们在实际项目开发中，只需要掌握其中的几个即可。

## 2、基本选择器（重点）

\#id ：根据元素的id属性获取元素，如$(‘#btnok’)

.class ：根据元素的class属性获取元素，如$(‘.cls’)

element ：根据元素的标签名称获取元素，如$(‘div’)

selector1,selector2,selectorN ：群组选择器，如$(‘div,p)

基本选择器的底层就是通过document.querySelector来获取元素

demo09_jiben.html 示例代码：

![img](wps8D43.tmp.jpg) 

## 3、层级选择器（重点）

ancestor空格descendant ：选择祖先元素下的所有后代元素（子元素、孙元素、孙孙元素...）

parent > child ：选择父元素下的所有子元素（子元素，一级关系）

prev + next ：选择当前元素紧邻的下一个同级元素

prev ~ siblings ：选择当前元素紧邻的所有下面的同级元素

demo10_cengji.html示例代码：

![img](wps8D44.tmp.jpg) 

## 4、其他选择器（重点）

:eq(index) ：索引选择器，根据元素的索引下标获取元素，默认从0开始

![img](wps8D45.tmp.jpg) 

## 5、表单选择器（重点）

:input ：获取页面中所有的表单元素，包括：input, textarea, select 和 button 元素

:text ：获取页面中所有的文本框

:checkbox ：获取所有的复选框

:checked ：获取所有选中的复选框

:selected ：获取被选中的下拉选框

demo12_form.html 示例代码：

![img](wps8D46.tmp.jpg) 

![img](wps8D47.tmp.jpg) 

# 三、DOM对象与jQuery对象（重点）

## 1、DOM对象

在JavaScript代码中，通过document.方式获取的所有对象都是DOM对象。

![img](wps8D48.tmp.jpg) 

运行结果：

![img](wps8D49.tmp.jpg) 

## 2、jQuery对象

在jQuery代码中，通过$()方式获取的对象都是jQuery对象。

![img](wps8D4A.tmp.jpg) 

运行结果：

![img](wps8D4B.tmp.jpg) 

所以由以上运行结果可知：DOM对象与jQuery对象是两个完全不同的对象。因为DOM对象下拥有很多属性，比如innerHTML。但是jQuery对象下，只有1个我们熟悉的属性，叫做length。

## 3、DOM对象与jQuery对象的关系

虽然DOM对象与jQuery对象是两个不同的对象，但是两者还有一定的关系。jQuery对象的本质是一个数组（因为拥有length属性，在JS中，只有数组才有length属性），但是其每个数组元素又是一个DOM对象。

![img](wps8D4C.tmp.jpg) 

## 4、DOM对象与jQuery对象的相互转换

① jQuery对象转换为DOM对象

DOM对象 = jQuery对象[索引下标]

![img](wps8D4D.tmp.jpg) 

② DOM对象转换为jQuery对象

jQuery对象 = $(DOM对象);

![img](wps8D4E.tmp.jpg) 

## 5、作业

获取页面中所有被选中的复选框的值。（提示：DOM对象或jQuery对象）

![img](wps8D4F.tmp.jpg) 
