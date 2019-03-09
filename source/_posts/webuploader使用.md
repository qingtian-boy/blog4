---
title: webuploader使用
date: 2019-03-06 19:47:34
tags: laravel
---
# 一、webuploader使用

官网：<https://fex.baidu.com/webuploader/>

## 1. 效果

使用webuploader，其显示效果如下：

![img](wps6BAE.tmp.jpg)

ajax无刷新上传文件并提供本地预览效果

注意，这里的本地预览，可以是上传成功以后的预览，也可以没上传前的预览。这里的预览，和后台无关的，这是前端js实现的。

ajax的XMLhttpRequest 对象下的uploader子对象 实现的进度条，所以低版本浏览器不支持。

<!-- more -->

下载

<https://fex.baidu.com/webuploader/download.html>

![img](wps6BAF.tmp.jpg)



## 2. 使用

先引入webuploader插件的css和js文件。

<link rel="stylesheet" type="text/css" href="./lib/webuploader/0.1.5/webuploader.css" />

<script type="text/javascript" src="./lib/webuploader/0.1.5/webuploader.min.js"></script>

 

## 3. 初始化

var uploader = WebUploader.create({

   配置项: 值,

   配置项: 值,

});

 

 

## 4. 配置项(Options)

### 4.1 自动上传文件

auto: true,

开启默认上传图片。

 

### 4.2 设置swf文件的路径

swf: "./lib/webuploader/0.1.5/Uploader.swf",

因为部分低版本浏览器并不支持HTML5的ajax文件上传，所以对于这部分浏览，可以设置告诉系统，使用swf来完成文件上传功能。

 

### 4.3 设置选择图片的按钮

​    pick: '#picker',

这里填写的选择符，使用jQuery选择器中的选择符

 

### 4.4 默认webuploader是压缩图片后才上传

​    resize: false,

这里，我们不需要压缩，可以关闭掉。

 

### 4.5 只允许选择图片文件

​    accept: {

​        title: "图片",

​        extensions: 'gif,jpg,jpeg,bmp,png',

​        mineTypes: 'image/*'

​    },

 

 

### 4.6 设置服务器的url

​    server: "{ {url('/admin/upload')} }"

即上传文件的处理页面地址

 

在Lravel中我们需要上传文件时使用post上传就必须附带 csrf的token令牌。

​		 formData: {

​		 	  '_token': '{ { csrf_token() } }',

​		 }


## 5. 上传图片时设置预览图片效果

var preview = $(‘#webuploader-img’);

uploader.on('fileQueued', function(file) {

​    uploader.makeThumb(file, function(error, src) {

​        $preview.empty();

​        if (error) {

​            layer.msg("不能预览图片。");

​            return;

​        }

​        $preview.html("<img src='" + src +"' />");

​    }, 100, 100);

});

​                <!--定义一个标签用来存放图片的预览图-->

​                <div id="webuploader-img" class="uploader-list"></div>

​                <!--选择图片的按钮-->

​                <div id="picker">选择图片</div>

fileQueued，事件，当有文件被选择上传时，则触发当前事件。

makeThumb 方法，设置生成缩略图。

 

$preview 为显示图片的html标签

 

## 6. 监听文件上传进度，显示到页面上

uploader.on('uploadProgress', function(file, percentage) {

​    $('#processing .sr-only').css('width', percentage * 100 + '%')

});

​                <div id="processing">

​                    <div class="progress" style="width: 400px">

​                        <span class="progress-bar">

​                            <span class="sr-only" style="width: 0%"></span>

​                        </span>

​                    </div>

​                </div>

 

 

# 二、zyload使用

<!--引入zyupload插件的源代码-->

<script type="text/javascript" src="./zyupload/core/zyFile.js"></script>

<script type="text/javascript" src="./zyupload/control/js/zyUpload.js"></script>

<link rel="stylesheet" type="text/css" href="./zyupload/control/css/zyUpload.css">

 

// 初始化zyupload

$('#carousel_imgs').zyUpload({

​    width: "550px",

​    url: "http://localhost:8000/admin/upload",

​    onSuccess: function(file, response) {// 服务器返回的数据是原始数据

​        // 把返回的json字符串转换成js中的对象

​        var rs = eval('(' + response + ')');

​        // console.log(rs);

​        if (rs.status === true) {

​            // 上传成功

​            $('#carousel_imgs-container').append('<input name="carousel_imgs[]" type="hidden" value="' + rs.path + '" />');

​            layer.msg(rs.message);

​            $('#uploadList_' + file.index).data('path', rs.path);// 在图片上传成功后，把图片的地址保存在预览框的div上

​        }

​    },

​    loadPicRemove: function() {

​        var path = $(this).parents('.upload_append_list').data('path');

​        $('input[value="' + path + '"]').remove();

​        $(this).parents('.upload_append_list').remove();

​    }

});

$(document).on('zyFile.formdata.created', function(event, formdata, zyFile) {

​    zyFile.uploadFileName = 'file';

​    formdata.append('_token', 'vOSEWFgq07qP0BjMlGX8pzna2RiUhw70AwMYUs1l');
});