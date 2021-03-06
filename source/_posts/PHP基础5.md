---
title: PHP基础5
date: 2019-02-20 17:00:02
tags: PHP
---
# 一、昨日回顾

## 1. 知识回顾

1. 能够理解文件包含（文件载入）的含义和作用

   其实就是将某个文件的代码引入到当前的程序文件中。

2. 能够解释4种不同载入方式的区别

   1) include    2) include_once   3) require     4) require_once

   区别：

   a） include系列和require系列的区别：include系列当引入一个不存在的文件时，将会报一个轻量级别的错误，并且不会中断程序继续往下执行；require系列当引入一个不存在的文件时，将会报一个致命的错误，并且会中断程序继续往下执行。

   b） 带once和不带once的区别：带once的会检查引入的文件之前是否已经引入过，如果已经引入过，则本次不再重复引入，如果没有引入才会引入这个文件；不带once则不会去检查当前这个文件是否已经引入过。
<!-- more -->
3. 能够理解文件载入时的路径设置

   相对路径和绝对路径

   相对路径：==./==和==../==  在PHP的程序中，相对路径相对的是当前被访问的程序文件而言；

   绝对路径：从盘符开始一直到某个具体的文件名为止的全路径地址。

4. 能够解释常用的6个转义字符

   单引号中输出单引号

   双引号中输出双引号

   双引号字符串中输出$符号

   双引号字符串中输出回车符(\r)和换行符(\n)

   双引号字符串中输出(\)符号

   双引号字符串中输出制表符(\t)

5. 能够理解数组的基本概念

   键值对的集合。

6. 能够对数组元素进行基本操作

   增：指定一个新的下标和新值    $arr[新的下标] = 新的值；

   改：下标不变，值改变    $arr[下标] = 新值；
   删：使用unset进行删除

   查：print_r或var_dump

7. 理解二维数组的概念

   数组的每一个元素的值又是一个一维数组。

8. 能够使用foreach遍历数组

   ```php
   foreach(目标数组 as [保存下标的变量=>]保存值的变量){
       遍历循环结构体
   }
   ```

  



# 二、知识路径

- 错误处理

  ​	错误的分类

  ​	错误的触发

  ​	错误的代号（级别）

  ​	错误（显示）的设置

  ​	错误日志设置

  ​	自定义错误处理

  ==目标：能够在程序中设置显示或关闭显示错误；在程序中能够指定存储错误信息的日志文件的位置==

- 函数

  ​	基本概念

  ​	函数的定义

  ​	函数名的命名规范

  ​	函数的参数详解

  ​	函数（参数）的值传递和引用传递

  ​	形式参数的默认值

  ​	函数体与函数的返回值

  ​	作用域

  ​	静态变量

  ​	伪类型的概念

  ​	匿名函数

  ​	常见的算法思想

  ​		递归思想

  ​		递推思想

  ==目标：能够在程序中任意封装自定义函数，同时还要掌握调用函数的细节==

# 三、今日课程内容：PHP基础

### 数组的排序

数组排序的算法种类非常多，我们将介绍两种：1）冒泡排序；2）选择排序；



#### 冒泡排序实现升序

冒泡排序的思路：先从第一个数开始，每次将==相邻==的两个数进行比较，如果发现前值比后值大，则互换位置，否则维持原位置不变。 



**==演示案例==**：将数组array(16, 4, 12, 9, 21, 18)的元素值进行排序

手动推导过程：

![1538184790041](2)



程序实现：code2.php

```php
<?php

$arr = array(16, 4, 12, 9, 21, 18);
print_r($arr);  echo '<hr/>';
/*

数组总共6个元素
冒泡排序总共比较了5轮

轮数       每一轮比较的次数
1                    5
2                    4
3                    3
4                    2
5                    1
*/

//控制总共比较的轮数
//                6-1
for($i=0; $i<count($arr)-1; $i++ ){ 

    /*
        第一轮   $i        本轮需要比较的次数     条件
        1           0                5                      $j<5
        2           1                4                      $j<4
        3           2                3                      $j<3
        4           3                2                      $j<2
        5           4                1                      $j<1
    */
    
    //每一轮比较的次数
    //                     6-1-$i
    for($j=0; $j<count($arr)-1-$i; $j++ ){ 
        
        /*
        $j        条件
        0         $arr[0]>$arr[1]
        1         $arr[1]>$arr[2]
        $j        $arr[$j]>$arr[$j+1]
        */
        if( $arr[$j]>$arr[$j+1] ){//如果前面的数比后面的数大，则交换位置
            
            $t = $arr[$j];
            $arr[$j] = $arr[$j+1];
            $arr[$j+1] = $t;
        }
    }
}

print_r($arr);
```

