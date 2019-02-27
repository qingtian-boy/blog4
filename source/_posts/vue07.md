---
title: vue07
date: 2019-02-20 15:50:43
tags: VUE
---
Vue项目(第4天)

## 16.30 角色权限分配

![img](wpsB972.tmp.jpg) 

![img](wpsB973.tmp.jpg) 
<!-- more -->
![img](wpsB974.tmp.jpg) 

![img](wpsB975.tmp.jpg) 

 

 

 

![img](wpsB976.tmp.jpg) 

​	1、添加功能入口 角色的操作栏中添加按钮 

![img](wpsB977.tmp.jpg) 

​	2、移植对话框代码 移植文本框 树形控件这些元素 构成分配权限的对话框

![img](wpsB978.tmp.jpg) 

​	3、准备对应的数据

![img](wpsB988.tmp.jpg) 

​	4、准备对话框的原始数据 角色名 可选的选项 旧权限数据

![img](wpsB989.tmp.jpg) 

​	5、给对话框的确定按钮绑定点击事件 提交数据

![img](wpsB98A.tmp.jpg) 

 

 

 

 

​	6、编写事件 提交数据 处理响应

![img](wpsB98B.tmp.jpg) 

​	7、接口函数

![img](wpsB98C.tmp.jpg) 

 

## 16.31 动态菜单

![img](wpsB98D.tmp.jpg) 

​	功能菜单与权限一一对应

![img](wpsB98E.tmp.jpg) 

![img](wpsB98F.tmp.jpg) 

 

 

​	1、编写接口函数 获取动态菜单

![img](wpsB990.tmp.jpg) 

​	2、根据后端已经定义好的路径信息 修改路由规则和组件名

![img](wpsB991.tmp.jpg) 

![img](wpsB992.tmp.jpg) 

![img](wpsB993.tmp.jpg) 

 

 

## 16.32 商品列表

​	1、编写接口函数

![img](wpsB994.tmp.jpg) 

​	2、创建商品组件

![img](wpsB995.tmp.jpg) 

![img](wpsB996.tmp.jpg) 

 

 

 

 

 

 

 

 

 

​	3、封装initList方法 获取列表数据 把数据保存在表格的数据源中

![img](wpsB9A7.tmp.jpg) 

​	4、编写表格当中的列 字段信息

![img](wpsB9A8.tmp.jpg) 

 

​	5、移植分页栏代码 设置相关数据和方法

![img](wpsB9A9.tmp.jpg) 

![img](wpsB9AA.tmp.jpg) 

 

 

 

 

 

 

## 16.33 商品分类页面 引入TreeGrid组件

​	1、点击动态菜单中的 商品分类 根据地址创建组件 编写路由规则

![img](wpsB9AB.tmp.jpg) 

![img](wpsB9AC.tmp.jpg) 

​	2、与他人合作 用他人封装的组件 阅读说明文档 了解基本用法

![img](wpsB9AD.tmp.jpg) 

​	3、把组件移植到 @/components中

![img](wpsB9AE.tmp.jpg) 

4、在Categories中注册私有组件

![img](wpsB9AF.tmp.jpg) 

 

 

 

 

 

 

5、移植案例代码 定义所需数据

![img](wpsB9B0.tmp.jpg) 

6、编写接口函数 引入和调用 

![img](wpsB9B1.tmp.jpg) 

 

7、获取分类列表数据 保存在数据源中

![img](wpsB9B2.tmp.jpg) 

## 16.34 新增商品分类

​	需要选择新分类的父级 

​		创建顶级分类 父级为空 0

​		创建二级分类 选择一个顶级分类作为父级

​		创建三级分类 选择一个二级分类作为父级

1、 创建功能入口 添加新增分类按钮

![img](wpsB9B3.tmp.jpg) 

 

 

 

 

​	2、移植对话框代码 设置对应的数据

![img](wpsB9B4.tmp.jpg) 

![img](wpsB9C5.tmp.jpg) 

 

​	3、查看接口文档 需要三个数据 准备两个表单元素

![img](wpsB9C6.tmp.jpg) 

 

​	4、认真阅读EU文档 了解级联选择器的使用方式

![img](wpsB9C7.tmp.jpg) 

![img](wpsB9C8.tmp.jpg) 

 

![img](wpsB9C9.tmp.jpg) 

![img](wpsB9CA.tmp.jpg) 

​	5、移植级联选择器代码 准备所需数据

![img](wpsB9CB.tmp.jpg) 

 

![img](wpsB9CC.tmp.jpg) 

​	6、向后台发起请求 把可选的父级分类数据 保存在数据源中

![img](wpsB9CD.tmp.jpg) 

 

 

​	7、给对话框的确定按钮绑定点击事件

![img](wpsB9CE.tmp.jpg) 

​	8、编写事件代码 cat_name自动获取 通过代码得出cat_level cat_pid

![img](wpsB9CF.tmp.jpg) 

​	9、数据准备完毕 发起请求 处理响应

![img](wpsB9D0.tmp.jpg) 

​	以下是本功能用到的接口函数

![img](wpsB9D1.tmp.jpg) 

 

 

 

​	为了优化前端性能 减少不必要的获取父级分类次数 减少用户等待选项事件步骤:

​	1、封装独立函数 把初始化父级选项代码封装在一个方法里

![img](wpsB9D2.tmp.jpg) 

2、在组件created钩子中 对列表数据初始化 对父级选项初始化

![img](wpsB9E2.tmp.jpg) 

 

 

3、为了减少用户等待选项事件 在组件created时就已经准备好了选项

​	在创建一个新分类时 操作成功分支中再次初始化父级选项数据

​	由于初始化数据在用户关闭对话框时完成 没有等待的感觉

![img](wpsB9E3.tmp.jpg) 

 

 

 

 

 

 

## 16.35 新增商品步骤条

​	步骤条 EU提供了步骤条

![img](wpsB9E4.tmp.jpg) 

![img](wpsB9E5.tmp.jpg) 

 

 

 

 

 

 

 

 

 

![img](wpsB9E6.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

​	准备对应的数据

![img](wpsB9E7.tmp.jpg) 

​	同步选项卡与步骤条

![img](wpsB9E8.tmp.jpg) 

## 16.36 新增商品入库

​	查看文档说明 了解接口对数据的需要 封装了接口函数

![img](wpsB9E9.tmp.jpg) 

​	提交数据 处理响应

![img](wpsB9EA.tmp.jpg) 

 

 

​	关于数据图推荐使用 HighCharts插件

![img](wpsB9EB.tmp.jpg) 

 

 

​	森哥 微信/手机号:18200899666 17:30后可接电话 支持QQ远程操作

​	欢迎到深圳就业