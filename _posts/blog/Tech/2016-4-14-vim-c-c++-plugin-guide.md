---
layout: post
category: "Tech"
title:  "vim c/c++ plugin使用指南"
description: 本文介绍了vim c/c++ plugin使用指南。
tags: [vim plugin guide]
---

### 1. 目标  ###
  学习linux下像source insight一样快速高效地阅读和编写c/c++源码, 同时速度和效率更高.

~~~
source insight常用功能:
1 建立工程index, 代码快速跳转.
2 源码高亮显示, 显示行号.
3 全局搜索关键字.
4 分框显示, 一般左边显示本文件函数或宏定义, 中间显示源码, 右边显示工程文件树, 下面是函数定义.
5 缺点: 补全功能比较弱和不支持UTF-8.
~~~

### 2. 使用langsim的现成vim配置 ###

#### 2.1 下载安装 ####

~~~
apt-get install ctags cscope wget unzip 
wget https://github.com/langsim/vim-ide/archive/master.zip -O master.zip
unzip -o master.zip
cp -rf vim-ide-master/.vim* ~
~~~

#### 2.2 使用说明 ####

1. open project: 打开工程
    * cd into project root dir，vim (press enter button), must in project root dir. press F2 to open file tree, and select file to open.
    * 在终端中切换到工程根目录, 输入vim并按enter键; 按F2去打开目录树, 再选择文件打开.
2. update index: 更新索引
    * first time open project or update some code in project, press F5 to update index，the index function is same to source insight index.
    * 第一次打开工程或者更新一些工程里的代码后, 按F5去更新索引, 函数索引和source insight索引一样.
3. read c/c++ code: 阅读c/c++代码
    * jump 跳转
        * F3         throught tagbar to jump to another function in the file
        * gd         jump to local varibale defination
        * ctrl-]     jump to variable or function defination in project。(can't jump to local variable defination) (:ts word)
        * [[         jump to start of function
        * ][         jump to end of function
        * ctrl-o     jump to cursor last position
        * ctrl-i     return after press ctrl-o
        * ctrl-h     move cursor to left window in vim
        * ctrl-j     move cursor to down window in vim
        * ctrl-k     move cursor to up window in vim
        * ctrl-l     move cursor to right window in vim
    * search
        * ctrl-[ s   search variable or function in project (:cs find s word)
        * ctrl-[ t   search text in project (:cs find t word)
        * F7         highlight word under the cursor 
        * /word      search word in the file
    * open another file in project
        * F2         throught file tree
        * F8         throught opened file list
        * F4         switch of include file and implement file
    * display
        * F6         switch of display invisible character or not 
        * F10        change tab display as space mount
4. write c/c++ code：
    * align
        * =          align selected code
        * ==         align current line code
    * comment
        * ,          comment selected code
        * .          uncomment selected code
    * replace
        * :%s /word1/word2/g  replace word1 to word2 in the file
    * expand tab to space or not
        * F9         exapnd tab to space or not

### 3. 使用ruchee的现成vim配置 ###

#### 3.1 下载安装 ####

1. `sudo apt-get install vim-gtk exuberant-ctags` [其他非 Debian 系的 Linux 请使用其自己的包管理器进行安装]

2. 删除个人主目录下的 .vim 文件夹和 .vimrc 文件，如果没有则不需要执行删除动作 [使用命令 `rm -rf ~/.vim ~/.vimrc`，请注意备份]

3. 使用 Git 下载本项目，然后将本项目的子目录拷贝到个人主目录下，取代已删文件的位置，然后将 vimfiles、_vimrc 改名为 .vim、.vimrc [命令为 `git clone https://github.com/ruchee/vimrc.git`、`mv ~/vimrc/vimfiles ~/.vim`、`mv ~/vimrc/_vimrc ~/.vimrc`]（也可以点击本页面的 Download ZIP 按钮下载）

4. 可上 `https://github.com/ruchee/backup/blob/master/download/MONACO.TTF?raw=true` 下载 Monaco 字体，下载后使用命令 `mv MONACO.TTF ~/.fonts` 将其丢到 `~/.fonts` 目录即可

5. 使用任意文本编辑器打开 .vimrc，将名字、邮箱、网址等全部替换为你自己的信息，如遇路径不同也全部替换为你本机的实际路径

6. 如此这般就配置好了，尽情享受编码的乐趣吧，使用说明全部集中在 .vimrc 文件的头部，配置的后半部分全是各插件的具体配置项，初学无需理会

### 参考  ###
* <a href="https://github.com/langsim/vim-ide">langsim vim-ide </a>
* <a href="https://github.com/ruchee/vimrc">ruchee vimrc </a>

