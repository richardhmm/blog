---
layout: post
category: "Tech"
title:  "近期技术疑难问题小结"
description: 本文介绍了openwrt系统下一些技术疑难问题小结。
tags: [openwrt 问题 小结 overlayfs vlan 敏捷开发]
---

### 0.背景 ### 
最近很忙很累，一方面参与应用敏捷开发，一方面还要处理系统稳定性问题，但收获也不少，现在做个简单的备忘录。

### 1.系统  ### 
1、overlayfs是个好东东。

以前做嵌入式文件系统时，nand rootfs有用过cramfs+ubifs，其中cramfs只读压缩rootfs，ubifs分区通常挂载在/opt/下，这样/opt/下可以存放定制应用，但假如系统库也有更新就只能重刷cramfs了，有点点麻烦。。。

但采用squashfs+jffs2/ubifs，其中squashfs只读rootfs挂载到／ROM，而jffs2 or ubifs or ext4 or other挂载到／overlay，将ROM和overlay通过overlayfs方法联系起来。

优点：方便恢复出厂设置，格式化overlay分区即可；避免重复掉电或异常掉电导致overlay分区挂载异常无法进入系统，此时可以进入squashfs只读rootfs，方便维护升级。

分析与实现过程：

~~~
1）确认内核是否支持overlayfs（cat /proc/filesystems，查看是否有overlayfs），假如没有找到内核版本对应的overlayfs补丁，打补丁。
2）确认是否有支持overlayfs的实现启动代码或脚本，默认openwrt spi flash支持squashfs+jffs2，假如nand flash需要自行添加支持脚本。方法见下述参考网址。
~~~

核心过程如下：

~~~
mount -n -t ext4 /dev/mtdblock3 -o rw,noatime /overlay
mount -n -t overlayfs overlayfs:/overlay -o rw,noatime,lowerdir=/,upperdir=/overlay /mnt
mount -n /proc -o noatime,--move /mnt/proc
pivot_root /mnt /mnt/rom 
mount -n /rom/dev -o noatime,--move /dev
mount -n /rom/tmp -o noatime,--move /tmp
mount -n /rom/sys -o noatime,--move /sys
mount -n /rom/overlay -o noatime,--move /overlay
~~~

参考：
<a href="http://blog.chinaunix.net/uid-27057175-id-4584360.html">  openwrt overlayfs 实现脚本</a>

<a href="http://blog.chinaunix.net/uid-27057175-id-4584305.html">   openwrt overlayfs 2.6.36内核补丁</a>

2、MT7621内部SWITCH有2个千兆RGMII总线，与CPU可以提供2个网卡eth0／eth1。

~~~
做2WAN口时，按照默认设置P6 P1-P3作为VLAN 1-->配置为eth0.1 LAN；
P6 P0作为VLAN 2-->配置为eth0.2 WAN；
P5 P4作为VLAN 3-->配置为eth1 WAN1；这样可以最大利用其总线能力。
比“设置P6 P1-P3作为VLAN 1-->配置为eth0.1 LAN；
P6 P0作为VLAN 2-->配置为eth0.2 WAN；
P6 P4作为VLAN 3-->配置为eth0.3 WAN1；”这种方法更好。哦也。。。
~~~

3、aufs也是个不错东东。
有的linux内核无法支持overlayfs，那么请换用aufs试试吧，原理相似，殊途同归，一样可以实现透明挂载。

### 2.应用 ### 

~~~
1、通信协议中，建议统一使用unsigned char(u8)，避免使用char(s8)，比较时主要U8和S8比较会出错。
2、TCP客户端必须要考虑断线重连机制。
3、持续奔跑的线程需要自己线程看门狗。
4、守护进程需要增加进程看门狗，方便起见使用shell脚本实现即可。
5、TCP服务器必须考虑心跳机制，简单可以开启协议本身的心跳机制，复杂一点可以自定义报文作为心跳包。
6、涉及HTML等文件字符编码显示时，需特别小心有些编辑器如source insight只支持ANSI不支持UTF-8，容易出现前端展示乱码。
7、注意使用消息队列来实现同步操作时，并发很大时如1000以上，可能会出现响应不及时。
8、基于libpcap抓包时，注意需求是要统计IP报数据量还是MAC帧数据量，默认抓的是IP报数据，将其总长度加起来就是IP报流量，对每包加上MAC层帧头帧尾就是传输过程的MAC帧数据流量。
~~~

### 3.敏捷开发的一点点体会 ### 

~~~
1、响应需求变化快，强调输出可使用软件。对需求不太明确但又需要尽快输出结果的开发，建议1-2月为一个迭代周期不断开发。
2、敏捷开发对人员要求很高，大家都在相同的水平上最好。
3、流程分析：
1）产品经理或架构师输出需求并讲解需求
2）需求分派
3）需求理解交流（主要是架构师和开发人员一起输出交互流程评估技术瓶颈），测试理解需求
4）需求（用户故事）分解任务并将用户故事上面板(任务分解工作量不大不小1-3天)，测试输出方案
5）全力开发
6）部分故事完成提交测试
7）集成开发与调试
8）完成所有开发正式提交测试
9）测试并解决bug
10）版本发布
11）交流总结。
~~~

