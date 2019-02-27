---
title: vue04
date: 2019-02-20 15:46:03
tags: VUE
---
Vue项目(第1天)

# 第15讲 项目前技术准备

​	电商后台管理系统的前端部分 3.5天

## 15.1 vue-cli脚手架工具

​	一个项目的启动工作 创建项目目录 安装webpack和相关需要的包

​	修改大量配置项 这种方式启动项目比较慢

​	手动配置环境 好比apm独立环境 非常熟悉这一套环境 适应性强

​	自动搭建环境 好比phpstudy 通过vue-cli快速搭建VUE项目

​		好处:项目启动快 配置简单

​	1、全局安装vue-cli脚手架工具
<!-- more -->
![img](wps4A2.tmp.jpg) 

​	2、创建项目文件夹 使用vue-cli对项目进行初始化 创建项目

![img](wps4B2.tmp.jpg) 

## 15.2 vue-cli项目结构 开发与产出

​	如果是手动搭建 自己了解项目结构

​	自动搭建项目 需要了解自动创建的目录分别是什么作用

​	vue-cli自动创建 按照常规vue项目的需要 安装依赖

在vue-cli快速搭建的基础上根据自己项目的需要对环境进行微调

vue-cli初始化完成了

![img](wps4B3.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

vue-cli自动化创建的目录有哪些作用:

![img](wps4B4.tmp.jpg) 

vue-cli根据默认配置安装了依赖 认识一下这些依赖

![img](wps4B5.tmp.jpg) 

![img](wps4B6.tmp.jpg) 

![img](wps4B7.tmp.jpg) 

![img](wps4B8.tmp.jpg) 

![img](wps4B9.tmp.jpg) 

![img](wps4BA.tmp.jpg) 

![img](wps4BB.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 15.3 webpack配置文件

![img](wps4BC.tmp.jpg) 

​	添加自己喜欢的参数 优化开发体验

![img](wps4BD.tmp.jpg) 

​	了解vue-cli在webpack配置文件中 设置的解析路径 别名

![img](wps4BE.tmp.jpg) 

 

 

 

 

 

 

## 15.4 使用vue-router

​	目前项目自带vue-router

​	在main.js中引入了 路由规则文件

![img](wps4BF.tmp.jpg) 

![img](wps4D0.tmp.jpg) 

 

 

 

 

 

​	在编写项目的过程中 应该遵循以下工作顺序

​	1、创建新页面 找src/components目录 创建新组建

![img](wps4D1.tmp.jpg) 

2、为了能够看见组件 编写新的路由规则 找src/router/index.js

![img](wps4D2.tmp.jpg) 

3、访问测试

![img](wps4D3.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 15.5 组件样式相关问题

​	发现组件样式有扩散的问题 子组件样式会对父组件产生影响

![img](wps4D4.tmp.jpg) 

![img](wps4D5.tmp.jpg) 

可以在组件的style标签上 加scoped属性

​	action scope作用域

![img](wps4D6.tmp.jpg) 

样式除了css之外 还有很多类型 less和scss

![img](wps4D7.tmp.jpg) 

​	可以在style标签中 添加lang属性 lang=”样式种类”

![img](wps4D8.tmp.jpg) 

 

 

 

 

​	vue-cli自动搭建项目 没有安装sass-loader

![img](wps4D9.tmp.jpg) 

安装了sass-loader之后 vue-cli会自动寻找这种载入器 不需要手动添加文件处理规则

![img](wps4DA.tmp.jpg) 

​	访问发现样式已经生效 说明问题解决了

 

今后凡是创建组件文件.vue文件 

需要注意 在style标签中 需要添加lang 和 scoped属性

 

 

 

 

 

 

 

## 15.6 创建VUE文件模板

![img](wps4DB.tmp.jpg) 

![img](wps4DC.tmp.jpg) 

 

 

 

 

 

​	把以下模板内容拷贝到 vue.json中即可

​	{

  "vue-template": {

​    "prefix": "vue",

​    "body": [

​    "<template>",

​    "\t<div>",

​    "\t\t$1",

​    "\t</div>",

​    "</template>",

​    "",

​    "<script>",

​    "export default {",

​    "\tdata() { ",

​    "\t\treturn {",

​    "",

​    "\t\t}",

​    "\t}",

​    "}",

​    "</script>",

​    "",

​    "<style lang=\"\" scoped>",

​    "</style>"

  

​    ],

​    "description": "vue template"

  }

}

 

 

 

 

 

 

 

 

## 15.7 elementUI库

​	组件库 因为自己编写网页元素 设置样式比较麻烦

elementUI官网:  [http://element-cn.eleme.io](http://element-cn.eleme.io/)

![img](wps4DD.tmp.jpg) 

​	使用

![img](wps4ED.tmp.jpg) 

​	安装:

![img](wps4EE.tmp.jpg) 

![img](wps4EF.tmp.jpg) 

​	

 

​	在main.js中引入

![img](wps4F0.tmp.jpg) 

​	使用EU时 先查看EU的文档 选中自己喜欢的元素 移植即可

![img](wps4F1.tmp.jpg) 

![img](wps4F2.tmp.jpg) 

![img](wps4F3.tmp.jpg) 

## 15.8 Vuex状态管理工具

## 15.9 部署项目后端

在项目资料中已有现成的后端

![img](wps4F4.tmp.jpg) 

把后端项目部署 并 运行起来

![img](wps4F5.tmp.jpg) 

​	把后端项目中db目录里 有一个数据库模拟数据文件

![img](wps4F6.tmp.jpg)
	把所有sql语句导入

修改数据库配置项

![img](wps4F7.tmp.jpg) 

初始化项目

![img](wps4F8.tmp.jpg) 

 

启动服务

![img](wps4F9.tmp.jpg) 

由于后端是提供接口 给前端进行请求的:

 

请记住以下重要信息！！！

​	后台项目接口说明文档: [http://47.96.21.88:8082/#10202](#10202)

​	管理员账号密码: admin 123456

![img](wps4FA.tmp.jpg) 

 

 

 

 

 

 

 

 

 

# 第16讲 VUE项目 商城管理系统

## 16.1 构建登陆页面

​	1、创建src/components/Login.vue 事先配置VUE模板

![img](wps4FB.tmp.jpg) 

 

 

 

 

 

 

 

​	2、改写路由规则文件 添加login组件对应的路径信息

![img](wps50C.tmp.jpg) 

3、观察elementUI文档 选择想要的组件 移植(复制)到项目中

![img](wps50D.tmp.jpg) 

![img](wps50E.tmp.jpg) 

​	从elementUI官网上移植合适的代码 根据自己的需要进行简单修改

​	4、需要在组件数据中准备对应的数据

![img](wps50F.tmp.jpg) 

​	查看数据功能是否正常:

![img](wps510.tmp.jpg) 

​	5、在课程材料中已经有写好的样式 直接移植即可

​	![img](wps511.tmp.jpg)

 

 

 

 

 

​	把样式移植到login组件中 并根据样式的类名 修改对应的HTML元素结构

![img](wps512.tmp.jpg) 

​	6、需要在头像的部分放置一张图片 把资源文件移植到项目中

![img](wps513.tmp.jpg) 

![img](wps514.tmp.jpg) 

 

 

 

 

7(额外步骤)、账号和密码的输入框中 添加一个字符图标 在项目资料中准备了图表集

![img](wps515.tmp.jpg) 

移植了字符图标所需的字体文件和样式文件

![img](wps516.tmp.jpg) 

 

 

​	8、在main.js中引入了主样式文件后 有4个字体文件的路径不正确

![img](wps517.tmp.jpg) 

![img](wps518.tmp.jpg) 

 

 

 

 

 

 

 

​	9、使用自己添加的字符图标

![img](wps519.tmp.jpg) 

![img](wps51A.tmp.jpg) 

​	查看EU文档 发现要通过 prefix-icon属性添加字符图标

![img](wps51B.tmp.jpg) 

 

 

## 16.2 登陆页面欢迎信息(侦听器 路由信息)

​	watch属性 识别用户访问的路径 给出对应的欢迎信息

​	如果用户访问/login 提示欢迎访问本站

​	如果用户访问其他路径 提示其他页面

1、 在App.vue组件中添加一个侦听器 针对路由路径信息进行侦听

![img](wps52C.tmp.jpg) 

![img](wps52D.tmp.jpg) 

 

 

 

2、 观察elementUI文档 移植美观的提示信息

![img](wps52E.tmp.jpg) 

提示会自动消失

 

 

 

 

 

 

 

 

 

## 16.3 构建注册页面

​	注册页面和登陆页面基本一致

​	1、复制Login组件 进行简单的修改

![img](wps52F.tmp.jpg) 

​	2、准备好组件中需要用到的数据

![img](wps530.tmp.jpg) 

​	

 

 

​	3、修改了部分样式名

![img](wps531.tmp.jpg) 

​	4、添加对应的路由规则

![img](wps532.tmp.jpg) 

## 16.4 登录页面表单验证

## 16.5 登录验证功能

## 16.6 抽离axios函数库

## 16.7 保持登陆状态

## 16.8 axios请求拦截器