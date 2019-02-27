---
title: GD图像处理技术
date: 2019-02-20 17:00:40
tags: PHP
---
# 一、昨日回顾

## 1. 知识回顾

1. 能够设置及读取COOKIE的值

   设置(增删改)操作：setcookie函数

   查询操作：$_COOKIE变量

2. 理解COOKIE的工作原理
<!-- more -->
   将数据持久化存储在浏览器端，同时让数据能够区分不同服务器的一种技术。

3. 能够理解session的工作原理

   将数据持久化存储在服务器端，同时让数据能够区分不同浏览器的一种技术。

4. 能够设置和读取session数据

   1）首先需要开启SESSION机制：session_start()

   2）然后像操作普通数组一样操作$_SESSION变量进行增、删、改、查数据即可。

5. 能够理解销毁session的含义

   通过session_destroy函数来进行销毁session操作，通过这个函数销毁的是SESSION数据区。

6. 能够理解session的垃圾回收机制

   当一定的时间内PHP发现还没有任何程序去访问该SESSION数据区，则PHP将会认为这个数据区可能是一个垃圾数据区；然后当访问程序的时候，将会以一定的概率去清除这个垃圾数据区。



## 2. 昨日反馈

![1540044693798](1.png)

# 二、知识路径

- GD图像处理技术相关概念

  ​	为什么使用GD图像处理技术

  ​	什么是GD图像处理技术

- 开启GD扩展

- GD扩展相关操作

  ​	操作分析

  ​	基本操作

  ​	创建画布相关操作

  ​	关闭画布操作

  ​	图像输出相关操作

  ​	画布内容相关操作

  ​	辅助相关操作

  ==目标：制作水印图，制作等比缩略图，制作验证码==

- 案例：制作水印图

  ​	功能分析

  ​	代码实现

- 案例：制作缩略图

  ​	固定宽高缩略图

  ​	等比缩略图

- 案例：制作验证码


  ​	功能分析

  	代码实现




# 三、今日课程内容：GD图像处理技术

## 1. GD图像处理技术相关概念

### 为什么使用GD图像处理技术

在WEB项目中，GD图像处理技术应用非常广泛，比如制作验证码图片，给图片打水印等。



### 什么是GD图像处理技术

GD图像处理技术 即 PHP通过使用==GD扩展==来操作图像的一种技术。



## 2. 开启GD扩展

扩展，意味着GD并不是PHP默认就是配置开启的，而是需要额外进行配置的。



**==开启步骤==**：

第一步，打开php.ini确认extension_dir配置项，

![1540085105102](2)

第二步，在php.ini中开启GD扩展的extension配置项，

![1540085178925](3)

第三步，确认php_gd2.dll文件在extendsion_dir配置项所指定的目录中是否存在，

![1540085238688](4)
第四步，重启apache，检查gd扩展是否开启成功，

![1540085299941](5)

在day15/code目录中创建code1.php检查gd扩展是否开启成功，

![1540085365402](6)



## 3. GD扩展相关操作

#### 操作分析

在程序中使用GD扩展操作图像，其实就相当于我们在电脑中使用各种绘图软件来绘制图像，比如PS软件。



我们以PS软件绘制图像的方式为切入点，来分析绘图过程中所需要的一些操作有哪些：

1. 安装并打开PS软件；
2. 创建一个画布；
3. 选个工具，并且选择颜色在画布上画点，线等图案；
4. 保存图片；
5. 关闭画布；



#### 创建画布相关操作

涉及的函数：

> **imagecreate**(宽,  高)   创建画布
>
> **imagecreatetruecolor**(宽,  高)    创建一个真彩色画布
>
> **imagecreatefromjpeg**(jpeg图片路径)    根据一张已有的jpeg图片创建画布
>
> **imagecreatefromgif**(gif图片路径)    根据一张已有的gif图片创建画布
>
> **imagecreatefrompng**(png图片路径)    根据一张已有的png图片创建画布



**==需求==**：根据以下要求完成操作，

1. 根据imagecreate和imagecreatetruecolor两个函数分别创建一个400*200的画布，打印返回值；
2. 根据一张已有的jpeg图片创建画布，打印返回值；

**==解答==**：创建名为code2.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布
var_dump( $resource ); echo '<hr/>';

#imagecreatetruecolor函数
$resource1 = imagecreatetruecolor(400, 200);
var_dump( $resource1 ); echo '<hr/>';

