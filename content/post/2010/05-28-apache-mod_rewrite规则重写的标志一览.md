---
date: Fri, 28 May 2010 15:01:32 +0000
title: Apache mod_rewrite规则重写的标志一览
author: tomheng
layout: post
permalink: /archives/663.html
robotsmeta:
  - index,follow
views: 611
categories:
  - LAMP
tags:
  - Apache
---
Apache mod\_rewrite确实很强大，不过也是需要下些功夫才能学好的东西，通过它可以实现很多有趣的功能。下面是Apache mod\_rewrite的学习笔记。

**一、Apache mod_rewrite规则重写的标志一览**

  1. R\[=code\](force redirect) 强制外部重定向强制在替代字符串加上http://thishost[:thisport]/前缀重定向到外部的URL.如果code不指定，将用缺省的302 HTTP状态码。
  2. F(force URL to be forbidden)禁用URL,返回403HTTP状态码。
  3. G(force URL to be gone) 强制URL为GONE，返回410HTTP状态码。
  4. P(force proxy) 强制使用代理转发。
  5. L(last rule) 表明当前规则是最后一条规则，停止分析以后规则的重写。
  6. N(next round) 重新从第一条规则开始运行重写过程。
  7. C(chained with next rule) 与下一条规则关联如果规则匹配则正常处理，该标志无效，如果不匹配，那么下面所有关联的规则都跳过。
  8. T=MIME-type(force MIME type) 强制MIME类型
  9. NS (used only if no internal sub-request) 只用于不是内部子请求
 10. NC(no case) 不区分大小写
 11. QSA(query string append) 追加请求字符串
 12. NE(no URI escaping of output) 不在输出转义特殊字符　　例如：RewriteRule /foo/(.*) /bar?arg=P1\%3d$1 [R,NE] 将能正确的将/foo/zoo转换/bar?arg=P1=zed
 13. PT(pass through to next handler) 传递给下一个处理　　例如：RewriteRule ^/abc(.*) /def$1 [PT] # 将会交给/def规则处理Alias /def /ghi
 14. S=num(skip next rule(s)) 跳过num条规则
 15. E=VAR:VAL(set environment variable) 设置环境变量

**二、实例分析：**

<span style="color: #0000ff;"> # BEGIN WordPress</span>

RewriteRule &#8220;^/dir/ ([^./]*) \.html&#8221; &#8220;/dir/script.cgi?doc=$1&#8243; [PT]  
RewriteEngine On  
RewriteBase /wordpress/  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d

**三、解释：**

1）RewriteRule &#8220;^/dir/ ([^./]*) \.html&#8221; &#8220;/dir/script.cgi?doc=$1&#8243; [PT]

PT(pass through to next handler) 传递给下一个处理  
将形如：/dir/abc.html 定向到/dir/script.cgi?doc=abc 并且将重定向的结果传递给下一条规则使用

2）RewriteEngine On  
开启url rewrite

3)RewriteBase /wordpress/  
设定重写的目录

4)  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d

这两句的意思是如果请求的url中有在服务器能找到的文件那么就直接访问文件，有能找到的目录就直接访问目录 ，并停止执行下面的重写规则
