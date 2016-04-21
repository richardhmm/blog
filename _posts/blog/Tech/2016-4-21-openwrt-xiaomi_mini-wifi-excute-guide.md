---
layout: post
category: "Tech"
title:  "小米mini路由器刷openwrt下wifi启动流程介绍"
description: 本文介绍了openwrt下wifi启动主要流程。
tags: [openwrt wifi startup mac80211]
---

### 1. 目标  ###
  学习openwrt下wifi模块驱动ok后, 应用如何启动执行wifi功能.

### 2. wifi启动流程  ###
分析对象: <a href="https://github.com/richardhmm/HMMCodeRepository/tree/master/xiaomi-rootfs">xiaomi_mini openwrt wifi shell配置源码  </a>

#### 2.1 boot脚本  ####

~~~
/etc/init.d/boot(/sbin/wifi detect > /tmp/wireless.tmp)
	->/sbin/wifi(include/scan_wifi/wifi_detect)
		->/lib/function.sh(include)
			->/lib/wifi/mac80211.sh(获取全局变量DRIVERS值为"mac80211", 包含shell文件中的函数待后续调用)
		->/sbin/wifi(scan_wifi--config_cb)
~~~

### 参考  ###
* <a href="http://blog.csdn.net/hui523hui523hui523/article/details/38493725">openwrt下wifi设置详细过程  </a>


