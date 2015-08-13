---
date: Sun, 08 Jun 2014 05:05:16 +0000
title: CyanogenMod源码目录简介
author: tomheng
layout: post
permalink: /archives/1799.html
views: 141
categories:
  - LAMP
tags:
  - Android
  - CyanogenMod
---
简单说明下，CyanogenMod的目录结构，方便大家编译自己的ROM。

**bionic/**

主要是一些Android 定制的[libc][1] 库文件，一般情况下无需修改此目录的文件。

**build/**

编译过程回用到的脚本和各种文件。如果对[Android编译][2]过程和各种Make 命令有兴趣，可以研究下这个目录。

**bootable/**

<span style="color: #000000;"> ClockworkMod recovery的目录。</span>

**dalvik/**  
运行Java程序的虚拟机dalvik。

**art/**

在Android 4.4中引入的新的Java运行环境，旨在替代dalvik，通过预编译APP提高运行速度。

**device/**

具体手机适配的文件相关的目录，目录一般组织为device/<vendor>/<codename>。比如小米2s的可以这样device/xiaomi/aries。做适配的时候大多都在这里进行。

**doc/**

包括 [Android Open Source][3] 网站的源码。

**external/**

被Android 或 CyanogenMod 引用，在编译过中可能会用到的，但不是Android体系内的一部分工具。

**frameworks/**

Android frameworks的核心部件。Android frameworks是Android架构中比较上层的一部分，主要的UI和编程所需要的接口大部分在这一层提供。当然还包括CyanogenMode为Qualcomm、OMA和其他架构所做的优化。

**hardware/**

平台和硬件相关的库文件。

**kernel/**

内核相关的部分，目录的组织结构类似device。

**ndk/**

支持可以用c 编写app，更多细节参考[这里][4]。

**out/**

编译的最终产出物都会放在这个目录。Android ROM相关的在out/target/product/<codename>目录下，包括system.img、recovery.img、boot.img 等。

**packages/**

Android app 的源码文件在packages/apps下。

**prebuilt/**

包括跨平台编译Android的工具链。

**system/**

Linux 底层的系统，包括shell、netd、fastboot等。

**vendor/**

存放从设备中提取的二进制资源文件（<b style="color: #000000;">proprietary blobs</b><span style="color: #000000;"> ）。</span>

 [1]: http://en.wikipedia.org/wiki/C_standard_library
 [2]: http://www.ibm.com/developerworks/cn/opensource/os-cn-android-build/
 [3]: http://source.android.com/
 [4]: http://developer.android.com/tools/sdk/ndk/index.html
