---
title: important属性
date: 2019-02-03 14:49:28
tags: CSS
---
# !important 属性

important 这个英文单词在英文中 表示**重要的**

它在CSS中表示权重最高  某一个属性的权重

> 语法：

```css
选择器 {
    属性：值 !important;
}

错误的写法：
1. 属性:值;!important  !important是写在分号里面
2. 属性:值 important ; 不能缺少感叹号 

```
<!-- more -->
> 作用：
>

- !important 它提升的是属性的权重  并不是选择器优先级

	![1536200687220](1536200687220.png)

- !important它不是提升继承过来的权重！

![1536201111824](1536201111824.png)

> !important有什么作用？
>

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>!important有什么作用</title>
	<style>
		.box {
			width: 600px;
			border: 1px solid #000;
		}
		.box div {
			width: 100px;
			height: 100px;
		}
		.div1 {
			background-color: #f00;
		}
		/*想将第二个盒子的宽度设置为200px*/
		.div2 {
			width: 200px !important;
			background-color: #0f0;
		}
		.div3 {
			background-color: #00f;
		}

	</style>
</head>
<body>
	<!-- 子元素选择器  >右边是左边的子元素   -->
	<div class="box">
		<div class="div1"></div>
		<div class="div2"></div>
		<div class="div3"></div>
	</div>
</body>
</html>
```

