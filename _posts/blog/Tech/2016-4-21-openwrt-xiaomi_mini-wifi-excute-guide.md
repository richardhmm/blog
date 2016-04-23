---
layout: post
category: "Tech"
title:  "小米路由器mini版刷openwrt下wifi启动流程分析介绍"
description: 本文介绍了openwrt下wifi启动主要流程。
tags: [openwrt wifi startup mac80211]
---

### 1. 目标  ###
  学习openwrt下wifi模块驱动ok后, 应用如何启动执行wifi功能.

### 2. wifi启动流程  ###
分析对象: <a href="https://github.com/richardhmm/HMMCodeRepository/tree/master/xiaomi-rootfs">xiaomi_mini openwrt wifi shell配置源码  </a>

#### 2.1 boot脚本  ####
主要作用自动生成或更新wireless配置脚本(实际产品应用中, 尽量不用此步骤, 避免产生配置丢失问题).

~~~
执行流程分析:
0 /etc/init.d/boot(/sbin/wifi detect > /tmp/wireless.tmp)
1 /sbin/wifi(include/scan_wifi/wifi_detect)
1.1 /lib/function.sh(include)
1.1.1 /lib/wifi/mac80211.sh(获取全局变量DRIVERS值为"mac80211", 
    包含shell文件中的函数待后续调用)
1.2 /sbin/wifi(scan_wifi--config_cb, 其中config_cb为config_load的
    section回调函数, 在此其作用是对wifi-device/wifi-iface这2个section
    指明一种特别的get和set方法; scan_wifi--config_load, 完成配置加载
    并进行config_cb回调.)
1.3 /sbin/wifi(wifi_detect--eval方法调用detect_mac80211函数)
1.3.1 /lib/wifi/mac80211.sh(detect_mac80211, 自动生成wireless配置文件)
~~~

#### 2.2 network脚本  ####
主要作用启动按配置文件进行wifi功能启动.

~~~
执行流程分析:
1 服务开启 /etc/init.d/network(service_running() -- /sbin/wifi reload_legacy)
2 重新加载 /etc/init.d/network(reload_service() -- /sbin/wifi reload_legacy)
3 服务停止 /etc/init.d/network(stop() -- /sbin/wifi down)

------------------------------------------------------
0 /sbin/wifi reload_legacy
1 /lib/function.sh(include)
1.1 /lib/wifi/mac80211.sh(获取全局变量DRIVERS值为"mac80211", 
    包含shell文件中的函数待后续调用)
2 /sbin/wifi(scan_wifi--config_cb, 其中config_cb为config_load的
    section回调函数, 在此其作用是对wifi-device/wifi-iface这2个section
    指明一种特别的get和set方法; scan_wifi--config_load, 完成配置加载
    并进行config_cb回调.)
3 /sbin/wifi(wifi_reload_legacy)
3.1 /sbin/wifi(_wifi_updown "disable" "$1"调用disable_mac80211())
3.1.1 /lib/wifi/mac80211.sh(按理应该disable_mac80211函数, 实际缺没有找到, 奇怪了...)
3.2 /sbin/wifi(scan_wifi)
3.3 /sbin/wifi(_wifi_updown "enable" "$1"调用enable_mac80211())
3.3.1 /lib/wifi/mac80211.sh(按理应该enable_mac80211函数, 实际缺没有找到, 奇怪了...)

------------------------------------------------------
0 /sbin/wifi down
1 /lib/function.sh(include)
1.1 /lib/wifi/mac80211.sh(获取全局变量DRIVERS值为"mac80211", 
    包含shell文件中的函数待后续调用)
2 /sbin/wifi(scan_wifi--config_cb, 其中config_cb为config_load的
    section回调函数, 在此其作用是对wifi-device/wifi-iface这2个section
    指明一种特别的get和set方法; scan_wifi--config_load, 完成配置加载
    并进行config_cb回调.)
3 /sbin/wifi(wifi_updown "disable" "$2")
3.1 /sbin/wifi(ubus_wifi_cmd "$cmd" "$2")
3.2 /sbin/wifi(_wifi_updown "$@")
~~~

### 参考  ###
* <a href="http://blog.csdn.net/hui523hui523hui523/article/details/38493725">openwrt下wifi设置详细过程  </a>


