---
title: MySQLI扩展
date: 2019-02-20 17:00:28
tags: MySQL
---
# 一、昨日回顾

## 1. 知识回顾

1. 表单传值的方式？

   总共两种，1) POST方式； 2）GET方式

2. PHP接收表单数据的方式？

   总共三种，
<!-- more -->
   $\_POST：只接收POST方式传递的数据；

   $\_GET：只接收GET方式传递的数据；

   ​		get方式传递数据的核心特征：以"?"专门用于分隔链接地址和GET数据的；以"&"专门用于分隔GET的数据数据的。

   $\_REQUEST：即包含$\_POST又包含$\_GET的数据；

3. 在程序中是通过哪个变量来接受文件上传的数据的呢？

   通过$_FILES来接收传递过来的文件数据的；

4. $\_FILES的结构？

   包含五个部分的内容：

   1) name      表示原文件的文件名

   2）type       表示文件的格式类型

   3）tmp_name      表示上传文件存储在临时目录中的全路径地址

   4）error      表示出现的错误编号

   5）size       表示上传的大小，单位字节






# 二、知识路径

- MYSQLI的概念

  ​	为什么使用MYSQLI扩展

  ​	什么是MYSQLI扩展

- MYSQLI扩展的使用

  ​	准备工作

  ​	回顾小黑窗口中对MYSQL的基本操作

  ​	使用MYSQLI实现连库基本操作

  ​	 ==**目标**：能够使用MYSQLI实现连接数据库相关操作==

  ​	使用MYSQLI实现关闭数据库连接操作

  ​	 ==**目标**：能够使用MYSQLI实现关闭数据库连接操作==

  ​	使用MYSQLI实现增删改操作

  ​	 ==**目标**：能够使用MYSQLI实现向数据表增删改数据操作==

  ​	使用MYSQLI实现查询操作

  ​	 ==**目标**：能够使用MYSQLI实现查询数据表一条数据和多条数据操作==

  ​	MYSQLI扩展中辅助操作函数

   ==**目标**：能够实现后台新闻管理系统==

- 案例：后台新闻管理系统

  ​	功能分析

  ​	建表

  ​	实现列表页

  ​	实现添加页面和功能

  ​	实现修改页面和功能

  ​	实现删除功能

  ​	实现分页效果



# 三、今日课程内容：MYSQLI扩展

## 1. MYSQLI的概念

### 为什么使用MYSQLI扩展

以前操作MYSQL使用的是黑窗口客户端：

```sequence
note left of 黑窗口客户端: 通过黑窗口进行操作
黑窗口客户端-->MYSQL数据库（服务器）:建立连接和认证
MYSQL数据库（服务器）-->黑窗口客户端:返回连接信息
note left of 黑窗口客户端: 通过黑窗口进行操作
黑窗口客户端-->MYSQL数据库（服务器）:执行各种MYSQL操作指令
MYSQL数据库（服务器）-->黑窗口客户端:返回执行的结果
```

现在操作MYSQL需要使用PHP代替黑窗口客户端：

```sequence
note left of PHP: 通过MYSQLI扩展进行操作
PHP-->MYSQL数据库（服务器）:建立连接和认证
MYSQL数据库（服务器）-->PHP:返回连接信息
note left of PHP: 通过MYSQLI扩展进行操作
PHP-->MYSQL数据库（服务器）:执行各种MYSQL操作指令
MYSQL数据库（服务器）-->PHP:返回执行的结果
```



### 什么是MYSQLI扩展

概念：MYSQLI扩展即PHP利用MYSQL提供的语言操作接口，封装出来的一系列操作MYSQL数据库的==函数==和操作类。 



## 2. MYSQLI扩展的使用

MYSQLI是PHP中的一个==扩展==，扩展的意思即不是默认就自带支持的，而是需要通过额外引入才能使用的。



所以我们在使用之前，需要先将MYSQLI扩展引入进PHP。

### 引入MYSQLI扩展

==步骤==：

第一步，打开php的配置文件php.ini，配置extension_dir配置项，

![1539567050765](2)

第二步，再在php.ini中开启MYSQLI相关的扩展配置项，

![1539567132559](3)

第三步，去到extension_dir指定的目录中确认php_mysqli.dll文件是否存在，

![1539567200099](4)

第四步，重启apache，

![1539567243872](5)

第五步，测试MYSQLI扩展是否开启成功，通过程序文件中的phpinfo查看配置信息，

![1539567340821](9)



### 回顾黑窗口对MYSQL数据表数据的基本操作

1. 连接数据库；
2. 设置字符集；
3. 选择数据库；
4. 对数据表中的数据做增、删、改、查操作；



