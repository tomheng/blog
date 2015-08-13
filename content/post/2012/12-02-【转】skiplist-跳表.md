---
date: Sun, 02 Dec 2012 13:17:59 +0000
title: 【转】SkipList 跳表
author: tomheng
layout: post
permalink: /archives/1582.html
views: 153
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - 算法
---
#### 为什么选择跳表

目前经常使用的平衡数据结构有：B树，红黑树，AVL树，Splay Tree, Treep等。

想象一下，给你一张草稿纸，一只笔，一个编辑器，你能立即实现一颗红黑树，或者AVL树

出来吗？ 很难吧，这需要时间，要考虑很多细节，要参考一堆算法与数据结构之类的书，

还要参考网上的代码，相当麻烦。

用跳表吧，跳表是一种随机化的数据结构，目前开源软件 Redis 和 LevelDB 都有用到它，

它的效率和红黑树以及 AVL 树不相上下，但跳表的原理相当简单，只要你能熟练操作链表，

就能轻松实现一个 SkipList。

#### 有序表的搜索

考虑一个有序表：

<img class="aligncenter size-full wp-image-1584" title="d5d03b36-abff-34ea-9c40-a1fbfb709a81" src="http://blog.webfuns.net/wp-content/uploads/2012/12/d5d03b36-abff-34ea-9c40-a1fbfb709a811.jpg" alt="" width="461" height="52" />

从该有序表中搜索元素 < 23, 43, 59 > ，需要比较的次数分别为 < 2, 4, 6 >，总共比较的次数

为 2 + 4 + 6 = 12 次。有没有优化的算法吗?  链表是有序的，但不能使用二分查找。类似二叉

搜索树，我们把一些节点提取出来，作为索引。得到如下结构：

<img class="aligncenter size-full wp-image-1586" title="7c904c3f-1f39-31af-b8cd-b6de27a94061" src="http://blog.webfuns.net/wp-content/uploads/2012/12/7c904c3f-1f39-31af-b8cd-b6de27a940611.jpg" alt="" width="464" height="108" />

这里我们把 < 14, 34, 50, 72 > 提取出来作为一级索引，这样搜索的时候就可以减少比较次数了。

我们还可以再从一级索引提取一些元素出来，作为二级索引，变成如下结构：

<img class="aligncenter size-full wp-image-1587" title="96983cb0-d60a-31da-953d-2dde4036ea6b" src="http://blog.webfuns.net/wp-content/uploads/2012/12/96983cb0-d60a-31da-953d-2dde4036ea6b.jpg" alt="" width="479" height="192" />

这里元素不多，体现不出优势，如果元素足够多，这种索引结构就能体现出优势来了。

#### 跳表

下面的结构是就是跳表：

其中 -1 表示 INT\_MIN， 链表的最小值，1 表示 INT\_MAX，链表的最大值。

<p style="text-align: center;">
  <img class="aligncenter  wp-image-1588" title="f4c149bd-d8ea-39ff-813f-93d809c90966" src="http://blog.webfuns.net/wp-content/uploads/2012/12/f4c149bd-d8ea-39ff-813f-93d809c90966.jpg" alt="" width="620" height="206" />
</p>

**跳表具有如下性质：**

(1) 由很多层结构组成

(2) 每一层都是一个有序的链表

(3) 最底层(Level 1)的链表包含所有元素

(4) 如果一个元素出现在 Level i 的链表中，则它在 Level i 之下的链表也都会出现。

(5) 每个节点包含两个指针，一个指向同一链表中的下一个元素，一个指向下面一层的元素。

#### 跳表的搜索

<p style="text-align: center;">
  <img class="aligncenter  wp-image-1589" title="ec9fd643-f85c-3072-8634-60cfc88ab334" src="http://blog.webfuns.net/wp-content/uploads/2012/12/ec9fd643-f85c-3072-8634-60cfc88ab334.jpg" alt="" width="622" height="179" />
</p>

&nbsp;

例子：查找元素 117

(1) 比较 21， 比 21 大，往后面找

(2) 比较 37,   比 37大，比链表最大值小，从 37 的下面一层开始找

(3) 比较 71,  比 71 大，比链表最大值小，从 71 的下面一层开始找

(4) 比较 85， 比 85 大，从后面找

(5) 比较 117， 等于 117， 找到了节点。

具体的搜索算法如下

