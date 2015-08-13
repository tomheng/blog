---
date: Thu, 30 Apr 2015 14:46:43 +0000
title: '[转]Go Data Structures'
author: tomheng
layout: post
permalink: /archives/1912.html
views: 44
categories:
  - 未分类
tags:
  - Golang
---
<p class="lp">
  When explaining Go to new programmers, I&#8217;ve found that it often helps to explain what Go values look like in memory, to build the right intuition about which operations are expensive and which are not. This post is about basic types, structs, arrays, and slices.
</p>

## Basic types

Let&#8217;s start with some simple examples:

<p class="pp">
  <img class="aligncenter size-full wp-image-1913" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata1.png" alt="godata1" width="409" height="303" />
</p>

<p class="lp">
  The variable <em>i</em> has type <em>int</em>, represented in memory as a single 32-bit word. (All these pictures show a 32-bit memory layout; in the current implementations, only the pointer gets bigger on a 64-bit machine—<em>int</em> is still 32 bits—though an implementation could choose to use 64 bits instead.)
</p>

<p class="pp">
  The variable <em>j</em> has type <em>int32</em>, because of the explicit conversion. Even though <em>i</em> and <em>j</em> have the same memory layout, they have different types: the assignment <em>i = j</em> is a type error and must be written with an explicit conversion: <em>i = int(j)</em>.
</p>

<p class="pp">
  The variable <em>f</em> has type <em>float</em>, which the current implementations represent as a 32-bit floating-point value. It has the same memory footprint as the <em>int32</em> but a different internal layout.
</p>

## Structs and pointers

<p class="pp">
  Now things start to pick up. The variable <em>bytes</em> has type <em>[5]byte</em>, an array of 5 <em>byte</em>s. Its memory representation is just those 5 bytes, one after the other, like a C array. Similarly, <em>primes</em>is an array of 4 <em>int</em>s.
</p>

<p class="pp">
  Go, like C but unlike Java, gives the programmer control over what is and is not a pointer. For example, this type definition:
</p>

<pre class="indent">type Point struct { X, Y int }
</pre>

<p class="lp">
  defines a simple struct type named <em>Point</em>, represented as two adjacent <em>int</em>s in memory.
</p>

<p class="lp">
  <img class="aligncenter size-full wp-image-1914" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata1a.png" alt="godata1a" width="285" height="151" />
</p>

<p class="lp">
  The <a href="http://golang.org/doc/go_spec.html#Composite_literals">composite literal syntax</a> <em>Point{10, 20}</em> denotes an initialized <em>Point</em>. Taking the address of a composite literal denotes a pointer to a freshly allocated and initialized <em>Point</em>. The former is two words in memory; the latter is a pointer to two words in memory.
</p>

<p class="pp">
  Fields in a struct are laid out side by side in memory.
</p>

<pre class="indent">type Rect1 struct { Min, Max Point }
type Rect2 struct { Min, Max *Point }</pre>

<img class="aligncenter size-full wp-image-1915" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata1b.png" alt="godata1b" width="443" height="151" />

<p class="lp">
  <em>Rect1</em>, a struct with two <em>Point</em> fields, is represented by two <em>Point</em>s—four ints—in a row. <em>Rect2</em>, a struct with two <em>*Point</em> fields, is represented by two <em>*Point</em>s.
</p>

<p class="pp">
  Programmers who have used C probably won&#8217;t be surprised by the distinction between <em>Point</em> fields and <em>*Point</em> fields, while programmers who have only used Java or Python (or &#8230;) may be surprised by having to make the decision. By giving the programmer control over basic memory layout, Go provides the ability to control the total size of a given collection of data structures, the number of allocations, and the memory access patterns, all of which are important for building systems that perform well.
</p>

## Strings

<p class="pp">
  With those preliminaries, we can move on to more interesting data types.
</p>

<p class="pp">
  <img class="aligncenter size-full wp-image-1916" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata2.png" alt="godata2" width="264" height="151" />
</p>

<p class="lp">
  (The gray arrows denote pointers that are present in the implementation but not directly visible in programs.)
</p>

