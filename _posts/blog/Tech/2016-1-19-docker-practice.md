---
layout: post
category: "Tech"
title:  "ubuntu 14.04下docker实践"
description: 本文介绍了一个docker小白在ubuntu 14.04下进行docker安装, 配置与使用
tags: [docker ubuntu]
---

###1. docker简介
docker小白, 此处不废话, 更多信息参考如下链接.

<a href="http://dockerpool.com/static/books/docker_practice/introduction/what.html">什么是 Docker</a>

###2. docker安装

#### 2.1 默认安装过程(64bit)
##### 通过Docker源安装最新版本
要安装最新的 Docker 版本，首先需要安装 apt-transport-https 支持，之后通过添加源来安装。
```
$ sudo apt-get install apt-transport-https
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
$ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
$ sudo apt-get update
$ sudo apt-get install lxc-docker
```

注：有个简单脚本可以用于这个过程
```
$ curl -sSL https://get.docker.com/ubuntu/ | sudo sh
```
验证所有的工作都如预期完成了
```
$ sudo docker run -i -t ubuntu /bin/bash
```

#### 2.2 ubuntu 14.04 i386 32bit安装
无奈本人机器装的是32bit ubuntu系统, 上述2.1安装失败.

##### 32bit ubuntu安装Docker
Run build-image.sh to build the docker image 32bit/ubuntu:14.04, or build-image.sh precise to build the docker image 32bit/ubuntu:precise.
<a href="https://github.com/docker-32bit/ubuntu">Build a docker image for ubuntu i386</a>

See also: http://mwhiteley.com/linux-containers/2013/08/31/docker-on-i386.html

###参考
* <a href="http://dockerpool.com/static/books/docker_practice/">Docker —— 从入门到实践</a>
* <a href="http://openwrt.io/docs/build-openwrt-package-using-docker/">使用Docker编译OpenWrt Package</a>
* <a href="http://www.520608.com/ji-yu-dockerbian-yi-openwrt/">基于Docker编译openwrt</a>
* <a href="https://wiki.openwrt.org/doc/howto/docker_openwrt_image">Docker OpenWRT Image</a>