---
date: Tue, 14 Apr 2015 02:41:24 +0000
title: '[转]How to write benchmarks in Go'
author: tomheng
layout: post
permalink: /archives/1881.html
views: 60
categories:
  - LAMP
tags:
  - Golang
---
This post continues a series on the testing package I started a few weeks back. You can read the previous article on writing table driven tests here. You can find the code mentioned below in the https://github.com/davecheney/fib repository.

**Introduction**

The Go testing package contains a benchmarking facility that can be used to examine the performance of your Go code. This post explains how to use the testing package to write a simple benchmark.

You should also review the introductory paragraphs of Profiling Go programs, specifically the section on configuring power management on your machine. For better or worse, modern CPUs rely heavily on active thermal management which can add noise to benchmark results.

**Writing a benchmark**

We’ll reuse the Fib function from the previous article.

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw4">func</span> Fib<span class="sy1">(</span>n <span class="kw4">int</span><span class="sy1">)</span> <span class="kw4">int</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> n < <span class="nu0">2</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> n<br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> Fib<span class="sy1">(</span>n<span class="sy3">-</span><span class="nu0">1</span><span class="sy1">)</span> <span class="sy3">+</span> Fib<span class="sy1">(</span>n<span class="sy3">-</span><span class="nu0">2</span><span class="sy1">)</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

Benchmarks are placed inside _test.go files and follow the rules of their Test counterparts. In this first example we’re going to benchmark the speed of computing the 10th number in the Fibonacci series.

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="co1">// from fib_test.go</span><br /> <span class="kw4">func</span> BenchmarkFib10<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// run the Fib function b.N times</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> n <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> n < b<span class="sy3">.</span>N<span class="sy1">;</span> n<span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Fib<span class="sy1">(</span><span class="nu0">10</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

Writing a benchmark is very similar to writing a test as they share the infrastructure from the testing package. Some of the key differences are

Benchmark functions start with Benchmark not Test.  
Benchmark functions are run several times by the testing package. The value of b.N will increase each time until the benchmark runner is satisfied with the stability of the benchmark. This has some important ramifications which we’ll investigate later in this article.  
Each benchmark must execute the code under test b.N times. The for loop in BenchmarkFib10 will be present in every benchmark function.

**Running benchmarks**

Now that we have a benchmark function defined in the tests for the fib package, we can invoke it with go test -bench=.

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="sy0">%</span> go <span class="kw3">test</span> <span class="re5">-bench</span>=.<br /> PASS<br /> BenchmarkFib10 &nbsp; <span class="nu0">5000000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">509</span> ns<span class="sy0">/</span>op<br /> ok &nbsp; &nbsp; &nbsp;github.com<span class="sy0">/</span>davecheney<span class="sy0">/</span>fib &nbsp; &nbsp; &nbsp; 3.084s
        </div>
      </td>
    </tr>
  </table>
</div>

Breaking down the text above, we pass the -bench flag to go test supplying a regular expression matching everything. You must pass a valid regex to -bench, just passing -bench is a syntax error. You can use this property to run a subset of benchmarks.

The first line of the result, PASS, comes from the testing portion of the test driver, asking go test to run your benchmarks does not disable the tests in the package. If you want to skip the tests, you can do so by passing a regex to the -run flag that will not match anything. I usually use

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
          go <span class="kw3">test</span> <span class="re5">-run</span>=XXX <span class="re5">-bench</span>=.
        </div>
      </td>
    </tr>
  </table>
</div>

The second line is the average run time of the function under test for the final value of b.N iterations. In this case, my laptop can execute Fib(10) in 509 nanoseconds. If there were additional Benchmark functions that matched the -bench filter, they would be listed here.

**Benchmarking various inputs**

As the original Fib function is the classic recursive implementation, we’d expect it to exhibit exponential behavior as the input grows. We can explore this by rewriting our benchmark slightly using a pattern that is very common in the Go standard library.

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw4">func</span> benchmarkFib<span class="sy1">(</span><span class="nu2">i</span> <span class="kw4">int</span><span class="sy1">,</span> b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> n <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> n < b<span class="sy3">.</span>N<span class="sy1">;</span> n<span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Fib<span class="sy1">(</span><span class="nu2">i</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span><br /> <span class="sy1">}</span><br /> <br /> <span class="kw4">func</span> BenchmarkFib1<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> &nbsp;<span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">1</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span><br /> <span class="kw4">func</span> BenchmarkFib2<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> &nbsp;<span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">2</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span><br /> <span class="kw4">func</span> BenchmarkFib3<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> &nbsp;<span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">3</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span><br /> <span class="kw4">func</span> BenchmarkFib10<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">10</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span><br /> <span class="kw4">func</span> BenchmarkFib20<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">20</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span><br /> <span class="kw4">func</span> BenchmarkFib40<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span> benchmarkFib<span class="sy1">(</span><span class="nu0">40</span><span class="sy1">,</span> b<span class="sy1">)</span> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

