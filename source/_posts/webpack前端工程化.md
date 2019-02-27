---
title: webpack前端工程化
date: 2019-02-20 15:54:03
tags: webpack前端工程化
---
webpack前端工程化

# 第15讲 webpack前端工程化

## 15.1 webpack是什么

中文官网: <https://www.webpackjs.com/>

![img](wpsBE7B.tmp.jpg) 

​	webpack是可以通过npm来安装
<!-- more -->
​	webpack所需要用到的插件、载入器等 都可以通过npm管理

 

 

 

 

 

 

 

## 15.2 webpack安装

| 本节主要指令              |                                                            |
| ------------------------- | ---------------------------------------------------------- |
| 安装webpack               | npm install [webpack](mailto:webpack@3.12.0)@版本号 -g |
| 安装webpack-cli           | npm install webpack-cli@版本号 -g                          |
| 查看webpack全部版本号     | npm view webpack versions                                  |
| 查看当前已安装webpack版本 | npm view webpack version                                   |
| 安装cnpm指令              | npm i cnpm -g 速度更快 成功率更高                          |

 

​	想要使用webpack需要同时安装 webpack webpack-cli

​	webpack3版本 webpack和webpack-cli是集成到一起的

​	webpack4及以上版本 这两者是分离的 需要分别独立安装

![img](wpsBE7C.tmp.jpg) 

4版本 18年第1季度推出了

webpack小版本更新非常快 webpack及它的插件之间有版本依赖关系

安装webpack4 直接安装最新版本的插件即可

安装webpack3 安装插件时 需要手动选择旧版本

本教程大部分指令都可以通过cnpm进行安装

​	cnpm与npm安装软件 下载镜像源 cnpm比npm速度更快 安装容易成功

 

安装webpack

![img](wpsBE7D.tmp.jpg) 

安装webpack-cli

![img](wpsBE7E.tmp.jpg) 

查看两个软件的版本 检查安装是否成功

