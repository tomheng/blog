---
date: Fri, 13 Aug 2010 02:27:53 +0000
title: Linux 设置网卡（IP 网关 DNS）信息命令
author: tomheng
layout: post
permalink: /archives/756.html
robotsmeta:
  - index,follow
views: 951
categories:
  - LAMP
tags:
  - Linux
---
**<span style="color: #ff0000;">环境：vmware  cent0s 5.5</span>**

一、**命令行方式**

**·**）设置IP

<span style="color: #3366ff;">ifconfig </span>eth0  192.168.0.99  <span style="color: #3366ff;">netmask</span> 255.255.255.0  <span style="color: #3366ff;">up</span>

<span style="color: #000000;">注释：eth0 网卡名称</span>

<span style="color: #000000;">192.168.0.99  ip地址</span>

<span style="color: #000000;">255.255.255.0 子网掩码</span>

·）设置网关

<span style="color: #3366ff;">route add default gw</span> 192.168.01

PS：<span style="color: #ff0000;">通过以上命令设置的信息，可以即时生效，但是重启系统之后失效</span>

·)设置DNS域名服务器

echo &#8220;nameserver 8.8.8.8 &#8220;>> /etc/resolv.conf

二、**直接修改配置文件**

·）配置IP的文件是<span style="color: #0000ff;">/etc/sysconfig/network-scripts/ifcfg-eth0</span>

IPADDR=192.168.0.66

NETMASK=255.255.255.0

·）配置网关的文件是 <span style="color: #0000ff;">/etc/sysconfig/network</span>

GATEWAY=192.168.0.2

PS:<span style="color: #ff0000;">重新启动网络配置</span>

<span style="color: #0000ff;">/etc/init.d/network restart</span>
