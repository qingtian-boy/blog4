---
title: vue00
date: 2019-02-20 15:38:42
tags: VUE
---
VUE前端框架(第1天)

# 第1讲 VUE简介

## 1.1 前端环境与思维方式

项目代码的运行环境主要是客户端

与用户的距离更近

 

一开始前端主要是 设计良好的界面样式

前端已经不是单纯地讨好用户，让用户更高效更舒适地使用应用

 
<!-- more -->
前端工作分两方面:

​		业务逻辑  现在可以让前端来分担一些业务逻辑

​		用户交互  科学美观界面 界面之间良好调度

 

 

 

 

 

 

 

 

## 1.2 VUE是什么

Vue是目前最火的前端框架![img](wpsD992.tmp.jpg)

许多成功的项目都正在使用vue框架 滴滴、美团、faceu

vue在github上收到的点赞数最多

 

近年来,互联网领域发生了很多变化

客户端普遍变强大 用户的胃口也越来越大 提现到技术上 项目结构越来越复杂

![img](wpsD993.tmp.jpg)![img](wpsD994.tmp.jpg) 

为了解决文件繁多 功能复杂 依赖关系不统一 就产生了对前端框架的需求

 

框架可以节省开发成本 让文件结构非常清晰 把不同工作种类区分开来

 

VUE能把UI样式等部分 与 前端业务逻辑分离开来

使用vue能够大量提高前端开发效率 让项目更容易维护

 

 

## 1.3 为什么要重视VUE框架

直接看招聘需求:

![img](wpsD995.tmp.jpg) 

![img](wpsD996.tmp.jpg) 

 

 

 

 

 

## 1.4 VUE官网资源与框架下载

​	从百度 VUE即可看到官网链接

官网地址: <https://cn.vuejs.org/>

![img](wpsD997.tmp.jpg) 

​	本次课程选用2.5以上版本

![img](wpsD998.tmp.jpg) 

 

 

 

 

 

 

 

# 第02讲 初识Vue

## 2.1 使用VUE

​	使用vue框架非常简单 就像jQuery一样 先在HTML引入vue.js 然后就可以使用了

![img](wpsD999.tmp.jpg) 

​	只要引入了这个文件 window全局作用域下就会提供一个Vue的构造器

​	在实例化Vue构造器时 需要传入配置项参数

![img](wpsD99A.tmp.jpg) 

 

 

 

 

 

 

 

第一个案例:

![img](wpsD99B.tmp.jpg) 

VUE能够把数据渲染到指定的网页元素上

用{{插值表达式}} 来使用vue data属性中定义的数据

逻辑表达式 算数表达式 字符串拼接都是表达式

![img](wpsD99C.tmp.jpg) 

插值表达式中可以写很多种代码 不要刻板认为只能写变量名/数据名

 

 

## 2.2 划分MVVM

​	因为Vue是一种 MVVM结构的框架

​	M:页面模型: 负责保存单个页面中需要用到的数据

​	V:页面视图:  用户看到的界面内容就是视图 页面中用于显示数据的HTML结构

​	VM: (模型与视图之间的调度者): 负责把数据分配显示到指定的结构中

 

​	回头观察2.1案例 找出上述三个部分:

![img](wpsD99D.tmp.jpg) 

​	VUE框架对于PHP学生来说有点像代替了smarty

1、 节省了模板引擎在服务器消耗的资源

2、 让数据渲染的工作在前端完成 进一步实现了前后端分离

 

 

 

## 2.3 挂载点与模板

​	el属性 element的缩写

​	实例化vue必须用el属性指定要操控的区域 也就是挂载点

![img](wpsD99E.tmp.jpg) 

​	vue不会识别挂载点之外的插值表达式 只对挂载点中的内容产生作用

​	挂载点中的所有内容 就是模板/视图

​	![img](wpsD99F.tmp.jpg)

​	vue可以通过template属性来定义模板 

注意: 一个el:代表一个容器 template:标签会占用整个el:其它的标签会被删除

![img](wpsD9A0.tmp.jpg) 

el template data VUE还有更多其他的属性 用来实现更多功能

# 第3讲 VUE指令热身

​	VUE还有很多强大和复杂的功能,有部分是用指令实现的

## 3.1 v-text指令

​	VUE的指令就是在VUE中封装的特殊的标签属性

​	<标签 v-text=’数据名’></标签>

​	可以把指定数据(覆盖)填充到当前标签中 

![img](wpsD9A1.tmp.jpg) 

![img](wpsD9B1.tmp.jpg) 

