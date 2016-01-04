---
layout: default
title: "备忘"
---
记录工作中遇到的一些小问题及解决办法，以防忘记后重头再找答案。

### Linux
* **1. 内网linux获取出口公网IP**

    $ curl http://members.3322.org/dyndns/getip
    
    $ wget http://members.3322.org/dyndns/getip 
    
    $ cat getip

* **2. 查看当前文件夹大小** 

    $ du -skh
    
* **3. 删除空文件** 

    $ find / -type f -size 0 -exec rm -rf {} \;
    
* **4. 获取当前内网IP地址**

    $ ifconfig |awk -F"[ ]+|[:]" 'NR==2 {print $4}'
    
* **5. 以内存大小排序列出进程**

    $ ps aux --sort=rss |sort -k 6 -rn
    
* **6. 目录中大量文件删除**

    $ ls | xargs rm
    
* **7. 查找进程pid并kill**

    $ pgrep nginx|xargs kill
    
    $ pidof nginx|xargs kill
    




