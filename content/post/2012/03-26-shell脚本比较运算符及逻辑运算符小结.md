---
date: Mon, 26 Mar 2012 14:53:00 +0000
title: shell脚本比较运算符及逻辑运算符小结
author: tomheng
layout: post
permalink: /archives/1460.html
views: 476
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - shell
---
**1、数值**

格式：

test &#8220;num1&#8243; opr &#8220;num2&#8243;

[ &#8220;num1&#8243; opr &#8220;num2&#8243; ]

opr 取值：

  * 相等：-eq
  * 不等：-ne
  * 大于：-gt
  * 小于：-lt 【l是字母L的小写】
  * 小于等于：-le
  * 大于等于：-ge

**2、字符串**

格式：

[ str1 opr str2]

[ opr str ]

opr取值：

  * 相等：=
  * 不等：!=
  * 空串：-z
  * 非空串：-n

**3、文件**

格式：

[ opr file ]

opr取值：

  * 目录： -d
  * 文件： -f
  * 链接： -L
  * 可读： -r
  * 可写： -w
  * 可执行： -x
  * 文件非空： -s

**4、逻辑运算符**

  * 逻辑与： -a 格式： [ condition1 -a condition2 ]
  * 逻辑或： -o 格式： [ condition1 -o condition2 ]
  * 逻辑否： ! 格式： [ ! condition ]

<span style="color: #ff0000;">注意</span>：[ 与condition 之间必须有空格，condition与] 之间也必须有空格

<span style="color: #ff0000;">注意</span>： -a -o 用在一个[]中间连接多个条件，而 && || 则用在多个[]之间，连接多个[]条件

<span style="color: #ff0000;">非法</span>： [ condition1 && condition2 ]
