---
date: Wed, 10 Jun 2015 10:25:02 +0000
title: '[转]How does the Go runtime work?'
author: tomheng
layout: post
permalink: /archives/1931.html
views: 82
categories:
  - LAMP
tags:
  - Golang
---
(This answer is for the Go compiler from golang.org. It is not about gccgo.)

The Go runtime is a Go package, which appears (in the documentation, the build process, the things it exports) to be like any other Go package. It is written in a combination of Go, C, and assembly. In order for it to do all the low level things it needs to do, there are special adaptations (hacks?) in the build system and in the compiler that are activated when compiling it. The runtime has architecture- and OS-specific things in it, which are compiled in according the the target platform.

At linking time, the linker creates a statically linked ELF (or PE or MachO) file which includes the runtime, your program, and every package that your program references. The starting address in the ELF header points into the runtime. It arranges for it&#8217;s own data structures to be initialized, then calls the initializers of all the packages (ordering of init is probably figured out at link time by the linker). It then transfers control to main.main, and your program is running. (There are situations where Go creates dynamically linked executables, but the majority of the code is still statically linked.)

When your program does things that could cause a crash, like a cast, or accessing an item in an array, the compiler emits calls into the runtime, which checks the data type with run time type info, or checks the bounds of the array. Likewise for memory allocation and for creating new goroutines, the runtime gets control. The runtime has a user-space scheduler in it, which implements cooperative multitasking; if a goroutine goes into a tight loop without calling any routines that would block (thereby giving the scheduler control) it can starve all the other goroutines. The runtime spawns a new system thread when needed to prevent the system from blocking on system calls. There can be fewer system threads in a Go system than the number of goroutines that are active.

The final aspect of the Go runtime that is very interesting is the per-goroutine stack. The runtime, together with the linker, arranges for the hardware stack to be non-contiguous, able to grow and shrink according to demand. As the stack shrinks after growing, it is freed and is available to be realloced as other types of objects by the memory allocator. This allows Go stacks to start very small (8 k), meaning that a Go program can launch hundreds of thousands of Goroutines without running out of address space. (The stack is becoming continuous in Go 1.5 but it can still be reallocated and moved when it runs out.)

When programming Go, the runtime is not in the front of your mind. You interact with the system library, and the runtime supports your code more or less silently. This is why the majority of information you&#8217;ll see about Go is how to use the libraries and how to use the channels to implement concurrent programming, and little about the runtime itself.

原文：http://www.quora.com/How-does-the-Go-runtime-work