#imagecreatefromjpeg函数
$resource2 = imagecreatefromjpeg('./source/28.jpg');
var_dump( $resource2 ); 
```

访问code2.php，输出的内容如下：

```mysql
resource(2) of type (gd)    #imagecreate函数结果的输出
resource(3) of type (gd)    #imagecreatetruecolor函数结果的输出
resource(5) of type (gd)    #imagecreatefromjpeg函数结果的输出
```



**==小结==**：创建画布函数执行操作完成之后，其返回值都是资源类型的数据。



#### 关闭画布操作

涉及的函数：

> **imagedestroy**(画布资源)    销毁画布资源



**==需求==**：根据以下要求完成操作，

1. 根据imagecreate创建一个400*200的画布，打印返回值；
2. 使用imagedestroy关闭打开的画布，再次打印"1"中创建的画布资源返回值；

**==解答==**：创建名为code3.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布
var_dump( $resource ); echo '<hr/>';

#imagedestroy函数
imagedestroy($resource);
var_dump( $resource ); 
```

访问code3.php，输出的内容如下：

```mysql
resource(2) of type (gd)     #销毁之前的值
resource(2) of type (Unknown)     #销毁之后的值
```



#### 图像输出相关操作

涉及的函数：

> imagejpeg()    以jpeg的格式输出图片到浏览器或保存成文件
>
> imagepng()    以png的格式输出图片到浏览器或保存成文件
>
> imagegif()    以gif的格式输出图片到浏览器或保存成文件



**==需求==**：创建code4.php程序文件，

1. 使用imagecreatefromjpeg创建一个画布；
2. 同时实现根据GET方式传递的type值判定，如果type值为1，则直接将"1"中创建的画布以jpeg图片的格式输出到浏览器；如果type值为2，则将"1"中创建的画布以jpeg格式保存成图片文件；

**==解答==**：创建名为code4.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreatefromjpeg('./source/m3.jpg');//创建400*200 px的画布

$type = isset($_GET['type']) ? $_GET['type'] : 1;

if( $type==1 ){//将图像直接输出到浏览器中

    header('Content-type:image/jpeg');//表示 在接下来的响应中告诉浏览器，要以image/jpeg的类型来对待返回给浏览器的响应数据
    imagejpeg($resource);//以jpeg格式的图片直接输出到浏览器

}elseif( $type==2 ){//将图像保存成jpeg格式的图片
    
    echo '保存成文件!';
    imagejpeg($resource, './aa.jpg');//将图像保存成名为aa.jpg的文件
}
```



==**注意：**==在输出图像之前，也就是上面这个案例中header('Content-type:....')之前不能有任何的字符输出，否则图像将会损坏无法正常输出。



#### 画布内容相关操作

涉及的函数：

> **imagecolorallocate**(画布资源,  R色值,  G色值,  B色值)    分配一个颜色
>
> **imagefill**(画布资源,  基点x坐标,  基点y坐标,  颜色)    向画布填充颜色
>
> **imageline**(画布资源,  起点x坐标,  起点y坐标,  终点x坐标,  终点y坐标,  颜色)    画线段操作
>
> **imagerectangle**(画布资源,  对角线起点x坐标,  对角线起点y坐标,  对角线终点x坐标,  对角线终点y坐标,  颜色)    画矩形
>
> **imagearc**(画布资源,  圆心x坐标,  圆心y坐标,  椭圆宽,  椭圆高,  起点角度,  终点角度,  颜色)    画圆弧线段
>
> **imagestring**(画布资源,  字体大小,  文字左上角x坐标,  文字左上角y坐标,  内容,  颜色)    根据系统字体写字
>
> **imagettftext**(画布资源,  字体大小,  角度,  文字左下角x坐标,  文字左下角y坐标,  颜色,  字体路径,  内容)    根据ttf格式的字体写字



**==需求1==**：创建code5.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答1==**：创建名为code5.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code5.php，效果为：

![1540090625787](7)

**==需求1小结==**：填充背景色的原理是以给定的坐标为基点，向四周扩散填充。

![1540090951271](8)



**==需求2==**：创建code6.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 在画布上画一条线段；
4. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答2==**：创建名为code6.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#画线段
$color = imagecolorallocate($resource, 233, 16, 78);
imageline($resource, 100, 150, 230, 75, $color);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code6.php，效果为：

![1540092088059](10)

**==需求2小结==**：

![1540092029131](9)



**==需求3==**：创建code7.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 在画布上画一个矩形；
4. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答3==**：创建名为code7.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#画矩形
$color = imagecolorallocate($resource, 233, 16, 78);
imagerectangle($resource, 100, 150, 230, 75, $color);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code7.php，效果为：

![1540092378936](12)



**==需求3小结==**：

![1540092331332](11)



**==需求4==**：创建code8.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 在画布上画一个椭圆；
4. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答4==**：创建名为code8.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#画椭圆
$color = imagecolorallocate($resource, 233, 16, 78);
imagearc($resource, 200, 100, 200, 110, 0, 360, $color);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code8.php，效果为：

![1540093582772](15)



**==需求4小结==**：

![1540092485475](13)

![1540093412142](14)



**==需求5==**：创建code9.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 在画布上使用imagestring写一句话："今天天气好晴朗"；
4. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答5==**：创建名为code9.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#根据系统字体写字
$color = imagecolorallocate($resource, 233, 16, 78);
//$str = '今天天气好晴朗';//如果写的是中文，将会出现乱码
$str = 'today is good day!';
imagestring($resource, 5, 100, 80, $str, $color);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code9.php，效果为：

![1540093980436](16)



**==需求5小结==**：

imagestring函数指定的字符起点坐标是第一个字符的左上角的坐标。



**==需求6==**：创建code10.php程序文件，

1. 使用imagecreate创建一个400*200的画布；
2. 将画布的背景色填充为RGB(255, 233, 85)；
3. 在画布上使用imagettftext写一句话："今天天气好晴朗"；
4. 将画布以jpeg格式的图片直接输出到浏览器；

**==解答6==**：创建名为code10.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreate(400, 200);//创建400*200 px的画布

#分配一个颜色
$color = imagecolorallocate($resource, 255, 233, 85);

#填充画布背景色
imagefill($resource, 0, 0, $color);

#根据ttf格式的字体写字
$color = imagecolorallocate($resource, 233, 16, 78);
$str = '今天天气好晴朗';
imagettftext($resource, 20, 0, 100, 80, $color, 'F:/home/class/day15/code/source/font1.ttf', $str);

#输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问code10.php，效果为：

![1540094427324](17)



**==需求6小结==**：

指定的ttf格式的字体必须为从盘符开始的绝对路径地址，否则图片生成失败。



#### 辅助相关操作

涉及的函数：

> ==**imagesx**==(画布资源)    获得图片的宽度
>
> **==imagesy==**(画布资源)    获得图片的高度
>
> **getimagesize**(图片路径)    获得图片的宽度和高度等信息



**==需求==**：创建code11.php程序文件，

1. 使用imagecreatefromjpeg创建一个画布；
2. 获取这个画布的宽度；
3. 获取这个画布的高度；
4. 获取这个画布的宽度、高度、位度等信息；

**==解答==**：创建名为code11.php的程序文件，代码如下：

```php
<?php

