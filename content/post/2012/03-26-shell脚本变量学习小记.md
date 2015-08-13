---
date: Mon, 26 Mar 2012 15:02:02 +0000
title: shell脚本变量学习小记
author: tomheng
layout: post
permalink: /archives/1464.html
views: 349
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - shell
---
转载自：<http://blog.csdn.net/love__coder/article/details/6744332>  
**1、查看所有的shell变量**

set

**2、查看所有的只读shell变量**

readonly

**3、变量设置值**

格式：var\_name=var\_value

<span style="color: #ff0000;">注意</span>：=连接变量名和变量值，=两侧不能有空格；当值var\_value含空格时，需要双引号把var\_value包起来

**4、输出变量值**

echo $var_name

echo ${var_name}

**5、变量值连接**

echo $var\_name1$var\_name2

<span style="color: #ff0000;">注意</span>: 两个变量之间没有空格

**6、查看所有环境变量**

env

**7、给环境变量设置值**

VAR_NAME=VALUE

export VAR_NAME

**8、清除变量**

unset var_name

**9、导出变量到子脚本中**

父脚本中定义好变量，然后 export var_name

子脚本中可以使用该变量. $var\_name或${var\_name}

**10、显示脚本执行状态**

执行完脚步，输入 echo $?

<span style="color: #ff0000;">注意</span>：0，表示成功

**11、脚本运行的当前进程id**  
$$

**12、传递给shell脚本的参数个数**  
$#

**13、反引号\`**

设置系统的命令输出到变量

echo &#8220;shell file name is :\`basename $0\` &#8221;

**14、以串行形式，打印当前整个目录**

echo *

**15、替换运算符**，

  * 1) ${var\_name:-def\_Val}如果变量var\_name存在且为非null，返回该变量的值，否则返回默认值def\_Val注意var_name与:之间没有空格，:与-之间可以有空格。主要用途，如果变量未定义，则用默认值.
  * 2) ${var\_name:=val}如果变量var\_name存在且为非null，返回该变量的值，否则，把val的值赋给变量var\_name，并返回var\_name的值val注意var_name与:之间没有空格，:与=之间也不能有空格。
  * 3)${var\_name:?message}，如果变量var\_name存在且为非null，返回该变量的值，否则返回该变量的名字var\_name:提示信息meesage，并退出当前命令或脚本注意var\_name与:之间没有空格，:与?之间也不能有空格。
  * 4) ${var\_name:+val}如果变量var\_name存在且为非null，返回val，否则返回null注意var_name与:之间没有空格，:与+之间也不能有空格。

**15、返回变量长度**

${#val_name}

**16、显示所有命令行参数**

$* 或 $@

**17、算术运算操作 $(())**

$((var1 opr var2))

只能是+-*/ 和()运算符，并且只能做整数运算

例如: $((5+1))

**18、命令代换$()**

类似于 反引号\`

例如：echo $(date)
