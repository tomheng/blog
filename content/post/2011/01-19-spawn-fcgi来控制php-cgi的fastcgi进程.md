---
date: Wed, 19 Jan 2011 07:46:33 +0000
title: Spawn-FCGI来控制php-CGI的FastCGI进程
author: tomheng
layout: post
permalink: /archives/947.html
robotsmeta:
  - index,follow
views: 975
categories:
  - LAMP
tags:
  - PHP
---
Spawn-FCGI来控制php-CGI的FastCGI进程命令

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          &nbsp; &nbsp; <span class="sy0">/</span>usr<span class="sy0">/</span>local<span class="sy0">/</span>bin<span class="sy0">/</span>spawn-fcgi <span class="re5">-a</span> 127.0.0.1 <span class="re5">-p</span> <span class="nu0">9000</span> <span class="re5">-C</span> <span class="nu0">5</span> <span class="re5">-u</span> www-data <span class="re5">-g</span> www-data <span class="re5">-f</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>php-CGI
        </div>
      </td>
    </tr>
  </table>
</div>

参数含义如下:

　　-f 指定调用FastCGI的进程的执行程序位置，根据系统上所装的PHP的情况具体设置  
　　-a 绑定到地址addr  
　　-p 绑定到端口port  
　　-s 绑定到unix socket的路径path  
　　-C 指定产生的FastCGI的进程数，默认为5(仅用于PHP)  
　　-P 指定产生的进程的PID文件路径  
　　-u和-g FastCGI使用什么身份(-u 用户 -g 用户组)运行，Ubuntu下可以使用www-data，其他的根据情况配置，如nobody、apache等
