---
title: vue01
date: 2019-02-20 15:41:35
tags: VUE
---
VUE前端框架(第2天)

# 第9讲 综合案例 品牌管理

​	昨天主要学习 VUE的基本使用 配置属性 基本指令

​	洗发水品牌列表(遍历显示数据)

品牌新增功能(表单)

品牌删除功能(数组操作)

品牌搜索功能(数组操作)

介绍基本样式:

![img](wps4E79.tmp.jpg) 
<!-- more -->
## 9.1 列表显示数据(v-for)

​	创建挂载点 创建品牌模拟数据

![img](wps4E7A.tmp.jpg) 

 

 

 

 

 

 

 

 

编写表格网页元素 并使用v-for指令打印tr标签

![img](wps4E8A.tmp.jpg) 

​	分配key属性时必须使用v-bind指令 否则按照原文分配key属性造成冲突

 

 

 

 

## 9.2 空数据提示(v-if/v-show)

​	创建空数据提示行 并通过v-show指令判断list数据长度 实现提示行显示与隐藏

![img](wps4E8B.tmp.jpg) 

 

 

 

 

## 9.3 添加数据功能(v-model v-on)

​	按照用户操作顺序 编写项目 缺啥补啥

​	需要新增品牌的信息输入框 新增按钮

![img](wps4E8C.tmp.jpg) 

在vue实例中 添加需要用到的数据 id和name

![img](wps4E8D.tmp.jpg) 

 

 

 

给新增按钮添加点击事件 编写事件所用方法add

![img](wps4E8E.tmp.jpg) 

![img](wps4E8F.tmp.jpg) 

体现出vue的优势 专注于业务逻辑 不要处理渲染问题

 

 

 

 

 

 

 

 

 

## 9.4 原生JS数组遍历操作

​	push pop shift unshift

​	遍历操作有6大方法

​		forEach map filter every some findIndex

​	

​	arr.forEach(回调函数) 回调函数每遍历一个数组元素就调用一次

​	arr.forEach(function(item,index,arr){

​		//item代表正在遍历的元素

​		//index代表正在遍历的索引

​		//arr代表正在遍历的元素来源于哪个数组

})

![img](wps4E90.tmp.jpg) 

 

 

​	arr.map 对数组元素进行批量处理 会返回一个新数组 不改变旧数组

![img](wps4E91.tmp.jpg) 

​	arr.filter 根据条件 过滤数组元素 会返回一个新数组 不改变旧数组

![img](wps4E92.tmp.jpg) 

​	要能接受代码的变形运用

![img](wps4E93.tmp.jpg) 

 

 

 

 

 

 

 

 

​	arr.every 试图证明每个数组元素都符合条件

​		遍历数组元素 如果当前元素符合条件 继续遍历

​					 如果当前元素不符合条件 停止遍历 返回false

​		遍历完所有元素都符合条件 停止遍历 返回true

![img](wps4E94.tmp.jpg) 

​	arr.some 试图证明一个数组元素符合条件

​		遍历数组元素 如果当前元素符合条件 停止遍历 返回true

​					 如果当前元素不符合条件 继续遍历

​		遍历完所有元素都不符合条件 停止遍历 返回false

![img](wps4E95.tmp.jpg) 

​	arr.findIndex 返回第1个符合条件的数组元素索引(跟字符串indexOf)

![img](wps4E96.tmp.jpg) 

## 9.5 删除数据功能(事件修饰符 数组遍历操作)

​	filter 意图是过滤数据 true保留数据 false过滤数据

​	some 意图证明至少一个元素符合提交

true这个元素就是我们要找的元素 

false继续遍历

​	findIndex 意图找到符合条件的元素索引

 

​		如何通过索引删除数组元素 arr.splice(索引,个数)

![img](wps4E97.tmp.jpg) 

 

​	按照用户操作的顺序 缺啥补啥 要在操作列中添加删除功能入口

![img](wps4E98.tmp.jpg) 

 

 

​	封装事件所需的del方法 在方法中用3种方式实现了数据删除

![img](wps4E99.tmp.jpg) 

![img](wps4EAA.tmp.jpg) 

![img](wps4EAB.tmp.jpg) 

 

 

 

 

 

 

## 9.6 关键字搜索功能(v-for结合函数 filter方法)

​	按照用户操作的顺序编写项目 添加关键字文本框 和搜索按钮

![img](wps4EAC.tmp.jpg) 

​	修改了遍历的数据源 遍历search方法返回的搜索结果数组

