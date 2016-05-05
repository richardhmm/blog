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
待更新

### 参考  ###

