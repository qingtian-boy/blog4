---
title: jQuery01
date: 2019-02-20 14:51:20
tags: JQ JS
---
# jQuery从入门到精通



 

# 一、DOM对象与jQuery对象（重点）

## 1、DOM对象

在JS中，通过document.方式获取的对象都是DOM对象

## 2、jQuery对象

在jQuery中，通过$()方式获取的对象都是jQuery对象

<!-- more -->

DOM对象转换jQuery对象 ：$(DOM对象)

jQuery对象转换为DOM对象 ：jQuery对象[索引]

## 3、获取复选框中被选中的元素（练习）

demo01_fuxuan.html 示例代码：

![img](wps20B1.tmp.jpg) 

# 二、jQuery中相关方法

在jQuery中，我们经常使用的属性只有一个，length属性：代表获取jQuery对象的长度。其jQuery中，大部分都是方法()。

## 1、attr方法：属性（重点：添加、删除、查询）

<img  src=’photo.jpg’  width=’400’  height=’400’ />

基本语法：

attr(name) ：根据属性的名称获取元素的属性值

attr(key,value) ：设置元素的属性，key属性，value属性值

attr(properties) ：一次为元素设置多个属性，要求参数是一个json格式数据

attr(key,fn) ：通过函数的返回值设置元素的属性

removeAttr(name) ：移除元素的属性

demo02_attr.html 示例代码：

![img](wps20B2.tmp.jpg) 

![img](wps20B3.tmp.jpg) 

## 2、class方法：class属性（添加与删除）

<div class=”cls”></div>

基本语法：

addClass(class) ：为元素添加class属性

removeClass(class) ：移除元素的class属性

toggleClass(class) ：切换元素的class属性，如果有就移除，没有就添加

hasClass(class) ：判断元素是否具有某个class属性,返回布尔类型

demo03_class.html 示例代码：

![img](wps20B4.tmp.jpg) 

## 3、css方法：style属性（获取、设置）

<div  style=”width:400px; height:200px; background-color:red”></div>

基本语法：

css(name) ：根据name获取元素的css属性值

css(name,value) ：设置元素的属性值

css(properties) ：一次为元素设置多个属性值，要求参数是一个json格式数据

demo04_css.html 示例代码：

![img](wps20B5.tmp.jpg) 

## 4、offset方法：获取元素的位置

<div  id=’food’  style=”position:absolute;  left:50px;  top:50;”></div>

基本语法：

offset() ：获取元素的横纵坐标，返回一个JSON对象，拥有left与top属性

offset(coordinates) ：设置元素位置要求是一个JSON格式数据，必须包含left与top属性

demo05_offset.html 示例代码：

![img](wps20B6.tmp.jpg) 

## 5、宽高设置（必须的）

<img  src=”photo.jpg”  width=”143”  height=”203” />

基本语法：

width() ：获取元素的宽度

width(value) ：设置元素的宽度

height() ：获取元素的高度

height(value) ：设置元素的高度

demo06_wh.html 示例代码：

![img](wps20B7.tmp.jpg) 

## 6、文本 & 表单值（重点）

<div><span>div1</span></div>

<input  type=”text”  value=”” />

 

DOM对象：

innerHTML/innerText进行设置

.value进行设置

 

jQuery对象：

html() ：获取元素的内容

html(val) ：设置元素的内容

 

text() ：获取元素的文本内容

text(val) ：设置元素的文本内容

 

val() ：获取表单元素的value值

val(val) ：设置表单元素的value值

demo07_text.html 示例代码：

![img](wps20B8.tmp.jpg) 

# 三、jQuery中事件编程

## 1、jQuery中的事件编程

为jQuery对象绑定相关的事件以及事件的处理程序。

## 2、页面载入事件（背下来window.onload与ready区别）

① DOM对象事件绑定，window.onload = function() {}

② jQuery对象事件绑定，$(function(){})，完整写法：

$(document).ready(function(){

​	//jQuery对象中的事件绑定

});

 

问题：window.onload与ready方法都是页面载入，两者有何区别呢？

答：两者虽然都可以完成页面载入事件，但是两者执行的顺序不同：

​    ① window.onload必须等待页面中的所有元素（DOM树、外部资源）全部加载完毕后，才开始执行。

​	② ready方法只需要等待页面中的DOM树加载完毕后，就立即开始执行。可能外部资源还没有加载完毕。

所以相对而言，ready方法的执行效率要略快于window.onload方法。

