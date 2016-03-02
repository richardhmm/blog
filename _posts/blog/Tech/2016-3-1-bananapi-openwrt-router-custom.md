---
layout: post
category: "Tech"
title:  "banana pi(BPI-M1)定制为openwrt系统的路由器"
description: 本文介绍了在banana pi(BPI-M1)定制为openwrt系统的路由器原理与方法
tags: [banana pi BPI-M1 openwrt 路由器 定制]
---

### 1. 定制路由器原理介绍  ###
**定制目标如下图**

![原理框图](/blog/images/bananapi/BPI_router.png)

### 2. 定制过程 ###

#### 2.1 增加USB RNDIS实现共享安卓手机上网 ####

**首先, 版本编译配置**

~~~
make menuconfig

# 进行如下配置
Kernel modules  ---> USB Support  ---> <*> kmod-usb-net
Kernel modules  ---> USB Support  ---> -*-   kmod-usb-net-cdc-ether
Kernel modules  ---> USB Support  ---> <*>   kmod-usb-net-rndis
Utilities  ---> <*> usbutils
Base system  ---> <*> udev

# 保存配置并编译运行
make V=99
~~~

**其次, 进行网络配置使能USB RNDIS共享上网**

~~~
root@BananaPi:/# cat /etc/config/network                                        
                                                                                
config interface 'loopback'                                                     
        option ifname 'lo'                                                      
        option proto 'static'                                                   
        option ipaddr '127.0.0.1'                                               
        option netmask '255.0.0.0'                                              
                                                                                
config globals 'globals'                                                        
        option ula_prefix 'fdb6:87de:16ab::/48'                                 
                                                                                
config interface 'wan'                                                          
        option ifname 'usb0'                                                    
        option proto 'dhcp'                                                     
                                                                                
config interface 'lan'                                                          
        option ifname 'eth0'                                                    
        option force_link '1'                                                   
        option type 'bridge'                                                    
        option proto 'static'                                                   
        option ipaddr '192.168.1.1'                                             
        option netmask '255.255.255.0'                                          
        option ip6assign '60'                                                   
                                                                                
root@BananaPi:/#

# 插上三星手机并开启USB网络共享功能
[  131.574646] usb 2-1: new high-speed USB device number 2 usim
[  132.044644] usb 4-1: new full-speed USB device number 2 using ohci-platform  
[  132.257665] usb 4-1: not running at top speed; connect to a high speed hub   
[  132.279670] usb 4-1: New USB device found, idVendor=04e8, idProduct=6860     
[  132.286397] usb 4-1: New USB device strings: Mfr=2, Product=3, SerialNumber=4
[  132.293527] usb 4-1: Product: SCH-N719                                       
[  132.297296] usb 4-1: Manufacturer: samsung                                   
[  132.301395] usb 4-1: SerialNumber: xxx                          
[  135.165451] usb 4-1: USB disconnect, device number 2                         
[  135.414652] usb 2-1: new high-speed USB device number 3 using ehci-platform  
[  135.884642] usb 4-1: new full-speed USB device number 3 using ohci-platform  
[  136.097677] usb 4-1: not running at top speed; connect to a high speed hub   
[  136.119680] usb 4-1: New USB device found, idVendor=04e8, idProduct=6860     
[  136.126411] usb 4-1: New USB device strings: Mfr=2, Product=3, SerialNumber=4
[  136.133539] usb 4-1: Product: SCH-N719                                       
[  136.137419] usb 4-1: Manufacturer: samsung                                   
[  136.141514] usb 4-1: SerialNumber: xxx                          
[  145.214372] usb 4-1: USB disconnect, device number 3                         
[  145.504650] usb 2-1: new high-speed USB device number 4 using ehci-platform  
[  146.044645] usb 2-1: device not accepting address 4, error -71               
[  146.504638] usb 4-1: new full-speed USB device number 4 using ohci-platform  
[  146.717683] usb 4-1: not running at top speed; connect to a high speed hub   
[  146.735676] usb 4-1: New USB device found, idVendor=04e8, idProduct=6863     
[  146.742385] usb 4-1: New USB device strings: Mfr=2, Product=3, SerialNumber=4
[  146.749548] usb 4-1: Product: SCH-N719                                       
[  146.753298] usb 4-1: Manufacturer: samsung                                   
[  146.757415] usb 4-1: SerialNumber: xxx                          
[  146.784775] rndis_host 4-1:1.0 usb0: register 'rndis_host' at usb-1c1c400.us6
                                                                                