![img](wpsBE7F.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

## 15.3 webpack打包

​	创建一个英文目录 避免指令中出现中文导致问题

![img](wpsBE80.tmp.jpg) 

​	通过编辑器打开这个目录 直接选中磁盘中这个路径

![img](wpsBE81.tmp.jpg) 

![img](wpsBE82.tmp.jpg) 

 

 

 

![img](wpsBE83.tmp.jpg) 

1、 当前脚本语法不符合浏览器运行环境 require是node运行时环境的代码

2、 main.js与function.js有依赖关系

 

用webpack一键打包

| 本节主要指令 |                                                       |
| ------------ | ----------------------------------------------------- |
| webpack4打包 | webpack 输入文件路径 -o 输出文件路径 -- mode 打包模式 |
| webpack3打包 | webpack 输入文件路径 输出文件路径                     |

![img](wpsBE84.tmp.jpg) 

![img](wpsBE85.tmp.jpg) 

​	--mode作用是设置打包模式 有两种配置 production development

​		production生产模式 打包产出的代码经过压缩 体积较小

​		development 开发模式 打包产出的代码未经过压缩 保持了代码格式

​		如果没有设置mode 默认以生产模式production打包

 

 

​	观察打包结果

![img](wpsBE86.tmp.jpg) 

​	webpack会把所有依赖文件 全部整合成一个文件

![img](wpsBE87.tmp.jpg) 

​	webpack会解决所有的 语法不兼容问题 最终产出的代码一定是浏览器能够运行的

![img](wpsBE98.tmp.jpg) 

​	在index.html文件中进入webpack打包的结果 而非源文件

## 15.4 webpack前端工程化

​	前端工程化可以理解为一套前端比较先进的开发环境

​	前端所需插件、工具比较多 为了良好地管理 用npm管理包的依赖关系

​	通过webpack对所有的相互依赖的代码统一打包 解除依赖关系

| 本节主要指令               |                                      |
| -------------------------- | ------------------------------------ |
| webpack打包                | webpack 输入文件路径 -o 输出文件路径 |
| npm初始化项目接管依赖关系  | npm init -y                          |
| npm本地安装 生产依赖jQuery | npm i jquery -s(--save 生产环境依赖) |

 

​	模拟符合前端工程化的项目

​	1、创建项目目录

![img](wpsBE99.tmp.jpg) 

 

2、为了能够良好管理第三方包 用npm初始化项目

![img](wpsBE9A.tmp.jpg) 

​	3、通过npm安装第三方包 npm i jquery

![img](wpsBE9B.tmp.jpg) 

 

 

 

 

​	4、用模块化编程方式 实现 各行换色

![img](wpsBE9C.tmp.jpg) 

​	5、通过webpack的打包 产生浏览器能够运行的 经过整合的代码

 

![img](wpsBE9D.tmp.jpg) 

​	6、修改index.html中引入的文件 引入webpack的打包结果

![img](wpsBE9E.tmp.jpg) 

 

 

 

 

 

## 15.5 webpack配置

​	学习webpack的详细配置 能够让使用的过程更方便

​	可以在项目中创建一个 webpack.config.js 文件 是webpack的配置文件

​	实际工作中 打包的输入文件和输出文件不会频繁更换 通常是固定的 能否省略webpack打包指令中的参数

​	在没有创建配置文件的情况下,不能直接使用webpack指令省略参数

​	![img](wpsBE9F.tmp.jpg)

​	进行输入文件和输出文件的配置 为了今后使用指令的方便:

![img](wpsBEA0.tmp.jpg) 

![img](wpsBEA1.tmp.jpg) 

配置好后 调用webpack不需要参数了

## 15.6 webpack-dev-server 安装

​	每次修改代码都需要重新打包

webpack-dev-server webpack开发服务 就相当于是node当中的 nodemon

能够在修改代码后自动打包

应该把webpack-dev-server服务安装为一个项目中的本地服务

需要依赖于webpack和webpack-cli 所以这三者都要本地安装

| 本节主要指令                  |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| 本地安装webpack-dev-server    | npm i [webpack-dev-server](mailto:webpack-dev-server@2.9.7) -D |
| 本地安装webpack支持w-d-s      | npm i [webpack](mailto:webpack@3.12.0) -D                    |
| 本地安装 webpack-cli支持w-d-s | npm i webpack-cli -D                                         |
| 运行npm脚本                   | npm run 配置脚本名                                           |

![img](wpsBEA2.tmp.jpg) 

​	w-d-s服务是本地安装 不是全局安装

![img](wpsBEA3.tmp.jpg) 

​	为了运行开发服务 在npm配置文件中 封装一个脚本dev

![img](wpsBEA4.tmp.jpg) 

​	通过npm run dev运行服务

![img](wpsBEA5.tmp.jpg) 

​	由于开发服务 把打包的结果放到了项目根目录 修改index.html中的引入路径

![img](wpsBEA6.tmp.jpg) 

​	只要有开发服务的存在 修改代码就不需要手动打包了

![img](wpsBEA7.tmp.jpg) 

​	因为我们会频繁修改代码 都需要重新打包 为了加快打包的效率

​	减少磁盘开销 所以w-d-s打包结果是虚拟化到内存中的 为了读写效率高

 

 

## 15.7 webpack-dev-server详细参数

​	解决具体问题:

1、 解决访问地址不能直接看见首页的问题

2、 能否自己设置一个不容易冲突的端口

3、 懒人必备 希望在运行w-d-s服务时 自动打开浏览器 访问项目

都可以通过webpack的详细参数实现

 

​	参数 --contentBase 把项目的访问根目录设置到指定位置

![img](wpsBEB8.tmp.jpg) 

![img](wpsBEB9.tmp.jpg) 

​	在index.html引入打包结果时 路径可以修改

![img](wpsBEBA.tmp.jpg) 

 

 

 

--port 端口号

![img](wpsBEBB.tmp.jpg) 

![img](wpsBEBC.tmp.jpg) 

 

​	--open 能够在启动w-d-s服务时 自动打开系统默认浏览器并访问项目

![img](wpsBEBD.tmp.jpg) 

​	--inline --hot 可以练习完后上网了解

 

 

 

 

 

 

 

 

 

 

 

 

## 15.8 主页虚拟化html-webpack-plugin

​	bundle.js已经虚拟化到内存中 读写效率高

​	能否将项目首页 index.html也虚拟化到内存中

​	需要借助插件html-webpack-plugin 把index.htm

虚拟化到内存中 能够自动注入打包结果

| 本节主要指令                |                                                              |
| --------------------------- | ------------------------------------------------------------ |
| 本地安装html-webpack-plugin | npm i [html-webpack-plugin](mailto:html-webpack-plugin@3.2.0) -D |
| 运行npm脚本                 | npm run 配置脚本名                                           |

![img](wpsBEBE.tmp.jpg) 

​	在webpack.config.js 配置文件中 添加插件的配置项说明

![img](wpsBEBF.tmp.jpg) 

启动w-d-s服务

![img](wpsBEC0.tmp.jpg) 

​	查看虚拟化首页index123与源文件index的不同

![img](wpsBEC1.tmp.jpg) 

 

 

 

 

 

 

 

 

​	修改index.html 不需要引入js文件

![img](wpsBEC2.tmp.jpg) 

​	修改配置项 把产出文件命名为index.html

![img](wpsBEC3.tmp.jpg) 

 

 

 

 

 

 

# 课程小结

​	安装了webpack cnpm i webpack webpack-cli -g

​	webpack4打包 webpack 输入文件 -o 输出文件 --mode production

​		--mode打包模式 有两种 production development

​			production 生产环境 压缩文件 体积较小

​			development 开发环境 保留了代码格式 可读性较强

​	前端工程化 一套比较先进的开发环境

​		所有的包/依赖 通过npm管理

​			npm init -y 初始化项目 得到一个package.json的npm配置文件

​		所有的包通过npm安装 cnpm i 包名 -S/-D

 

​	webpack详细配置 为了使用webpack方便

​		创建webpack.config.js webpack配置文件 说明了输入文件和输出文件

 

​	webpack-dev-server 是为了实现自动打包 每次修改文件能够马上看到效果

​		cnpm i webpack-dev-server webpack webpack-cli -D

 

​	由于webpack-dev-server不是全局安装 所以需要通过npm的脚本调用

​		封装脚本“dev”:”webpack-dev-server”

​		启动服务 npm run dev

 

​	学习w-d-s详细参数 为了使用w-d-s方便

​	“dev”:”webpack-dev-server --contentBase 根目录 --port 端口号 --open”

 

​	学习hwp 为了 首页虚拟化 省略index.html中对js脚本的引入 彻底摆脱文件依赖

​		cnpm i html-webpack-plugin -D

​		需要在webpack.config.js中进行插件的配置工作

 

​	上述的过程 使用webpack搭建了比较先进的前端工程化开发环境

​	解决了文件数量多、依赖复杂的问题

 

 

 

 

 

 

 

 

 

 

 

 

 

## 15.9 webpack打包css

​	创建index.css样式文件 希望在main.js中引入

![img](wpsBEC4.tmp.jpg) 

![img](wpsBEC5.tmp.jpg) 

由于webpack不能识别和解析.css文件 所以报错

![img](wpsBED5.tmp.jpg) 

​	根据提示 安装css载入器 style-loader css-loader

| 本节主要指令                |                                  |
| --------------------------- | -------------------------------- |
| 安装style-loader css-loader | npm i style-loader css-loader -D |

![img](wpsBED6.tmp.jpg) 

需要在webpack的配置文件中 说明文件处理规则 .css文件 用这两个载入器进行处理

![img](wpsBED7.tmp.jpg) 

## 15.10 webpack打包less

![img](wpsBED8.tmp.jpg) 

​	创建了index.less文件 并在main.js中引入 观察报错

![img](wpsBED9.tmp.jpg)![img](wpsBEDA.tmp.jpg) 

![img](wpsBEDB.tmp.jpg) 

根据报错 安装对应的载入器 less-loader less

| 本节主要指令     |                      |
| ---------------- | -------------------- |
| 安装less-loader  | npm i less-loader -D |
| 安装内部依赖less | npm i less -D        |

![img](wpsBEDC.tmp.jpg) 

​	在webpack的配置文件中 添加文件处理规则

![img](wpsBEDD.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 15.11 webpack打包scss/sass

![img](wpsBEDE.tmp.jpg) 

 

![img](wpsBEDF.tmp.jpg) 

​	创建index.scss样式文件 并在main.js中引入

![img](wpsBEE0.tmp.jpg)![img](wpsBEE1.tmp.jpg) 

![img](wpsBEF2.tmp.jpg) 

​	根据提示安装对应的载入器 sass-loader node-sass(一定使用cnpm)

| 本节主要指令          |                                                    |
| --------------------- | -------------------------------------------------- |
| 安装sass-loader       | npm i sass-loader -D                               |
| 安装内部依赖node-sass | cnpm i node-sass -D (npm太慢,而且容易失败) |

![img](wpsBEF3.tmp.jpg) 

​	添加文件处理规则

![img](wpsBEF4.tmp.jpg) 

## 15.12 webpack打包图片

​	在项目中放一张图片

![img](wpsBEF5.tmp.jpg) 

![img](wpsBEF6.tmp.jpg) 

![img](wpsBEF7.tmp.jpg) 

​	根据提示安装 载入图片文件类型所需载入器 url-loader file-loader

| 本节主要指令            |                      |
| ----------------------- | -------------------- |
| 安装url-loader          | npm i url-loader -D  |
| 安装内部依赖file-loader | npm i file-loader -D |

![img](wpsBEF8.tmp.jpg) 

 

 

​	添加文件处理规则: .jpg .jpeg .png .gif .bmp

![img](wpsBEF9.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 15.13 webpack结合babel

​	处理高级JS语法

![img](wpsBEFA.tmp.jpg) 

![img](wpsBEFB.tmp.jpg) 

![img](wpsBEFC.tmp.jpg) 

​	借助babel语法编译器 把高级语法转化为低级语法

![img](wpsBEFD.tmp.jpg) 

 

 

​	5个部分:

​		babel-core    babel核心包

​		babel-loader  js文件载入器 加载js代码后 会进行语法转化

​		babel-plugin-transform-runtime 语法解析/转换插件

​		babel-preset-env

​		babel-preset-stage-0 语法转换的语法包

| 本节主要指令                                   |                                        |
| ---------------------------------------------- | -------------------------------------- |
| 共5个包                                        | npm i babel-core babel-loader@7 -D |
| npm i babel-plugin-transform-runtime -D        |                                        |
| npm i babel-preset-stage-0 babel-preset-env -D |                                        |

![img](wpsBEFE.tmp.jpg) 

 

 

 

 

 

 

添加文件处理规则:

![img](wpsBF0E.tmp.jpg) 

​	发现启动服务时 加载babel-loader有报错

![img](wpsBF0F.tmp.jpg) 

​	babel要求 在项目根目录中添加 .babelrc配置文件 说明语法包和插件 

![img](wpsBF10.tmp.jpg) 

webpack已经能够处理高级js语法了

![img](wpsBF11.tmp.jpg) 

 

 

 

# 课程小结 webpack处理各类型文件

​	css 载入器 cnpm i style-loader css-loader -D

​	less 载入器 cnpm i less-loader less -D

​	scss 载入器 cnpm i sass-loader node-sass -D

​	url载入器 cnpm i url-loader file-loader -D

​	js高级语法转换器 cnpm i babel-core babel-loader@7 -D

​	js语法转换插件 cnpm i babel-plugin-transform-runtime -D

​	js语法包 cnpm i babel-preset-env babel-preset-stage-2 -D

​	设置文件处理规则:

![img](wpsBF12.tmp.jpg) 

​	babel要求在项目根目录设置配置文件

![img](wpsBF13.tmp.jpg) 

webpack已经能够处理各种类型文件 今后开展新项目使用完整的环境依赖即可

## 15.14 webpack结合Vue

## 15.15 webpack中的VUE组件