访问code2.php:

![1538186007593](3)



#### 选择排序实现升序

选择排序的思路：

第一轮循环，假设第一号位上的数是最小的，然后跟第二个数进行比较，一直比到最后，找出最小的数，然后把最小值与第一号位上的值互换；

再进行下一轮循环，找出最小值与第二号位的值互换；以后的循环都重复这种操作，一直到排序完成。



**==演示案例==**：将数组array(16, 4, 12, 9, 21, 18)的元素值进行排序

手动推导过程：

![1538187608835](4)



程序实现：将数组array(16, 4, 12, 9, 21, 18)的元素值进行排序

code4.php

```php
<?php

$arr = array(16, 4, 12, 9, 21, 18);
print_r($arr); echo '<hr/>';
/*
元素的总个数：6
总共需要比较的轮数：5
*/

//控制总共比较的轮数
//                     6-1
for($i=0; $i<count($arr)-1; $i++ ){ 
    /*
    轮数   $i      假设最小值的下标为
       1    0                 0
       2    1                 1
       3    2                 2
       4    3                 3
       5    4                 4
    */
    $t = $arr[$i];//假设一个最小数
    $tk = $i;//同时记录下该假设元素的下标

    /*
    轮数      每一轮比较的次数     每一轮第一个和假设最小数比较的值他的下标
    1               5                                1
    2               4                                2
    3               3                                3
    4               2                                4
    5               1                                5
    */
    /*
     轮数      $j初始值      每一轮比较的次数      条件
     1               1                   5                      $j<6
     2               2                   4                      $j<6
     3               3                   3                      $j<6
     4               4                   2                      $j<6
     5               5                   1                      $j<6

    */
    for($j=$i+1; $j<count($arr); $j++ ){ 
        
        if( $t>$arr[$j] ){//如果假设的最小数比比较的数还要大，则改变$t的值
            $t = $arr[$j];
            $tk = $j;
        }
    }

    //互换假设位置上的数和实际的最小数
    $arr[$tk] = $arr[$i];
    $arr[$i] = $t;
}

print_r($arr);
```

访问程序效果为：

![1538189465586](5)

## 1. 错误处理

由于人为的操作失误或者环境因素，有时候PHP程序在运行的过程中会产生各种各样的错误，针对不同性质的错误，PHP都会有相应的错误处理方式。



### 错误的分类

TIPS：PHP针对不同性质的错误划分为三种分类：1）编译时（语法）错误； 2）运行时错误；3）逻辑错误；



**==演示案例==**：编译时错误

![1538190512866](6)



**==演示案例==**：运行时错误

![1538190641754](7)



**==演示案例==**：逻辑错误   逻辑错误本质上是不存在语法问题的，只不过违背的了项目的业务逻辑，我们认为把它定性成为错误。

![1538190852496](8)



### 错误的触发

TIPS：在PHP中，错误的触发按角色划分，分为两类：1）由系统触发；2）由人为手动触发；



#### 系统触发

TIPS：本质上还是人为的在代码中犯了错误，违背了PHP中的某种规定或规则从而导致错误的产生。如前面所讲的编译时错误和运行时错误。



#### 手动触发

TIPS：手动触发指的是我们可以在PHP中通过==trigger_error==函数人为触发一个错误。 



**==演示案例==**：

![1538191157748](9)



### 错误的代号（级别）

TIPS：PHP已经做出了相关规定，根据错误的严重程度，划分了错误级别。



#### 错误级别的分类

TIPS：我们在前面已经了解到，错误的触发分为两类：1）系统触发；2）用户触发；



然而PHP不仅划分了错误触发的分类，同时还进一步对不同触发分类又划分了更加细化的==错误级别==。



==由系统触发==的，被称为==内置错误级别==，它包括：

E_WARNING、E_NOTICE、E_ERROR

==由用户触发==的，被称为==用户错误级别==，它包括：

E_USER_WARNING、E_USER_NOTICE、E_USER_ERROR



**==演示案例1==**：程序中可以通过trigger_error的==第二个参数==，来==改变用户错误的级别==。 

![1538191720207](10)



### 错误（显示）的设置

TIPS：在PHP中，我们可以通过php.ini中的error_reporting和display_errors配置项配置错误报告功能。 



**==配置说明==**：

![1533970125526](191)

![1533970198499](192)



### 错误日志设置

