---
layout: post
category: "Tech"
title:  "ubuntu 14.04下docker实践"
description: 本文介绍了一个docker小白在ubuntu 14.04下进行docker安装, 配置与使用
tags: [docker ubuntu]
---

### 1. docker简介 ###
docker小白, 此处不废话, 更多信息参考如下链接.

<a href="http://dockerpool.com/static/books/docker_practice/introduction/what.html">什么是 Docker</a>

### 2. docker安装 ###

#### 2.1 默认安装过程(64bit) ####
**通过Docker源安装最新版本**

要安装最新的 Docker 版本，首先需要安装 apt-transport-https 支持，之后通过添加源来安装。

    $ sudo apt-get install apt-transport-https
    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
    $ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
    $ sudo apt-get update
    $ sudo apt-get install lxc-docker


注：有个简单脚本可以用于这个过程

    $ curl -sSL https://get.docker.com/ubuntu/ | sudo sh

验证所有的工作都如预期完成了

    $ sudo docker run -i -t ubuntu /bin/bash


#### 2.2 ubuntu 14.04 i386 32bit安装 ####
无奈本人机器装的是32bit ubuntu系统, 上述2.1安装失败.

**32bit ubuntu安装Docker**

Run build-image.sh to build the docker image 32bit/ubuntu:14.04, or build-image.sh precise to build the docker image 32bit/ubuntu:precise.

<a href="https://github.com/docker-32bit/ubuntu">Build a docker image for ubuntu i386</a>

See also: http://mwhiteley.com/linux-containers/2013/08/31/docker-on-i386.html

**32bit ubuntu安装Docker OK后**

    $ docker
    Usage: docker [OPTIONS] COMMAND [arg...]

    A self-sufficient runtime for linux containers.

    Options:
      --api-cors-header=                   Set CORS headers in the remote API
      -b, --bridge=                        Attach containers to a network bridge
      --bip=                               Specify network bridge IP
      -D, --debug=false                    Enable debug mode
      -d, --daemon=false                   Enable daemon mode
      --default-ulimit=[]                  Set default ulimits for containers
      --dns=[]                             DNS server to use
      --dns-search=[]                      DNS search domains to use
      -e, --exec-driver=native             Exec driver to use
      --fixed-cidr=                        IPv4 subnet for fixed IPs
      --fixed-cidr-v6=                     IPv6 subnet for fixed IPs
      -G, --group=docker                   Group for the unix socket
      -g, --graph=/var/lib/docker          Root of the Docker runtime
      -H, --host=[]                        Daemon socket(s) to connect to
      -h, --help=false                     Print usage
      --icc=true                           Enable inter-container communication
      --insecure-registry=[]               Enable insecure registry communication
      --ip=0.0.0.0                         Default IP when binding container ports
      --ip-forward=true                    Enable net.ipv4.ip_forward
      --ip-masq=true                       Enable IP masquerading
      --iptables=true                      Enable addition of iptables rules
      --ipv6=false                         Enable IPv6 networking
      -l, --log-level=info                 Set the logging level
      --label=[]                           Set key=value labels to the daemon
      --log-driver=json-file               Containers logging driver
      --mtu=0                              Set the containers network MTU
      -p, --pidfile=/var/run/docker.pid    Path to use for daemon PID file
      --registry-mirror=[]                 Preferred Docker registry mirror
      -s, --storage-driver=                Storage driver to use
      --selinux-enabled=false              Enable selinux support
      --storage-opt=[]                     Set storage driver options
      --tls=false                          Use TLS; implied by --tlsverify
      --tlscacert=~/.docker/ca.pem         Trust certs signed only by this CA
      --tlscert=~/.docker/cert.pem         Path to TLS certificate file
      --tlskey=~/.docker/key.pem           Path to TLS key file
      --tlsverify=false                    Use TLS and verify the remote
      -v, --version=false                  Print version information and quit

    Commands:
        attach    Attach to a running container
        build     Build an image from a Dockerfile
        commit    Create a new image from a container's changes
        cp        Copy files/folders from a container's filesystem to the host path
        create    Create a new container
        diff      Inspect changes on a container's filesystem
        events    Get real time events from the server
        exec      Run a command in a running container
        export    Stream the contents of a container as a tar archive
        history   Show the history of an image
        images    List images
        import    Create a new filesystem image from the contents of a tarball
        info      Display system-wide information
        inspect   Return low-level information on a container or image
        kill      Kill a running container
        load      Load an image from a tar archive
        login     Register or log in to a Docker registry server
        logout    Log out from a Docker registry server
        logs      Fetch the logs of a container
        port      Lookup the public-facing port that is NAT-ed to PRIVATE_PORT
        pause     Pause all processes within a container
        ps        List containers
        pull      Pull an image or a repository from a Docker registry server
        push      Push an image or a repository to a Docker registry server
        rename    Rename an existing container
        restart   Restart a running container
        rm        Remove one or more containers
        rmi       Remove one or more images
        run       Run a command in a new container
        save      Save an image to a tar archive
        search    Search for an image on the Docker Hub
        start     Start a stopped container
        stats     Display a stream of a containers' resource usage statistics
        stop      Stop a running container
        tag       Tag an image into a repository
        top       Lookup the running processes of a container
        unpause   Unpause a paused container
        version   Show the Docker version information
        wait      Block until a container stops, then print its exit code

    Run 'docker COMMAND --help' for more information on a command.
    $ cat /etc/os-release 
    NAME="Ubuntu"
    VERSION="14.04, Trusty Tahr"
    ID=ubuntu
    ID_LIKE=debian###
    PRETTY_NAME="Ubuntu 14.04 LTS"
    VERSION_ID="14.04"
    HOME_URL="http://www.ubuntu.com/"
    SUPPORT_URL="http://help.ubuntu.com/"
    BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/" 

