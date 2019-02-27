---
title: vue03
date: 2019-02-20 15:44:39
tags: VUE
---
Vue前端框架(第4天)

# 第14讲 前端路由

## 14.1 前端路由介绍

​	前端路由根据URL中路径信息的不同 显示对应的界面

​	在VUE中可以把界面当做组件来看待

​	根据URL中路径信息的不同 显示对应的组件

 <!-- more -->

 

vue-router插件 可以专门提供 路由功能

​	下载地址: <https://unpkg.com/vue-router@3.0.1/dist/vue-router.js>

![img](wps11CD.tmp.jpg) 

 

 

 

 

 

 

 

## 14.2 vue-router基本使用

​	1、引入代码库和插件

![img](wps11CE.tmp.jpg) 

​	引入vue-router插件后的效果

![img](wps11CF.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

​	2、根据路径信息 显示 对应组件 制作需要使用的组件 编写模板 注册组件

![img](wps11D0.tmp.jpg) 

 

 

 

 

 

3、实例化VueRouter类 制定路由规则 路径对应组件

![img](wps11D1.tmp.jpg) 

​	4、绑定路由规则

![img](wps11D2.tmp.jpg) 

 

 

 

​	5、需要在页面中设置显示组件所用的视图区域 <router-view>

![img](wps11D3.tmp.jpg) 

![img](wps11D4.tmp.jpg) 

6、测试效果并观察路由相关的数据变化

![img](wps11E4.tmp.jpg) 

 

 

 

 

 

 

 

## 14.3 router-link标签

​	点击跳转按钮后 可以实现 改变url中的路径信息

​	利用a标签制作跳转链接 不能少了#

![img](wps11E5.tmp.jpg) 

路由插件提供了一个专门用于制作跳转按钮的标签 <router-link to=”/news”>

​	to: 说明要跳转的路径 这个路径不需要带#

​	tag: 说明要扮演的标签 默认为a链接

![img](wps11E6.tmp.jpg) 

 

 

 

 

 

 

## 14.4 重定向(redirect属性)

​	利用重定向技术 实现默认访问功能

​	如果用于访问/根路径时 让他转而访问 /news 重定向

 

​	在路由规则中利用redirect属性设置 重定向路由

![img](wps11E7.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 14.5 选中链接高亮提示

​	选中的选项卡/跳转链接 应该与 未选中的链接样式有所不同

 

​	VueRouter提供了一个专用于高亮提示的样式名

​		.router-link-active{高亮样式}

![img](wps11E8.tmp.jpg) 

在实例化路由插件时 new VueRouter时可以设置一个配置项

​	linkActiveClass:类样式名

![img](wps11E9.tmp.jpg) 

## 14.6 获取get形式参数

​	路由与url密切关联 url?参数名=参数值&….

 

![img](wps11EA.tmp.jpg) 

![img](wps11EB.tmp.jpg) 

![img](wps11EC.tmp.jpg) 

 

 

 

 

 

## 14.7 获取pathinfo形式参数

​	url/参数/参数/参数

​	如果想要获取pathinfo形式参数 修改路由规则

避免参数影响路由匹配 给数据按顺序起名

![img](wps11ED.tmp.jpg) 

![img](wps11EE.tmp.jpg) 

![img](wps11EF.tmp.jpg) 

![img](wps11F0.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 14.8 嵌套路由(children属性定义子级路由)

​	/account 账户组件

​	/account/login 登陆组件

​	/account/register 注册组件

![img](wps11F1.tmp.jpg) 

1、 在页面中引入VUE和vur-router

![img](wps1202.tmp.jpg) 

2、 定义组件 定义模板 注册组件

![img](wps1203.tmp.jpg) 

![img](wps1204.tmp.jpg) 

 

 

 

3、 实例化VueRouter插件 定义路由规则

![img](wps1205.tmp.jpg) 

4、 绑定路由规则

![img](wps1206.tmp.jpg) 

 

 

 

5、 设置两个显示组件的视图区域

![img](wps1207.tmp.jpg) 

6、 在合适的地方设置 跳转链接

![img](wps1208.tmp.jpg) 

 

 

 

 

 

​	7、测试访问效果

![img](wps1209.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 14.9 多组件路由处理办法(components 视图区域name属性)

![img](wps120A.tmp.jpg) 

在下方两个功能中 一个路径需要对应多个组件 在设置路由规则时 有所不同

1、 引入工作

![img](wps120B.tmp.jpg) 

 

 

 

 

 

2、创建所需组件 总共4个

![img](wps120C.tmp.jpg) 

 

 

 

 

 

 

 

 

 

![img](wps120D.tmp.jpg) 

​	3、实例化VueRouter制定路由规则

![img](wps120E.tmp.jpg) 

​	4、绑定路由规则

![img](wps120F.tmp.jpg) 

 

5、设置显示组件的视图区域

![img](wps121F.tmp.jpg) 

​	6、在合适地方 设置导航菜单 其中包含跳转链接

![img](wps1220.tmp.jpg) 

 

 

 

 

 

 

 

 

 

7、 flex布局 可以复制样式

![img](wps1221.tmp.jpg) 

![img](wps1222.tmp.jpg) 