---
layout: post
category: "Tech"
title:  "Ubuntu下使用git 命令行管理coding.net上的开源库"
description: 本文介绍了Ubuntu下使用git 命令行管理coding.net上的开源库。
tags: [Ubuntu git coding.net]
---

### 0.参考 ### 
1. <a href="http://blog.csdn.net/u010842019/article/details/52738441"> Linux(Ubuntu)下安装使用git</a>

### 1.Shell命令行 Home路径安装git  ### 
sudo apt-get install git

### 2.Shell命令生成ssh秘钥 ### 
ssh-keygen -t rsa -C "xxx@gmail.com"    // 对应注册coding账号的email 或者能收到邮件的email

### 3.Home路径下复制生成的 id_rsa.pub 秘钥字符串 ### 
cat ~/.ssh/id_rsa.pub         //将打印出的字符串复制到剪切板

### 4.网站增加SSH公钥 ### 
登录到coding.net个人账户--SSH公钥中，按提示将RSA密钥保存到网站上。

## ---------------------从git克隆代码到本地------------------- ##

### 1.获取需要管理的repo  ### 
在coding.net官网找到一个要克隆到本地的 Repository仓库的路径，比如：https://git.coding.net/xxx/xxx.git

### 2.在SHELL命令下切换到对应本地代码的文件夹 ### 
mkdir XXXX
cd XXXX

### 3.执行命令 ### 
git clone https://git.coding.net/xxx/xxx.git

拉完进入本地仓库 
cd XXXX

### 4.git add 添加修改文件 ### 
使用“git commit”查看是否有修改的文件，根据提示使用git add 将相关文件或目录添加
git add xxx

### 5. 提交修改 ### 
git commit -m ”说明这次的提交“

### 6.上传 ### 
git  push
根据提示输入用户名和密码

