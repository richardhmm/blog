---
layout: post
category: "Tech"
title:  "banana pi(BPI-M1) openwrt系统的网络设置"
description: 本文介绍了在banana pi(BPI-M1) openwrt系统的网络设置过程
tags: [banana pi BPI-M1 openwrt network 设置]
---

### 1. banana pi openwrt启动ok  ###
上一篇文章介绍了openwrt系统编译过程及烧写过程, 之后M1上电可以正常启动, 然而, M1自带的RJ45网卡默认被配置为WAN口(eth0), 而LAN(eth1)实际不存在.
因此, 没法使用浏览器访问openwrt系统, 那么接下来我就将解决这个问题.

### 2. 网络访问openwrt系统配置过程 ###
认真分析过网络后,决定修改/etc/config/network来使得eth0分派到LAN上, 从而保证PC或手机可以直接web访问香蕉派.

#### 2.1 network配置修改前 ####

~~~
root@BananaPi:/etc/config# cat network.bak0                                     
                                                                                
config interface 'loopback'                                                     
        option ifname 'lo'                                                      
        option proto 'static'                                                   
        option ipaddr '127.0.0.1'                                               
        option netmask '255.0.0.0'                                              
                                                                                
config globals 'globals'                                                        
        option ula_prefix 'fd30:8bf1:b154::/48'                                 
                                                                                
config interface 'wan'                                                          
        option ifname 'eth0'                                                    
        option proto 'dhcp'                                                     
                                                                                
config interface 'wan6'                                                         
        option ifname 'eth0'                                                    
        option proto 'dhcpv6'                                                   
                                                                                
config interface 'lan'                                                          
        option ifname 'eth1'                                                    
        option force_link '1'                                                   
        option type 'bridge'                                                    
        option proto 'static'                                                   
        option ipaddr '192.168.1.1'                                             
        option netmask '255.255.255.0'                                          
        option ip6assign '60'                                                   
                                                                                
root@BananaPi:/etc/config# 
~~~

#### 2.2 network配置修改后 ####

~~~
root@BananaPi:/etc/config# cat network                                          
                                                                                
config interface 'loopback'                                                     
        option ifname 'lo'                                                      
        option proto 'static'                                                   
        option ipaddr '127.0.0.1'                                               
        option netmask '255.0.0.0'                                              
                                                                                
config globals 'globals'                                                        
        option ula_prefix 'fd30:8bf1:b154::/48'                                 
                                                                                
config interface 'wan'                                                          
        option ifname 'usb0'       # usb0 后面要用                                             
        option proto 'dhcp'                                                     
                                                                                
config interface 'wan6'                                                         
        option ifname 'usb0'            # usb0 后面要用                                                
        option proto 'dhcpv6'                                                   
                                                                                
config interface 'lan'                                                          
        option ifname 'eth0'            # eth1改为eth0                                               
        option force_link '1'                                                   
        option type 'bridge'                                                    
        option proto 'static'                                                   
        option ipaddr '192.168.42.11'        # 改为与家中网络同处于一个网段, 方便PC直接访问                                   
        option netmask '255.255.255.0'                                          
        option ip6assign '60'                                                   
                                                                                
root@BananaPi:/etc/config# 
~~~

#### 2.3 网络服务重启 ####

~~~
root@BananaPi:/etc/config# /etc/init.d/network restart                          
root@BananaPi:/etc/config# [ 4378.388049]  RX IPC Checksum Offload disabled     
[ 4378.392527]  No MAC Management Counters available                            
[ 4378.398260] device eth0 entered promiscuous mode                             
[ 4378.406010] IPv6: ADDRCONF(NETDEV_UP): br-lan: link is not ready             
                                                                                                                                  
root@BananaPi:/etc/config# ifconfig                                             
br-lan    Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          inet addr:192.168.42.11  Bcast:192.168.42.255  Mask:255.255.255.0     
          inet6 addr: fd30:8bf1:b154::1/60 Scope:Global                         
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:0                                             
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)                                
                                                                                
eth0      Link encap:Ethernet  HWaddr 02:88:07:C1:62:AA                         
          UP BROADCAST MULTICAST  MTU:1500  Metric:1                            
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0                    
          TX packets:7 errors:0 dropped:0 overruns:0 carrier:0                  
          collisions:0 txqueuelen:1000                                          
          RX bytes:0 (0.0 B)  TX bytes:1010 (1010.0 B)                          
          Interrupt:117                                                         
                                                                                
lo        Link encap:Local Loopback                                             
          inet addr:127.0.0.1  Mask:255.0.0.0                                   
          inet6 addr: ::1/128 Scope:Host                                        
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                              
          RX packets:13464 errors:0 dropped:0 overruns:0 frame:0                
          TX packets:13464 errors:0 dropped:0 overruns:0 carrier:0              
          collisions:0 txqueuelen:0                                             
          RX bytes:797040 (778.3 KiB)  TX bytes:797040 (778.3 KiB)              
                                                                                
root@BananaPi:/etc/config# 
~~~

#### 2.4 web正常访问openwrt ####


### 参考  ###
* <a href="http://www.finklabs.org/articles/using-docker-on-banana-pi.html">Using Docker on Banana Pi</a>
