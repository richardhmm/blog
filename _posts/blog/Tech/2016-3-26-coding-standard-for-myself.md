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
1、内核、驱动等底层部分：函数和变量名称都使用lower_case命名风格。

~~~
void get_version(void);
bool is_man = TRUE;
~~~

2、应用部分：函数采用CamelCase命名风格；变量名称使用camelCase命名风格。

~~~
void GetVersion(void); // 函数
unsigned char aucName[32] = "richard"; // 变量, auc指unsigned char的数组
unsigned short usResult = 0; // 变量, us指unsigned short
unsigned int ulResult = 0; // 变量, ul指unsigned int
struct NEW_STRU stResult; // 变量, st指struct
char *pcName = NULL; // 变量, pc值char型pointer
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
 * @create Richard.hmm 2016.1.1
 * @modify Richard.hmm 2017.1.1 增加应用代码风格说明
 * @param[in] pucIn
 * @param[out] pucOut
 * @return 结果。
 */
~~~

### 参考  ###
* linux内核代码风格
