---
title: 初识HTML
date: 2019-01-25 15:28:58
tags: HTML
---

#### 锚点定位 （难点）

通过创建锚点链接，用户能够快速定位到目标内容。
创建锚点链接分为两步：

每一个标签里面都会有一个属性叫id 属性  我们可以通过这个属性来设置锚点  属性值称之为锚点名 

```html
1.使用相应的id名标注跳转目标的位置。 找到锚点链接  id属性每一个标签都有  
 <h3 id="two">第2集</h3> 

2.使用“<a href=”#id属性值>“链接文本"</a>创建链接文本（被点击的）
<a href="#two">找到锚点</a>   
```

#### base 标签
<!-- more -->
**语法：**

```html
<base target="_blank" />
```

![1535618838640](1535618838640.png)

**总结： **

1. base 可以设置整体链接的打开状态   
2. base 写到  <head>  </head>  之间
3. 把所有的连接 都默认添加 target="_blank"


# 10. 路径(重点、难点)



<img src="lj.png" />



路径的主要作用是用于描述文件 的位置 

描述文件的位置的有绝对路径和相对路径 



实际工作中，我们的文件不能随便乱放，否则用起来很难快速的找到他们，因此我们需要一个文件夹来管理他们。

**目录文件夹： **

就是普通文件夹，里面只不过存放了我们做页面所需要的 相关素材，比如 html文件， 图片 等等。

<img src="g.png" />

**根目录 **  

打开目录文件夹的第一层  就是 根目录 

<img src="gg.png" />

页面中的图片会非常多， 通常我们再新建一个文件夹专门用于存放图像文件（images），这时再插入图像，就需要采用“路径”的方式来指定图像文件的位置。路径可以分为： 相对路径和绝对路径

## 相对路径

相对路径：是以某一个文件作为参照物去描述另外一个文件位置   

相对路径有三种关系：

- 平级关系     目标文件与当前文件在同一个目录下     

  ```html
  <img src="目标文件的文件名" />
  <img src="./目标文件的文件名" />
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
  	<meta charset="UTF-8">
  	<title>相对路径之平级关系</title>
  </head>
  <body>
  	<!-- 相对路径之平级关系:目标文件与当前文件在同一个目录下面 
  	./  表示当前目录 
  
  	-->
  	<img src="1.jpg" alt="">
  	<img src="./1.jpg" alt="">
  </body>
  </html>
  ```

  ![1535621241146](1535621241146.png)

- 上级关系   目标文件在当前文件上一级或者上N级  

  

  ```html
  ../  表示上一级
  ../../ 表示上二级
  ../../../ 表示上三级
  
  
  上一级：<img src="../目标文件的文件名" />
  上二级：<img src="../../目标文件的文件名" />
  ```

- 下级关系   目标文件的文件夹与当前文件在同一个目录中

  ![1535621488371](1535621488371.png)

  ```html
  <img src="images/2.jpg"  />
  <img src="./images/2.jpg"  />
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
  	<meta charset="UTF-8">
  	<title>相对路径之下级 关系</title>
  </head>
  <body>
  	<img src="images/2.jpg" alt="">
  	<img src="./images/2.jpg" alt="">
  </body>
  </html>
  ```

  



**总结： **

> 相对路径，是从代码所在的这个文件出发， 去寻找我们的目标文件的。

## 绝对路径

绝对路径指的是以盘符开头的路径！

D:\itcast\33期\01-HTML5\代码\1.jpg  



```html
<!-- <img src="图片的地址可以是相对路径也可以是绝对路径" alt=""> 
	绝对路径:以是盘符开头去描述文件的位置 
	-->
	<img src="d:/itcast/33期/01-HTML5/代码/1.jpg" alt="">
```


> 相对路径与绝对的路径问题：

- 在移动相对路径时要注意将目标文件同时移动 
- 绝对路径的文件在移动时没有问题  

# @拓展阅读

#### 1. html5发展之路



<img src="html5.png" width="600"/>



#### 2. 什么是XHTML

XHTML 是更严格更纯净的 HTML 代码。

- XHTML 指**可扩展超文本标签语言**（EXtensible HyperText Markup Language）。
- XHTML 的目标是取代 HTML。
- XHTML 与 HTML 4.01 几乎是相同的。
- XHTML 是更严格更纯净的 HTML 版本。  
- XHTML 是作为一种 XML 应用被重新定义的 HTML。
- XHTML 是一个 W3C 标准。

#### 3. HTML和 XHTML之间有什么区别?

- XHTML 指的是可扩展超文本标记语言
- XHTML 与 HTML 4.01 几乎是相同的
- XHTML 是更严格更纯净的 HTML 版本
- XHTML 是以 XML 应用的方式定义的 HTML
- XHTML 是 2001 年 1 月发布的 W3C 推荐标准
- XHTML 得到所有主流浏览器的支持
- XHTML 元素是以 XML 格式编写的 HTML 元素。XHTML是严格版本的HTML，例如它要求标签必须小写，标签必须被正确关闭，标签顺序必须正确排列，对于属性都必须使用双引号等。

# @深入阅读

[HTML5的崛起之路](http://www.chinaz.com/manage/2015/0720/424831.shtml)

# EMMET语法

https://www.w3cplus.com/tools/emmet-cheat-sheet.html