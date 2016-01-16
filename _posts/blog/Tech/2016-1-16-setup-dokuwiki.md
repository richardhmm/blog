---
layout: post
category: "Tech"
title:  "ubuntu 14.04下dokuwiki的搭建与配置"
description: 本文介绍了ubuntu下进行小型开源wiki dokuwiki的搭建与配置使用
tags: [dokuwiki ubuntu]
---

###1. Dokuwiki介绍
Dokuwiki是一个用PHP写成的小巧wiki程序，不需要数据库，简洁而容易上手，又拥有很不错的权限管理体制和许多插件。它可以作为个人和中小型组织的知识库，也可以用作个人博客。

####核心特点

* **PHP语言写成** 
* **纯文本存储，不需要数据库** 
* **可与多个CMS整合，比如WordPress** 

    注意：有些介绍中说Dokuwiki对文章标题的中文化支持不好，这个问题在现在的版本中已经不存在了。

####其他特点

Dokuwiki是一个简洁小型的wiki程序，如果你用过Mediawiki可能会觉得它缺少某些功能，不过这些“缺少的功能”大都可以通过插件实现。

* **权限控制：Dokuwiki拥有非常不错的ACL机制，简洁有效** 
* **基本的协作功能：页面锁定（防止冲突）、无限制的页面修订版本、最近更改、差异比较。不过没有Mediawiki中的“待添加页面”之类功能，但可以加装插件实现。** 
* **命名空间：文章以命名空间分类，自动生成索引** 
* **编辑：有工具栏和快捷键，可以分段编辑，自动生成文章目录。（文章目录没有Mediawiki中“1，1.1”这样的效果）** 
* **Feed：支持RSS和ATOM，可以通过调整Feed URL参数获得不同效果** 
* **搜索：可以全文搜索** 
* **缓存：内置的页面缓存** 

###2. 安装apache2、php5

    $ sudo apt-get install apache2 php5

###3. 下载dokuwiki

    $ cd /var/www/html/
    $ wget -c  http://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
    $ tar -xvf dokuwiki-stable.tgz    //解压缩并改名为dokuwiki
    
###4. 设置权限

    $ cd dokuwiki
    $ sudo chown -R www-data:www-data data conf // www-data为apache2安装时自动创建的用户

###5. 安装
* 在浏览器输入：http://127.0.0.1/dokuwiki/install.php
* 先在右上角选好语言，简体中文zh
* 填写好网站的名称，管理员等，协议等
* 安装完后，为了安全，要删除install.php文件，然后再用刚才设置的用户名密码登陆

###6. 安全配置
访问http://127.0.0.1/dokuwiki/data/pages/wiki/dokuwiki.txt, 如果你能通过上面这个链接，访问到dokuwiki.txt文件，那么表明你的网站的数据是不安全，因为dokuwiki是文本数据库，也就是别人可以直接拖库了。
官方要求是data   conf   bin   inc, 这四个目录是不能通过web访问浏览的。所以，我们要设置这些目录的权限，保证网站的数据安全。
sudo vi /etc/apache2/sites-available/default
将
<Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride none
                Order allow,deny
                allow from all
</Directory>
改为
<Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride all
                Order allow,deny
                allow from all
</Directory>
sudo service apache2 restart

###参考
* <a href="https://www.lainme.com/doku.php/blog/2010/04/dokuwiki%E4%BB%8B%E7%BB%8D">Dokuwiki介绍</a>
* <a href="http://www.dabu.info/dokuwiki-installation-and-configuration.html">dokuwiki安装与配置教程</a>
* <a href="http://phylab.fudan.edu.cn/doku.php?id=howtos:wiki_start0">从零开始学习wiki建站</a>
* <a href="https://www.dokuwiki.org/security#web_access_security">dokuwiki Security</a>