### 使用MYSQLI实现连库基本操作

使用MYSQLI实现连库基本操作  相当于  打开黑窗口连接数据库，选择默认的数据库和设置字符集操作。



> 涉及的函数：
>
> **mysqli_connect**(数据库ip地址,  帐号,  密码,  默认选择的数据库)  
>
> **mysqli_set_charset**(mysqli连接,  字符集编码)
>
> **mysqli_select_db**(mysqli连接, 数据库名) 



**==操作需求1==**：使用MYSQLI实现连接数据库，选择默认的数据库为test，设置字符集为utf8操作。

**==解答==**：构建code1.php程序页面，代码如下

![1539568111548](10)



**==操作需求2==**：在"操作需求1"的基础上，实现切换选择数据库为db3数据库操作。

**==解答==**：构建code2.php程序页面，代码如下

![1539568449193](11)

**==小结==**：

1. mysqli_connect能够实现的两个操作：1）连接数据库     2）选择默认的数据库
2. mysqli_set_charset设置什么样的字符集取决于当前所写在的程序文件使用什么样的字符集编码，比如当前程序文件code2.php使用的是utf8编码，所以设置的字符集也应该是utf8编码。



### 使用MYSQLI实现关闭数据库连接操作

使用MYSQLI实现关闭数据库操作  相当于  在黑窗口退出数据库操作。



> 涉及的函数：
>
> **mysqli_close**(mysqli连接) 



**==操作需求==**：使用MYSQLI实现关闭数据库操作。

**==解答==**：构建code3.php程序页面，代码如下

![1539569722521](12)



**==小结==**：我们即使不直接使用mysqli_close关闭mysql连接，当程序运行结束时，$link也会被自动销毁，也就是说mysql也会自动关闭。



### 使用MYSQLI实现设置(增删改)操作

使用MYSQLI实现设置操作  相当于  在黑窗口实现对数据表数据执行增、删、改SQL语句操作。



> 涉及的函数：
>
> **mysqli_query**(mysqli连接,  sql语句)



**==操作需求1==**：使用MYSQLI操作db3数据库中的stu5表，实现往数据表stu3中新增一条数据的操作。

**==解答==**：构建code4.php程序页面，代码如下

![1539570394479](13)



**==操作需求2==**：使用MYSQLI操作db3数据库中的stu5表，实现修改数据。

**==解答==**：构建code5.php程序页面，代码如下

![1539570535818](14)

执行成功依然是返回true,执行失败是返回false.



**==操作需求3==**：使用MYSQLI操作db3数据库中的stu5表，删除name为"钟离昧"的数据。

**==解答==**：构建code6.php程序页面，代码如下

![1539570663436](15)



**==小结==**：增删改操作都是通过mysqli_query函数来实现的，执行成功返回true,执行失败返回false;



### 使用MYSQLI实现查询操作

使用MYSQLI实现查询操作  相当于  在黑窗口查询SQL语句操作。



> 涉及的函数：
>
> **mysqli_query**(mysqli连接,  sql语句)
>
> **mysqli_fetch_assoc**(结果集)
>
> **mysqli_fetch_row**(结果集)
>
> **mysqli_fetch_all**(结果集[, 数组形态])       数组形态可指定MYSQLI_ASSOC、MYSQLI_NUM（默认值）或MYSQLI_BOTH



**==操作需求1==**：使用MYSQLI操作db3数据库中的stu5表，查询age小于14的所有数据，要求得到的每条数据结果是一个关联数组。

**==解答==**：构建code7.php程序页面，代码如下

![1539571530517](16)

执行查询有三步操作：1）构建查询数据的SQL语句，2）执行查询数据的SQL语句， 3）解析对象结果集（当前使用了mysqli_fetch_assoc）；



**==操作需求1小结==**：

mysqli_fetch_assoc的特点：

1. 执行一次只能得到一条数据；
2. 每次得到的数据是一个关联类型的数组，其元素的下标为字段名；
3. 当执行获得最后一条数据之后，再次去执行，则只能获得NULL值；







**==操作需求2==**：使用MYSQLI操作db3数据库中的stu5表，查询age小于14的所有数据，要求得到的每条数据结果是一个索引数组。

**==解答==**：构建code8.php程序页面，代码如下

![1539571786731](17)



**==操作需求2小结==**：mysqli_fetch_row函数和mysqli_fetch_assoc一模一样，只有一点区别：mysqli_fetch_row函数每次执行获得的数据是一个索引类型的数组。





**==操作需求3==**：