<div class="codecolorer-container c blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />
        </div>
      </td>
      
      <td>
        <div class="c codecolorer">
          <span class="coMULTI">/* 如果存在 x, 返回 x 所在的节点， <br /> &nbsp;* 否则返回 x 的后继节点 */</span> &nbsp;<br /> find<span class="br0">&#40;</span>x<span class="br0">&#41;</span> &nbsp; <br /> <span class="br0">&#123;</span> &nbsp;<br /> &nbsp; &nbsp; p <span class="sy0">=</span> top<span class="sy0">;</span> &nbsp;<br /> &nbsp; &nbsp; <span class="kw1">while</span> <span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> &nbsp;<br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">while</span> <span class="br0">&#40;</span>p<span class="sy0">-></span>next<span class="sy0">-></span>key <span class="sy0"><</span> x<span class="br0">&#41;</span> &nbsp;<br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; p <span class="sy0">=</span> p<span class="sy0">-></span>next<span class="sy0">;</span> &nbsp;<br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span>p<span class="sy0">-></span>down <span class="sy0">==</span> NULL<span class="br0">&#41;</span> &nbsp; <br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> p<span class="sy0">-></span>next<span class="sy0">;</span> &nbsp;<br /> &nbsp; &nbsp; &nbsp; &nbsp; p <span class="sy0">=</span> p<span class="sy0">-></span>down<span class="sy0">;</span> &nbsp;<br /> &nbsp; &nbsp; <span class="br0">&#125;</span> &nbsp;<br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

#### 跳表的插入

先确定该元素要占据的层数 K（采用丢硬币的方式，这完全是随机的）

然后在 Level 1 &#8230; Level K 各个层的链表都插入元素。

<span style="text-align: center;">例子：插入 119， K = 2</span>

<p style="text-align: center;">
  <img class="aligncenter  wp-image-1591" title="bb72be16-6162-3fee-b680-311f25dd7c3a" src="http://blog.webfuns.net/wp-content/uploads/2012/12/bb72be16-6162-3fee-b680-311f25dd7c3a1.jpg" alt="" width="626" height="177" />
</p>

如果 K 大于链表的层数，则要添加新的层。

例子：插入 119， K = 4

<p style="text-align: center;">
  <img class="aligncenter  wp-image-1593" title="6eac083f-45d9-37f9-867f-0d709d9659d3" src="http://blog.webfuns.net/wp-content/uploads/2012/12/6eac083f-45d9-37f9-867f-0d709d9659d31.jpg" alt="" width="625" height="194" />
</p>

#### 丢硬币决定 K

插入元素的时候，元素所占有的层数完全是随机的，通过一下随机算法产生：

<div class="codecolorer-container c blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />
        </div>
      </td>
      
      <td>
        <div class="c codecolorer">
          <span class="kw4">int</span> random_level<span class="br0">&#40;</span><span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> K <span class="sy0">=</span> <span class="nu0">1</span><span class="sy0">;</span> <br /> <br /> <span class="kw1">while</span> <span class="br0">&#40;</span>random<span class="br0">&#40;</span><span class="nu0"></span><span class="sy0">,</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="br0">&#41;</span><br /> K<span class="sy0">++;</span><br /> <br /> <span class="kw1">return</span> K<span class="sy0">;</span><br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

相当与做一次丢硬币的实验，如果遇到正面，继续丢，遇到反面，则停止，  
用实验中丢硬币的次数 K 作为元素占有的层数。显然随机变量 K 满足参数为 p = 1/2 的几何分布，  
K 的期望值 E[K] = 1/p = 2. 就是说，各个元素的层数，期望值是 2 层。

#### 跳表的高度

n 个元素的跳表，每个元素插入的时候都要做一次实验，用来决定元素占据的层数 K，  
跳表的高度等于这 n 次实验中产生的最大 K，待续。。。

#### 跳表的空间复杂度分析

根据上面的分析，每个元素的期望高度为 2， 一个大小为 n 的跳表，其节点数目的  
期望值是 2n。

#### 跳表的删除

<p style="text-align: left;">
  在各个层中找到包含 x 的节点，使用标准的 delete from list 方法删除该节点。<br /> 例子：删除 71<br /> <img class="aligncenter  wp-image-1596" title="7bab9ad1-9f5a-37d0-bc38-89ee50d1bc0d" src="http://blog.webfuns.net/wp-content/uploads/2012/12/7bab9ad1-9f5a-37d0-bc38-89ee50d1bc0d2.jpg" alt="" width="625" height="177" />
</p>

<p style="text-align: left;">
  原文：<a href="http://kenby.iteye.com/blog/1187303">http://kenby.iteye.com/blog/1187303</a>
</p>

<p style="text-align: left;">
  &#8212;&#8212;
</p>

<p style="text-align: left;">
  比较典型的用空间换取时间的优化算法。
</p>
