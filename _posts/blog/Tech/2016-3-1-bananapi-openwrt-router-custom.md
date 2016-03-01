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

1. 版本编译配置
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

2. 网络配置使能USB RNDIS共享上网
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

### 参考  ###
* <a href="http://blog.csdn.net/whfyzg/article/details/47125273">openwrt 使用 android 手机usb tether联网 </a>
* <a href="http://blog.csdn.net/hui523hui523hui523/article/details/38493725">openwrt下wifi设置详细过程  </a>