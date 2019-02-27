---
title: apache设置代理访问nodejs
date: 2019-02-26 15:50:04
tags: nodejs
---
# Apache设置代理访问nodejs

后面会学习nginx服务器，一样也可以设置代理访问。



这里以phpstudy环境配置为例： ==设置域名访问nodejs服务==


<!-- more -->
1.修改apache配置文件httpd.conf配置文件，开启以下模块，去掉前面的`#`号即可：

```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
```

2.设置域名如：`http://api.vueshop.com`代理nodejs的`3000`端口服务

需要修改虚拟主机配置文件：`vhosts.conf`

```
<VirtualHost *:80>
  ServerName api.vueshop.com
  ProxyRequests Off
  <Proxy *>
    Require all granted  
  </Proxy>
  <Location />
    ProxyPass http://127.0.0.1:3000/
    ProxyPassReverse http://127.0.0.1:3000/
  </Location>
</VirtualHost>
```

3.设置hosts文件映射

```
127.0.0.1 api.vueshop.com
```

/重启phpstudy，使配置生效。

4.开启nodejs的3000端口服务

```javascript
var express = require('express');
var app = express();

app.get('/',function(req,res){
    res.end('hello nodejs');
});

app.listen("3000",function(){
    console.log("server at http://127.0.0.1:3000");
});
```

5.浏览器中输入`http://api.vueshop.com`访问可以看到以下响应结果：

```
hello nodejs
```