![img](wps4EAD.tmp.jpg) 

 

 

​	编写search方法 注意 通过实例化构造器形式在正则中解析变量

![img](wps4EAE.tmp.jpg) 

 

 

## 9.7 数据过滤器

​	所谓过滤器 在模板中 可以在数据显示之前 对数据进行一些处理操作

​	{ { 数据 | 过滤器 } }

​		| 通常叫管道符

​		过滤器本质是函数

​	Vue.filter(过滤器名,回调函数)

​	过滤器会把管道符左侧的数据 当做第1个实参 自动传入

![img](wps4EAF.tmp.jpg) 

 

 

 

 

 

​	把智慧两个字 替换 为菩提

![img](wps4EB0.tmp.jpg) 

​	关于过滤器的高级使用场景

![img](wps4EB1.tmp.jpg) 

​	管道符左侧的数据会作为过滤器的第1个实参 通过第1个形参接收

其他手动传入的实参 要从第2个形参开始接受

## 9.8 全局与私有过滤器

​	过滤器有全局和私有之分

​	全局与私有过滤器的定义形式不同 使用范围也不相同

![img](wps4EB2.tmp.jpg) 

 

 

 

 

 

 

 

​	私有过滤器需要在当前Vue实例中 通过filters属性定义

私有过滤器只能在当前Vue实例范围中使用