1. 使用MYSQLI操作db3数据库中的stu5表，查询age小于14的所有数据
2. 要求一次性得到所有数据的结果；
3. 数据结果分别要求输出关联数组数据一份，索引数组数据一份，同时包含关联和索引数组元素的数组一份；

**==解答==**：构建code9.php程序页面，代码如下

一次性获得所有数据操作：

![1539572104210](18)

注意：以上使用mysqli_fetch_all函数默认得到的是一个索引类型的二维数组；

如果我们想获得关联类型数组，则我们需要指定第二个参数MYSQLI_ASSOC：

![1539572459836](19)

指定MYSQLI_BOTH参数：

![1539572562016](20)



**==操作需求3小结==**：

1. 我们可以通过mysqli_fetch_all函数来一次性得到所有查询的数据；默认情况下获得的是一个索引类型的二维数组数据，如果指定参数为MYSQLI_ASSOC则获得的将会是关联类型的二维数组数据；



### MYSQLI扩展中辅助操作函数



> 涉及的函数：
>
> **mysqli_field_count**(mysqli连接)	返回==最近一次==查询语句查询数据中的总列数
>
> **mysqli_num_fields**(结果集)	获得查询的结果集中字段的个数
>
> **mysqli_num_rows**(结果集)	获得查询结果集中记录的总行数
>
> **mysqli_errno**(mysqli连接)		获得错误的错误码值
>
> **mysqli_error**(mysqli连接)		获得错误的错误码值对应的错误信息
>
> **mysqli_insert_id**(mysqli连接) 		获得==最近一次==新增数据的主键id值



**==操作需求1==**：

1. 使用MYSQLI操作db3数据库中的stu5表，执行两次查询操作；
2. 第一次查询所有数据，但是限制最终只获得2条数据；
3. 第二次查询所有数据，但是限制最终只获得3条数据；
4. 打印出最近一次查询语句查询数据中的总列数；
5. 打印第一次查询结果中的总列数；
6. 打印第一次查询结果集中数据的总行数；

**==解答==**：构建code10.php程序页面，代码如下

![1539574424704](21)



**==操作需求1小结==**：mysqli_field_count是获得最近一次的查询结果的总列数，局限性比较大；



**==操作需求2==**：

1. 使用MYSQLI执行连库操作，要求要连库成功；同时执行设置字符集操作，要求设置的字符集为"utf8888"，使得设置字符集操作失败；
2. 打印出MYSQLI操作失败的错误信息；
3. 打印出MYSQLI操作失败错误信息对应的错误码值；

**==解答==**：构建code11.php程序页面，代码如下

![1539574763457](22)



**==操作需求2小结==**：

1. 我们在程序中经常会使用以上两个函数获得具体出错的信息原因以便排查错误。



**==操作需求3==**：

1. 使用MYSQLI操作db3数据库中的goods_info表，向数据表中新增一条数据；
2. 打印出最近一次新增数据时的主键id；

**==解答==**：构建code12.php程序页面，代码如下

![1539575208417](23)



**==操作需求3小结==**：mysqli_insert_id可以获得最近一次新增数据的主键id；



## 3. 综合项目：后台新闻管理系统

### 预览效果

> ==列表页==：
>
> ![1529708158948](6)
>
> ==添加新闻页==：
>
> ![1529708158948](7.png)
>
> ==编辑新闻页==：
>
> ![1529708158948](8)



### 功能分析

1. 新闻列表页；
2. 添加页和添加功能；
3. 编辑页和编辑功能；
4. 删除功能；
5. 分页功能；



### 建表

创建一个新闻表，表字段的要求如下：

```mysql
##建表字段要求：
新闻表
news
id,标题,简介,内容,添加时间
id,title,intro,content,post_date

##建表语句：
create table news(
id int unsigned primary key auto_increment,
title varchar(30) not null default '' comment '新闻标题',
intro varchar(255) not null default '' comment '新闻简介',
content text comment '新闻内容',
post_date int unsigned not null default 0 comment '添加时间'
);
```



### 实现添加页面和功能

**==步骤==**：

第一步，创建名为newsad.php的程序文件，代码如下：

```html
<!DOCTYPE html>
<HTML>
<head>
    <meta charset="UTF-8">
    <title>添加新闻</title>
</head>
<body>
    <form method="post" action="http://www.home.com/class/day12/code/newsadh.php" >
        <p>
            <span>新闻标题：</span>
            <input type="text" name="title" />
        </p>
        <p>
            <span>新闻简介：</span>
            <input type="text" name="intro" style="width:300px;" />
        </p>
        <p>
            <span>新闻内容：</span>
            <textarea name="content" cols=50 rows=10></textarea>
        </p>
        <p>
            <input type="submit" value="立即添加" />
        </p>
    </form>

    <p>
        <a href="http://www.home.com/class/day12/code/newslist.php">返回新闻列表页</a>
    </p>
</body>
</HTML>
```