## 3.2 v-html指令

​	<标签 v-html=’数据名’></标签>

![img](wpsD9B2.tmp.jpg) 

 

 

 

 

 

 

 

 

## 3.3 v-bind指令

​	如果渲染数据的位置正好是一个属性 会怎样

![img](wpsD9B3.tmp.jpg) 

![img](wpsD9B4.tmp.jpg) 

​	v-bind指令专门用于给标签属性 绑定动态数据

​	v-bind:属性名 = 数据名

![img](wpsD9B5.tmp.jpg) 

 

 

 

 

## 3.4 指令与表达式

​	指令 等号的右边 都可以写表达式

​	用v-bind指令 实现对超链接的 href属性处理

![img](wpsD9B6.tmp.jpg)
	指令当中不仅可以写数据名 还可以写表达式

## 3.5 data属性对数据的处理

![img](wpsD9B7.tmp.jpg) 

​	如果想手动调用vue中定义的数据 Vue对象.数据名

# 第4讲 v-on事件绑定

## 4.1 v-on指令

​	给按钮绑定一个点击事件

![img](wpsD9B8.tmp.jpg) 

​	事件就是 满足特定条件时 浏览器会自动执行一些代码

​	事件包括三要素 网页元素.事件类型=事件处理函数

​		dom.onclick = function

​		$().click(function(){})

​	在采用了VUE框架之后,要尽量使用VUE的方式实现事件绑定,不推荐再使用jQuery的方式甚至是原生的方式实现事件绑定.

​	 v-on:事件类型 = 函数名

 

请注意:

1、 vue事件绑定中使用的函数 要用methods属性进行注册

2、 v-on指令绑定事件时 事件类型不带on前缀的

![img](wpsD9B9.tmp.jpg) 

 

 

 

 

 

 

3、data定义的数据和methods定义的方法 要避免重名

