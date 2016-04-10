---
layout: default
title: "备忘"
---
记录工作中遇到的一些小问题及解决办法，以防忘记后重头再找答案。

### Linux
* **1. 内网linux获取出口公网IP**

~~~
$ curl http://members.3322.org/dyndns/getip
$ wget http://members.3322.org/dyndns/getip 
$ cat getip
~~~

* **2. 查看当前文件夹大小** 

~~~
$ du -skh
~~~

* **3. 删除空文件** 

~~~
$ find / -type f -size 0 -exec rm -rf {} \;
~~~

* **4. 获取当前内网IP地址**

~~~
$ ifconfig |awk -F"[ ]+|[:]" 'NR==2 {print $4}'
~~~

* **5. 以内存大小排序列出进程**

~~~
$ ps aux --sort=rss |sort -k 6 -rn
~~~

* **6. 目录中大量文件删除**

~~~
$ ls | xargs rm
~~~

* **7. 查找进程pid并kill**

~~~
$ pgrep nginx|xargs kill
$ pidof nginx|xargs kill
~~~

* **8. 查看linux版本**

~~~
$ cat /proc/version
$ uname -a
$ uname -r
$ cat /etc/os-release
~~~

* **9. 查看linux系统运行时间**

~~~
$ uptime
~~~

* **10. 查看linux系统进程使用情况**

~~~
$ top -bn1
~~~

* **11. 查看用户与负载**

~~~
$ w
$ who
$ users
~~~

* **12. Shell特殊变量：Shell $0, $#, $*, $@, $?, $$**

~~~
$0 	当前脚本的文件名
$n 	传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。
$# 	传递给脚本或函数的参数个数。
$* 	传递给脚本或函数的所有参数。
$@ 	传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。
$? 	上个命令的退出状态，或函数的返回值。
$$ 	当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。
~~~

* **13. Shell替换**

~~~
命令替换的语法：

    `command`

注意是反引号，不是单引号，这个键位于 Esc 键下方。
~~~

~~~
 变量替换
变量替换可以根据变量的状态（是否为空、是否定义等）来改变它的值

可以使用的变量替换形式：
${var} 	变量本来的值
${var:-word} 	如果变量 var 为空或已被删除(unset)，那么返回 word，但不改变 var 的值。
${var:=word} 	如果变量 var 为空或已被删除(unset)，那么返回 word，并将 var 的值设置为 word。
${var:?message} 	如果变量 var 为空或已被删除(unset)，那么将消息 message 送到标准错误输出，可以用来检测变量 var 是否可以被正常赋值。
若此替换出现在Shell脚本中，那么脚本将停止运行。
${var:+word} 	如果变量 var 被定义，那么返回 word，但不改变 var 的值。
~~~

* **14. Shell运算符**

~~~
算术运算符列表

运算符 	说明 	举例
+ 	加法 	`expr $a + $b` 结果为 30。
- 	减法 	`expr $a - $b` 结果为 10。
* 	乘法 	`expr $a \* $b` 结果为  200。
/ 	除法 	`expr $b / $a` 结果为 2。
% 	取余 	`expr $b % $a` 结果为 0。
= 	赋值 	a=$b 将把变量 b 的值赋给 a。
== 	相等。用于比较两个数字，相同则返回 true。 	[ $a == $b ] 返回 false。
!= 	不相等。用于比较两个数字，不相同则返回 true。 	[ $a != $b ] 返回 true。
~~~

~~~
关系运算符列表 

运算符 	说明 	举例
-eq 	检测两个数是否相等，相等返回 true。 	[ $a -eq $b ] 返回 true。
-ne 	检测两个数是否相等，不相等返回 true。 	[ $a -ne $b ] 返回 true。
-gt 	检测左边的数是否大于右边的，如果是，则返回 true。 	[ $a -gt $b ] 返回 false。
-lt 	检测左边的数是否小于右边的，如果是，则返回 true。 	[ $a -lt $b ] 返回 true。
-ge 	检测左边的数是否大等于右边的，如果是，则返回 true。 	[ $a -ge $b ] 返回 false。
-le 	检测左边的数是否小于等于右边的，如果是，则返回 true。 	[ $a -le $b ] 返回 true。
~~~

~~~
布尔运算符列表 

运算符 	说明 	举例
! 	非运算，表达式为 true 则返回 false，否则返回 true。 	[ ! false ] 返回 true。
-o 	或运算，有一个表达式为 true 则返回 true。 	[ $a -lt 20 -o $b -gt 100 ] 返回 true。
-a 	与运算，两个表达式都为 true 才返回 true。 	[ $a -lt 20 -a $b -gt 100 ] 返回 false。
~~~

~~~
字符串运算符列表 

运算符 	说明 	举例
= 	检测两个字符串是否相等，相等返回 true。 	[ $a = $b ] 返回 false。
!= 	检测两个字符串是否相等，不相等返回 true。 	[ $a != $b ] 返回 true。
-z 	检测字符串长度是否为0，为0返回 true。 	[ -z $a ] 返回 false。
-n 	检测字符串长度是否为0，不为0返回 true。 	[ -z $a ] 返回 true。
str 	检测字符串是否为空，不为空返回 true。 	[ $a ] 返回 true。
~~~

~~~
文件测试运算符列表 
操作符 	说明 	举例
-b file 	检测文件是否是块设备文件，如果是，则返回 true。 	[ -b $file ] 返回 false。
-c file 	检测文件是否是字符设备文件，如果是，则返回 true。 	[ -b $file ] 返回 false。
-d file 	检测文件是否是目录，如果是，则返回 true。 	[ -d $file ] 返回 false。
-f file 	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 	[ -f $file ] 返回 true。
-g file 	检测文件是否设置了 SGID 位，如果是，则返回 true。 	[ -g $file ] 返回 false。
-k file 	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。 	[ -k $file ] 返回 false。
-p file 	检测文件是否是具名管道，如果是，则返回 true。 	[ -p $file ] 返回 false。
-u file 	检测文件是否设置了 SUID 位，如果是，则返回 true。 	[ -u $file ] 返回 false。
-r file 	检测文件是否可读，如果是，则返回 true。 	[ -r $file ] 返回 true。
-w file 	检测文件是否可写，如果是，则返回 true。 	[ -w $file ] 返回 true。
-x file 	检测文件是否可执行，如果是，则返回 true。 	[ -x $file ] 返回 true。
-s file 	检测文件是否为空（文件大小是否大于0），不为空返回 true。 	[ -s $file ] 返回 true。
-e file 	检测文件（包括目录）是否存在，如果是，则返回 true。 	[ -e $file ] 返回 true。
~~~