#imagecreate函数
$resource = imagecreatefromjpeg('./aa.jpg');

#获得宽度
$w = imagesx($resource);
echo '宽度为：' . $w . '<br/>';

#获得高度
$h = imagesy($resource);
echo  '高度为：' . $h . '<br/>';

#获得宽高等信息
$info = getimagesize('./aa.jpg');
print_r($info);
```

访问code11.php，效果为：

![1540104540209](18)



## 4. 案例：制作水印图

### 功能分析

1. 根据一张已有的目标图片创建画布（擎天柱）;
2. 根据一张已有的LOGO图片创建画布；
3. 在目标图片（擎天柱）选择一个坐标点；
4. 在LOGO图片上将左上角的点选择为坐标基点；
5. 将LOGO图片拖拽进目标图片中，让两个选择好的坐标基点重合对齐；
6. 调整在目标图片（擎天柱）中LOGO图片的透明度；
7. 将最终的成品图到处成一张jpeg格式的图片；
8. 关闭目标图片（擎天柱）画布；
9. 关闭LOGO图片画布；



### 代码实现

![1540105503939](19)

创建名为code12.php的文件，代码如下：

```php
<?php

#1. 根据一张已有的目标图片创建画布（擎天柱）
$dst = imagecreatefromjpeg('./source/001.jpg');

#2. 根据一张已有的LOGO图片创建画布
$src = imagecreatefrompng('./source/logo.png');

/*
3. 在目标图片（擎天柱）选择一个坐标点；
4. 在LOGO图片上将左上角的点选择为坐标基点；
5. 将LOGO图片拖拽进目标图片中，让两个选择好的坐标基点重合对齐；
6. 调整在目标图片（擎天柱）中LOGO图片的透明度；
*/
$dst_w = imagesx($dst);//目标图的宽度
$dst_h = imagesy($dst);//目标图的高度

$src_w = imagesx($src);//LOGO图的宽度
$src_h = imagesy($src);//LOGO图的高度