![img](wpsD9BA.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

## 4.2 指令缩写

​	定义属性值 事件绑定 这些工作属于高频工作

​	我们会经常使用到 v-bind和v-on这两个指令

​		v-bind:属性 à :属性

​		v-on:事件类型 à @事件类型

![img](wpsD9BB.tmp.jpg) 

 

 

 

 

 

 

## 4.3 this关键字

​	在vue的事件函数 this关键字指向谁

![img](wpsD9BC.tmp.jpg) 

![img](wpsD9BD.tmp.jpg) 

​	methods中定义的函数里 this指向当前vue对象 可以通过this关键字来访问vue绑定的数据和方法

 

 

 

 

 

 

 

## 4.4 训练案例 VUE制作跑马灯效果

​	预期效果:

​		点击滚动按钮开始滚动,点击停止按钮停止滚动

![img](wpsD9BE.tmp.jpg) 

构建网页元素:

![img](wpsD9BF.tmp.jpg) 

![img](wpsD9C0.tmp.jpg) 

​	发现VUE帮我们省略了DOM操作 数据修改完成后会自动渲染到对应位置

 

​	完成两个按钮的事件:

![img](wpsD9C1.tmp.jpg) 

 

 

 

 

 

 

## 4.5 事件修饰符

​	学习原生事件时,学了很多与事件相关的知识点:

​		1、事件监听 兼容性问题

​		2、事件冒泡 事件捕获

​		3、事件默认行为

 

​	Vue的事件也会需要上述相关问题,提供了事件修饰符解决这些问题

​		v-on:事件类型.事件修饰符

 

​		.stop 阻止冒泡

​		.prevent 阻止默认行为

​		.capture 把事件触发模式 从默认的冒泡模式切换到捕获模式

​		.self 只允许事件由元素本身触发

​		.once 规定当前事件只能触发一次

 

 

 

 

 

 

 

 

​	.stop阻止冒泡

![img](wpsD9D2.tmp.jpg) 

![img](wpsD9D3.tmp.jpg) 

 

 

 

 

​	.prevent阻止默认行为 网页元素本身具有的行为 表单的提交按钮  超链接

![img](wpsD9D4.tmp.jpg) 

​	.capture 把默认冒泡模式 切换成 捕获模式

![img](wpsD9D5.tmp.jpg) 

 

 

 

 

 

​	.self 事件只能由元素本身触发

​	难道一个元素的事件能够由其他元素触发吗?

![img](wpsD9D6.tmp.jpg) 

​	self只会约束 事件只能由元素自身触发 但是不会影响事件传递

 

​	.once 绑定一次性事件

![img](wpsD9D7.tmp.jpg) 

 

 

 

# 课程小结 VUE的基本使用

![img](wpsD9D8.tmp.jpg) 

​	data 定义当前界面需要用的数据

​	methods 定义当前界面需要用的方法

![img](wpsD9D9.tmp.jpg) 

template 可以用于定义模板

指令:

​	:属性 动态绑定标签属性值

​	@事件 绑定VUE事件

# 第5讲 v-model表单数据绑定(双向数据绑定)

## 5.1 v-model指令

​	什么是双向数据绑定 什么又是单向数据绑定

在Vue中定义的数据 只要发生修改 就会马上体现在网页上,这种特性叫做单向数据绑定

![img](wpsD9DA.tmp.jpg) 

​	这种数据同步/绑定 是单方向的 修改网页内容 不会影响VUE中的数据

 

 

 

 

 

 

 

 

 

 

​	有些时候我们希望这种绑定关系是双向的:

​	在用户填写表单元素时 如果用户填写的数据 能够实时地同步到VUE的数据中

​	那就好了 这种需求就是双向数据绑定,用v-model指令实现

![img](wpsD9DB.tmp.jpg) 

​	要注意 v-model以及双向数据绑定的需求是专门针对表单元素

​	如果在非表单元素中使用v-model指令会报错:

![img](wpsD9DC.tmp.jpg) 

## 5.2 Vue-devtools安装

![img](wpsD9DD.tmp.jpg) 

![img](wpsD9DE.tmp.jpg) 

在浏览器右上角的三个点中 找到扩展程序的管理界面

![img](wpsD9DF.tmp.jpg) 

 

 

开启开发者模式,选择 加载已解压的扩展程序:

![img](wpsD9E0.tmp.jpg) 

确保VUE开发工具打开 重启浏览器

![img](wpsD9F1.tmp.jpg) 

发现在浏览器地址栏旁 多了一个VUE开发工具按钮:

![img](wpsD9F2.tmp.jpg) 

还可以从F12开发人员工具中发现多了一个VUE面板

![img](wpsD9F3.tmp.jpg) 

通过这种方式查看双向数据绑定的数据变化更加方便了

## 5.3 训练案例 VUE制作网页计算器

​	构建网页元素

![img](wpsD9F4.tmp.jpg) 

了解eval函数的作用

![img](wpsD9F5.tmp.jpg) 

了解eval函数对json数据处理的妙用

![img](wpsD9F6.tmp.jpg) 

![img](wpsD9F7.tmp.jpg) 

完成运算事件:

![img](wpsD9F8.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 5.4 数据绑定修饰符

​	v-model也支持几种修饰符:

​		v-model.修饰符

1、 v-model.lazy 懒绑定 添加了这个修饰符之后 

数据的同步会在用户输入完成后再进行

![img](wpsD9F9.tmp.jpg) 

![img](wpsD9FA.tmp.jpg) 

​	2、v-model.trim 去空 去掉作用两边的空 中间的空不影响

![img](wpsD9FB.tmp.jpg) 

 

 

​	3、v-model.number 数值转换

​		表单文本框中填写的内容默认都是字符串类型

​		字符串类型参与加法运算会被认为是字符串拼接

![img](wpsD9FC.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

# 第6讲 计算属性与侦听器

## 6.1 计算属性

​	有时我们需要在前端处理业务逻辑 需要对原始数据进行运算才能使用数据

​	如下案例:

![img](wpsD9FD.tmp.jpg) 

 

 

 

 

1、 可以在插值表达式中直接完成拼接

![img](wpsD9FE.tmp.jpg) 

如果在模板中写太过复杂的逻辑处理 就会导致模板过重 破坏了工作分离的思想

2、 可以通过vue的方法实现 methods定义方法

![img](wpsD9FF.tmp.jpg) 

 

 

 

 

插值表达式可以手动调用 通过methods属性注册的vue方法

![img](wpsDA00.tmp.jpg) 

 

 

 

 

 

 

 

​	3、可以使用计算属性来实现 计算属性是 由原始数据通过运算得来的数据

​	要用computed属性 注册计算属性

![img](wpsDA01.tmp.jpg) 

 

 

 

 

 

 

## 6.2 计算属性与方法的不同

​	1、计算属性的引用方式与方法的引用方式不同 方法要加括号调用 计算属性直接使用

​	2、计算属性的性能比methods中的方法更佳

​	计算属性只会在 依赖数据发生改变时调用

​	计算属性用到哪些数据计算出结果 那些数据就是依赖数据

![img](wpsDA02.tmp.jpg) 

​	methods中的方法 就会每一次调用都要执行函数体代码

​	computed中的计算属性 如果原始数据没有发生变化 只会在第一次真正的进行运算

​	后续延用计算缓存

 

 

 

 

 

 

 

 

## 6.3 侦听器

​	侦听器通过 watch的属性 添加

​	侦听器主要是检测 数据是否发生改变 如果发生改变触发对应的侦听器

![img](wpsDA12.tmp.jpg) 

# 课程小结 VUE的属性

​	new Vue({配置属性})

​		el:定义挂载点

​		data:定义数据

​		methods:定义方法 方法可以手动调用 也可以用于事件绑定

​		template:定义模板

​		computed:定义计算属性 用于对原始数据进行一些处理

​		watch:定义侦听器/监听器 用于对数据的变化进行处理	

# 第7讲 样式绑定

## 7.1 class样式绑定

​	定义一些类样式用于测试

​	1、可以通过v-bind:class 绑定类样式 指令右侧的区域要写表达式 要注意字符串引号

![img](wpsDA13.tmp.jpg) 

 

 

​	2、可以通过数组形式批量绑定样式

![img](wpsDA14.tmp.jpg) 

3、在第2点的基础上 可以结合 三目运算 来实现对样式的管控

![img](wpsDA15.tmp.jpg) 

​	4、还可以通过VUE的数据渲染样式 要小心vue数据名不能用到关键字

![img](wpsDA16.tmp.jpg) 

![img](wpsDA17.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 7.2 style样式绑定

​	可以用v-bind指令绑定style属性

​	1、可以直接用对象形式绑定样式

![img](wpsDA18.tmp.jpg) 

​	2、通过vue数据绑定单个或多个样式

![img](wpsDA19.tmp.jpg) 

​	vue本身是希望样式、业务逻辑、模板元素分离 学习组件之后会有专门的样式分离

# 第8讲 循环与判断

## 8.1 v-for指令的四种用法

​	设定用于遍历的数据

![img](wpsDA1A.tmp.jpg) 

 

 

 

 

 

 

​	1、遍历一维数组

![img](wpsDA1B.tmp.jpg) 

![img](wpsDA1C.tmp.jpg) 

 

 

 

 

 

 

2、遍历二维/复合数组

![img](wpsDA1D.tmp.jpg) 

 

 

 

 

 

 

 

 

3、遍历对象

![img](wpsDA1E.tmp.jpg) 

4、简单循环 单纯的想要进行指定次数的循环

![img](wpsDA1F.tmp.jpg) 

## 8.2 v-for指令与key的注意事项

​	在vue官方文档中有一条针对v-for的警告信息:

![img](wpsDA20.tmp.jpg) 

​	观察以下案例 ,发现 v-for不用key的后果:

​	人员信息列表

![img](wpsDA21.tmp.jpg) 

 

 

 

 

 

 

​	vue部分代码:

![img](wpsDA32.tmp.jpg) 

​	观察vue的功能:

![img](wpsDA33.tmp.jpg) 

![img](wpsDA34.tmp.jpg) 

​	vue能够在刷新/重新渲染列表数据时帮我们记住表单元素的状态

​	vue忽略了表单元素与遍历数据之间的关联关系  忽略了批次关系

​	可以通过v-bind指令 在遍历的元素中绑定一个key属性 说明每次遍历的批次

![img](wpsDA35.tmp.jpg) 

​	key是具有唯一性的

![img](wpsDA36.tmp.jpg) 

 

 

 

 

 

 

## 8.3 v-if与v-show

​	v-if和v-show的功能是相近的 通过判断简单条件 决定显示或隐藏网页元素

![img](wpsDA37.tmp.jpg) 

​	v-if和v-show隐藏元素的实现方式不同

​	v-if通过dom操作性能消耗比较大

​	v-show通过样式display控制元素的隐藏

如果网页元素的内容很多,频繁切换v-if的显示与隐藏会导致性能开销很大,可能导致浏览器卡顿。建议使用v-show,能够优化大量性能。

 

 

 

 

# 课程小结 VUE指令

​	v-text:把普通文本数据渲染到当前元素的文本内容中 会覆盖原本的文本内容

​	v-html:把HTML代码渲染到当前元素的文本内容中 可以解析HTML元素

​	v-bind:把数据解析到标签属性中 缩写 :属性名

​	v-on: 绑定事件 缩写 @事件类型

​	v-model: 双向数据绑定/获取表单元素值

​	v-for: 循环 通常用于遍历显示数据

v-if、v-show: 控制元素的显示与隐藏 推荐使用v-show