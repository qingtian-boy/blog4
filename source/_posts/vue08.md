---
title: vue08
date: 2019-02-20 15:51:54
tags: VUE 
---
# 第14讲 打包神器webpack@3

​	webpack3版本参考版本依赖

​	![img](wpsF858.tmp.jpg)

<!-- more -->
## 14.1 webpack是什么

​	webpack是一个模块化的打包工具,基于nodejs开发的

​	中文官网: <https://www.webpackjs.com/>

![img](wpsF859.tmp.jpg) 

​	webpack专门解决前端项目中的几个问题:

1、 让复杂的文件引用关系简单化(打包 产出 把所有引用文件整合成一个文件)

2、 帮我们节省请求次数,尽可能地避免二次请求

3、 把各种格式的文件转化成浏览器可以运行的类型

 

 

 

 

 

 

## 14.2 webpack安装

| 本节主要指令          |                                                        |
| --------------------- | ------------------------------------------------------ |
| 安装webpack           | npm install [webpack@3.12.0](mailto:webpack@3.12.0) -g |
| 检查webpack版本号     | npm webpack -v                                         |
| 查看webpack全部版本号 | npm view webpack versions                              |

​	首先需要有node环境 npm

​	查看当前可供选择的webpack版本,本次课程选用3.12.0

![img](wpsF85A.tmp.jpg) 

webpack4版本和3版本需要注意的区别:

1、 指令写法有所不同

2、 模块和插件之间是有版本依赖关系

以3.12.0版本为例,在后续的安装模块和插件的过程中,一定要按照笔记中指令的版本来进行安装,经过学习之后,可以再去了解4版本的指令写法和安装详情。

 

 

 

 

 

 

如果在npm安装过程中网速较慢,可以尝试用nrm切换镜像源:

​		npm i nrm -g //安装nrm

​		nrm ls //查看支持哪些镜像源

![img](wpsF85B.tmp.jpg) 

​		nrm use 镜像源 //切换镜像源

 

​	安装过cnpm的同学,本课程中的所有安装指令都可以用cnpm完成

 

 

 

 

 

 

 

 

 

 

 

 

## 14.3 webpack打包

| 本节主要指令 |                                   |
| ------------ | --------------------------------- |
| webpack打包  | webpack 输入文件路径 输出文件路径 |

​	webpack可以把浏览器无法执行的文件,经过打包后,产生一个浏览器可以执行的文件。

​	编写一段浏览器暂时无法运行的代码:

![img](wpsF85C.tmp.jpg) 

![img](wpsF86C.tmp.jpg) ![img](wpsF86D.tmp.jpg)

 

 

 

 

 

用webpack把浏览器所不支持的main.js进行一次打包 产出bundle.js

![img](wpsF86E.tmp.jpg) 

修改index.html引用的js文件 查看运行结果 成功

