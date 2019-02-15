---
title: bloge
date: 2019-01-25 09:51:34
tags: bloge
---
# hexo+github pages搭建博客



## 常见搭建博客的工具

- 自己搭建：

  php: typecho、wordpress

  nodejs: 常见hexo+github pages，还有一个（忘了）

- 借助第三方平台：

  csdn、[segmentfault.com](https://segmentfault.com/)、博客园、简书等等

## 为什么要搭建个人博客

- 记录与备忘

- 优化技术，更优的技术

- 求职

- 坚持

- 个人品牌

  .....



## hexo+github pages搭建博客



###  安装要求

保证自己电脑安装以下工具：

- git
- nodejs




### 安装步骤

<!-- more -->
​	1.hexo init  建文件夹

​	2.cnpm  install     安装依赖

​	cnpm install hexo-asset-image --save  图片

​	配置-config.yml

​	3.hexo n 文章名

 	4.hexo g   生成静态文件

​	5.hexo s  启动本地服务

​	6.ssh-keygen  生成私钥与公钥 

​	7.cnpm install hexo-deployer-git --save  安装部署

​	8.hexo d  线上仓库 

​	备份:

​	1.git init  建仓

​	2.git status  查看

​	3.git add . 添加

​	4.git commit -m '备注'

​	6.git remote -v  查看远程地址

​	7.git remote add origin 自己的地址

​	8.git push -u origin master  推送

​	9.

	注意事项: md 里面有{{插值表达式}}里面没有内容会报错 空格也行
			< img>与一些标签名 要空格隔开

官网地址： https://hexo.io/zh-cn/  

文档：https://hexo.io/zh-cn/docs/ 

hexo模板主题：https://hexo.io/themes/



安装hexo-cli

```
npm install hexo-cli -g  --registry https://registry.npm.taobao.org
```



hexo常用的几个命令：

hexo  n   '文章的名称'  

hexo g  ：   生成静态页面

hexo s    ：  启动本地服务

hexo d ：发布到线上仓库 ==username.github.io==

​	需要配置线上仓库：

​	1.配置 _config.yml
```
deploy:
  type: git
  repo: git@github.com:qingtian-boy/qingtian-boy.github.io.git
  branch: master
```
2. 安装一个工具



```
npm install hexo-deployer-git --save
```



测试图片

![1548343447540](cat.png)



### 换主题

参考地址：https://hexo.io/themes/



##  在创建一个仓库，用于备份博客源码

