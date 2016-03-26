---
layout: post
category: "Tech"
title:  "写给自己的c语言编码规范"
description: 本文介绍了一套简单, 易操作的编码规范。
tags: [编码规范 c lua linux]
---

### 1. 目标  ###
  本文写给自己, 主要参考的linux内核风格, 提炼了常用的几个方面, 一个备忘录。

### 2. 约定 ###

#### 2.1 文件和目录 ####
文件和目录名称都使用camelCase命名风格。
~~~
versionInfo.h  ### h文件
versionInfo.c  ### c文件
commProject  ### 目录
~~~

#### 2.2 函数和变量 ####
函数和变量名称都使用小写字母下划线命名风格。
~~~
void get_version(void);
bool is_man = TRUE;
~~~

#### 2.3 宏和枚举定义 ####
宏和枚举值的命名采用UPPER_CASE风格，即所有字母都大写，单词之间使用下划线分隔。
~~~
#ifndef _TYPE_H_
#define _TYPE_H_
#endif /* _TYPE_H_ */

#define MAX_BUFF 1024
~~~

#### 2.4 注释 ####
函数头和文件头推荐采用javadoc注释规范。
~~~
/**
 * @file main.c
 * @brief main
 * @details
 * @author Copyright (C) 2015 Richard.hmm  <sunhyx@gmail.com>
 */
 
/**
 * @brief 函数功能。
 * @param[in] cmd_buff 命令指针。
 * @return 结果。
 */
~~~

### 参考  ###
* linux内核代码风格
