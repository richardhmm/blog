---
layout: post
category: "Tech"
title:  "openwrt下ddr由512MB升级为1GB时出现了一个小问题"
description: 本文介绍了openwrt下ddr由512MB升级为1GB时出现了一个小问题, 并最终解决的记录。
tags: [openwrt ddr problem]
---

### 1. 问题  ###
 由于项目需求, 要将一块单板ddr由512MB升级为1GB, 系统采用openwrt. 配置OK之后, 系统启动之后, 发现free命令下内存大小仅为600多M; 512MB物理内存时, free命令下查看有490多M.为什么呢, 该如何解决呢?困扰许久...

### 2. 解决  ###
将linux内核配置Memory split由VMSPLIT_3G改为VMSPLIT_2G, 问题得以解决.

~~~
Location:  
  │     -> Kernel Features              
  │       -> Memory split (<choice> [=y]) 
~~~
  
### 3. 分析  ###
CONFIG_VMSPLIT is used to set the physical address of the RAM, so you have to use the "right" setting. That might be a kernel "bug" that could be fixed.
<a href="https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=142852">raspberrypi forums</a>


<a href="https://github.com/raspberrypi/linux/issues/1394">raspberrypi github issues</a>
~~~
With a 3G/1G split, the kernel virtual addresses can only cover 768MB (lowmem) because address space is needed for other areas such as the 240MB vmalloc area:

[    0.000000] Memory: 760276K/786432K available (6478K kernel code, 436K rwdata, 1732K rodata, 476K init, 775K bss, 17964K reserved, 8192K cma-reserved)
[    0.000000] Virtual kernel memory layout:
[    0.000000]     vector  : 0xffff0000 - 0xffff1000   (   4 kB)
[    0.000000]     fixmap  : 0xffc00000 - 0xfff00000   (3072 kB)
[    0.000000]     vmalloc : 0xf0800000 - 0xff800000   ( 240 MB)
[    0.000000]     lowmem  : 0xc0000000 - 0xf0000000   ( 768 MB)
[    0.000000]     modules : 0xbf000000 - 0xc0000000   (  16 MB)
[    0.000000]       .text : 0xc0008000 - 0xc080cd64   (8212 kB)
[    0.000000]       .init : 0xc080d000 - 0xc0884000   ( 476 kB)
[    0.000000]       .data : 0xc0884000 - 0xc08f10e8   ( 437 kB)
[    0.000000]        .bss : 0xc08f4000 - 0xc09b5c24   ( 776 kB)
On Pi2 and Pi3 the top 16MB of RAM is not addressable by the ARM because the peripherals are mapped there, leaving 240MB of RAM that is physically addressable but can't be used without temporarily mapping it into the vmalloc or other area; this is what enabling highmem does. You could shrink the vmalloc area (and others), but the problem will remain to some extent.

The reason the kernel doesn't boot with a 3G/1G split is down to the placement of the initial Device Tree blob. It used to be located at 0x100, but this limits it to (0x4000 - 0x100 = 0x3f00) bytes (about 16KB), and some users have already exceeded this limit with sizeable (but reasonable) overlays. One of the first things the kernel does is "unflatten" the DTB, building internal data structures that are more convenient to work with. After this point, the source memory is free to be used for something else. I changed the firmware logic to place the DTB at the top of memory (just below the space reserved for the GPU and the initrd/initramfs), which works well with the 2G/2G split but can be too high for 3G/1G - the kernel faults trying to copy it.

The next (or next but one) firmware release will place a 768MB cap on the initial DTB (and initrd/initramfs) placement, avoiding this booting problem. Until then you achieve the same result by setting gpu_mem=240 (or higher), although this reduces the total memory usable by the ARM.
~~~

### 参考  ###
* <a href="https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=142852">raspberrypi forums</a>
* <a href="https://github.com/raspberrypi/linux/issues/1394">raspberrypi github issues</a>
