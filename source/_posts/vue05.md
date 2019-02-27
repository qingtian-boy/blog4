---
title: vue05
date: 2019-02-20 15:47:56
tags: VUE
---
Vue项目(第2天)

# 第16讲 VUE项目 商城管理系统

## 16.4 登录页面表单验证

​	登陆表单中要求 账号5~10位 密码6~12位

​	1、认真阅读EU文档 了解表单验证功能如何使用

![img](wps400F.tmp.jpg) 

![img](wps4010.tmp.jpg) 
<!-- more -->
​	2、修改Login.vue中表单代码 添加rules和prop属性

![img](wps4011.tmp.jpg) 

 

 

 

​	3、在当前组件的数据中添加验证规则rules

![img](wps4012.tmp.jpg) 

​	4、认真阅读EU文档 查看数据验证的方式

![img](wps4013.tmp.jpg) 

![img](wps4014.tmp.jpg) 

 

 

 

5、修改Login组件的表单和代码 在登陆按钮中添加事件 事件中进行数据验证

![img](wps4015.tmp.jpg) 

![img](wps4026.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 16.5 登录验证功能

​	数据需要通过异步请求发送到后端 

1、 在项目shop-admin目录下 本地安装axios插件

![img](wps4027.tmp.jpg) 

2、 认真阅读后端接口文档 根据接口的需要准备数据 发起请求

![img](wps4028.tmp.jpg) 

![img](wps4029.tmp.jpg) 

3、 认真阅读axios文档 查看post请求如何发出

![img](wps402A.tmp.jpg) 

4、 在表单提交的 数据验证通过的 分支中 发起post请求 观察响应

![img](wps402B.tmp.jpg) 

![img](wps402C.tmp.jpg) 

 

 

 

 

 

5、 修改login组件中 处理响应的代码 对接口返回的status进行判断 200成功

![img](wps402D.tmp.jpg) 

![img](wps402E.tmp.jpg) 

​	面向接口编程

​		查看接口文档 了解接口要求

​		在前端准备接口所需的数据 发起请求

​		先查看响应的具体情况 了解数据结构

​		对响应数据进行处理

 

 

 

 

 

## 16.6 创建后台首页组件

​	1、在src/components目录下 创建新组建 Home

![img](wps402F.tmp.jpg) 

​	2、移植样式代码 在div中添加class=home

![img](wps4030.tmp.jpg) 

​	3、添加home组建对应的路由规则

![img](wps4031.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 16.7 抽离axios函数库

​	为了项目的长期发展 优化代码结构 把所有的接口请求函数单独放到一个文件中

​	形成接口函数库

1、 在src目录下 创建api目录 添加index.js文件

2、 需要在接口函数库中 定义请求方法

![img](wps4032.tmp.jpg) 

3、 修改Login组件代码 改变了引入的方式

![img](wps4033.tmp.jpg) 

![img](wps4034.tmp.jpg) 

 

 

 

 

4、 处理响应

![img](wps4045.tmp.jpg) 

5、 认真阅读axios文档 请求配置项部分 利用baseURL配置项封装基础路径

![img](wps4046.tmp.jpg) 

![img](wps4047.tmp.jpg) 

## 16.8 会话技术token

​	会话技术

​		cookie 在浏览器上记录一个cookie数据 向后台发起请求

自动把cookie携带在请求头中

 

​		session 基于cookie的会话技术 cookie保存PHPSESSID

​			服务端根据PHPSESSID找到在服务器上的session文件

 

​		token 令牌 身份的象征、权利的象征 有令牌 有权操作 没有令牌 无权操作

​		![img](wps4048.tmp.jpg) 

![img](wps4049.tmp.jpg) 

![img](wps404A.tmp.jpg) 

​	在登陆时 登陆成功 需要把返回数据中的token保存在浏览器中

​	在请求其他接口时 通过代码把token设置到请求头中

​	浏览器中可以通过 localStorage 本地化读写数据

​		localStorage 浏览器自带的 全局作用域下的 本地存储对象

![img](wps404B.tmp.jpg) 

![img](wps404C.tmp.jpg) 

## 16.9 保持登陆功能(axios拦截器)

​	1、修改Login.vue组件中 登陆成功后的代码 把token存储在浏览器上

![img](wps404D.tmp.jpg) 

​	2、对接口请求进行测试 编写getUserList 获取用户列表接口函数

![img](wps404E.tmp.jpg) 

![img](wps404F.tmp.jpg) 

​	3、认真阅读axios文档 如何在发起请求时 设置请求头

![img](wps4050.tmp.jpg) 

![img](wps4051.tmp.jpg) 

​	4、修改接口函数库 设置请求拦截器 设置token请求头

![img](wps4052.tmp.jpg) 

​	5、重新访问Home组件 测试用户列表数据接口

![img](wps4053.tmp.jpg) 

![img](wps4063.tmp.jpg) 

## 16.10 防翻墙功能

​	用户登录成功的 验证标准 是什么? 

login、register不需要token就可以访问

其他页面比如 home 需要token才能访问

 

​	1、认真阅读VueRouter文档 全局路由守卫

![img](wps4064.tmp.jpg) 

![img](wps4065.tmp.jpg) 

![img](wps4066.tmp.jpg) 

 

​	2、在main.js中设置全局路由守卫

​		在router/index.js 引入路由模块 获得Router的构造器

​		new Router实例化获得路由对象 导出对象

​		在mian.js中导入router/index.js 导入了路由对象

​		全局路由守卫需要通过路由对象调用

![img](wps4067.tmp.jpg) 

 

 

 

 

 

 

 

## 16.11 首页整体布局

![img](wps4068.tmp.jpg) 

1、 认真阅读EU文档 布局容器

![img](wps4069.tmp.jpg) 

​	2、把对应代码移植到 Home组件中

![img](wps406A.tmp.jpg) 

## 16.12 首页侧边栏

​	1、认真阅读EU文档 导航菜单

​	2、把导航菜单移植到 Home组件的 aside当中

![img](wps406B.tmp.jpg) 

​	删除了两个不需要的事件

​	尽量减少子菜单数量 得出最小模板

​	修改index属性 避免index重复

​	3、在侧边栏的顶端添加一个项目logo

![img](wps406C.tmp.jpg) 

## 16.13 首页头部

​	1、在首页头部中 添加一个汉堡包/三道杠图标

![img](wps406D.tmp.jpg) 

​	2、认真阅读EU文档 查看 导航菜单 折叠功能如何实现

![img](wps406E.tmp.jpg) 

![img](wps406F.tmp.jpg) 

 

 

 

 

 

 

​	3、在el-menu元素上添加 :collapse属性 绑定数据 isCollapse

![img](wps4070.tmp.jpg) 

​	4、给汉堡包图标添加点击事件 在点击事件中 修改isCollapse数据

![img](wps4081.tmp.jpg) 

​	5、修改侧边栏的宽度样式为自适应

![img](wps4082.tmp.jpg) 

## 16.14 首页头部信息

​	1、在头部 添加系统标题 添加用户欢迎信息 添加退出按钮

![img](wps4083.tmp.jpg) 

​	2、修改Login组件代码 登陆成功后 保存当前用户的账号名

![img](wps4084.tmp.jpg) 

 

 

 

 

 

​	3、修改Home组件代码 创建一个username数据 从LS中读取用户名

![img](wps4085.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

## 16.15 退出登录功能

1、 给退出按钮 添加点击事件 绑定方法

![img](wps4086.tmp.jpg) 

2、 在方法中清空浏览器中的token数据 返回到login页面

![img](wps4087.tmp.jpg) 

 

 

 

## 16.16 用户列表页

​	1、创建User组件 移植样式

![img](wps4088.tmp.jpg) 

 

 

 

 

 

 

 

 

​	2、添加User组件对应的路由规则

![img](wps4089.tmp.jpg) 

![img](wps408A.tmp.jpg) 

 

 

​	3、搭建用户组件的网页元素 带搜索按钮的搜索框 主要按钮

![img](wps408B.tmp.jpg) 

![img](wps408C.tmp.jpg) 

​	4、在当前组件中 准备好query数据

![img](wps408D.tmp.jpg) 

 

 

 

 

## 16.17 用户列表显示数据

​	1、认真阅读EU文档 了解表格的使用方式

![img](wps408E.tmp.jpg) 

​	2、移植EU中 表格代码 到User组件中

![img](wps408F.tmp.jpg) 

 

 

 

 

 

![img](wps409F.tmp.jpg) 

​	3、修改getUserList接口函数 在User组件中导入getUserList方法

![img](wps40A0.tmp.jpg) 

![img](wps40A1.tmp.jpg) 

 

 

​	4、创建created钩子函数 调用 getUserList方法 请求用户数据 保存到表格数据源

![img](wps40A2.tmp.jpg) 

![img](wps40A3.tmp.jpg) 

 

 

 

​	5、通过column元素 定义表格字段 设置prop属性 设置label表头内容

![img](wps40A4.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 16.18 用户搜索功能

​	1、在搜索按钮 以及 搜索框上 绑定合适的事件

![img](wps40A5.tmp.jpg) 

​	2、优化了代码结构 专门封装一个initList用于初始化表格

![img](wps40A6.tmp.jpg) 

![img](wps40A7.tmp.jpg) 

 

 

 

 

 

 

 

## 16.19 用户列表分页

​	1、认真阅读EU文档 了解分页栏的使用方法

![img](wps40A8.tmp.jpg) 

![img](wps40A9.tmp.jpg) 

​	2、把分页栏 移植到项目中 小心改写数据和方法

![img](wps40AA.tmp.jpg) 

![img](wps40AB.tmp.jpg) 

​	3、调整initList当中的代码 及时保存总记录数

![img](wps40AC.tmp.jpg) 

 

 

 

 

​	4、需要编写 分页相关的两个事件 阅读EU文档 了解两个事件的用法

![img](wps40AD.tmp.jpg) 

![img](wps40BE.tmp.jpg) 

​	5、修改initList方法 向服务端发送真实的分页参数

![img](wps40BF.tmp.jpg) 