imagecopymerge($dst, $src, $dst_w/5, $dst_h*2/5, 0, 0, $src_w, $src_h, 30);

#直接将打好水印的图输出到浏览器
header('Content-type:image/jpeg');
imagejpeg($dst);

#关闭画布
imagedestroy($dst);
imagedestroy($src);
```

访问后的效果：

![1540105895671](20)



## 5. 案例：制作缩略图

### 固定宽高缩略图

#### 功能分析

1. 根据一张已有的需要缩小的图片（川普）创建一个画布；
2. 根据目标尺寸（200×300）创建一个目标空白画布；
3. 在目标空白画布上将(0,0)点选择为坐标基点；
4. 在需要缩小的图片（川普）画布中将(0, 0)点选择坐标基点；
5. 将需要缩小的图片（川普）拖拽进目标空白图片画布中，并且将两个图片的坐标基点对齐重合；
6. 将需要缩小图片（川普）的宽度调整为和目标空白画布一样的宽度；将需要缩小图片（川普）的高度调整为和目标空白画布一样的高度；
7. 将最终的成品缩略图到处成一张jpeg格式的图片；
8. 关闭目标图片画布；
9. 关闭需要缩小图片的画布；

#### 代码实现

![1540106815958](21)

创建名为code13.php的程序文件，代码如下：

```php
<?php

#1. 根据一张已有的需要缩小的图片（川普）创建一个画布
$src = imagecreatefromjpeg('./source/m1.jpg');

#2. 根据目标尺寸（200×300）创建一个目标空白画布
$dst = imagecreatetruecolor(200, 300);

/*
3. 在目标空白画布上将(0,0)点选择为坐标基点；
4. 在需要缩小的图片（川普）画布中将(0, 0)点选择坐标基点；
5. 将需要缩小的图片（川普）拖拽进目标空白图片画布中，并且将两个图片的坐标基点对齐重合；
6. 将需要缩小图片（川普）的宽度调整为和目标空白画布一样的宽度；将需要缩小图片（川普）的高度调整为和目标空白画布一样的高度；
*/
$src_w = imagesx($src);//需要缩小画布的宽度
$src_h = imagesy($src);//需要缩小画布的高度

imagecopyresampled($dst, $src, 0, 0, 0, 0, 200, 300, $src_w, $src_h);

#7. 输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($dst);

#8.关闭画布资源
imagedestroy($dst);
imagedestroy($src);
```



访问后的效果：

![1540107061025](22)



固定宽高的缩略图，其实是没有考虑到原图的宽高比例的，实际缩小以后，很可能会变形。



### 等比缩略图

创建名为code14.php的程序文件，代码如下：

```php
<?php

#1. 根据一张已有的需要缩小的图片（川普）创建一个画布
$src = imagecreatefromjpeg('./source/m1.jpg');

$src_w = imagesx($src);//原图的宽度
$src_h = imagesy($src);//原图的高度

#2. 根据目标尺寸（200×300）创建一个目标空白画布
$maxW = 200;//设定最终缩略图的最大宽度为200px
$maxH = 300;//设定最终缩略图的最大高度为300px


if( $src_w<=$maxW&&$src_h<=$maxH ){//如果原图的宽度和高度都小于最大限定值，则维持原图的宽高不变即可
    $dstW = $src_w;//最终缩略图的宽度等于原图的宽度
    $dstH = $src_h;//最终缩略图的高度等于原图的高度
}else{
    
    $dstW = $maxW;//先将原图的宽度缩小为限定宽度的最大值
    //然后按照原图的宽高比例，计算最终缩略图的高度
    /*
    $src_w/$src_h=$dstW/?
    ? = $dstW/($src_w/$src_h)
    ? = $dstW*$src_h/$src_w
    */
    $dstH = $dstW*$src_h/$src_w;//然后根据原图宽高比例计算最终缩略图的高度

    if( $dstH>$maxH ){//当最终计算得到的缩略图高度比限定的最大高度还要大时，则重新计算缩略图的宽高

        $dstH = $maxH;//先将高度缩小为极大值
    /*
    $src_w/$src_h=?/$dstH
    $src_w*$dstH = ?*$src_h
    ? = $src_w*$dstH/$src_h
    */
        $dstW = $src_w*$dstH/$src_h;//然后根据原图宽高比例计算最终缩略图的宽度
    }
}

$dst = imagecreatetruecolor($dstW, $dstH);

