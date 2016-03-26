---
layout: post
category: "Tech"
title:  "banana pi(BPI-M1)下openwrt开启lighttpd+php并搭建dokuwiki"
description: 本文介绍了在banana pi(BPI-M1)下openwrt新增lighttpd+php并搭建dokuwiki过程
tags: [banana pi BPI-M1 openwrt lighttpd+php dokuwiki]
---

### 1. 目标  ###
  在香蕉派openwrt中新增lighttpd服务器, 并开启php服务, 在此环境下搭建个人dokuwiki.

### 2. 实现过程 ###

#### 2.1 搭建web服务器lighttpd ####

#### 2.2 增加PHP服务 ####
Utilities  ---> database  ---> <*> mysql-server
Utilities  ---> Filesystem  ---> <*> nfs-utils
Utilities  ---> Filesystem  ---> <*> ntfs-3g
Utilities  ---> disc  ---> <*> fdisk
### 参考  ###
* <a href="http://blog.csdn.net/whfyzg/article/details/47125273">openwrt 使用 android 手机usb tether联网 </a>