root@BananaPi:/# ifconfig                                                       
br-lan    Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0        
          inet6 addr: fdb6:87de:16ab::1/60 Scope:Global                         
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:0                                             
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)                                
                                                                                
eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:920 (920.0 B)                            
          Interrupt:117                                                         
                                                                                
lo        Link encap:Local Loopback                                             
          inet addr:127.0.0.1  Mask:255.0.0.0                                   
          inet6 addr: ::1/128 Scope:Host                                        
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                              
          RX packets:1552 errors:0 dropped:0 overruns:0 frame:0                 
          TX packets:1552 errors:0 dropped:0 overruns:0 carrier:0               
          collisions:0 txqueuelen:0                                             
          RX bytes:94728 (92.5 KiB)  TX bytes:94728 (92.5 KiB)                  
                                                                                
usb0      Link encap:Ethernet  HWaddr 02:01:67:31:62:36                         
          inet addr:192.168.42.220  Bcast:192.168.42.255  Mask:255.255.255.0    
          inet6 addr: fe80::1:67ff:fe31:6236/64 Scope:Link                      
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1                    
          RX packets:387 errors:0 dropped:0 overruns:0 frame:0                  
          TX packets:378 errors:0 dropped:0 overruns:0 carrier:0                
          collisions:0 txqueuelen:1000                                          
          RX bytes:25412 (24.8 KiB)  TX bytes:45056 (44.0 KiB)                  
                                                                                
root@BananaPi:/# route                                                          
Kernel IP routing table                                                         
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface   
default         192.168.42.129  0.0.0.0         UG    0      0        0 usb0    
192.168.1.0     *               255.255.255.0   U     0      0        0 br-lan  
192.168.42.0    *               255.255.255.0   U     0      0        0 usb0    
192.168.42.129  *               255.255.255.255 UH    0      0        0 usb0       


# ping test ok                                           
root@BananaPi:/# ping www.baidu.com                                             
PING www.baidu.com (14.215.177.37): 56 data bytes                               
64 bytes from 14.215.177.37: seq=0 ttl=50 time=75.508 ms                        
64 bytes from 14.215.177.37: seq=1 ttl=50 time=72.230 ms                        
64 bytes from 14.215.177.37: seq=2 ttl=50 time=68.092 ms                        
^C                                                                              
--- www.baidu.com ping statistics ---                                           
3 packets transmitted, 3 packets received, 0% packet loss                       
round-trip min/avg/max = 68.092/71.943/75.508 ms                                
root@BananaPi:/# 
~~~

#### 2.2 增加USB WIFI AP ####
笔者手上有一个TP-LINK TL-WN823N 1.0的usb wifi网卡, 直接插到系统中, 不能正常识别为USB WIFI, 打印如下:

~~~
[ 1850.481766] usb 1-1: USB disconnect, device number 2        
[ 1956.593682] usb 1-1: new high-speed USB device number 3 using ehci-platform  
[ 1956.745456] usb 1-1: New USB device found, idVendor=0bda, idProduct=8178     
[ 1956.752164] usb 1-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 1956.759333] usb 1-1: Product: USB WLAN                                       
[ 1956.763091] usb 1-1: Manufacturer: 802.11n                                   
[ 1956.767217] usb 1-1: SerialNumber: 00e04c000001 
~~~

首先, 需要增加USB WIFI驱动和hostapd工具软件.

~~~
make menuconfig

# 配置
Kernel modules  ---> USB Support  ---> <*> kmod-usb-ohci
Kernel modules  ---> USB Support  ---> <*> kmod-usb-uhci
Kernel modules  ---> USB Support  ---> <*> kmod-usb2
Kernel modules  ---> Wireless Drivers  ---> -*- kmod-cfg80211
Kernel modules  ---> Wireless Drivers  ---> <*> kmod-lib80211
Kernel modules  ---> Wireless Drivers  ---> <*> kmod-mac80211
Kernel modules  ---> Wireless Drivers  ---> <*> kmod-rtl8192cu

Network  ---> <*> hostapd
Network  ---> -*- hostapd-common
Network  ---> <*> hostapd-utils
~~~

其次, 编译并烧结新固件.

~~~
make V=99

# 烧结
BPI-OpenWRT/bin/sunxi# dd bs=4M if=openwrt-sunxi-BPI-M1-sdcard-vfat-ext4.img of=/dev/sdb
~~~

然后, TL-WN823N插入单板, 上电启动.

