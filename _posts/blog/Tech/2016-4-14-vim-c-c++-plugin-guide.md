---
layout: post
category: "Tech"
title:  "vim config使用指南 for c/c++"
description: 本文介绍了vim config使用指南。
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

#### 1.1 我使用的vim config  ####
<a href="https://github.com/richardhmm/HMMCodeRepository/tree/master/vimConfig">my vim config </a>

* 常用命令

~~~
1 F2打开tags列表窗口(居左部).
2 F3打开文件浏览窗口(居右部).
3 F4打开最近阅读的源码(居下部).
4 F5创建或更新ctags和cscope.
5 Ctrl-o返回上次位置.
6 :cs find s wrod查找工程内变量或函数. 其快捷键为",ss": 查找光标下的变量或函数.
7 :cs find t word查找工程内文本. 其快捷键为",st": 查找光标下的文本.
7.1 :cs find f word查找工程内文件. 其快捷键为",sf": 查找光标下的文件.
8 Ctrl-]跳转到工程内变量或函数的定义处.
8.1 ctrl+t：返回上一个查找的地方.
9 移动操作
gg         将光标定位到文件第一行起始位置。
G          将光标定位到文件最后一行起始位置。
NG或Ngg    将光标定位到第 N 行的起始位置。

10 分屏与标签页
:split（可用缩写 :sp）            上下分屏
:vsplit（可用缩写 :vsp）          左右分屏
vim -On file1 file2...   打开 file1 和 file2 ，垂直分屏
vim -on file1 file2...   打开 file1 和 file2 ，水平分屏
Ctrl+ww                  切换到当前分屏的左边一屏,循环操作
vimdiff file1 file2      Vim里分屏显示两个文件内容的对比结果

11 选择文本块
v：按字符选择。经常使用的模式，所以亲自尝试一下它。
V：按行选择。这在你想拷贝或者移动很多行的文本的时候特别有用。
CTRL＋v：按块选择。非常强大，只在很少的编辑器中才有这样的功能。你可以选择一个矩形块，并且在这个矩形里面的文本会被高亮。

复制选定块到缓冲区，用y；复制整行，用yy
剪切选定块到缓冲区，用d；剪切整行用dd
粘贴缓冲区中的内容，用p

12 vimgrep
vimgrep /匹配模式/[g][j] 要搜索的文件/范围 
g：表示是否把每一行的多个匹配结果都加入
j：表示是否搜索完后定位到第一个匹配位置

vimgrep /pattern/ %           在当前打开文件中查找
vimgrep /pattern/ *             在当前目录下查找所有
vimgrep /pattern/ **            在当前目录及子目录下查找所有
vimgrep /pattern/ *.c          查找当前目录下所有.c文件
vimgrep /pattern/ **/*         只查找子目录
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

### 参考  ###
* <a href="https://github.com/langsim/vim-ide">langsim vim-ide </a>
* <a href="http://blog.csdn.net/wooin/article/details/1858917">
手把手教你把Vim改装成一个IDE编程环境(图文)  </a>
* <a href="https://github.com/yangyangwithgnu/use_vim_as_ide">像 IDE 一样使用 vim </a>
* <a href="http://www.jianshu.com/p/bcbe916f97e1">Vim入门基础 </a>


