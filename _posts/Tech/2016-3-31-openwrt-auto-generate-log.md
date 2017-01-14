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

if [ -x /mnt/log/autoGenLog.sh ] 
then
    /mnt/log/autoGenLog.sh &
fi

exit 0
~~~

#### 2.2 实现脚本 ####
/mnt/log/autoGenLog.sh

~~~
#!/bin/sh
# atuo Generate Log

mkdir -p /mnt/log
LOG_SYS_FILE=/mnt/log/log.sys
LOG_KERNEL_FILE=/mnt/log/log.kernel
MAX_FILE_SIZE=10485760 # 10M
TIME_SLEEP=3600 # 1h

echo "====save system log====" >> $LOG_SYS_FILE
echo "====date:" >> $LOG_SYS_FILE
date >> $LOG_SYS_FILE
logread >> $LOG_SYS_FILE
logread -f >> $LOG_SYS_FILE &

echo "====save kernel log====" >> $LOG_KERNEL_FILE
while true
do
    echo "====date:" >> $LOG_KERNEL_FILE
    date >> $LOG_KERNEL_FILE
    echo "====uptime:" >> $LOG_KERNEL_FILE
    uptime >> $LOG_KERNEL_FILE
    dmesg -c >> $LOG_KERNEL_FILE
    
    echo "====date:" >> $LOG_SYS_FILE
    date >> $LOG_SYS_FILE
    echo "====uptime:" >> $LOG_SYS_FILE
    uptime >> $LOG_SYS_FILE
    echo "====top -bn1:" >> $LOG_SYS_FILE
    top -bn1 >> $LOG_SYS_FILE
    
    sleep $TIME_SLEEP
    now=$(date +%Y%m%d-%s)
    
    # 分割文件
    sys_file_size=$(ls -l $LOG_SYS_FILE | awk '{print $5}')
    if [ $sys_file_size -ge $MAX_FILE_SIZE ]
    then
        mv $LOG_SYS_FILE $LOG_SYS_FILE.bak.$now
    fi
    
    kernel_file_size=$(ls -l $LOG_KERNEL_FILE | awk '{print $5}')
    if [ $kernel_file_size -ge $MAX_FILE_SIZE ]
    then
        mv $LOG_KERNEL_FILE $LOG_KERNEL_FILE.bak.$now
    fi
done
~~~

### 参考  ###
* linux shell