~~~
# 启动信息, 如下反应显示识别了USB WIFI
[    1.694721] usb 2-1: new high-speed USB device number 2 using ehci-platform  
[    1.846597] usb 2-1: New USB device found, idVendor=0bda, idProduct=8178     
[    1.853307] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    1.860492] usb 2-1: Product: USB WLAN                                       
[    1.864245] usb 2-1: Manufacturer: 802.11n                                   
[    1.868404] usb 2-1: SerialNumber: 00e04c000001                              
[    1.884479] init: Console is alive                                           
[    1.888198] init: - watchdog -                                               
[    2.892113] init: - preinit -                                                
Detected bpi-m1 // BPI M1                                                       
[    3.002254] random: mktemp urandom read with 5 bits of entropy available     
Press the [f] key and hit [enter] to enter failsafe mode                        
Press the [1], [2], [3] or [4] key and hit [enter] to select the debug level    
[    6.071012] mount_root: mounting /dev/root                                   
[    6.080777] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)                    
[    6.091685] procd: - early -                                                 
[    6.094782] procd: - watchdog -                                              
[    6.900228] procd: - ubus -                                                  
[    7.915836] procd: - init -                                                  
Please press Enter to activate this console.                                    
[    8.625908] NET: Registered protocol family 10                               
[    8.637094] ip6_tables: (C) 2000-2006 Netfilter Core Team                    
[    8.650913] Netfilter messages via NETLINK v0.30.                            
[    8.657082] ip_set: protocol 6                                               
[    8.682995] sunxi-rtc 1c20d00.rtc: rtc core: registered rtc-sunxi as rtc0    
[    8.690624] sunxi-rtc 1c20d00.rtc: RTC enabled                               
[    8.756566] Loading modules backported from Linux version master-2015-03-09-5
[    8.764501] Backport generated by backports.git backports-20150129-0-gdd4a670
[    8.773712] ip_tables: (C) 2000-2006 Netfilter Core Team                     
[    8.782877] lib80211: common routines for IEEE802.11 drivers                 
[    8.793851] nf_conntrack version 0.5.0 (16024 buckets, 64096 max)            
[    8.931123] xt_time: kernel timezone is -0000                                
[    8.936902] usbcore: registered new interface driver cdc_ether               
[    8.958896] cfg80211: Calling CRDA to update world regulatory domain         
[    8.965660] cfg80211: World regulatory domain updated:                       
[    8.972038] cfg80211:  DFS Master region: unset                              
[    8.976523] cfg80211:   (start_freq - end_freq @ bandwidth), (max_antenna_ga)
[    8.986446] cfg80211:   (2402000 KHz - 2472000 KHz @ 40000 KHz), (N/A, 2000 )
[    8.994484] cfg80211:   (2457000 KHz - 2482000 KHz @ 40000 KHz), (N/A, 2000 )
[    9.002634] cfg80211:   (2474000 KHz - 2494000 KHz @ 20000 KHz), (N/A, 2000 )
[    9.010772] cfg80211:   (5170000 KHz - 5250000 KHz @ 80000 KHz), (N/A, 2000 )
[    9.018924] cfg80211:   (5250000 KHz - 5330000 KHz @ 80000 KHz, 160000 KHz A)
[    9.028540] cfg80211:   (5490000 KHz - 5730000 KHz @ 160000 KHz), (N/A, 2000)
[    9.036714] cfg80211:   (5735000 KHz - 5835000 KHz @ 80000 KHz), (N/A, 2000 )
[    9.044827] cfg80211:   (57240000 KHz - 63720000 KHz @ 2160000 KHz), (N/A, 0)
[    9.090765] PPP generic driver version 2.4.2                                 
[    9.096431] NET: Registered protocol family 24                               
[    9.102236] usbcore: registered new interface driver rndis_host              
[    9.120953] rtl8192cu: Chip version 0x11                                     
[    9.207462] rtl8192cu: MAC address: 0c:82:68:23:17:4c                        
[    9.212566] rtl8192cu: Board Type 0                                          
[    9.216315] rtl_usb: rx_max_size 15360, rx_urb_num 8, in_ep 1                
[    9.222291] rtl8192cu: Loading firmware rtlwifi/rtl8192cufw_TMSC.bin         
[    9.231879] usbcore: registered new interface driver rtl8192cu 


# 查询网络设备发现wlan0, 但未使能
root@BananaPi:/# ifconfig                                                       
eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          inet6 addr: fe80::88:7ff:fec1:62aa/64 Scope:Link                      
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:7 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:1087 (1.0 KiB)                           
          Interrupt:117                                                         
                                                                                