第二步，构建新闻添加处理页面newsadh.php，代码如下：

```php
<?php

#接收表单提交的数据
$title = $_POST['title'];
$intro = $_POST['intro'];
$content = $_POST['content'];
$post_date = time();

#连库基本操作
//连接数据库和选择默认的数据库
$link = mysqli_connect('localhost', 'root', '123abc', 'db7');

//设置字符集
mysqli_set_charset($link, 'utf8');

#执行新增操作
//构建SQL语句
$sql = "insert into news values (null, '{$title}', '{$intro}', '{$content}', {$post_date})";

//执行SQL语句
$re = mysqli_query($link, $sql);

//根据执行的结果进行判断
if( $re ){//执行成功
    echo '恭喜您添加新闻成功哟！～';
}else{//执行失败
    echo '执行失败，请联系管理员！'; 
}

$url = 'http://www.home.com/class/day12/code/newslist.php';
header("Refresh:3; url={$url}");
```

第三步，测试使用效果，

在添加页构建测试数据，

![1539587555435](24)

点击添加新闻按钮，效果为：

![1539587568167](25)

3秒后，跳转到列表页。

![1539587579146](26)

### 实现列表页

**==步骤==**：

第一步，创建名为newslist.php程序页面，代码如下：

```html
<?php 

#连库基本操作
//连接数据库和选择默认的数据库
$link = mysqli_connect('localhost', 'root', '123abc', 'db7');

//设置字符集
mysqli_set_charset($link, 'utf8');

#查询列表页需要的数据
//构建SQL语句
$sql = "select id, title, intro, post_date from news where 1";

//执行SQL语句
$result = mysqli_query($link, $sql);

//一次性解析得到所有查询的数据
$rows = mysqli_fetch_all($result, MYSQLI_ASSOC);

?>
<!DOCTYPE html>
<HTML>
<head>
    <meta charset="UTF-8">
    <title>新闻列表</title>
</head>
<body>

<p>
    <a href="http://www.home.com/class/day12/code/newsad.php">添加新闻</a>
</p>


<table border=1>
    <thead>
        <tr>
            <td>序号</td>
            <td>ID</td>
            <td>新闻标题</td>
            <td>新闻简介</td>
            <td>添加时间</td>
            <td>操作</td>
        </tr>
    </thead>
    <tbody>
    <?php foreach($rows as $rows_key=>$row){ ?>
        <tr>
            <td><?php echo $rows_key+1; ?></td>
            <td><?php echo $row['id']; ?></td>
            <td><?php echo $row['title']; ?></td>
            <td><?php echo $row['intro']; ?></td>
            <td><?php echo date('Y-m-d H:i:s', $row['post_date']); ?></td>
            <td>
                <a href="">编辑新闻</a>
                <a href="">删除新闻</a>
            </td>
        </tr>
        <?php } ?>
    </tbody>
</table>

<p>
    <a href="">上一页</a>
    <a href="">1</a>
    <a href="">2</a>
    <a href="">3</a>
    <a href="" style="color:red;">4</a>
    <a href="">5</a>
    <a href="">下一页</a>
</p>

</body>
</HTML>
```

第二步，访问newslist.php页面，效果如下：

![1539588982262](27)



### 实现修改页面和功能

**==步骤==**：

第一步，修改列表页中  编辑新闻  按钮的链接地址，

![1539591603936](28)

构建名为newsupd.php页面，代码如下：

```php
<?php 

#接收GET方式传递的新闻id值
$id = $_GET['id'];

#连库基本操作
//连接数据库和选择默认的数据库
$link = mysqli_connect('localhost', 'root', '123abc', 'db7');

//设置字符集
mysqli_set_charset($link, 'utf8');

#查询需要回显的本条数据
//构建SQL语句
$sql = "select id, title, intro, content from news where id={$id}";

//执行SQL语句
$result = mysqli_query($link, $sql);

//解析对象结果集
$row = mysqli_fetch_assoc($result);
?>
<!DOCTYPE html>
<HTML>
<head>
    <meta charset="UTF-8">
    <title>编辑新闻</title>
</head>
<body>
    <form method="post" action="http://www.home.com/class/day12/code/newsupd_h.php?id=<?php echo $row['id']; ?>" >
        <p>
            <span>新闻标题：</span>
            <input type="text" name="title" value="<?php echo $row['title']; ?>" />
        </p>
        <p>
            <span>新闻简介：</span>
            <input type="text" name="intro" style="width:300px;" value="<?php echo $row['intro']; ?>" />
        </p>
        <p>
            <span>新闻内容：</span>
            <textarea name="content" cols=50 rows=10><?php echo $row['content']; ?></textarea>
        </p>
        <p>
            <input type="submit" value="立即修改" />
        </p>
    </form>

    <p>
        <a href="http://www.home.com/class/day12/code/newslist.php">返回新闻列表页</a>
    </p>
</body>
</HTML>
```