TIPS：在PHP中，我们可以通过php.ini中的log_errors和error_log配置项配置错误日志功能。 



**==配置说明==**：

![1533971224911](193)

![1533971312290](194)

php默认是不开启error_log配置的，因为apache也有错误日志记录文件。我们在配置环境的时候，已经将PHP作为apache的一个模块进行配置了，所以今后php出现了错误，默认都会记录到apache的错误日志文件中。

![1533971454269](195)



### 自定义错误处理

TIPS：在项目中，如果我们直接修改配置文件将影响服务器中所有的项目，所以通常情况下，配置的原则应该是哪个项目需要就在哪个项目中自定义进行配置使用。 



**==演示案例1==**：实现在程序中局部关闭E_NOTICE内置错误级别的错误信息显示和记录。

![1538193265191](11)

经过单独关闭E_NOTICE级别的错误，NOTICE提示的信息就消失了。



**==演示案例2==**：实现开启所有的错误级别，并且改变默认记录错误日志的文件路径。

![1538193652718](12)



## 2. 函数

在项目中，我们经常会开发一些功能代码。但是在整个网站里，需要用到这个功能代码的地方可能不止一处。那么，如果每次用到都去重复写一遍，显然是不合理的，这会极大的降低开发的效率。



所以在PHP中，我们可以使用函数将某些功能代码打包封装起来，以后如果需要用到，则只需要去调用函数的名字即可，而不需要再次反复的去重写相同的功能代码。



### 基本概念

概念：函数是具有特定功能的一段代码。



### 函数的定义和调用

基本语法定义结构：

```php
function  函数名(){
       函数体
}
```



**==需求==**：构建一个函数，求20+30的和。

**==解答==**：

![1538203986260](13)



**==小结==**：定义函数的关键字function。



**==细节1==**：函数的定义只相当于一个结构声明，在没有调用之前，函数的函数体是不会被执行的。

![1538204279283](14)



**==细节2==**：函数的结构是在==编译时==就已经确定的，而==调用操作是在程序执行到调用函数的代码时==才生效的，所以，无论在定义前调用，还是在定义后调用，都不会出现任何的错误。

![1538204999667](15)



==**小结**：==

1. 函数只需要指定函数的函数名加上小括号来进行调用；
2. 函数无论在定义前还是定义后调用，都能够调用成功。



### 函数名的命名规范

规范：函数名包括数字，字母，下划线。函数名不能以数字开头。



**==演示案例==**：

![1538205277521](16)



**==小结==**：函数名不能以数字开头。



### 函数的参数详解

TIPS：在构建函数时，我们是可以为函数指定参数的。

为函数指定参数实际上是提升函数内部的数据活性，只要定义了参数，我们可以在调用时通过参数传递任意的数据。

指定参数的语法：

```php
function 函数名([参数1[, 参数2[, ...参数n]]]){
    函数体
}
```



**==需求==**：构建一个函数，求任意两个数的和。

**==解答==**：

![1538205677632](17)



**==小结==**：给函数指定参数的方式是直接在函数定义的小括号中去构建参数，构建参数可以提高功能灵活性。



#### 形参和实参

TIPS：形参和实参实际上就是一种==对参数的称呼==。

形参：我们通常也称为==形式参数==，==即**定义**函数时所指定的参数==；

实参：我们通常也称为==实际参数==，==即**调用**函数时所传递的参数==；



**==演示案例==**：

![1538206078378](18)





#### 形参的默认值

TIPS：我们在定义函数指定形参时，可以为形参指定默认值。



**==需求==**：构建一个函数，求任意两个数的和，同时为第一个参数指定默认值为20，为第二个参数指定默认值为30。

**==解答==**：

![1538206383142](19)



**==小结==**：

1. 如果函数形参有默认值，则调用时不传参，那么函数将会直接使用默认值；如果传了参数，则使用传递的参数值；
2. 我们通常在实际的开发项目中，会将有默认值的形参定义在靠后面的位置。



### 函数的返回值

TIPS：在函数体中，我们可以为一个函数通过使用==return语句==结构指定函数的==返回值==。



**==注意==**：一个函数==只会==有一个返回值。



**==需求==**：构建一个函数，求任意两个参数的和，将求得的和作为函数的返回值返回。

**==解答==**：

![1538207778087](20)



**==小结==**：在函数体内部使用return语句来构建函数的返回值。



**==细节1==**：如果一个函数没有return语句，那么，函数的返回值将会是NULL值。

![1538207884133](21)





### 函数（参数）的值传递和引用传递