lo        Link encap:Local Loopback                                             
          inet addr:127.0.0.1  Mask:255.0.0.0                                   
          inet6 addr: ::1/128 Scope:Host                                        
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                              
          RX packets:96 errors:0 dropped:0 overruns:0 frame:0                   
          TX packets:96 errors:0 dropped:0 overruns:0 carrier:0                 
          collisions:0 txqueuelen:0                                             
          RX bytes:7416 (7.2 KiB)  TX bytes:7416 (7.2 KiB)                      
                                                                                
root@BananaPi:/# ifconfig -a                                                    
eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          inet6 addr: fe80::88:7ff:fec1:62aa/64 Scope:Link                      
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:7 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:1087 (1.0 KiB)                           
          Interrupt:117                                                         
                                                                                
lo        Link encap:Local Loopback                                             
          inet addr:127.0.0.1  Mask:255.0.0.0                                   
          inet6 addr: ::1/128 Scope:Host                                        
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                              
          RX packets:104 errors:0 dropped:0 overruns:0 frame:0                  
          TX packets:104 errors:0 dropped:0 overruns:0 carrier:0                
          collisions:0 txqueuelen:0                                             
          RX bytes:7824 (7.6 KiB)  TX bytes:7824 (7.6 KiB)                      
                                                                                
wlan0     Link encap:Ethernet  HWaddr 0C:82:68:23:17:4C                         
          BROADCAST MULTICAST  MTU:1500  Metric:1                               
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)                                
                                                                                
root@BananaPi:/# cat /etc/config/wireless                                       
config wifi-device  radio0                                                      
        option type     mac80211                                                
        option channel  11                                                      
        option hwmode   11g                                                     
        option path     'soc@01c00000/1c1c000.usb/usb2/2-1/2-1:1.0'             
        option htmode   HT20                                                    
        # REMOVE THIS LINE TO ENABLE WIFI:                                      
        option disabled 1                                                       
                                                                                
config wifi-iface                                                               
        option device   radio0                                                  
        option network  lan                                                     
        option mode     ap                                                      
        option ssid     OpenWrt-BPI                                             
        option encryption none                                                  
                                                                                
root@BananaPi:/#

# 配置/etc/config/wireless使能wifi AP, 同时/etc/config/network按照2.1重新配置并重启网络服务.

~~~
root@BananaPi:/# cat /etc/config/wireless  # 已经使能wifi ap                                       
config wifi-device  radio0                                                      
        option type     mac80211                                                
        option channel  11                                                      
        option hwmode   11g                                                     
        option path     'soc@01c00000/1c1c000.usb/usb2/2-1/2-1:1.0'             
        option htmode   HT20                                                    
        # REMOVE THIS LINE TO ENABLE WIFI:                                      
        option disabled 0                                                       
                                                                                
config wifi-iface                                                               
        option device   radio0                                                  
        option network  lan                                                     
        option mode     ap                                                      
        option ssid     OpenWrt-BPI                                             
        option encryption none                                                  
                                                                                
root@BananaPi:/# 
root@BananaPi:/# /etc/init.d/network restart  # 重启网络服务                                    
[  991.330108] device eth0 left promiscuous mode                                
[  991.334586] br-lan: port 1(eth0) entered disabled state                      
[  991.348363] IPv6: ADDRCONF(NETDEV_UP): eth0: link is not ready               
root@BananaPi:/# [  993.284846]  RX IPC Checksum Offload disabled               
[  993.289242]  No MAC Management Counters available                            
[  993.295211] device eth0 entered promiscuous mode                             
[  993.301789] IPv6: ADDRCONF(NETDEV_UP): br-lan: link is not ready             
[  993.797445] rtl8192cu: MAC auto ON okay!                                     
[  993.834994] rtl8192cu: Tx queue select: 0x05                                 
[  994.300507] IPv6: ADDRCONF(NETDEV_UP): wlan0: link is not ready              
[  994.309464] device wlan0 entered promiscuous mode                            
[  994.331242] br-lan: port 2(wlan0) entered forwarding state                   
[  994.336911] br-lan: port 2(wlan0) entered forwarding state                   
[  994.342812] IPv6: ADDRCONF(NETDEV_CHANGE): wlan0: link becomes ready         
[  994.358747] IPv6: ADDRCONF(NETDEV_CHANGE): br-lan: link becomes ready        
[  996.334702] br-lan: port 2(wlan0) entered forwarding state                   
                                                                                                                                
