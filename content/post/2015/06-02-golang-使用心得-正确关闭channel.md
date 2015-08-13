---
date: Tue, 02 Jun 2015 13:48:36 +0000
title: Golang 使用心得-正确关闭channel
author: tomheng
layout: post
permalink: /archives/1927.html
views: 84
categories:
  - LAMP
tags:
  - Golang
---
相信很多同学都是因为Golang有相当简单的并发模型才转过来的。Golang的并发模型主要由三部分构成：Goroutine、Channel和Select。

其中Channel是Goroutine之间通信的桥梁，简单优雅的解决了并行开发中的同步问题。

但是初学者（比如我）通常会遇到** send on closed channel**的错误，这是因为我们向一个已经关闭channle推送数据造成的。通常这个错误是发生在生成消费模型中。channel用来传送需要执行的任务，channel一端是生产者，另一端是消费者。

如下代码：

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">package</span> main<br /> <br /> <span class="kw1">import</span> <span class="sy1">(</span><br /> &nbsp; &nbsp; <span class="st0">"fmt"</span><br /> &nbsp; &nbsp; <span class="st0">"sync"</span><br /> <span class="sy1">)</span><br /> <br /> <span class="kw4">func</span> main<span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; jobs <span class="sy2">:=</span> <span class="kw3">make</span><span class="sy1">(</span><span class="kw4">chan</span> <span class="kw4">int</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">var</span> wg sync<span class="sy3">.</span>WaitGroup<br /> &nbsp; &nbsp; wg<span class="sy3">.</span>Add<span class="sy1">(</span><span class="nu0">10</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> <span class="nu2">i</span> < <span class="nu0">10</span><span class="sy1">;</span> <span class="nu2">i</span><span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; jobs <<span class="sy3">-</span> <span class="nu2">i</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"produce:"</span><span class="sy1">,</span> <span class="nu2">i</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="kw1">range</span> jobs <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"consume:"</span><span class="sy1">,</span> i<span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; wg<span class="sy3">.</span><span class="me1">Done</span><span class="sy1">()</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; wg<span class="sy3">.</span><span class="me1">Wait</span><span class="sy1">()</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

注意上面的代码我们没有主动关闭channel，因为我一开始就知道执行的任务数，所以直接等待任务完成即可，没有必要主动关闭channel。

假设我们的场景中，生产者没有生成数量的限制，只有一个时间限制。那么代码演变成下面这样：

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />26<br />27<br />28<br />29<br />30<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">package</span> main<br /> <br /> <span class="kw1">import</span> <span class="sy1">(</span><br /> &nbsp; &nbsp; <span class="st0">"fmt"</span><br /> &nbsp; &nbsp; <span class="st0">"sync"</span><br /> &nbsp; &nbsp; <span class="st0">"time"</span><br /> <span class="sy1">)</span><br /> <br /> <span class="kw4">func</span> main<span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; jobs <span class="sy2">:=</span> <span class="kw3">make</span><span class="sy1">(</span><span class="kw4">chan</span> <span class="kw4">int</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">var</span> wg sync<span class="sy3">.</span>WaitGroup<br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; time<span class="sy3">.</span>Sleep<span class="sy1">(</span>time<span class="sy3">.</span>Second <span class="sy3">*</span> <span class="nu0">3</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">close</span><span class="sy1">(</span>jobs<span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> <span class="sy1">;</span> <span class="nu2">i</span><span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; jobs <<span class="sy3">-</span> <span class="nu2">i</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"produce:"</span><span class="sy1">,</span> <span class="nu2">i</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; wg<span class="sy3">.</span>Add<span class="sy1">(</span><span class="nu0">1</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">defer</span> wg<span class="sy3">.</span>Done<span class="sy1">()</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="kw1">range</span> jobs <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"consume:"</span><span class="sy1">,</span> i<span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; wg<span class="sy3">.</span><span class="me1">Wait</span><span class="sy1">()</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这时程序就会因为**send on closed channel**而崩溃。那么首先想到的解决方案可能是在放入jobs的时候检查jobs是否已关闭。幸好Golang有办法来检测channel是否被关闭，但是非常不好用。Golang中可以用comma ok的方式检测channel是否关闭，如下：

i, ok := <- jobs 如果channel关闭那么ok返回的是false，但是这样的话，如果jobs没有关闭，那么就会漏掉一个job。当然你可以想办好hack掉漏掉的这个job。但是这样的代码不是很优雅，也是那么直观合理。 在实践中我是用的如下代码处理这个问题。 

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />26<br />27<br />28<br />29<br />30<br />31<br />32<br />33<br />34<br />35<br />36<br />37<br />38<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">package</span> main<br /> <br /> <span class="kw1">import</span> <span class="sy1">(</span><br /> &nbsp; &nbsp; <span class="st0">"fmt"</span><br /> &nbsp; &nbsp; <span class="st0">"sync"</span><br /> &nbsp; &nbsp; <span class="st0">"time"</span><br /> <span class="sy1">)</span><br /> <br /> <span class="kw4">func</span> main<span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; jobs <span class="sy2">:=</span> <span class="kw3">make</span><span class="sy1">(</span><span class="kw4">chan</span> <span class="kw4">int</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">var</span> wg sync<span class="sy3">.</span>WaitGroup<br /> &nbsp; &nbsp; timeout <span class="sy2">:=</span> <span class="kw3">make</span><span class="sy1">(</span><span class="kw4">chan</span> <span class="kw4">bool</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; time<span class="sy3">.</span>Sleep<span class="sy1">(</span>time<span class="sy3">.</span>Second <span class="sy3">*</span> <span class="nu0">3</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; timeout <<span class="sy3">-</span> <span class="kw2">true</span><br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> <span class="sy1">;</span> <span class="nu2">i</span><span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">select</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">case</span> <<span class="sy3">-</span>timeout<span class="sy1">:</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">close</span><span class="sy1">(</span>jobs<span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">default</span><span class="sy1">:</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; jobs <<span class="sy3">-</span> <span class="nu2">i</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"produce:"</span><span class="sy1">,</span> <span class="nu2">i</span><span class="sy1">)</span><br /> <br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; wg<span class="sy3">.</span>Add<span class="sy1">(</span><span class="nu0">1</span><span class="sy1">)</span><br /> &nbsp; &nbsp; <span class="kw1">go</span> <span class="kw4">func</span><span class="sy1">()</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">defer</span> wg<span class="sy3">.</span>Done<span class="sy1">()</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> <span class="nu2">i</span> <span class="sy2">:=</span> <span class="kw1">range</span> jobs <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"consume:"</span><span class="sy1">,</span> i<span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span> &nbsp; <br /> &nbsp; &nbsp; <span class="sy1">}()</span> <br /> &nbsp; &nbsp; wg<span class="sy3">.</span><span class="me1">Wait</span><span class="sy1">()</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

实际情况可能还要复杂很多，比如会有多个生产者，这个时候代码又该如何处理呢？大家可以想一想。。。