Making benchmarkFib private avoids the testing driver trying to invoke it directly, which would fail as its signature does not match func(*testing.B). Running this new set of benchmarks gives these results on my machine.

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          BenchmarkFib1 &nbsp; <span class="nu0">1000000000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">2.84</span> ns<span class="sy0">/</span>op<br /> BenchmarkFib2 &nbsp; <span class="nu0">500000000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span class="nu0">7.92</span> ns<span class="sy0">/</span>op<br /> BenchmarkFib3 &nbsp; <span class="nu0">100000000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">13.0</span> ns<span class="sy0">/</span>op<br /> BenchmarkFib10 &nbsp; <span class="nu0">5000000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">447</span> ns<span class="sy0">/</span>op<br /> BenchmarkFib20 &nbsp; &nbsp; <span class="nu0">50000</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">55668</span> ns<span class="sy0">/</span>op<br /> BenchmarkFib40 &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">2</span> &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">942888676</span> ns<span class="sy0">/</span>op
        </div>
      </td>
    </tr>
  </table>
</div>

Apart from confirming the exponential behavior of our simplistic Fib function, there are some other things to observe in this benchmark run.

Each benchmark is run for a minimum of 1 second by default. If the second has not elapsed when the Benchmark function returns, the value of b.N is increased in the sequence 1, 2, 5, 10, 20, 50, … and the function run again.  
The final BenchmarkFib40 only ran two times with the average was just under a second for each run. As the testing package uses a simple average (total time to run the benchmark function over b.N) this result is statistically weak. You can increase the minimum benchmark time using the -benchtime flag to produce a more accurate result.

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="sy0">%</span> go <span class="kw3">test</span> <span class="re5">-bench</span>=Fib40 <span class="re5">-benchtime</span>=20s<br /> PASS<br /> BenchmarkFib40 &nbsp; &nbsp; &nbsp; &nbsp;<span class="nu0">50</span> &nbsp; &nbsp; &nbsp; &nbsp; <span class="nu0">944501481</span> ns<span class="sy0">/</span>op
        </div>
      </td>
    </tr>
  </table>
</div>

**Traps for young players**

Above I mentioned the for loop is crucial to the operation of the benchmark driver. Here are two examples of a faulty Fib benchmark.

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw4">func</span> BenchmarkFibWrong<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> n <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> n < b<span class="sy3">.</span>N<span class="sy1">;</span> n<span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Fib<span class="sy1">(</span>n<span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span><br /> <span class="sy1">}</span><br /> <br /> <span class="kw4">func</span> BenchmarkFibWrong2<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; Fib<span class="sy1">(</span>b<span class="sy3">.</span>N<span class="sy1">)</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

On my system BenchmarkFibWrong never completes. This is because the run time of the benchmark will increase as b.N grows, never converging on a stable value. BenchmarkFibWrong2 is similarly affected and never completes.

**A note on compiler optimisations**

Before concluding I wanted to highlight that to be completely accurate, any benchmark should be careful to avoid compiler optimisations eliminating the function under test and artificially lowering the run time of the benchmark.

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">var</span> result <span class="kw4">int</span><br /> <br /> <span class="kw4">func</span> BenchmarkFibComplete<span class="sy1">(</span>b <span class="sy3">*</span>testing<span class="sy3">.</span>B<span class="sy1">)</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">var</span> r <span class="kw4">int</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">for</span> n <span class="sy2">:=</span> <span class="nu0"></span><span class="sy1">;</span> n < b<span class="sy3">.</span>N<span class="sy1">;</span> n<span class="sy2">++</span> <span class="sy1">{</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// always record the result of Fib to prevent</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// the compiler eliminating the function call.</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; r <span class="sy2">=</span> Fib<span class="sy1">(</span><span class="nu0">10</span><span class="sy1">)</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy1">}</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// always store the result to a package level variable</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// so the compiler cannot eliminate the Benchmark itself.</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; result <span class="sy2">=</span> r<br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>

**Conclusion**

The benchmarking facility in Go works well, and is widely accepted as a reliable standard for measuring the performance of Go code. Writing benchmarks in this manner is an excellent way of communicating a performance improvement, or a regression, in a reproducible way.

原文：<http://dave.cheney.net/2013/06/30/how-to-write-benchmarks-in-go>
