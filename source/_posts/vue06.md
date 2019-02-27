---
title: vue06
date: 2019-02-20 15:48:49
tags: VUE
---
Vue项目(第3天)

## 16.20 新增用户

​	面向接口开发 先查看后端接口数据需要 然后在前端准备对应的数据

​	掌握常见的几种 合理利用页面空间的手段

 

​	1、阅读后端接口文档 查看 新增用户需要哪些数据

![img](wpsFF1E.tmp.jpg) 
<!-- more -->
2、认真阅读EU文档 了解对话框如何使用

![img](wpsFF1F.tmp.jpg) 

3、移植EU文档中的对话框代码 写好HTML注释

![img](wpsFF20.tmp.jpg) 

​	4、准备了对话框需要的变量

![img](wpsFF21.tmp.jpg) 

 

 

 

5、根据需要 修改对话框中的元素 添加表单元素

![img](wpsFF22.tmp.jpg) 

​	6、准备表单需要的数据

![img](wpsFF23.tmp.jpg) 

​	7、编写接口请求函数 

![img](wpsFF24.tmp.jpg) 

​	8、给对话框的确认按钮绑定点击事件 提交新增用户表单数据

![img](wpsFF25.tmp.jpg) 

 

 

 

 

 

 

​	9、在提交事件中发起请求 处理响应

![img](wpsFF26.tmp.jpg) 

 

 

 

 

 

## 16.21 删除用户

​	1、阅读接口文档 查看删除用户接口需要哪些数据

![img](wpsFF27.tmp.jpg) 

​	2、在用户列表中添加删除按钮 了解插槽的用法

![img](wpsFF38.tmp.jpg) 

![img](wpsFF39.tmp.jpg) 

​	3、给删除按钮绑定点击事件 传入当前行的用户id

![img](wpsFF3A.tmp.jpg) 

 

​	4、编写接口函数 delUser

![img](wpsFF3B.tmp.jpg) 

​	5、编写删除按钮的事件方法 deleteUser 发起请求 处理响应

![img](wpsFF3C.tmp.jpg) 

 

 

 

 

 

## 16.22 编辑用户

​	修改用户的基本信息

​	1、查看接口对数据的要求

