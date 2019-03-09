---
title: datatable使用
date: 2019-03-06 19:49:58
tags: laravel
---
# 一、datatable使用

官网：<https://datatables.net/>

中文官网：<http://datatables.club/>

## 1. 效果
<!-- more -->
使用datatable可以做数据列表显示，其显示效果如下：

![img](wpsFB5D.tmp.jpg)

上图的 显示条数、检索数据、字段排序、分页 等功能都可以通过datatable实现出来

#### 1.1.1 引入插件的js文件![img](wpsFB5E.tmp.jpg)

 

## 2. 初始化

$('table标签选择符').DataTable({

   配置项: 值,

   配置项: 值,

});

 

 

## 3. 配置项(Options)

"lengthMenu": [ 20, 30, 40, 50, 100 ]

代表你可以把表格设置为每页 20/30/40/50/100 条数据显示

"lengthMenu": [ 10, 25, 50, -1 ]

<http://datatables.club/manual/daily/2016/05/06/option-lengthMenu.html>

第一个数组是具体的值，理解为 <option value=""></option> value 对应的值

第二个数组是用来显示的文字，理解为 <option value="">显示的文字</option>

这里需要提的是，数字要 >0 ，但是有个特殊值 -1 它代表显示全部数据

![img](wpsFB5F.tmp.jpg) 

 

 

"paging": true,

数据做分页显示设置，一般设置为true即可

![img](wpsFB60.tmp.jpg) 

 

 

"info":     [true]/false,	

//分页辅助信息，第几条到第几条

![img](wpsFB61.tmp.jpg) 

 

 

 

"searching": true

<http://datatables.club/reference/option/searching.html>

此选项用来开启、关闭Datatables的搜索功能

![img](wpsFB62.tmp.jpg) 

 

 

"ordering": [true]/false

http://datatables.club/reference/option/ordering.html

允许或禁止对各个数据列使用排序

![img](wpsFB63.tmp.jpg) 

注意：如果开启此选项，那么数据库操作的时候就不雅使用order by 条件了

 

 

 

"order": [[ 1, "asc/desc" ]],

设置默认第2列正排序

![img](wpsFB73.tmp.jpg) 

 

 

"stateSave": true,

<http://datatables.club/reference/option/stateSave.html>

开启或者禁用状态储存。当你开启了状态储存，Datatables会存储一个状态到浏览器上， 包含分页位置，每页显示的长度，过滤后的结果和排序。当用户重新刷新页面，表格的状态将会被设置为之前的设置

 

<http://datatables.club/reference/option/columnDefs.html>

"columnDefs": [{

   "targets": [0,8],

   "orderable": false

}]

指定列不参与order排序

![img](wpsFB74.tmp.jpg) 

 

 

"processing": true

<http://datatables.club/reference/option/processing.html>

当表格处在处理过程（例如排序）中时，启用或者禁止 'processing'指示器的显示。当处理大数据时，处理过程耗费的时间很明显，这个功能就显得非常有用。

![img](wpsFB75.tmp.jpg) 

 

## 4. Datatables配合Ajax的使用

<http://datatables.club/reference/option/ajax.data.html>

'serverSide': false,

"ajax": {

​    "url": "data.json",

​    "type": "POST",

​    'headers': { 'X-CSRF-TOKEN' : '{ { csrf_token() } }' },

}

<http://datatables.club/reference/option/serverSide.html>

设置服务器端处理，需要与ajax合作完成

 

"columns": [

​    {'data':'字段名称',"defaultContent": "默认值",'className':'类名'},

​    ......

{'data':'字段名称',"defaultContent": "默认值",'className':'类名'},

],

总的数量与表格的列数必须一致，不能多也不能少

<https://datatables.net/reference/option/>

服务器端返回数据后的填充属性信息设置

data:字段名称，data:’username’,data:’mg_sex’就是服务器返回数据对应的字段名称，

defaultContent：‘<input type=checkbox>’，

className:’td-manager’

columns要对td表格的内容进行填充，同时可以给td设置class属性

注意：如果data接收类似a或b的信息，实际服务器没有返回该信息，那么一定要同时设置defaultContent属性，否则报错

 

 

"createdRow":function(row,data,dataIndex){}

创建tr/td时的回调函数，可以继续修改、优化tr/td的显示，里边有遍历效果，会依次扫描生成的每个tr

row:创建好的tr的dom对象

data:数据源，代表服务器端返回的每条记录的实体信息

dataIndex:数据源的索引号码

 

Ajax+Processing服务器端处理recordsTotal/recordsFiltered：

dataTables与服务器通信协议参考地址：

<http://datatables.club/manual/server-side.html>

要求服务器端返回数据格式：

**$shuju** = $subject->get();    // 数据

$cnt   = $subject->count();  // 数据的总数量

$info = [

   'draw'=>$request->get('draw'),

   'recordsTotal'=>$cnt,

   'recordsFiltered'=>$cnt,

   'data'=>**$shuju**,

];

return $info;

draw: 客户端调用服务器端次数标识

recordsTotal: 获取数据记录总条数

recordsFiltered: 数据过滤后的总数量

注意：

​    recordsTotal和recordsFiltered都设置为记录的总条数，避免后期效果不佳

data: 获得的具体数据，使用模型获取

 