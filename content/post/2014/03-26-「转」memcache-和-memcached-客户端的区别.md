---
date: Wed, 26 Mar 2014 14:29:38 +0000
title: 「转」Memcache 和 Memcached 客户端的区别
author: tomheng
layout: post
permalink: /archives/1782.html
views: 92
categories:
  - LAMP
tags:
  - memcache
  - Memcached
---
# PHP Client Comparison

There are primarily two clients used with PHP. One is the older, more widespread <a href="http://pecl.php.net/package/memcache" rel="nofollow">pecl/memcache</a> and the other is the newer, less used, more feature rich <a href="http://pecl.php.net/package/memcached" rel="nofollow">pecl/memcached</a>.

Both support the basics such as multiple servers, setting vaules, getting values, increment, decrement and getting stats.

Here are some more advanced features and information.

<table border="1" cellpadding="5">
  <tr>
    <th>
    </th>
    
    <th>
      <a href="http://pecl.php.net/package/memcache" rel="nofollow">pecl/memcache</a>
    </th>
    
    <th>
      <a href="http://pecl.php.net/package/memcached" rel="nofollow">pecl/memcached</a>
    </th>
  </tr>
  
  <tr>
    <th align="left">
      First Release Date
    </th>
    
    <td align="center">
      2004-06-08
    </td>
    
    <td align="center">
      2009-01-29 (beta)
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Actively Developed?
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      External Dependency
    </th>
    
    <td align="center">
      None
    </td>
    
    <td align="center">
      <a href="http://tangent.org/552/libmemcached.html" rel="nofollow">libmemcached</a>
    </td>
  </tr>
  
  <tr>
    <td colspan="3" align="center">
      <strong>Features</strong>
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Automatic Key Fixup<sup>1</sup>
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      No
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Append/Prepend
    </th>
    
    <td align="center">
      No
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Automatic Serialzation<sup>2</sup>
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Binary Protocol
    </th>
    
    <td align="center">
      No
    </td>
    
    <td align="center">
      Optional
    </td>
  </tr>
  
  <tr>
    <th align="left">
      CAS
    </th>
    
    <td align="center">
      No
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Compression
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Communication Timeout
    </th>
    
    <td align="center">
      Connect Only
    </td>
    
    <td align="center">
      Various Options
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Consistent Hashing
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Delayed Get
    </th>
    
    <td align="center">
      No
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Multi-Get
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Session Support
    </th>
    
    <td align="center">
      Yes
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Set/Get to a specific server
    </th>
    
    <td align="center">
      No
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
  
  <tr>
    <th align="left">
      Stores Numerics
    </th>
    
    <td align="center">
      Converted to Strings
    </td>
    
    <td align="center">
      Yes
    </td>
  </tr>
</table>

  1. pecl/memcache will convert an invalid key into a valid key for you. pecl/memcached will return false when trying to set/get a key that is not valid.
  2. You do not have to serialize your objects or arrays before sending them to the set commands. Both clients will do this for you.

&#8212;end&#8211;

原文链接：https://code.google.com/p/memcached/wiki/PHPClientComparison

<span style="color: #ff0000;">PS：Memcache会将数字转成字符串存储这个坑刚踩到过～</span>
