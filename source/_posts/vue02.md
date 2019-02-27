---
title: vue02
date: 2019-02-20 15:43:20
tags: VUE
---
VUE前端框架(第三天)

# 第11讲 Vue通信技术

## 11.1 HTTP协议简要复习

​	请求 一个完整的请求 应该包含4个部分 请求行 请求头 空行 请求体(数据)

![img](wpsCE6E.tmp.jpg) 

<!-- more -->
​	响应 一个完整的响应 应该包含4个部分 响应行 响应头 空行 响应体(数据)

![img](wpsCE6F.tmp.jpg) 

 

 

​	get请求和post请求的不通:

1、 请求数据的位置不同 get可以放在url中 post必须放在请求体中

2、 post有专属的请求头 post请求通常在提交表单时产生

![img](wpsCE80.tmp.jpg) 

​	Content-type:application/x-www-form-urlencoded

![img](wpsCE81.tmp.jpg) 

get类型的请求 没有上述特别的请求头

![img](wpsCE82.tmp.jpg) 

请求数据的类型

![img](wpsCE83.tmp.jpg) 

 

 

 

 

 

## 11.2 PHP构建支持jsonp后端

​	跨域技术:

​		一个来源于[www.A.com](http://www.A.com)的网页 源[www.A.com](http://www.A.com)

​		向[www.B.com](http://www.B.com)发起请求 就是跨域/跨源请求

 

​	同源策略:

​		一个来源于[www.A.com](http://www.A.com)的网页 只能向[www.A.com](http://www.A.com)发起请求

 

​	为了研究跨域技术 要创建两个不同的域名(虚拟主机)

![img](wpsCE84.tmp.jpg) 

​	创建本地域名解析

![img](wpsCE85.tmp.jpg) 

![img](wpsCE86.tmp.jpg) 

![img](wpsCE87.tmp.jpg) 

![img](wpsCE88.tmp.jpg) 

​	我们发现浏览器上为了安全性 有同源策略的机制 这种机制阻止我们发起跨域请求

​	同源策略的漏洞 主要在于src属性

来源于[www.a.com](http://www.a.com)的网页禁止读取除了[www.a.com](http://www.a.com)之外的资源
![img](wpsCE89.tmp.jpg)

聪明的程序员们就利用这种漏洞实现自己的技术需要 创建了jsonp跨域技术

![img](wpsCE8A.tmp.jpg) 

![img](wpsCE8B.tmp.jpg) 

获得了一个支持jsonp的服务端 并且无论前端怎么修改 后端代码都无需再改
	![img](wpsCE8C.tmp.jpg)

前端必须按照后端要求 传递参数 参数名是callback

使用jQuery发起jsonp请求时

![img](wpsCE8D.tmp.jpg) 

![img](wpsCE8E.tmp.jpg) 

## 11.3 vue-resource 异步通信

​	通过上一讲已经获得了一个能够支持jsonp的服务端

​	![img](wpsCE8F.tmp.jpg)

​	实现在项目中 可以在前端获取后端数据 采用异步通信技术Ajax

​	可以采用原生ajax代码 可以采用jQuery

 

​	vue-resource是一个搭配VUE使用的异步通信插件(不能独立使用)

​	vue-resource就像jQuery 引入然后调用方法即可

该项目可以再github中下载和查看说明文档: 

​	<https://github.com/pagekit/vue-resource/blob/develop/docs/http.md>

​	在课程材料中已经准备好了 v-r插件

![img](wpsCE90.tmp.jpg) 

开启一个新页面 尝试引入并调用v-r

![img](wpsCEA0.tmp.jpg) 

![img](wpsCEA1.tmp.jpg) 

​	通过全局调用的方式 尝试发起各种请求

![img](wpsCEA2.tmp.jpg) 

​	参数简要说明:

​		<url:请求地址>

​		[body]:可选参数 请求体 请求数据

​		[config]:可选参数 手动配置项

 

 

​	为了请求测试准备好服务端:

![img](wpsCEA3.tmp.jpg) 

![img](wpsCEA4.tmp.jpg) 

 

 

 

 

 

 

 

 

发起get请求 观察响应对象

![img](wpsCEA5.tmp.jpg) 

![img](wpsCEA6.tmp.jpg) 

![img](wpsCEA7.tmp.jpg) 

​	还可以通过以下写法发起get请求

![img](wpsCEA8.tmp.jpg) 

 

 

 

 

 

 

 

 

发起post请求 要注意通过配置项修改请求头content-type

![img](wpsCEA9.tmp.jpg) 

![img](wpsCEAA.tmp.jpg) 

​	查看请求头和请求数据的状态

![img](wpsCEAB.tmp.jpg) 

​	查看响应数据

![img](wpsCEAC.tmp.jpg) 

 

 

 

​	发送jsonp请求

![img](wpsCEAD.tmp.jpg) 

​	查看响应内容

![img](wpsCEAE.tmp.jpg) 

​	查看请求参数与响应的一致性

![img](wpsCEAF.tmp.jpg)
	简单小结

​	发起get请求 Vue.http.get(url?参数)

​	发起post请求 Vue.http.post(url,data,{ emulateJSON:true})

​	发起jsonp请求 Vue.http.jsonp(url?参数) 发起jsonp请求必须在网页加载后

 

 

 

 

 

 

## 11.4 axios 异步通信

​	axios是一个独立的插件 可以独立使用

​	该项目可以在github中下载: <https://github.com/axios/axios/>

​	另外还有一份npm英文说明文档: <https://www.npmjs.com/package/axios>

​	有中文说明文档,建议逐渐习惯看英文文档: 

<https://www.kancloud.cn/yunye/axios/234845>

​	在课程材料中已经准备好了axios

![img](wpsCEC0.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

查看文档说明:

![img](wpsCEC1.tmp.jpg) 

![img](wpsCEC2.tmp.jpg) 

发起get请求

![img](wpsCEC3.tmp.jpg) 

![img](wpsCEC4.tmp.jpg) 

观察响应数据对象

 

![img](wpsCEC5.tmp.jpg) 

 

 

 

​	查看说明文档

​	![img](wpsCEC6.tmp.jpg)

​	要注意 post请求需要设置对应的请求头 通过配置项来设置

![img](wpsCEC7.tmp.jpg) 

​	发起post请求

![img](wpsCEC8.tmp.jpg) 

​	查看响应数据对象

![img](wpsCEC9.tmp.jpg) 

 

 

 

​	axios没有提供jsonp请求方法

![img](wpsCECA.tmp.jpg) 

 

​	小结:

​		axios.get(url?参数)

​		axios.get(url,{params:参数})

​		axios.post(url,参数,{

headers:{“Content-type”:”application/x-www-form-urlencoded”}

})

 

 

 

 

 

 

## 11.5 训练项目 品牌管理 前后端交互

​	在课程材料中提供了品牌信息接口项目

![img](wpsCECB.tmp.jpg) 

​	引入文件

![img](wpsCECC.tmp.jpg) 

​	当打开页面时 查询后端的所有品牌数据 使用了created钩子函数

![img](wpsCECD.tmp.jpg) 

​	改写新增功能 需要改写新增功能的add方法 把数据提交给服务端

![img](wpsCEDE.tmp.jpg) 

![img](wpsCEDF.tmp.jpg) 

​	改写删除功能 把要删除的id发送给后端接口

![img](wpsCEE0.tmp.jpg) 

![img](wpsCEE1.tmp.jpg) 

 

 

 

 

# 课程小结 前后端分离

​	分离思想一直存在 让工作分门别类 并让开发人员职能明确

​	不同功能的代码分离到不同的文件和目录中 便于项目长久的维护和迭代

 

​	前端只负责对用户交互和前端业务逻辑

​					接口

​	后端只负责对数据库的读写和后端业务逻辑

 

 

 

 

 

 

 

 

 

 

 

 

 

 

# 第12讲 过渡动画

## 12.1 自定义过渡动画

​	主要是为了实现界面调度 产生过渡动画

​	所谓动画就是网页元素从一个状态到另一个状态

​	原生过渡动画 利用了transition的样式

![img](wpsCEE2.tmp.jpg) 

vue基于这种动画效果 提供了一个<transition>

用transition标签包裹动画的主角 上述案例中 是 div#box

在transition标签上 可以用name属性给动画起名

 

需要根据动画名 设置两种动画样式 离场动画 入场动画

​	入场动画 需要设置三种样式

​		动画名-enter: 定义元素入场之前的状态(看不见)

​		动画名-enter-active: 两种状态之间的过渡期

​		动画名-enter-to: 定义元素入场之后的状态(看得见)

 

​	离场动画 需要设置三种样式

​		动画名-leave: 定义元素离场之前的状态(看得见)

​		动画名-leave-active: 两种状态之间的过渡期

​		动画名-leave-to: 定义元素入场之后的状态(看不见)

 

 

 

 

 

 

 

 

 

 

 

 

![img](wpsCEE3.tmp.jpg) 

 

 

 

 

 

 

 

## 12.2 第三方动画库 animate.css

​	借助第三方动画库 节省设置和调试样式的时间 第三方动画更好看

animate.css官网: <https://daneden.github.io/animate.css/>

![img](wpsCEE4.tmp.jpg) 

在课程材料中已有本地文件:

![img](wpsCEE5.tmp.jpg) 

 

 

 

 

1、 引入动画库

![img](wpsCEE6.tmp.jpg) 

2、 在transition标签上 提供了两个属性 

enter-active-class:入场动画

leave-active-class:离场动画

3、 需要设置公共样式animated

![img](wpsCEE7.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

## 12.3 训练案例 列表动画

​	NBA球员名单列表

​	ul li实现球员列表

![img](wpsCEE8.tmp.jpg) 

​	设置列表基本样式:

![img](wpsCEE9.tmp.jpg) 

​	编写数据新增与删除功能:

![img](wpsCEEA.tmp.jpg) 

​	用transition标签包裹动画主体 但是发现并解决了一些问题

![img](wpsCEFA.tmp.jpg) 

![img](wpsCEFB.tmp.jpg) 

![img](wpsCEFC.tmp.jpg) 

​	用新学的两个属性结合第三方动画库 animate.css实现元素的过渡动画

![img](wpsCEFD.tmp.jpg) 

 

​	过渡动画是为了实现网页元素的简单动画效果

​	或者界面切换之间的缓和过渡动画效果

 

 

# 第13讲 组件

## 13.1 组件的概念

​	组件化 模块化 

​	组件就是可以复用的VUE实例

 

​	一个vue实例具有几个属性和功能的,data管理自己的数据,methods封装所需方法

​	把网页看作是很多功能模块:

![img](wpsCEFE.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 13.2 定义组件

​	Vue.component(组件名,配置属性) component组件

![img](wpsCEFF.tmp.jpg) 

![img](wpsCF00.tmp.jpg) 

 

 

 

​	![img](wpsCF01.tmp.jpg)

​	定义一个组件有以下步骤:

1、调用Vue.component(组件名,{

​			template:”模板内容”

})

​		2、定义模板 使用template标签定义模板 模板中只能有一个根标签

![img](wpsCF02.tmp.jpg) 

4、 组件名可以当做标签使用

 

 

 

 

 

 

## 13.3 私有组件

​	组件也有全局/私有之分

 

​	全局组件在vue实例外部定义的组件 可以在所有vue实例中使用

![img](wpsCF03.tmp.jpg) 

 

 

 

 

​	私有组件在vue实例内部定义 只能在当前vue实例中使用 components

![img](wpsCF04.tmp.jpg) 

​	在#app2中使用私有组件 报错

 

 

 

 

 

 

 

 

 

 

 

 

## 13.4 定义组件 数据和方法

​	定义一个计数器组件

![img](wpsCF05.tmp.jpg) 

​	定义组件数据 组件中的数据 是一个 返回数据对象的函数:

​	![img](wpsCF06.tmp.jpg)

​	给按钮绑定事件 并在组件中定义方法:

​	![img](wpsCF07.tmp.jpg)

 

 

 

 

 

![img](wpsCF08.tmp.jpg) 

组件的数据是独立管理的 同一套组件数据不会共享 不会互相影响

![img](wpsCF19.tmp.jpg) 

 

 

 

 

## 13.5 组件中的data注意事项

​	组件的数据是独立管理的 相互之间不影响

​	data属性 是 返回数据对象的 函数

![img](wpsCF1A.tmp.jpg) 

​	要注意避免这种错误

 

 

 

 

 

 

 

 

## 13.6 双组件切换(v-if v-show)

​	经常会看到几个网页界面之间的调度

![img](wpsCF1B.tmp.jpg) 

​	有两个组件 一个登陆组件 一个注册组件 两个按钮

![img](wpsCF1C.tmp.jpg) 

​	通过一个flag变量 结合 v-show指令 实现界面切换

## 13.7 多组件切换(动态组件 is属性)

​	模范技术论坛 实现多个文章列表组件切换

​	模拟3种文章 PHP文章 前端文章 Java文章

​	![img](wpsCF1D.tmp.jpg)

​	在页面中创建导航栏 显示3个组件

![img](wpsCF1E.tmp.jpg) 

 

通过一个name数据 控制要显示的文章列表 结合v-show指令实现组件切换

![img](wpsCF1F.tmp.jpg) 

介绍新技术完成上述需求 VUE提供一个动态组件标签

<component :is=要扮演的组件名></component>

![img](wpsCF20.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 13.8 组件切换动画(transition mode)

![img](wpsCF21.tmp.jpg) 

​	发生页面上同时出现多个文章列表的情况

![img](wpsCF22.tmp.jpg) 

​	transition标签提供了一个mode属性 设置动画模式 out-in

​		一个组件离开之后 才允许下一个组件进入

![img](wpsCF23.tmp.jpg) 

 

 

 

 

 

 

 

 

 

## 13.9 props属性定义组件数据

​	定义组件数据可以使用data属性来定义

也可以使用props属性定义 props属性可以把组件中的标签属性注册为数据

![img](wpsCF24.tmp.jpg) 

 

 

 

 

## 13.10 父组件向子组件传递数据

​	组件的数据都是独立管理的 不共享的

​	父组件中的数据不能在子组件中使用

​	网页元素与模板

![img](wpsCF25.tmp.jpg) 

注册组件 实例化VUE：

![img](wpsCF26.tmp.jpg) 

父组件数据传递给子组件:

![img](wpsCF36.tmp.jpg) 

 

 

 

 

 

 

 

 

 

 

 

## 13.11 父组件向子组件传递方法

​	构建父子组件:

![img](wpsCF37.tmp.jpg) 

 

 

 

 

 

 

​	在父组件中定义方法 通过props属性传递给子组件

![img](wpsCF38.tmp.jpg) 

 

 

 

 

 

 

​	可以通过给子组件绑定事件的方式 传递父组件方法

​	在子组件中 使用this.$emit(事件类型) 强制触发指定类型的事件

![img](wpsCF39.tmp.jpg) 

 

 

 

 

# 课程小结

​	vue-resource

​		get:

​			Vue.http.get(url?参数).then(处理响应)

​			Vue.http.get(url,{params:参数}).then()

​		post:

​			Vue.http.post(url,参数,{emulateJSON:true}).then()

​		jsonp:

​			根get基本一致

​			Vue.http.jsonp(url?参数)

​	axios

​		get: 

axios.get(url?参数).then()

axios.get(url,{params:参数}).then()

​		post:

​			axios.post(url,参数,{

​				headers:{

​					“Content-type”:”application/x-www-form-urlencoded”

}

})

 

​	过渡动画:

1、 引入动画库

2、 用transition标签包裹动画主角/主体

3、 用enter-active-class leave-active-class定义入场/离场动画

需要设置公共基本样式enter-active-class=”animated 动画名”

4、 如果动画的主体不是单一的 用 transition-group 别忘了分配key属性

5、 mode属性可以设置动画模式 out-in in-out根据需要设置

 

过渡动画可以制作细小网页元素动画效果 也可以制作大型网页界面切换效果

 

 

 

 

 

 

 

 

 

 

 

 

 

组件的概念: 组件是可以复用的VUE实例

1、 组件是vue实例 所以它可以接受data、methods、filters等等vue实例应该具备的配置属性

2、 组件是可以复用的 通过注册 组件名可以当做标签使用

 

组件的定义方式

​	全局: Vue.component(组件名,{template:模板})

​	私有: 

new Vue({

components:{

​	组件名:{

​		template:模板

}

}

})

 

​	定义组件模板的小技巧:

​		建议在HTML代码中 用template标签定义模板 代码可读性、维护性好

 

​	定义组件数据时 data是一个返回数据对象的函数

 

​	动态组件 <component :is=要扮演的组件名>

​	父组件向子组件传递数据和方法都可以通过props结合标签属性实现

​	父组件向子组件传递方法 还可以通过 子组件.$emit 结合事件实现
