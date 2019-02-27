---
title: 使用html5的localStorage实现本地存储
date: 2019-02-26 15:50:47
tags:  HTML
---
# 使用html5的localStorage实现本地存储



**一、什么是localStorage？**   

在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k左右，这个因浏览器而异)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。`且cookie每次随HTTP请求一起发送，浪费带宽 `,而localStorage是不会随着http请求发送。
<!-- more -->
**二、localStorage的优势与局限localStorage**

**localStorage的优势**

1、localStorage拓展了cookie的4K左右限制

2、localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的



**localStorage的局限**

1、浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性

2、目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换

3、localStorage在浏览器的隐私模式下面是不可读取的

4、localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡

5、localStorage不能被爬虫抓取到

localStorage与sessionStorage的唯一一点区别就是localStorage属于`永久性`存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空。

**三、localStorage的使用**



存储数据

```javascript
console.log(localStorage); // Storage {length: 0} 没有数据

localStorage.setItem('name','大锤') //存储名字为name值为大锤的变量
//等价于上面的命令
localStorage.name = "大锤"; 

console.log(localStorage) // Storage{name: "大锤", length: 1} 
```

读取数据

```javascript
localStorage.getItem("name")  //读取保存在localStorage对象里名为name的变量的值
//等价于上面的命令
localStorage.name // "大锤"
```

 

删除某个变量

```javascript
localStorage.removeItem("name"); //undefined
console.log(localStorage) // Storage {length: 0} length为0,说明删除了
```

清空localStorage

```javascript
localStorage.clear()  // undefined  
console.log(localStorage)  //Storage {length: 0} 
```

将JSON存储到localStorage里

```javascript
var user = {"name":"大锤","age":24};
user  = JSON.stringify(user); // 将JSON转为字符串存
localStorage.setItem('user',user);//将变量存到localStorage里

var newUser = localStorage.getItem("user"); // 获取值
newUser = JSON.parse(newUser); //转为JSON
console.log(newUser); // 打印出原先对象
```