GD2库相关概念：小故事

小明、小强，背景：小明暗恋小强，小明写情书给小强

① 准备一张纸

② 纸润色

③ 写字

④ 封装到信封发送给小强

⑤ 小强觉得恶心，销毁了...

demo08_ready.html 示例代码：

![img](wps20C9.tmp.jpg) 

demo08.php 示例代码：

![img](wps20CA.tmp.jpg) 

## 3、jQuery中的事件绑定（重点）

bind基本语法：为jQuery对象绑定事件

jQuery对象.bind(‘没有on前缀事件’,function(){});

demo09_bind.html 示例代码：

 

![img](wps20CB.tmp.jpg) 

one基本语法：为jQuery对象进行一次绑定，这个事件只能触发一次

jQuery对象.one(‘事件’,function(){});

demo10_one.html 示例代码：

![img](wps20CC.tmp.jpg) 

特别事件绑定，基本语法：

jQuery对象.事件(function(){});

如：$(‘#btnok’).click(function(){});

unbind基本语法：移除jQuery对象的某个事件

jQuery对象.unbind(‘事件’);

demo11_teshu.html 示例代码：

![img](wps20CD.tmp.jpg) 

目前我们使用的jquery版本：1.8.3版本（经典版本），在这个版本中，我们进行事件绑定时，可以使用bind方法，移除事件可以使用unbind方法。

但是在jquery3版本之后，这两个方法已经被弃用了，其使用了on方法进行事件绑定，使用off方法进行移除事件，所以应用jquery绑定时一定要认清jquery版本号。

$(‘#btnok’).on(‘click’,function(){

​	...

});

$(‘#btnDel’).on(‘click’,function(){

​	$(‘#box’).off(‘mouseout’);

});

## 4、jQuery事件绑定的本质

事件绑定：① 行内绑定 ② 动态绑定 ③ 事件监听

问题：jQuery事件绑定属于动态绑定还是事件监听呢？

答：事件监听

demo12_bind.html 示例代码：

![img](wps20CE.tmp.jpg) 

## 5、jQuery常用的事件

① 获得焦点：focus，失去焦点：blur

② 单击事件click

③ 键盘按下：keydown，键盘弹起：keyup

④ 鼠标悬浮：mouseover，鼠标离开：mouseout

⑤ 表单提交事件：submit

⑥ 当滚动条滚动时触发：scroll

基本语法：

jQuery对象.bind(‘事件’,function(){

​	//代码...

});

## 6、事件切换方法（重点）

① hover方法 ：鼠标悬浮与鼠标离开事件

当鼠标移动到一个匹配的元素上面时，会触发指定的第一个函数。

当鼠标移出这个元素时，会触发指定的第二个函数。

基本语法：

hover(function(){},function(){});

demo13_hover.html 示例代码：

![img](wps20CF.tmp.jpg) 

 

② toggle(fn1,fn2,fn3...，fnN) ：点击切换事件

触发原理：第一次点击执行fn1,第二次点击执行fn2,第三次点击执行fn3，

当所有函数执行完后再点击，则再次从第一个开始执行；

 

demo14_toggle.html 示例代码：

## ![img](wps20D0.tmp.jpg)7、案例：折叠菜单（重点）

demo15_menu.html 示例代码：
![img](wps20D1.tmp.jpg)

## 8、阻止事件冒泡

问题：什么是事件冒泡

在JS中，如果元素之间存在嵌套关系。当我们为所有元素绑定事件时，事件的响应会像水泡一样上升至最顶级对象。这个过程就称之为“事件冒泡”。

![img](wps20D2.tmp.jpg) 

demo16_bubble.html 示例代码：

![img](wps20D3.tmp.jpg) 

![img](wps20D4.tmp.jpg) 

但是有些情况下，我们并需要事件冒泡，所以冒泡行为需要被禁止。基本语法：

$(选择器).bind(‘事件’,function(event){

​	//event就是jQuery中的事件对象

​	event.stopPropagation();

});

demo16_bubble.html 示例代码：

![img](wps20D5.tmp.jpg) 

记住：在jQuery中，event.stopPropagation()既可以W3C浏览器下的冒泡问题也解决IE内核浏览器下的冒泡问题。

## 9、阻止元素的默认行为

在HTML代码中，有些元素如a标签、submit按钮都拥有自己的默认行为。但是有些情况下，我们并不想让元素拥有这些默认行为，所以默认行为也需要被禁止。

基本语法：