### 3. Docker OpenWRT 15.05 Image ###
导入OpenWRT 15.05 Image: 

    #docker import http://downloads.openwrt.org/chaos_calmer/15.05/x86/generic/openwrt-15.05-x86-generic-Generic-rootfs.tar.gz  openwrt-x86-generic-rootfs
    Downloading from http://downloads.openwrt.org/chaos_calmer/15.05/x86/generic/openwrt-15.05-x86-generic-Generic-rootfs.tar.gz
    Importing 2.272 MB/2.272 MB
    ee4136038b675906eab4f21d672bd71d9261b10509d3c5e42558d3a73468d2aa
    # docker images
    REPOSITORY                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    openwrt-x86-generic-rootfs         latest              ee4136038b67        27 seconds ago      5.2 MB

运行一个简单cat命令:

    # docker run -i openwrt-x86-generic-rootfs cat /etc/banner
      _______                     ________        __
     |       |.-----.-----.-----.|  |  |  |.----.|  |_
     |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
     |_______||   __|_____|__|__||________||__|  |____|
              |__| W I R E L E S S   F R E E D O M
     -----------------------------------------------------
     CHAOS CALMER (15.05, r46767)
     -----------------------------------------------------
      * 1 1/2 oz Gin            Shake with a glassful
      * 1/4 oz Triple Sec       of broken ice and pour
      * 3/4 oz Lime Juice       unstrained into a goblet.
      * 1 1/2 oz Orange Juice
      * 1 tsp. Grenadine Syrup
     -----------------------------------------------------
    #

运行一个基础命令ifconfig:

    # docker run -i openwrt-x86-generic-rootfs ifconfig
    eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02  
          inet addr:172.17.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe11:2/64 Scope:Link
          UP BROADCAST  MTU:1500  Metric:1
          RX packets:1 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:90 (90.0 B)  TX bytes:90 (90.0 B)

    lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

运行在交互shell中:

    # docker run -i -t openwrt-x86-generic-rootfs /bin/ash


    BusyBox v1.23.2 (2015-07-25 07:32:21 CEST) built-in shell (ash)

    / # ps
	  PID USER       VSZ STAT COMMAND
	    1 root      1068 S    /bin/ash
	    8 root      1068 R    ps
	    / # ifconfig 
	    eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:04  
	          inet addr:172.17.0.4  Bcast:0.0.0.0  Mask:255.255.0.0
	          inet6 addr: fe80::42:acff:fe11:4/64 Scope:Link
	          UP BROADCAST RUNNING  MTU:1500  Metric:1
	          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:0 
	          RX bytes:508 (508.0 B)  TX bytes:508 (508.0 B)
	
	    lo        Link encap:Local Loopback  
	          inet addr:127.0.0.1  Mask:255.0.0.0
	          inet6 addr: ::1/128 Scope:Host
	          UP LOOPBACK RUNNING  MTU:65536  Metric:1
	          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:0 
	          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
	
	    / #

### 参考 ###
* <a href="http://dockerpool.com/static/books/docker_practice/">Docker —— 从入门到实践</a>
* <a href="http://openwrt.io/docs/build-openwrt-package-using-docker/">使用Docker编译OpenWrt Package</a>
* <a href="http://www.520608.com/ji-yu-dockerbian-yi-openwrt/">基于Docker编译openwrt</a>
* <a href="https://wiki.openwrt.org/doc/howto/docker_openwrt_image">Docker OpenWRT Image</a>