---
layout: post
category: "Tech"
title:  "关于linux内存使用分析"
description: 本文介绍了linux内存使用分析的2篇文章，加深了个人对linux内存及进程内存分配的理解。
tags: [tech product plan]
---

### 1. 问题  ###
测试同事对linux系统内存used情况统计时，将cached内存也算进里面，认为应用进程占用的内存包括了cached内存（进程会读写硬盘产生了cached内存），对此计算方法我表示质疑，但却无法提供有力证据进行说明，于是心中一直耿耿于怀，审视不爽。昨日，一不小心找到了2篇文章，充分证明了我的观点，下面简单介绍一下。

### 2. Linux Used内存到底哪里去了？  ###
淘宝褚霸的<a href="http://blog.yufeng.info/archives/2456">Linux Used内存到底哪里去了？</a>。  
主要讲到了：内存主要消耗在三个方面：1. 进程消耗。 2. slab消耗 3.pagetable消耗。

### 3. 也看linux内存去哪儿了  ###
一个linux运维人员的<a href="http://www.361way.com/whereis-the-memory/4036.html">也看linux内存去哪儿了</a>。  
从实际案例中，重新分析了下内存消耗情况，特别是对进程消耗有了进一步说明，slab和pagetable也扩展说明了一下。

