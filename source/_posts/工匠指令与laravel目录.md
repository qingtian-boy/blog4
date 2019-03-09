---
title: 工匠指令与laravel目录
date: 2019-02-28 19:11:06
tags: laravel
---
E:\phpStudy\WWW\lv>php artisan
Laravel Framework version 5.3.30

Usage:
  command [选项] [参数]

Options:
  -V, --version        Display this application version
                       显示当前laravel框架的版本信息
<!-- more -->
Available commands:
  clear-compiled       Remove the compiled class file   
                       清除编译生成的文件，相当于 artisan optimize 的 反操作
  down                 Put the application into maintenance mode
                       将站点设为维护状态[使用后，站点无法访问]
  env                  Display the current framework environment
                       显示当前框架的环境信息
  help                 Displays help for a command
                       显示指定一个命令的帮助信息
  inspire              Display an inspiring quote
                       显示一条名言
  list                 Lists commands
                       显示详细artisan所有的命令信息[ artisan list 等同于 artisan ]
  migrate              Run the database migrations
                       执行数据迁移
  optimize             Optimize the framework for better performance
                       优化应用程序性能，生成自动加载文件，且产生聚合编译文件 bootstrap/compiled.php
  serve                Serve the application on the PHP development server
                       使用 PHP 内置的开发服务器启动应用 【要求 PHP 版本在 5.4 或以上】
  tinker               Interact with your application
                       进入与当前应用环境绑定的 控制台，相当于 mysql 的 命令行工具，可以直接执行php代码
  up                   Bring the application out of maintenance mode
                       将站点设回可访问状态[使用后，站点恢复正常]
 app
  app:name             Set the application namespace
                       设置应用程序命名空间
 auth
  auth:clear-resets    Flush expired password reset tokens
                       清除过期的密码重置密钥
 cache
  cache:clear          Flush the application cache
                       清除应用程序缓存
  cache:table          Create a migration for the cache database table
                       创建一个缓存数据库表的迁移
 config
  config:cache         Create a cache file for faster configuration loading
                       创建一个加载配置的缓存文件 
  config:clear         Remove the configuration cache file
                       删除配置的缓存文件
 db
  db:seed              Seed the database with records
                       填充种子测试数据到数据库记录中
 event
  event:generate       Generate the missing events and listeners based on registration
                       在记录上生成错过的事件和基础程序
 key
  key:generate         Set the application key
                       设置程序密钥
 make
  make:auth            Scaffold basic login and registration views and routes
                       搭建基本的登录和注册的视图和路由
  make:command         Create a new Artisan command
                       生成一个新的 Artisan 指令
  make:controller      Create a new controller class
                       生成一个控制器类
  make:event           Create a new event class
                       生成一个事件类
  make:job             Create a new job class
                       生成一个队列工作类
  make:listener        Create a new event listener class
                       生成一个监听器类
  make:mail            Create a new email class
                       生成一个邮件类
  make:middleware      Create a new middleware class
                       生成一个中间件
  make:migration       Create a new migration file
                       生成一个迁移文件
  make:model           Create a new Eloquent model class
                       生成一个Eloquent 模型类
  make:notification    Create a new notification class
                       生成一个通知类
  make:policy          Create a new policy class
                       生成一个策略类
  make:provider        Create a new service provider class
                       生成一个服务提供商的类
  make:request         Create a new form request class
                       生成一个表单消息类
  make:seeder          Create a new seeder class
                       生成一个填充类
  make:test            Create a new test class
                       生成一个测试类
 migrate
  migrate:install      Create the migration repository
                       在项目中对数据迁移进行初始化[在数据库中创建 迁移记录表 migrations ]
  migrate:refresh      Reset and re-run all migrations
                       重置并重新运行所有的迁移
  migrate:reset        Rollback all database migrations
                       重置，回滚全部数据库迁移
  migrate:rollback     Rollback the last database migration
                       回滚到最后一个数据库迁移
  migrate:status       Show the status of each migration
                       显示每一次数据迁移的状态信息
 notifications
  notifications:table  Create a migration for the notifications table
                       创建一个消息通知的数据库迁移文件
 queue
  queue:failed         List all of the failed queue jobs
                       列出全部失败的队列工作
  queue:failed-table   Create a migration for the failed queue jobs database table
                       创建一个迁移的失败的队列数据库工作表
  queue:flush          Flush all of the failed queue jobs
                       清除全部失败的队列工作
  queue:forget         Delete a failed queue job
                       删除一个失败的队列工作
  queue:listen         Listen to a given queue
                       监听一个确定的队列工作
  queue:restart        Restart queue worker daemons after their current job
                       重启现在正在运行的所有队列工作
  queue:retry          Retry a failed queue job
                       重试一个失败的队列工作
  queue:table          Create a migration for the queue jobs database table
                       创建一个迁移的队列数据库工作表
  queue:work           Start processing jobs on the queue as a daemon
                       进行下一个队列任务
 route
  route:cache          Create a route cache file for faster route registration
                       为了更快的路由登记，创建一个路由缓存文件
  route:clear          Remove the route cache file
                       清除路由缓存文件
  route:list           List all registered routes
                       列出全部的注册路由 
 schedule
  schedule:run         Run the scheduled commands
                       运行预定命令
 session
  session:table        Create a migration for the session database table
                       创建一个迁移的SESSION数据库工作表
 storage
  storage:link         Create a symbolic link from "public/storage" to "storage/app/public"
                       创建一个符号链接，可以让用户通过 /public/storage 就可以访问到 /storage/app/public
 vendor
  vendor:publish       Publish any publishable assets from vendor packages
                       发表一些可以发布的有用的资源来自服务提供商的插件包
 view
  view:clear           Clear all compiled view files
                       清除系统中所有的视图编译文件