![img](wps4EB3.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 9.9 完善时间格式(过滤器 padStart/padEnd)

​	找到表格中显示时间的位置 调用过滤器

![img](wps4EB4.tmp.jpg) 

​	选用全局过滤器解决问题

![img](wps4EB5.tmp.jpg) 

 

 

​	padStart和padEnd方法都是字符串类型方法

![img](wps4EB6.tmp.jpg) 

​	padStart在头部补全字符串 原始字符的左侧

​	padEnd  在尾部补全字符串 原始字符的右侧

​	padStart(要补全到多少位,用什么字符补全)

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 9.10 按键修饰符

​	使用Vue绑定键盘事件时 可以用按键修饰符约束触发事件的特定按键

![img](wps4EB7.tmp.jpg) 

​	在绑定键盘事件时 可以在事件类型后 @keydown.enter

![img](wps4EB8.tmp.jpg) 

 

 

​	知道每个按键对应的 keycode 在VUE事件中仍然可以使用event事件对象

![img](wps4EB9.tmp.jpg) 

 

​	VUE允许直接使用 按键编码作为按键修饰符

![img](wps4ECA.tmp.jpg) 

 

​	VUE允许自己封装自定义的按键修饰符 比如把wsad绑定为方向键按键修饰符

​	Vue.config.keyCodes.按键修饰符名 = 按键值

![img](wps4ECB.tmp.jpg) 

## 9.11 完善添加功能(按键修饰符)

​	优化用户输入体验 给品牌信息文本框添加回车事件

![img](wps4ECC.tmp.jpg) 

​	用户输入完成后 清空表单中的旧信息

![img](wps4ECD.tmp.jpg) 

 

 

 

 

 

 

 

 

## 9.12 自定义指令

​	vue提供了很多指令 它们都是以v-开头 v-指令前缀

​	不同的指令负责不同的功能 Vue允许自己定义指令实现特殊需求

​	Vue.directive(指令名,回调函数) //指令名不带v-前缀

​	<元素 v-指令名=”指令表达式”></元素>

 

​	希望封装一个改变背景颜色的指令 v-color

![img](wps4ECE.tmp.jpg) 

​	如果真的要修改一个网页元素的背景颜色

​		dom.style.backgroundColor = blue

​		目前缺少两个重要参数 dom对象 指令表达式结果

 

​	自定义指令参数

​	Vue.directive(指令名,function(el,binding){

​		el:代表调用指令的网页元素 在上述案例中el就是p元素

​		binding:是一个数据对象

​			binding.name:不带v-前缀的指令名 在上述案例中color

​			binding.expression:指令表达式的原文 在上述案例中color

​			binding.value:指令表达式的解析结果 在上述案例中blue

})

![img](wps4ECF.tmp.jpg) 

 

 

 

 

## 9.13 指令钩子函数

​	钩子函数 在某个特殊时刻 自动调用的一种函数

​	定义指令时 可以设置多个钩子函数

​	Vue.directive(指令名,{钩子函数对象})

​	Vue.directive(指令名,{

​		//bind 当指令被绑定到元素上时 触发的钩子函数 元素还未追加到DOM中

​		//inserted 当网页元素已经追加到DOM树时 触发的钩子函数

​		bind(){},

​		inserted(){}

})

![img](wps4ED0.tmp.jpg) 

 

![img](wps4ED1.tmp.jpg) 

## 9.14 搜索栏自动聚焦功能(自定义指令 钩子函数) 

​	了解DOM元素追加的时间点对功能的影响

![img](wps4ED2.tmp.jpg) 

​	实现项目搜索框 自动聚焦功能

![img](wps4ED3.tmp.jpg) 

 

​	封装自定义指令 实现聚焦功能

![img](wps4ED4.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 9.15 全局与私有指令

​	指令也是有全局和私有之分的

​	全局指令是在全局作用域中定义的 并 可以在任意VUE实例中使用

​	私有指令时在VUE实例中定义的 只能在当前实例中使用

![img](wps4ED5.tmp.jpg) 

 

 

 

​	私有指令通过 directives 属性 进行定义的

![img](wps4ED6.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

# 课程小结 项目小结

​	六个数组遍历操作方法

​		arr.forEach

​		arr.map 批量处理

​		arr.filter 数据过滤

​		arr.every 每个数据检测

​		arr.some 至少一个数据监测

​		arr.findIndex 返回第1个符合条件元素索引

​	字符串补全方法

​		padStart 从左侧补全

padEnd  从右侧补全

str.padStart(补全到多少位,用什么字符补)

​	全局过滤器

​		Vue.filter(过滤器名,回调函数)

​	私有过滤器

​		filters属性

​	全局指令

​		Vue.directive(指令名,钩子函数)

​	私有指令

​		directives属性

 

​	按键修饰符 针对键盘事件 @keydown.按键修饰符

​		以按键名作为按键修饰符

![img](wps4ED7.tmp.jpg) 

​		使用按键值作为按键修饰符 @keydown.enter 等价于 @keydown.13

​		VUE允许自定义按键修饰符 Vue.config.keyCodes.自定义按键修饰符 = 按键值

​		实例化Vue时可传入的配置属性

new Vue({

​	el:挂载点

​	data:数据

​	methods:方法

​	template:模板

​	computed:计算属性

​	watch:监听器/侦听器

​	filters:私有过滤器

​	directives:私有指令

})

# 第10讲 Vue生命周期钩子函数

## 10.1 生命周期与钩子函数

​	在官方文档中有一个生命周期图示

<https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA>

 

​	生命周期 从实例化(new Vue开始)到 看得见数据(数据渲染到页面中)

​	这个过程中经历了很多步骤 生命周期

![img](wps4ED8.tmp.jpg) 

 

 

 

​	生命周期钩子函数 在生命周期的过程中 会按照时间顺序 自动调用的函数

![img](wps4EE8.tmp.jpg) 

![img](wps4EE9.tmp.jpg)

 

 

 

 

 

 

关于渲染的钩子函数

![img](wps4EEA.tmp.jpg) 

​	除了从实例化到看见数据之外 在持续运行的过程中也有生命周期和钩子函数

![img](wps4EEB.tmp.jpg) 

 

 

 

 

 

## 10.2 ref属性引用DOM元素

​	在编写VUE项目时 有时需要获取DOM元素

​	在使用VUE时不推荐使用原生js获取dom元素 不推荐同时使用jQuery

 

​	使用ref属性 在要捕获的元素上 打上标记 ref中给元素命名

![img](wps4EEC.tmp.jpg) 

​	被ref属性标记过的元素 会被保存到vue实例.$refs属性中

![img](wps4EED.tmp.jpg) 


 

 

​	如果要在方法中访问这些元素 vue实例.$refs属性

![img](wps4EEE.tmp.jpg) 

 


 

## 10.3 ref属性与生命钩子函数的关系

​	数据渲染前后 beforeMount mounted

![img](wps4EEF.tmp.jpg) 

![img](wps4EF0.tmp.jpg) 

# 课程小结 生命周期钩子函数小结

​	学习了6个钩子函数

​			//beforeCreate

1、 创建vue实例

//created 是能够读写vue数据的最早时间

//beforeMount

2、 渲染数据

//mounted 是能够使用$refs属性的最早时间点

//beforeUpdate

3、 更新数据

//updated

 

​	如果要在vue中使用ref属性捕获DOM元素 不能在页面中手动调用函数使用

![img](wps4EF1.tmp.jpg) 

​	可以在事件中直接使用$refs中保存的DOM元素引用关系

​	如果用户能够看到并且触发事件 数据的渲染(mounted)一定已经完成了