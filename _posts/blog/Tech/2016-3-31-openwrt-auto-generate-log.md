---
layout: post
category: "Tech"
title:  "openwrt下一种简单易用的log日志保存脚本"
description: 本文介绍了openwrt下一种简单的log日志保存脚本。
tags: [openwrt log logread shell]
---

### 1. 目标  ###
  为了方便调试, 或者查看路由器运行状态, 编写了一个简单易用的系统日志和内核日志自动保存脚本。记下来, 防止忘记, 留着备用。

### 2. 实现 ###

#### 2.1 启动 ####
在/etc/rc.local下增加启动脚本。

~~~
# Put your custom commands here that should be executed once
# the system init finished. By default this file does nothing.

if [ -x /mnt/sda3/log/autoGenLog.sh ] 
then
    /mnt/sda3/log/autoGenLog.sh &
fi

exit 0
~~~

#### 2.2 实现脚本 ####
/mnt/sda3/log/autoGenLog.sh

~~~
#!/bin/sh
# atuo Generate Log

echo "====save system log====" >> /mnt/sda3/log/log.sys
echo "====date:" >> /mnt/sda3/log/log.sys
date >> /mnt/sda3/log/log.sys
logread >> /mnt/sda3/log/log.sys
logread -f >> /mnt/sda3/log/log.sys &

echo "====save kernel log====" >> /mnt/sda3/log/log.kernel
while true
do
    echo "====date:" >> /mnt/sda3/log/log.kernel
    date >> /mnt/sda3/log/log.kernel
    echo "====uptime:" >> /mnt/sda3/log/log.kernel
    uptime >> /mnt/sda3/log/log.kernel
    dmesg -c >> /mnt/sda3/log/log.kernel
    
    echo "====date:" >> /mnt/sda3/log/log.sys
    date >> /mnt/sda3/log/log.sys
    echo "====uptime:" >> /mnt/sda3/log/log.sys
    uptime >> /mnt/sda3/log/log.sys
    echo "====top -bn1:" >> /mnt/sda3/log/log.sys
    top -bn1 >> /mnt/sda3/log/log.sys
    
    sleep 3600
done
~~~

### 参考  ###
* linux shell