------------------------------------------------------------------------------------------------------------

laravel目录

Laravel/
 |- .env                       环境配置文件 [ .env文件中的所有变量都会被加载到PHP的$_ENV中 ]
 |- .env.example               环境配置文件 [ demo，用于还原.env ]
 |- .gitattributes             Git配置文件  [ git相关，用于设置非文本文件的对比合并方式 ]
 |- .gitignore                 Git配置文件  [ git相关，用于设置git上传时需要忽略的文件 ]
 |- app/                       应用目录，项目应用程序主要存放目录 [ 相当于 ThinkPHP的 appcation目录 ]
    |- Console/                命令行程序目录
        |- Kernel.php          命令调用内核文件，包含commands变量(命令清单，自定义的命令需加入到这里)和schedule方法(用于任务调度，即定时任务) 
    |- Exceptions/             包含了自定义错误和异常处理类
    |- Http/                   应用的Http请求处理都在这里进行
        |- Controllers/        控制器存储目录 【 控制器 】
        |- Middleware/         中间件存储目录
        |- Kernel.php          包含http中间件和路由中间件的内核文件 
    |- Providers/              服务提供者目录
    |- User.php/               系统自带的模型实例[ Laravel作者不认可MVC，所以5.0版本以后去掉了Models目录， ]
 |- bootstrap/                 系统启动引导目录，用来存放系统启动时需要的文件，包含框架为优化性能而生成的一些文件，这些文件会被如index.php这样的文件调用。
    |- cache/                  路由和服务缓存文件存储目录，存放框架启动时生成和需要的缓存文件
    |- app.php                 创建框架应用实例的引导文件
    |- autoload.php            自动加载文件[ 注册composer自动加载器 ]
 |- config/                    配置文件存储目录[ 上线时修改这里的配置，开发阶段和线下维护时修改 .env中的配置 ]
    |- app.php                 系统顶级配置文件
    |- database.php            数据库配置文件
 |- database/                  数据库相关目录
    |- factories/              工厂类目录，用于数据填充
    |- migrations/             存储数据库迁移文件
    |- seeds/                  存放数据填充类的目录
 |- public/                    系统的唯一访问入口，存放所有对外开放的资源目录，如 CSS、JavaScript 以及图片等等皆被存放在此。
    |- index.php               入口文件
 |- resources/                 资源文件存储目录
    |- assets/    	           可存放包含LESS、SASS、CoffeeScript在内的原始资源文件
    |- lang/                   本地化文件目录【语言包】
    |- views/                  blade模板文件存储目录【视图】
 |- routes/                    路由文件目录
    |- web.php                 主路由文件【路由文件】
 |- storage/                   临时数据和运行日志存储目录，于存放 Session、Cache 这类临时文件,包含渲染后的View, 该目录可能需要可写权限
    |- app/                    系统生成文件存储目录
    |- framework/              框架生成的文件及缓存文件存储目录
       |- cache/               框架缓存文件目录
       |- sessions/            session数据存储目录
       |- views/               模板缓存目录【视图生成】
    |- logs/                   应用的运行日志文件存储目录
 |- tests/                     自动化测试的文件目录，用于 PHPUnit【单元测试】
 |- vendor/                    Laravel源代码和第三方依赖包存储目录
 |- artisan                    工匠指令，是命令行工具，在app/Console/Commands下编写自定义命令
 |- composer.json              存放依赖关系的文件[ 用于 composer 安装插件 ]
 |- composer.lock              锁文件，存放安装时依赖包的真实版本
 |- gulpfile.js                gulp的配置文件[ 前端构建工具 ]
 |- package.json               gulp的配置文件
 |- phpunit.xml                PHPUnit的配置文件[ PHP测试框架 ]
 |- readme.md
 |- server.php                 入口文件[ 使用 artisan serve 命令时的入口目录 ]