第二步，构建名为newsupd_h.php的文件，代码如下：

```php
<?php

#接收表单提交的数据
$id = $_GET['id'];//文章的id

$title = $_POST['title'];//文章的标题
$intro = $_POST['intro'];//文章的简介
$content = $_POST['content'];//文章的内容

#连库基本操作
//连接数据库和选择默认的数据库
$link = mysqli_connect('localhost', 'root', '123abc', 'db7');

//设置字符集
mysqli_set_charset($link, 'utf8');

#执行更新数据操作
//构建SQL语句
$sql = "update news set title='{$title}', intro='{$intro}', content='{$content}' where id={$id}";

//执行SQL语句
$re = mysqli_query($link, $sql);

//根据执行的结果进行判断
if( $re ){//执行成功
    echo '你好棒棒哟～';
}else{//执行失败
    echo '不要气馁哟～请联系管理员MM';
}

$url = "http://www.home.com/class/day12/code/newsupd.php?id={$id}";
header("Refresh:3; url={$url}");
exit;
```

第三步，测试使用效果：

进入更新页面，构建更新数据：

![1539591702557](29)

点击编辑按钮：

![1539591716268](30)

3秒后，跳回更新页面，

![1539591736887](31)



### 实现删除功能

**==步骤==**：

第一步，调整列表页删除按钮的链接地址，

![1539592679019](32)

第二步，构建newsdel.php页面，代码如下：

```php
<?php 

#接收GET方式传递的新闻id值
$id = $_GET['id'];

#连库基本操作
//连接数据库和选择默认的数据库
$link = mysqli_connect('localhost', 'root', '123abc', 'db7');

//设置字符集
mysqli_set_charset($link, 'utf8');

#执行删除操作
//构建SQL语句
$sql = "delete from news where id={$id}";

//执行SQL语句
$re = mysqli_query($link, $sql);

//判断执行的结果
if( $re ){//执行成功

    echo '客官！你好狠心哟！～'; 
}else{//执行失败
    echo '就知道你是这样的人，哈哈，删不掉我吧！'; 
}

//3秒后跳回到列表页
$url = 'http://www.home.com/class/day12/code/newslist.php';
header("Refresh: 3; url={$url}");
exit;

```

第三步，测试使用效果，

点击删除按钮，

![1539593000236](33)

效果为：

![1539593013145](34)

3秒后跳回到列表页：

![1539593023440](35)



### 实现分页功能

**==步骤==**：

第一步，在newslist.php中增加分页相关参数，

![1539595430753](36)

第二步，在newslist.php中使用上分页相关参数，

![1539595479720](37)

第三步，调整newslist.php中的显示分页的html代码，

![1539595523811](38)

第四步，查看效果，

![1539595553640](39)



## 4. 全天总结

1. 能够使用php连接mysql、设置字符集、选择数据库

   连接数据库和选择数据库：mysqli_connect函数

   设置字符集：mysqli_set_charset函数

2. 能够使用mysqli_query()函数执行增删改操作

   1）构建增、删、改操作的SQL语句；2）使用mysqli_query执行SQL语句

3. 能够使用mysqli_query()函数执行查询操作

   1）构建查询数据的SQL语句； 2）使用mysqli_query函数执行SQL语句； 3）解析执行之后得到的对象结果集；  

   mysqli_fetch_assoc函数，每次解析一条数据，得到的是关联类型的一维数组；

   mysqli_fetch_all函数，一次执行得到全部的数据，默认是一个索引类型的二维数组，如果指定第二个参数为MYSQLI_ASSOC则将会得到一个关联类型的二维数组。

4. 能够对结果集进行遍历读取并显示到网页上

   ```
   <?php foreach($rows as $rows_key=>$row){ ?>
   	<tr>
   		<td>....</td>
   		<td>....</td>
   	</tr>
   <?php } ?>
   ```

   

5. 能够使用php实现新闻的添加模块

6. 能够使用php实现新闻的查看模块

7. 能够使用php实现新闻的修改模块

8. 能够使用php实现新闻的删除模块



### 课后练习

实现综合项目：后台新闻管理系统



