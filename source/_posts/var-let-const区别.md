---
title: var-let-const区别
date: 2019-02-26 15:18:02
tags: var-let-const区别
---


1. 首先，ECMAScript和JavaScript关系：      

   ECMAScript是一个国际通过的标准化脚本语言。JavaScript由ECMAScript和DOM、BOM三者组成。可以简单理解为：ECMAScript是JavaScript的语言规范，JavaScript是ECMAScript的实现和扩展。 

   ECMAScript6:这是规定js语言的核心语法

   DOM: Document Object Model 文档对象模型，document

   BOM:Browser Object MOdel  浏览器对象模型,  location、  history、navigator

​      顶层对象window.
<!-- more -->
# var-let-const声明变量的区别 

## 块作用域{ }

JS中作用域有：全局作用域、局部作用域。没有块作用域的概念。ECMAScript 6(简称ES6)中新增了==块级作用域==。 
块作用域由` { }` 括起来的部分，if语句和for语句里面的{ }也属于块作用域。



## var声明变量

- var在块{}中声明变量

```javascript
if(true){
    var a = 1;
    console.log(a); // 1
}
console.log(a); // 1 说明var定义的变量可以在块外访问
```

- var在函数function中声明变量

```javascript
function foo(){
    var a = 1; //局部变量
    console.log(a); // 1
}
foo();
console.log(a); // 报错 a is not defined ，说明var定义的变量他的作用域是当前函数
```

##  let声明变量

- let在块{}中声明变量

```javascript

if(true){
    let a = 1;
    console.log(a); // 1
}
console.log(a); // a is not defined   说明let声明的变量只能在块{}内访问
```

- let在函数function中声明变量

```javascript
function foo(){
    let a = 1; //局部变量（私有变量-）
    console.log(a); // 1
}
foo();
console.log(a); // 报错 a is not defined ，说明let声明的变量和var的作用域是一样，只在当前函数内可访问
```

在js函数中，怎么去访问函数内的局部变量？通过闭包实现。

```javascript
function fu(){
    var a = 1; //局部变量（私有变量-）
    return function(){
        return a;
    }
}
//调用
var son = fu();
var a = son();
console.log(a); // 1
```



##  const声明常量

- const在块{}中声明常量

```javascript
if(true){
    const a = 111;
}
console.log(a); // // 报错 a is not defined  说明const声明的常量只能在块{}内访问
```

- 不可对const声明的常量进行修改赋值

```javascript
if(true){
    const a = 111;
    a = 222; // 报错 常量的值不可以修改
    console.log(a);
}
```

## 小结

1. var定义的变量，没有块的概念，可以跨块访问, 但不能跨函数访问。
2. let定义的变量，只能在块作用域内访问，更也不能跨函数访问。
3. const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域内访问，而且不能修改



==注==：一个js函数如果没有return语句，默认返回undefined。

