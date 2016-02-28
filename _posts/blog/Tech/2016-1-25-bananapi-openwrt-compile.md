---
layout: post
category: "Tech"
title:  "banana pi(BPI-M1) openwrt系统的编译"
description: 本文介绍了在banana pi(BPI-M1) openwrt系统的编译过程
tags: [banana pi BPI-M1 openwrt 编译]
---

###1. banana pi简介
    朋友搞过开源硬件, 送我一块banana pi BPI-M1. 笔者对openwrt系统很感兴趣, 且这个单板机功能蛮强大的, 可以刷openwrt, 性能比一般的路由器强大不少, 尝试了官方的openwrt版本, 感觉不错, 想尝试自己定制下openwrt系统增加更多的功能. 

    另外有人拿banana pi Archlinux系统中安装了docker, 感觉相当不错, 后续有时间也想玩玩.
    
<a href="http://dockerpool.com/static/books/docker_practice/introduction/what.html">什么是 Docker</a>

<a href="http://www.finklabs.org/articles/using-docker-on-banana-pi.html">Using Docker on Banana Pi</a>

###2. BPI-M1 openwrt系统的编译

#### 2.1 编译环境
ubuntu14.04 32bit

#### 2.2 安装openwrt编译环境依赖包

    apt-get install subversion build-essential libncurses5-dev zlib1g-dev gawk git ccache gettext libssl-dev xsltproc unzip subversion file

#### 2.3 获取源码

    git clone https://github.com/BPI-SINOVOIP/BPI-OpenWRT.git
    cd BPI-OpenWRT
    ./scripts/feeds update -a # 更新软件包
    ./scripts/feeds install -a    # 安装软件包

#### 2.4 定制

    make menuconfig
    

#### 2.5 编译

    make V=99
    



###参考
* <a href="http://www.lichanglin.cn/Bananapi%E7%B3%BB%E5%88%97%20openwrt%E7%B3%BB%E7%BB%9F%E7%BC%96%E8%AF%91%E6%95%99%E7%A8%8B/">Bananapi系列 openwrt系统编译教程</a>
* <a href="http://www.lizhaozhong.info/archives/1230">从0构建一个基于BananaPi的OpenWrt系统</a>
* <a href="http://www.finklabs.org/articles/using-docker-on-banana-pi.html">Using Docker on Banana Pi</a>
