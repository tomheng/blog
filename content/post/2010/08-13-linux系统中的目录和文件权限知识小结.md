---
date: Fri, 13 Aug 2010 12:33:57 +0000
title: Linux系统中的目录和文件权限知识小结
author: tomheng
layout: post
permalink: /archives/763.html
robotsmeta:
  - index,follow
views: 623
categories:
  - LAMP
tags:
  - Linux
---
<span style="color: #ff0000;">这是一篇小白的笔记，高手速闪。</span>

### **一、在Linux中如何表示目录权限**

执行<span style="color: #3366ff;">ls -lh</span> 可以看到目录的权限

<div id="_mcePaste">
  <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;">d</span><span style="line-height: 20px;"><span style="color: #0000ff;">rwx</span></span><span style="line-height: 20px;"><span style="color: #008000;">r-x</span></span><span style="line-height: 20px;"><span style="color: #00ccff;">r-x</span></span> 1 7155 wheel 6.1K 03-25 04:30  ylwrap
</div>

<div>
  Linux中就是用上面标出的彩色部分表示一个目录的权限的。我用三种颜色分了三组，这三组分表对应于所有者的权限，所有者所在用户组的权限和其他用户的权限。每一组权限由三个字符组成，可选的表示字符有：
</div>

<div>
  <ul>
    <li>
      <span style="font-family: georgia, verdana, Arial, helvetica, sans-seriff; line-height: 20px; color: #4b4b4b;">r(Read，读取)：对文件而言，具有读取文件内容的权限；对目录来说，具有浏览目 录的权限。</span>
    </li>
    <li>
      <span style="font-family: georgia, verdana, Arial, helvetica, sans-seriff; line-height: 20px; color: #4b4b4b;">w(Write,写入)：对文件而言，具有新增、修改文件内容的权限；对目录来说，具有删除、移动目录内文件的权限。</span>
    </li>
    <li>
      <span style="font-family: georgia, verdana, Arial, helvetica, sans-seriff; line-height: 20px; color: #4b4b4b;">x(eXecute，执行)：对文件而言，具有执行文件的权限；对目录了来说该用户具有进入目录的权限。</span>
    </li>
    <li>
      <span style="font-family: georgia, verdana, Arial, helvetica, sans-seriff; line-height: 20px; color: #4b4b4b;">－：表示不具有该项权限。</span>
    </li>
  </ul>
</div>

所以在上面输出中第一组用户所有者的权限为：rwx 表示，它具体写入，读取和执行的权限。

### 二、权限的数字表示

用rwx三个字符可以表示目录和文件的权限，Linux中还提供另一种表示文件和目录权限的方式&#8211;数字字符。rwx三个权限可以用二进制表示，1表示有此权限，0代表无此权限。

举例：

rwx:  111             -rw: 011

接下来我们可以把三位二进制数转化成十进制数：

rwx:  111  7      -rw:011   3

我们可以把rwx-四个字符对应到十进制数子字符

  * <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;"> r: 4</span>
  * <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;"> w: 2</span>
  * <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;"> x：1</span>
  * <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;">－：0</span>

### 三、更改目录的权限

1）更改文件的所有者 ：<span style="color: #3366ff;">chown tomheng hello.dir </span>，把hello.dir的所有权改用tomheng这个用户

2）用数字字符 ：<span style="line-height: 20px;"><span style="color: #3366ff;">chmod 777 hello.dir</span></span>

<span style="font-family: monospace; line-height: 20px; color: #4b4b4b;">3）</span><span style="line-height: 20px;"><span style="color: #000000;">用字符表示</span></span><span style="font-family: monospace; line-height: 20px; color: #4b4b4b;">:</span>

  * <span style="font-family: monospace; line-height: 20px; color: #4b4b4b;"> </span><span style="line-height: 20px;"><span style="color: #3366ff;">chmod +r hello.dir <span style="color: #000000;">目录添加r权限</span></span></span>
  * <span style="line-height: 20px; color: #3366ff;"> chmod -w hello.dir <span style="color: #000000;">目录去除w权限</span></span>