jQuery对象.bind(‘事件’,function(event){

​	event.preventDefault();

​	或

​	return false;

});

demo17_moren.html 示例代码：

![img](wps20D6.tmp.jpg) 

# 四、jQuery中的动画（重点）

## 1、显示与隐藏动画

hide([speed,[fn]]) 隐藏显示的元素

speed : 三种预定速度之一的字符串("slow","normal", or "fast")或

表示动画时长的毫秒数值(如：1000);

fn:在动画完成时执行的函数。

 

show([speed,[fn]]) 显示隐藏的匹配元素

speed : 三种预定速度之一的字符串("slow","normal", or "fast")或

表示动画时长的毫秒数值(如：1000);

fn:在动画完成时执行的函数。

demo18_jiben.html 示例代码：

![img](wps20E7.tmp.jpg) 

## 2、上下滑动动画

slideDown([speed],[fn]) 显示（向下滑动）

通过高度变化（向下增大）来动态地显示所有匹配的元素，在显示完成后可选地触发一个回调函数。

slideUp([speed,[fn]]) 隐藏（向上滑动）

通过高度变化（向上减小）来动态地隐藏所有匹配的元素，在隐藏完成后可选地触发一个回调函数。

slideToggle([speed],[fn]) 向上与向下切换

通过高度变化来切换所有匹配元素的可见性，并在切换完成后可选地触发一个回调函数。

参数说明：

speed ：动画所执行的时间，单位：毫秒

fn ：一个匿名函数，代表动画完成时所执行的程序

demo19_huadong.html 示例代码：

![img](wps20E8.tmp.jpg) 

## 3、透明度动画

fadeIn([speed],[fn]) 淡入，透明度从0(全透明)到1(不透明)

通过不透明度的变化来实现所有匹配元素的淡入效果，并在动画完成后可选地触发一个回调函数。

fadeOut([speed],[fn]) 淡出，透明度从1(不透明)到0(全透明)

通过不透明度的变化来实现所有匹配元素的淡出效果，并在动画完成后可选地触发一个回调函数。

fadeTo(speed, 透明度0-1) ：设置元素的透明度

 

参数说明：

speed ：动画所执行的时间，单位：毫秒

fn ：一个匿名函数，代表动画完成时所执行的程序

demo20_toumingdu.html 示例代码：

![img](wps20E9.tmp.jpg) 

## 4、自定义动画

animate(params,[speed],[easing],[fn]) ：自定义动画

参数说明：

params:一组包含作为动画属性和终值的样式属性和及其值的集合，要求是一个JSON数据

speed:动画的持续时间，单位毫秒

easing:要使用的擦除效果的名称(需要插件支持).默认jQuery提供"linear" 和 "swing".

fn:在动画完成时执行的函数，每个元素执行一次。

![img](wps20EA.tmp.jpg) 

## 5、案例：一组图片的淡入淡出

要使用到的知识点：this关键字，在jQuery事件绑定中，this指向哪里呢？

① 行内绑定，this指向window

② 动态绑定与事件监听，this指向当前dom对象

③ jQuery中事件绑定函数内部的this指向哪里呢？

![img](wps20EB.tmp.jpg) 

所以由以上代码运行结果可知，jQuery事件绑定中，其事件处理程序内部的this指向当前正在操作的dom对象。

demo22_img.html 示例代码：

![img](wps20EC.tmp.jpg) 

# 五、jQuery中文档操作

DOM中的文档操作：增删改查，同理，在jQuery中它也提供了很多方法，实现增删改查。

## 1、插入操作（在什么时候用什么方法）

<div><p>p1</p> content hello content <p>p1</p></div>

 

① 内部插入

append(content) ：向匹配的元素内部追加内容。

prepend(content) ：内容前置到匹配的元素内部。

appendTo(content) ：把匹配的元素追加到另一个指定的元素集合中

prependTo(content) ：把匹配的元素前置到另一个、指定的 元素 集合中。

demo23_charu.html 示例代码：

![img](wps20ED.tmp.jpg) 

 

② 外部插入

<p>world</p> content <div>hello</div> content<p>world</p>

 

after(content) ：在匹配的元素之后插入内容。

before(content) ：在匹配的元素之前插入内容。

insertAfter(content) ：把匹配的元素插入到另一个、指定的元素集合的后面。

insertBefore(content) ：把匹配的元素插入到另一个、指定的元素集合的前面。

demo25_charu.html 示例代码：

![img](wps20EE.tmp.jpg) 