TIPS：在调用一个函数同时为这个函数传递了参数时，实际上就相当于给函数的形参变量赋了值。一旦存在为变量赋值的操作，就会有值传递和引用传递两种传递方式。



#### 函数的值传递

体现的效果：在函数内部改变形参的值，并不会影响调用时传递给这个形参的变量。



**==演示案例==**：

![1538208432526](22)





#### 函数的引用传递

体现的效果：在函数内部改变形参的值，将会影响调用时传递给这个形参的变量。



**==演示案例==**：

![1538208694004](23)



### 作用域

在PHP中，存在：1）全局作用域；2）局部作用域；



全局作用域：可以直观的认为就是==函数体外围的空间==；

局部作用域：当前我们可以直观的认为就是==函数内部的空间==；



**==细节==**：全局作用域与局部作用域不重叠。

![1538209398494](24)



**==小结==**：

1. 局部作用域和全局作用域不重叠；
2. 局部作用域： 我们当前就直观的认为是函数体内部的区域；
3. 全局作用域：指的就是函数体外部的区域；



#### 局部作用域使用全局作用域变量问题

TIPS：正因为存在作用域，而局部作用域与全局作用域是不重叠的，所以，正常状况下，我们无法在局部作用域直接使用全局作用域的变量。但是，我们可以使用一种特殊的变量==$GLOBALS==，来规避当前这种状况。



**==注意==**：$GLOBALS属于九大超全局预定义数组变量，它是==超全局的==，即==没有作用域的限制==。



**==演示案例==**：直接在函数体内部通过$GLOBALS调用全局作用域中的变量。

![1538211044121](25)



**==小结==**：

1. 要想使用$GLOBALS调取到全局作用域下的变量，可以将变量名作为$GLOBALS数组的下标名来指定即可；



**==细节1==**：如果我们往$GLOBALS中添加一个元素，则PHP也会自动在全局作用域中自动新增一个变量，该变量的名为元素的下标名； 

![1538211458595](26)

反之也成立：

![1538211615850](27)



**==细节2==**：函数体中的局部变量，在函数执行完后将会被全部自动销毁；

![1538211863367](28)



### 静态变量

TIPS：由于函数体中的局部变量在函数执行结束后，都将会被自动销毁，所以无法被沿用到以后的调用过程中。但是，PHP提供了另外一种方式，在函数体内部可以通过关键字==static==来定义==静态变量==，静态变量在函数被调用后是不会被销毁的，而会继续被沿用。



**==演示案例==**： 

![1538212231542](29)



### 伪类型的概念

TIPS：伪类型并不是真实存在于PHP中真实的数据类型。

伪类型只是在==文档==当中用于==描述某些特殊形参支持传递参数数据类型==的一种描述。



==mixed类型==，表示支持多种数据类型的参数 

![1533980114792](216)



==callback类型==，学名：回调函数，表示传递的这个参数是一个函数 

![1533980200591](217)

![1538213170600](30)





## 3. 全天总结

1. 能够理解数组冒泡、选择排序算法

2. 能够理解通常的错误分类概念

   分类：1）系统触发错误；2）用户触发错误；

3. 能够说出3-5个错误代号

   E_NOTICE、E_WARNING、E_ERROR

   E_USER_NOTICE、E_USER_WARNING、E_USER_ERROR

4. 能够人为触发一个错误

   使用trigger_error函数触发一个人为的错误；

5. 能够设置显示错误信息以及记录错误日志，能够理解自定义错误处理机制

   ```php
   //设置报告哪些错误级别的错误，E_ALL表示所有错误的级别都要报告
   ini_set('error_reporting', E_ALL);
   
   //设置错误显示的，如果值为Off表示关闭显示错误
   ini_set('display_errors', 'Off');
   
   //设置记录错误的文件的位置路径
   ini_set('error_log', 'F:/xxx/xx/err.log');
   
   //设置是否开启记录错误到日志文件中，1表示是开启，0表示关闭
   ini_set('log_errors', 1);
   ```

   

6. 能够运用函数基本语法

   ```
   function 函数名(){
       函数体
   }
   ```

   

7. 能够解释形参和实参的概念

   形参： 定义函数时指定的参数

   实参： 调用函数时传递给该函数的参数

8. 能够解释静态变量的含义

   使用static关键字在函数内部定义的变量就是静态变量。

9. 能够解释变量作用域的含义

   包括：1）全局作用域； 2）局部作用域；

   局部作用域：  当前可以直观的理解为就是函数体内部的区域；

   全局作用域： 就是函数体外部的区域；