![img](wpsFF3D.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

2、在表格中添加邮箱和手机列

![img](wpsFF3E.tmp.jpg) 

3、在操作栏中添加 编辑用户 按钮

![img](wpsFF3F.tmp.jpg) 

 

 

​	4、移植EU中对话框代码 编辑用户对话框

![img](wpsFF40.tmp.jpg) 

​	5、移植EU表单代码 显示正在修改的账号 采集邮箱和手机

![img](wpsFF41.tmp.jpg) 

 

 

 

 

 

 

​	6、准备对话框和表单所需数据

![img](wpsFF42.tmp.jpg) 

 

 

 

 

 

 

 

 

 

​	7、查看 根据用户id查询 旧数据接口

![img](wpsFF43.tmp.jpg) 

​	8、编写接口函数 查询旧数据

![img](wpsFF44.tmp.jpg) 

 

 

 

 

 

​	9、在打开编辑用户对话框按钮事件中 请求旧数据 显示在表单中

![img](wpsFF45.tmp.jpg) 

​	10、编写接口函数 提交新数据

![img](wpsFF46.tmp.jpg) 

![img](wpsFF57.tmp.jpg) 

 

 

 

 

​	11、给对话框确认按钮绑定提交事件

![img](wpsFF58.tmp.jpg) 

​	12、编写确认按钮绑定点击事件 提交表单数据 处理响应

![img](wpsFF59.tmp.jpg) 

## 16.23 分配角色

​	了解RBAC中 后台 用户表 和 角色表 关系:

![img](wpsFF5A.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

​	了解接口对数据的需要

![img](wpsFF5B.tmp.jpg) 

![img](wpsFF5C.tmp.jpg) 

![img](wpsFF5D.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

​	阅读EU文档 学习下拉选框的用法

​	![img](wpsFF5E.tmp.jpg)

 

![img](wpsFF5F.tmp.jpg) 

 

 

 

 

 

 

1、 在操作栏中添加一个按钮

![img](wpsFF60.tmp.jpg) 

​	2、移植对话框代码 并 对话框中添加两个表单元素 显示账号文本框 选角色下拉选框

![img](wpsFF61.tmp.jpg) 

 

 

 

 

 

 

 

​	3、添加所需的数据和方法

![img](wpsFF62.tmp.jpg) 

​	4、封装接口函数 获取可选角色数据

![img](wpsFF63.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

5、引入并调用接口函数 把用户数据保存在当前组件中

![img](wpsFF64.tmp.jpg) 

 

 

 

 

 

 

 

 

 

​	6、遍历v-for 产生角色选项

![img](wpsFF65.tmp.jpg)
	7、查询当前用户旧数据 显示正在修改的账号名 默认选中角色选项

![img](wpsFF66.tmp.jpg) 

8、编写接口函数 提交新角色

![img](wpsFF76.tmp.jpg) 

​	9、给对话框的确认按钮绑定点击事件

![img](wpsFF77.tmp.jpg) 

 

 

 

 

 

 

 

 

​	10、引入接口函数 编写事件代码 调用接口函数 处理响应

![img](wpsFF78.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 16.24 设置账号管理状态

![img](wpsFF79.tmp.jpg) 

![img](wpsFF7A.tmp.jpg) 

![img](wpsFF7B.tmp.jpg) 

​	1、在用户列表中添加列 用于显示 管理状态

![img](wpsFF7C.tmp.jpg) 

​	2、给开关绑定change事件

![img](wpsFF7D.tmp.jpg) 

![img](wpsFF7E.tmp.jpg) 

​	3、编写接口函数

![img](wpsFF7F.tmp.jpg) 

 

 

 

 

 

 

 

​	4、引入接口函数 编写change事件代码

![img](wpsFF80.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 16.25 权限列表

​	1、创建新组件 Auth Right

![img](wpsFF81.tmp.jpg) 

​	2、设置路由规则

![img](wpsFF82.tmp.jpg) 

 

​	3、查看接口文档 编写接口函数

![img](wpsFF83.tmp.jpg) 

![img](wpsFF94.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

​	4、在Auth组件中引入 并获取权限列表数据 进行无限极分类排序

![img](wpsFF95.tmp.jpg) 

​	用递归输出1~10  return在递归入口前  return在递归入口后

 

 

 

 

 

 

​	5、移植EU表格代码 编写列字段

![img](wpsFF96.tmp.jpg) 

 

 

 

 

 

 

 

 

​	6、根据层级制作缩进 用过滤器

![img](wpsFF97.tmp.jpg) 

![img](wpsFF98.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 16.26 角色列表

​	角色与权限直接关联

1、 创建角色组件 添加路由规则

![img](wpsFF99.tmp.jpg) 

![img](wpsFF9A.tmp.jpg) 

2、 查看接口文档 编写接口函数 获取角色列表数据

![img](wpsFF9B.tmp.jpg) 

3、认真阅读EU文档 了解展开行的使用

![img](wpsFF9C.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

​	4、移植展开行代码 清空了展开行 定义字段

![img](wpsFF9D.tmp.jpg) 

​	5、创建created钩子函数 引入调用接口函数 把角色数据保存在数据源中tableData

![img](wpsFF9E.tmp.jpg) 

## 16.27 栅格布局显示权限

![img](wpsFF9F.tmp.jpg) 

![img](wpsFFA0.tmp.jpg) 

​	一个行就是一个 el-row元素

​	一个列就是一个 el-col元素

​	通过:span定列的宽度 一行当中所有宽度总和是24即可

 

​	用栅格布局在 展开行中 显示当前角色的权限数据 节省空间

![img](wpsFFA1.tmp.jpg) 

​	每个角色的数据中 children属性保存了顶级权限

​	顶级权限数据中 children属性保存二级权限

​	二级权限数据中 children属性保存三级权限

![img](wpsFFA2.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 16.28 标签化权限渲染

​	标签化是一种非常节省空间的界面设计

​	文章属性 商品属性 

![img](wpsFFB3.tmp.jpg) 

![img](wpsFFB4.tmp.jpg) 

​	在遍历显示权限的地方 全部用标签代替即可

![img](wpsFFB5.tmp.jpg) 

 

 

## 16.29 角色权限删除

​	阅读接口文档 

![img](wpsFFB6.tmp.jpg) 

​	阅读EU文档 了解 关闭标签事件

![img](wpsFFB7.tmp.jpg) 

 

 

 

 

 

 

 

 

 

​	1、给标签添加 close事件 传入必要的参数

![img](wpsFFB8.tmp.jpg) 

​	2、封装接口函数

![img](wpsFFB9.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

3、 编写删除权限事件 发起请求 处理响应

![img](wpsFFBA.tmp.jpg) 

​	4、改变事件的传参 利用引用传递的特性 在函数的内部修改 当前行的权限数据

![img](wpsFFBB.tmp.jpg) 