/*
3. 在目标空白画布上将(0,0)点选择为坐标基点；
4. 在需要缩小的图片（川普）画布中将(0, 0)点选择坐标基点；
5. 将需要缩小的图片（川普）拖拽进目标空白图片画布中，并且将两个图片的坐标基点对齐重合；
6. 将需要缩小图片（川普）的宽度调整为和目标空白画布一样的宽度；将需要缩小图片（川普）的高度调整为和目标空白画布一样的高度；
*/
//$src_w = imagesx($src);//需要缩小画布的宽度
//$src_h = imagesy($src);//需要缩小画布的高度

imagecopyresampled($dst, $src, 0, 0, 0, 0, $dstW, $dstH, $src_w, $src_h);

#7. 输出图像到浏览器
header('Content-type:image/jpeg');
imagejpeg($dst);

#8.关闭画布资源
imagedestroy($dst);
imagedestroy($src);
```

最终的效果为：

![1540109828062](23)

## 6. 案例：制作验证码

###   功能分析

1. 创建一个有背景色的画布；
2. 构建随机字符；
3. 在画布上写字；
4. 设置干扰元素（干扰点，干扰线）；
5. 将图像输出到浏览器；



###   代码实现

创建名为code15.php的程序文件，代码如下：

```php
<?php

#创建一个空白画布 200*80
$w = 200;
$h = 90;
$resource = imagecreatetruecolor($w, $h);

#给画布填充一个背景色
$color = imagecolorallocate($resource, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));
imagefill($resource, 0, 0, $color);

#构建随机字符
$arr = array_merge(range('a', 'z'), range('A', 'Z'), range(0, 9));
$str = '';

for($i=0; $i<4; $i++ ){
    $str .= $arr[mt_rand(0, count($arr)-1)];
}

#往画布上写字
$bx = $w/4;//文字起点的左下角x坐标
$by = $h*2/3;//文字起点的左下角y坐标
$color = imagecolorallocate($resource, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));//构建随机色
imagettftext($resource, $h/3, 0, $bx, $by, $color, 'F:/home/class/day15/code/source/font1.ttf', $str);

#设置干扰元素
//设置干扰点
for($i=0; $i<300; $i++ ){ 
    $bx = mt_rand(0, $w);//起点x坐标
    $by = mt_rand(0, $h);//起点y坐标

    $ex = mt_rand($bx-2, $bx+2);//终点的x坐标
    $ey = mt_rand($by-2, $by+2);//终点的y坐标

    $color = imagecolorallocate($resource, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));//构建随机色

    imageline($resource, $bx, $by, $ex, $ey, $color);
}

//设置干扰线
for($i=0; $i<10; $i++ ){ 
    $bx = mt_rand(0, $w/2);//起点x坐标
    $by = mt_rand(0, $h);//起点y坐标

    $ex = mt_rand($w/2, $w);//终点的x坐标
    $ey = mt_rand(0, $h);//终点的y坐标

    $color = imagecolorallocate($resource, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));//构建随机色

    imageline($resource, $bx, $by, $ex, $ey, $color);
}


#将图像输出到浏览器
header('Content-type:image/jpeg');
imagejpeg($resource);
```

访问页面，最终效果为：

![1540112532160](24)



## 7. 全天总结

1. 能够使用多种方式创建画布资源

   imagecreate函数    根据宽高创建画布 （色彩还原度低，建议单色系图片使用）

   imagecreatetruecolor函数      根据宽高创建画布（色彩还原度高，建议丰富色系图片使用）

   imagecreatefromjpeg函数    根据一张已有的jpeg格式图片创建画布

   imagecreatefromgif函数      根据一张已有的gif格式图片创建画布

   imagecreatefrompng函数      根据一张已有的png格式图片创建画布

2. 能够操作画布资源

   填充背景色：  imagefill函数

   画线（画点）：imageline函数

   画矩形（正方形）：imagerectangle函数

   画圆弧（椭圆圆弧，正圆）：imagearc函数

   根据指定的ttf格式的字体写字：imagettftext函数

   分配颜色：imagecolorallocate函数

3. 能够获取画布或图片的宽高尺寸信息

   获得画布的宽高：1) imagesx函数获得宽度；2)imagesy函数获得高度；

   获得指定图片的宽高等信息： getimagesize函数

4. 能够输出画布资源为图片文件或网页图

   imagejpeg函数      以jpeg格式的图片输出到浏览器或者保存成文件

   imagegif函数      以gif格式的图片输出到浏览器或者保存成文件

   imagepng函数      以png格式的图片输出到浏览器或者保存成文件

5. 能够销毁画布资源

   imagedestory函数

6. 能够使用GD技术制作验证码图片

7. 能够实现一张图片的缩略图

   imagecopyresampled函数

8. 能够给一张图片添加水印图

   imagecopymerge函数