root@BananaPi:/# ifconfig        # 显示wlan0网络设备启动OK                                               
br-lan    Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0        
          inet6 addr: fe80::88:7ff:fec1:62aa/64 Scope:Link                      
          inet6 addr: fdd9:5fd:97ff::1/60 Scope:Global                          
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1                    
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:9 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:0                                             
          RX bytes:0 (0.0 B)  TX bytes:1254 (1.2 KiB)                           
                                                                                
eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:7 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:1087 (1.0 KiB)                           
          Interrupt:117                                                         
                                                                                
lo        Link encap:Local Loopback                                             
          inet addr:127.0.0.1  Mask:255.0.0.0                                   
          inet6 addr: ::1/128 Scope:Host                                        
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                              
          RX packets:3216 errors:0 dropped:0 overruns:0 frame:0                 
          TX packets:3216 errors:0 dropped:0 overruns:0 carrier:0               
          collisions:0 txqueuelen:0                                             
          RX bytes:192648 (188.1 KiB)  TX bytes:192648 (188.1 KiB)              
                                                                                
wlan0     Link encap:Ethernet  HWaddr 0C:82:68:23:17:4C                         
          inet6 addr: fe80::e82:68ff:fe23:174c/64 Scope:Link                    
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1                    
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:18 errors:0 dropped:0 overruns:0 carrier:0                 
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:2432 (2.3 KiB)                           
                                                                                
root@BananaPi:/# cat /var/dhcp.leases          # 手机连接wifi热点后                                 
1456971789 34:23:ba:df:b6:ac 192.168.1.246 android-xxx 01:34:23:bac
root@BananaPi:/# cat /proc/net/arp                                              
IP address       HW type     Flags       HW address            Mask     Device  
192.168.1.246    0x1         0x2         34:23:ba:df:b6:ac     *        br-lan  
~~~

最后, 大功告成, show一下.

~~~
root@BananaPi:/# cat /proc/net/arp 
IP address       HW type     Flags       HW address            Mask     Device
192.168.1.188    0x1         0x2         00:ea:01:17:97:ec     *        br-lan        # RJ45连接PC(LAN)
192.168.42.129   0x1         0x2         16:16:89:51:07:d3     *        usb0        # USB连接安卓手机并开启USB共享上网(WAN)
192.168.1.246    0x1         0x2         34:23:ba:df:b6:ac     *        br-lan        # wifi连接手机(LAN)
root@BananaPi:/# ifconfig 
br-lan    Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA  
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::88:7ff:fec1:62aa/64 Scope:Link
          inet6 addr: fdd9:5fd:97ff::1/60 Scope:Global
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:22077 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2109 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:1539394 (1.4 MiB)  TX bytes:708324 (691.7 KiB)

eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2127 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1945 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:255020 (249.0 KiB)  TX bytes:689627 (673.4 KiB)
          Interrupt:117 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:121 errors:0 dropped:0 overruns:0 frame:0
          TX packets:121 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:9842 (9.6 KiB)  TX bytes:9842 (9.6 KiB)

usb0      Link encap:Ethernet  HWaddr 02:01:67:31:62:36  
          inet addr:192.168.42.220  Bcast:192.168.42.255  Mask:255.255.255.0
          inet6 addr: fe80::1:67ff:fe31:6236/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:435 errors:1 dropped:0 overruns:0 frame:1
          TX packets:20037 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:78807 (76.9 KiB)  TX bytes:2507416 (2.3 MiB)

wlan0     Link encap:Ethernet  HWaddr 0C:82:68:23:17:4C  
          inet6 addr: fe80::e82:68ff:fe23:174c/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:20024 errors:0 dropped:0 overruns:0 frame:0
          TX packets:343 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1601549 (1.5 MiB)  TX bytes:45959 (44.8 KiB)

root@BananaPi:/# 
~~~

**WEB界面**

![总览1](/blog/images/bananapi/router_overview1.png)

![总览2](/blog/images/bananapi/router_overview2.png)


### 参考  ###
* <a href="http://blog.csdn.net/whfyzg/article/details/47125273">openwrt 使用 android 手机usb tether联网 </a>
* <a href="http://blog.csdn.net/hui523hui523hui523/article/details/38493725">openwrt下wifi设置详细过程  </a>
* <a href="http://blog.csdn.net/qingfengtsing/article/details/41040353">openwrt x86虚拟机如何挂载usb无线网卡 </a>