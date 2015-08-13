---
date: Fri, 03 Jul 2009 12:25:06 +0000
title: php中的字符串（single quote，double quote，heredoc,nowdoc）(一)
author: tomheng
layout: post
permalink: /archives/129.html
views: 757
categories:
  - LAMP
tags:
  - LAMP
---
本文大部分出自php在线手册相关页面，少量内容为个人经验（如有错误欢迎指正）,你也可以参看此页面获得更详细的内容

**简单介绍：**

一、关于php中的字符串

Php中的字符串和c语言中的类似，是一个字符数组，这个数组的大小是没有限制，只要内存允许，你可以存足够多的字符！

二、单引号

单引号是定义字符串的最简单形式。在单引号内只有单引号（single quote )和反斜线（back slash）需要转义。  
实例：

<pre lang="php">echo $a='Here is 'webfuns.cn'';     //error
echo $aa='Here is \'webfuns.cn'\'; //ok
echo $b=' back slash is \';          //error
echo $bb='back slash is \\';          //ok</pre>

三、双引号

双引号相对于单引号有更多需要转义的字符，列表如下：

<table border="0">
  <tr bgcolor="#ddd">
    <td width="107" valign="center">
      转义字符
    </td>
    
    <td width="469" valign="center">
      含义
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \n
    </td>
    
    <td width="469" valign="center">
      换行符(LF or 0x0A (10) in ASCII)
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \r
    </td>
    
    <td width="469" valign="center">
      回车 (CR or 0x0D (13) in ASCII)
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \t
    </td>
    
    <td width="469" valign="center">
      水平制表符 (HT or 0x09 (9) in ASCII)
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \v
    </td>
    
    <td width="469" valign="center">
      垂直制表符(VT or 0x0B (11) in ASCII) (since PHP 5.2.5)
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \f
    </td>
    
    <td width="469" valign="center">
      换页符 (FF or 0x0C (12) in ASCII) (since PHP 5.2.5)
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \\
    </td>
    
    <td width="469" valign="center">
      反斜线\
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \$
    </td>
    
    <td width="469" valign="center">
      美元符号$
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \&#8221;
    </td>
    
    <td width="469" valign="center">
      双引号
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \[0-7]{1,3}
    </td>
    
    <td width="469" valign="center">
      正则表达式中匹配八进制字符
    </td>
  </tr>
  
  <tr>
    <td width="107" valign="center">
      \x[0-9A-Fa-f]{1,2}
    </td>
    
    <td width="469" valign="center">
      正则表达式中匹配十六进制字符
    </td>
  </tr>
</table>

四、heredoc

用法：

<pre lang="php">echo&lt;&lt;&lt;EOT
Here is webfuns,you are welcome;
EOT;
</pre>

<span style="color: #ff0000;">注意：<br /> 最后一个标识符(在本例即EOT要和<<<后的字符一致，大小写都可以，但是一定要一致<br /> 最后一个EOT必须顶着开头写不能有任何的空格</span>  
五、Nowdoc  
用法基本和heredoc相同，只是EOT换成了'EOT'，加了个单引号。

<pre lang="php">echo&lt;&lt;&lt;'EOT'
Here is webfuns,you are welcome;
EOT;
</pre>