![img](wpsF86F.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 14.4 webpack配置

| 本节主要指令               |                                   |
| -------------------------- | --------------------------------- |
| webpack打包                | webpack 输入文件路径 输出文件路径 |
| npm初始化项目接管依赖关系  | npm init -y                       |
| npm本地安装 生产依赖jQuery | npm i jquery -s                   |

 

​	希望在调用webpack时 不需要再传入 输入文件 输出文件 两个参数

​	在项目的路径中可以编写一个 webpack.config.js文件 webpack配置文件

​		模仿常规的项目结构,创建以下目录:

![img](wpsF870.tmp.jpg) 

在index.html中 编写一些ul li元素

![img](wpsF871.tmp.jpg) 

 

需要在项目中本地安装jQuery npm管理依赖

用 npm init -y指令初始化当前项目

![img](wpsF872.tmp.jpg) 

​	初始化后就会在项目目录中产生一个package.json 文件 npm配置文件

​	本地化安装jQuery 下载到当前项目中

![img](wpsF873.tmp.jpg) 

![img](wpsF874.tmp.jpg) 

在main.js中引入jQuery,编写隔行换色代码:

![img](wpsF875.tmp.jpg) 

 

对main.js进行打包,得到bundle.js

![img](wpsF876.tmp.jpg) 

 

​	通过配置文件webpack.config.js优化省略webpack的参数

![img](wpsF877.tmp.jpg) 

 

 

 

 

 

 

 

## 14.5 webpack-dev-server 安装

| 本节主要指令               |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| 本地安装webpack-dev-server | npm i [webpack-dev-server@2.9.7](mailto:webpack-dev-server@2.9.7) -D |
| 本地安装webpack支持w-d-s   | npm i [webpack@3.12.0](mailto:webpack@3.12.0) -D             |
| 运行npm脚本                | npm run 配置脚本名                                           |

 

​	webpack-dev-server相当于 node当中的 nodemon

​	[在项目本地安装webpack-dev-server@2.9.7](mailto:在项目本地安装webpack-dev-server@2.9.7) 

![img](wpsF878.tmp.jpg) 

[这个版本是依赖于本地安装的webpack@3.0.0](mailto:这个版本是依赖于本地安装的webpack@3.0.0)以上版本

npm i [webpack-dev-server@2.9.7](mailto:webpack-dev-server@2.9.7) [webpack@3.12.0](mailto:webpack@3.12.0) -D

![img](wpsF879.tmp.jpg) 

​	由于webpack-dev-server不是全局安装,所以不能直接运行指令

![img](wpsF87A.tmp.jpg) 

​	定义npm脚本

![img](wpsF87B.tmp.jpg) 

 

 

​	用npm run 脚本启动w-d-s

![img](wpsF87C.tmp.jpg) 

​	在index中引入 w-d-s所说的在跟目录的输出结果:

![img](wpsF87D.tmp.jpg) 

​	发现确实一有改动,就会有一个新的打包结果 实现了开发的同时就能观察到效果

​	原本在/dist当中的bundle文件,一直没有改动过.

 

​	问题: 为什么磁盘中看不到根目录下有bundle.js文件呢?

​	由于w-d-s要频繁的进行打包,为了避免浪费磁盘开销,把这一过程放在内存中完成。

## 14.6 webpack-dev-server详细参数

| 本节主要指令               |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| webpack-dev-server附加参数 | webpack-dev-server –open –port 端口号 –contentBase 主页路径 |
| 运行npm脚本                | npm run 配置脚本名                                           |

​	还有其他参数可以学些 如 --inline --hot

​	在package.json中封装一个脚本

![img](wpsF88E.tmp.jpg) 

​	当执行webpack-dev-server指令时,自动打开系统默认浏览器 参数—open

​	最好能自定义服务端口,避免和主流的服务端口冲突 --port

​	每次访问到服务器时,最好能直接看到主页 --contentBase 主页所在路径

![img](wpsF88F.tmp.jpg) 

​	当设置了 --contentBase之后,让服务器的根目录变成了 src

​	所以这会导致在src上级的目录就无法访问了,比如项目目录/dist就无法访问了

​	![img](wpsF890.tmp.jpg)

​	由于webpack的输出总会放到根目录上,所以并不担心服务的跟目录切换

![img](wpsF891.tmp.jpg) 

## 14.7 主页虚拟化html-webpack-plugin

| 本节主要指令                |                                                              |
| --------------------------- | ------------------------------------------------------------ |
| 本地安装html-webpack-plugin | npm i [html-webpack-plugin](mailto:html-webpack-plugin@3.2.0) -D |
| 运行npm脚本                 | npm run 配置脚本名                                           |

 

​	现在 webpack打包结果bundle存在于 内存中

​	能不能让 首页 index.html也能像 bundle.js那样虚拟化到内存中呢?

![img](wpsF892.tmp.jpg) 

​	html-webpack-plugin 它是一个基于webpack的插件,所以需要在webpack的配置文件中注册这个插件才能运行。webpack.config.js配置文件

1、 引入html-webpack-plugin模块

![img](wpsF893.tmp.jpg) 

2、 用plugins属性注册webpack的插件 需要两个参数 template filename

![img](wpsF894.tmp.jpg) 

服务器当前的默认访问页面是源文件 index.html

index123.html 要手动输入url才能访问

![img](wpsF895.tmp.jpg) 

 

 

由于插件帮我们引入了文件,所以主页中不再需要手动引入:

![img](wpsF896.tmp.jpg) 

希望用虚拟化的index.html代替源文件,所以把虚拟化的输出文件名也改为index.html

![img](wpsF897.tmp.jpg) 

由于修改了配置文件,需要重启w-d-s服务: 再次访问,可以看到bundle的效果。

![img](wpsF898.tmp.jpg) 

观察服务启动后的提示:

![img](wpsF899.tmp.jpg) 

我们发现,webpack的输出新增了一个。他们都放在了服务的根目录

​	因为主页会被虚拟化到根目录,所以不再需要访问真实文件,也就不需要在指令当中添加- -contentBase了

![img](wpsF89A.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 14.8 webpack打包css

| 本节主要指令                |                                         |
| --------------------------- | --------------------------------------- |
| 安装style-loader css-loader | npm i style-loader css-loader@0.28.7 -D |

 

​	编写css样式文件如下:

​	![img](wpsF89B.tmp.jpg)

​	在main.js中引入样式文件:

​	![img](wpsF8AC.tmp.jpg)

![img](wpsF8AD.tmp.jpg) 

​	需要安装两个加载器 style-loader css-loader

![img](wpsF8AE.tmp.jpg) 

​	上述提示css-loader需要依赖webpack4以上版本，但目前又不想更新webpack,可以卸载css-loader转而安装webpack3可以支持的版本。

​	安装完成后,需要在webpack.config.js文件中注册一下 第三方模块

​	用module属性注册模块

![img](wpsF8AF.tmp.jpg) 

​	重启服务,查看效果成功了。

 

 

 

 

 

 

 

 

 

## 14.9 webpack打包less

| 本节主要指令     |                            |
| ---------------- | -------------------------- |
| 安装less-loader  | npm i less-loader@4.0.5 -D |
| 安装内部依赖less | npm i less -D              |

​	用less类型文件编写样式如下:

​	![img](wpsF8B0.tmp.jpg)

​	在main.js中引入:

​	![img](wpsF8B1.tmp.jpg)

​	查看报错信息:

![img](wpsF8B2.tmp.jpg) 

​	安装less-loader 和内部依赖less

 

 

 

 

 

![img](wpsF8B3.tmp.jpg)这意味着 less经过了 less-loader之后会转变成CSS所以还需要css-loader和style-loader的后续处理

​	配置文件处理规则

![img](wpsF8B4.tmp.jpg) 

## 14.10 webpack打包scss/sass

| 本节主要指令          |                                          |
| --------------------- | ---------------------------------------- |
| 安装sass-loader       | npm i sass-loader@6.0.6 -D               |
| 安装内部依赖node-sass | cnpm i node-sass -D (如果npm太慢,用cnpm) |

​	编写scss类型的样式文件如下:

​	![img](wpsF8B5.tmp.jpg)

 

 

​	在mian中引入:

​	![img](wpsF8B6.tmp.jpg)

​	观察报错

![img](wpsF8B7.tmp.jpg) 

​	安装 sass-loader 内部依赖 node-sass(npm装会很慢 建议使用cnpm)

​	添加文件处理规则:

![img](wpsF8B8.tmp.jpg) 

 

 

## 14.11 webpack打包图片

| 本节主要指令            |                           |
| ----------------------- | ------------------------- |
| 安装url-loader          | npm i url-loader@0.6.2 -D |
| 安装内部依赖file-loader | npm i file-loader -D      |

 

​	在index.html中创建一个div,并用样式添加背景图片

![img](wpsF8B9.tmp.jpg) 

​	在样式文件中,为div添加背景图:

![img](wpsF8C9.tmp.jpg) 

![img](wpsF8CA.tmp.jpg) 

 安装 url-loader 和内部依赖file-loader

 

 

 

 

 

添加文件处理规则

![img](wpsF8CB.tmp.jpg) 

## 14.12 webpack结合babel

| 本节主要指令                                                 |                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| 共5个包                                                      | npm i babel-core babel-loader@7.1.0 babel-preset-env -D |
| npm i babel-preset-stage-0 babel-plugin-transform-runtime -D |                                                         |

​	处理不同高低版本的JS语法

![img](wpsF8CC.tmp.jpg) 

​	写一些比较高级的语法,观察现象

![img](wpsF8CD.tmp.jpg) 

 

​	让webpack处理.js文件时,就要使用我们刚刚安装的 语法转换器

​	在配置文件中添加一个文件处理规则

![img](wpsF8CE.tmp.jpg) 

​	使用babel-loader需要在项目的根目录创建一个.babelrc 的配置文件

​	![img](wpsF8CF.tmp.jpg)

​	重启服务器,查看运行结果成功

![img](wpsF8D0.tmp.jpg) 

 

 

## 14.13 webpack结合Vue

​	安装Vue 写一段基本的VUe代码,发现报错

![img](wpsF8D1.tmp.jpg) 

​	import Vue from ‘vue’

1、 在项目根目录中有个node_modules文件夹

2、 根据包名,找到node_modules下的vue文件夹

3、 在vue文件中,找一个叫做package.json的配置文件

4、 在这个文件中 读取一个main属性 属性制定了在包被加载时的入口文件

![img](wpsF8D2.tmp.jpg) 

 

 

 

 

 

​	发现vue默认加载的文件不是我们想要的,希望能修改并获取vue.js

​	方法1:直接修改vue包中的package配置文件 main 和 module属性

![img](wpsF8D3.tmp.jpg) 

​	方法2:引入文件时 不再贪方便使用包名 而是直接通过相对路径引入vue.js(推荐)

![img](wpsF8D4.tmp.jpg) 

​	方法3:可以在webpack.config.js文件中 添加一个解析别名

![img](wpsF8D5.tmp.jpg) 

## 14.14 webpack中的VUE组件

| 本节主要指令   |                                                  |
| -------------- | ------------------------------------------------ |
| 安装vue-loader | npm i vue-loader@13.3.0 vue-template-compiler -D |

​	在webpack中,一个Vue组件单独一个文件 后缀为.vue

![img](wpsF8D6.tmp.jpg) 

main.js中引入组件 注册组件

![img](wpsF8D7.tmp.jpg) 

在html中使用

![img](wpsF8E8.tmp.jpg) 

观察报错,发现要安装对应.vue的loader

![img](wpsF8E9.tmp.jpg) 

安装 vue-loader vue-template-compiler

![img](wpsF8EA.tmp.jpg) 

​	重启服务器,观察现象。

​	render方法 可以载入组件

![img](wpsF8EB.tmp.jpg) 

​	绑定了render方法后,在div区域中不再需要使用标签形式显示组件。而且,div区域中的所有内容都会被 render渲染的组件覆盖掉。

## 14.15 webpack结合vue-router

| 本节主要指令   |                     |
| -------------- | ------------------- |
| 安装vue-router | npm i vue-router -D |

1、安装vue-router

​	2、在main.js中引入包名

## 14.16 vue-cli脚手架工具

​	webpack改变了Vue项目的编写过程。但是安装各种包实在是太痛苦了

​	vue-cli 是Vue官方提供的专门用于Vue项目的自动化项目结构搭建工具

 

1、 安装vue-cli

2、 在项目的目录下 运行 vue init webpack 项目名称 

![img](wpsF8EC.tmp.jpg) 

 

![img](wpsF8ED.tmp.jpg) 

 