<p class="pp">
  A <em>string</em> is represented in memory as a 2-word structure containing a pointer to the string data and a length. Because the <em>string</em> is immutable, it is safe for multiple strings to share the same storage, so <a href="http://www.blogger.com/post-edit.g?blogID=8082954141980125536&postID=65253524121904390">slicing</a> <em>s</em> results in a new 2-word structure with a potentially different pointer and length that still refers to the same byte sequence. This means that slicing can be done without allocation or copying, making string slices as efficient as passing around explicit indexes.
</p>

<p class="pp">
  (As an aside, there is a <a href="http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4513622">well-known gotcha</a> in Java and other languages that when you slice a string to save a small piece, the reference to the original keeps the entire original string in memory even though only a small amount is still needed. Go has this gotcha too. The alternative, which we tried <a href="http://code.google.com/p/go/source/detail?r=70fa38e5a5bb">and rejected</a>, is to make string slicing so expensive—an allocation and a copy—that most programs avoid it.)
</p>

## Slices

<img class="aligncenter size-full wp-image-1917" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata3.png" alt="godata3" width="470" height="151" />

<p class="pp">
  A <a href="http://golang.org/doc/effective_go.html#slices">slice</a> is a reference to a section of an array. In memory, it is a 3-word structure contaning a pointer to the first element, the length of the slice, and the capacity. The length is the upper bound for indexing operations like <em>x[i]</em> while the capacity is the upper bound for slice operations like <em>x[i:j]</em>.
</p>

<p class="pp">
  Like slicing a string, slicing an array does not make a copy: it only creates a new structure holding a different pointer, length, and capacity. In the example, evaluating the composite literal<em>[]int{2, 3, 5, 7, 11}</em> creates a new array containing the five values and then sets the fields of the slice <em>x</em> to describe that array. The slice expression <em>x[1:3]</em> does not allocate more data: it just writes the fields of a new slice structure to refer to the same backing store. In the example, the length is 2—<em>y[0]</em> and <em>y[1]</em> are the only valid indexes—but the capacity is 4—<em>y[0:4]</em> is a valid slice expression. (See <a href="http://golang.org/doc/effective_go.html#slices">Effective Go</a> for more about length and capacity and how slices are used.)
</p>

Because slices are multiword structures, not pointers, the slicing operation does not need to allocate memory, not even for the slice header, which can usually be kept on the stack. This representation makes slices about as cheap to use as passing around explicit pointer and length pairs in C. Go originally represented a slice as a pointer to the structure shown above, but doing so meant that every slice operation allocated a new memory object. Even with a fast allocator, that creates a lot of unnecessary work for the garbage collector, and we found that, as was the case with strings above, programs avoided slicing operations in favor of passing explicit indices. Removing the indirection and the allocation made slices cheap enough to avoid passing explicit indices in most cases.

## New and Make

<p class="pp">
  Go has two data structure creation functions: <em>new</em> and <em>make</em>. The distinction is a common early point of confusion but seems to quickly become natural. The basic distinction is that<em>new(T)</em> returns a <em>*T</em>, a pointer that Go programs can dereference implicitly (the black pointers in the diagrams), while <em>make(T, </em><i>args</i><em>)</em> returns an ordinary <em>T</em>, not a pointer. Often that <em>T</em> has inside it some implicit pointers (the gray pointers in the diagrams). <em>New</em> returns a pointer to zeroed memory, while <em>make</em> returns a complex structure.
</p>

<p class="pp">
  <img class="aligncenter size-full wp-image-1918" src="http://blog.webfuns.net/wp-content/uploads/2015/04/godata4.png" alt="godata4" width="470" height="627" />
</p>

There is a way to unify these two, but it would be a significant break from the C and C++ tradition: define *make(*T)* to return a pointer to a newly allocated *T*, so that the current*new(Point)* would be written *make(*Point)*. We tried this for a few days but decided it was too different from what people expected of an allocation function.

## Coming soon&#8230;

<p class="pp">
  This has already gotten a bit long. Interface values, maps, and channels will have to wait for future posts.
</p>

<p class="pp">
  原文：http://research.swtch.com/godata
</p>
