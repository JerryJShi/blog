# Linux基础知识
## 计算机系统

计算机是由硬件系统和软件体统组成。  
硬件系统：根据冯诺依曼体系结构，可以分为运算器（ALU）,控制器，存储器，输入和输出设备。  
软件系统：系统软件，应用软件。

服务器是计算机的一种，是为客户端提供各种服务的高性能计算机。既可以是具体的计算机硬件，也可以是虚拟的云主机。

操作系统（Operating System）组成部分：
硬件驱动，进程管理，内存管理，网络管理，安全管理，文件管理。

![系统调用](https://user-images.githubusercontent.com/43201545/45917125-56341280-bea3-11e8-993d-2a3f95812d6d.jpg)

## 用户空间和内核空间

用户空间：User space  
用户程序的运行空间，程序不能直接调用系统资源，必须通过系统接口，执行系统调用，才能向内核发出指令，调用资源。

内核空间：Kernel space
是Linux内核的运行空间，用于调用系统资源。

![用户空间和内核空间](https://user-images.githubusercontent.com/43201545/45917120-4d434100-bea3-11e8-9c4c-357b11244e2a.jpg)

操作系统分类：桌面操作系统，服务器操作系统，移动操作系统。

服务器操作系统：Windows，Linux，Unix 三大类。

Linux最为一款免费使用，自由传播的操作系统，受到越来越多人的青睐，市场占有率逐年上升，所以掌握Linux是成为一名优秀IT人才的必备技能。

### Linux与Windows的区别：  
1. Linux 最主要的哲学思想是一切皆文件（包括硬件），而Windows则是一切皆图形，所见及所得。
2. Linux 是开源的，免费使用的操作系统，而Windows是比源的，需要付费获取微软认证才能使用的操作系统。
3. Linux 是多用户操作系统，同一时间允许多个用户登录，Windows是单用户操作系统。同一时间只允许一个用户登录。
4. Linux 配置数据存储在文件中，/etc 目录下，Windows配置数据存储在注册表中。

## Linux发行版
* slackware
  * SLES
  * OpenSuse
* debian
  * ubuntu
  * mint
* redhat
  * RHEL
  * CenOS
  * Fedora
* Android

## 开源协议
GPlv2, GPLv3, LGPL, Apache, BSD, Mozilla, MIT

# Linux系统使用
## CentOS系统基础知识

### 用户登录：  
分为 root用户和普通用户。

### 终端分类：  
设备终端，物理终端，虚拟终端，图形终端，串行终端，伪终端。

### SHELL是什么  
1. Linux系统的用户界面，提供了用户与内核进行交互操作的一种接口，它把用户输入的命令传入操作系统内核去执行。
2. Linux的命令解释器（command interpreter）
3. 高级程序设计语言

### 命令格式：
COMMAND [OPTIONS...] [ARGUMENTS...]  
命令分为内部命令和外部命令。
内部命令：shell程序自带的命令，这些命令由shell程序识别并在shell程序内部完成运行，Linux系统启动后shell被加载的系统内存中，其执行速度比外部命令块，CentOS默认使用bash，可以使用echo $SHELL 查看当前使用的shell程序。  
```bash
[root@CentOS6-1 ~]#echo ${SHELL}
/bin/bash
[root@CentOS6-1 ~]#echo $SHELL
/bin/bash
[root@CentOS6-1 ~]#cat /etc/shells
/bin/sh
/bin/bash
/sbin/nologin
/bin/dash
/bin/tcsh
/bin/csh
[root@CentOS6-1 ~]#/bin/csh
[root@CentOS6-1 ~]# > f1
Invalid null command.
[root@CentOS6-1 ~]# exit
exit
[root@CentOS6-1 ~]#> f1
[root@CentOS6-1 ~]#ls
anaconda-ks.cfg  Documents  f1           install.log.syslog  Pictures  Templates
Desktop          Downloads  install.log  Music               Public    Videos
```
查看内部命令  
```bash
[root@CentOS6-1 ~]#help
[root@CentOS6-1 ~]#enable
```
外部命令：在文件系统路径下有对应的可执行程序文件
```bash
[root@CentOS6-1 ~]#which cat
/bin/cat
[root@CentOS6-1 ~]#whereis cat
cat: /bin/cat /usr/share/man/man1p/cat.1p.gz /usr/share/man/man1/cat.1.gz
```
查看命令是内部命令还是外部命令,使用type 命令
```bash
[root@CentOS6-1 ~]#type echo
echo is a shell builtin
[root@CentOS6-1 ~]#type man
man is /usr/bin/man
```
### Hash表  
&ensp;&ensp;&ensp;&ensp;系统初始hash表为空，当外部命令执行时，默认会从PATH路径下寻找该命令，找到后会将这条命令的路径记录到hash表中，当再次使用该命令时，shell解释器首先会查看hash表，存在就执行，如果不存在，就去PATH路径下寻找。

### 别名  
为命令指定一个名字，简化操作，提高效率。
```bash
[root@CentOS6-1 ~]#alias vimnet='vim /etc/sysconfig/network-scripts/ifcfg-eth0'
```
定义的别名仅对当前shell进程有效，想要永久有效，必须保存在配置文件中。

如果别名同原命令同名，要执行原命令，可使用
\ALIASNAME
"ALIASNAME"
'ALIASNAME'
command ALIASNAME
/path/command

### 配置文件
有两类配置文件，~/.bashrc对当前用户有效，/etc/bashrc对所有用户有效。这两个文件功用为定义本地变量和命令别名。  
仅root用户可以修改全局配置文件，普通用户只能修改用户目录下的配置文件。

### 命令的执行顺序
alias(当与内部命令同名时) --> 内部命令(shell) --> hash表(记录执行过的外部命令的路径) --> $PATH --> 提示命令无法找到

注意：  
+ 多个选项以及多参数和命令之间使用空白字符分隔  
+ 取消和结束命令执行：Ctrl+c，Ctrl+d  
+ 多个命令可以用;符号分开  
+ 一个命令可以用\分成多行

### 日期和时间

系统时钟，硬件时钟

时间修改格式：date MMDDHHmm[[CC]YY][.ss]  
例如：date 091921112018.30
 
查看时区：  
CentOS6  cat /etc/sysconfig/clock  
CentOS7  timedatectl | grep "Time zone"

hwclock，clock: 显示硬件时钟  
-s, --hctosys以硬件时钟为准，校正系统时钟  
-w, --systohc以系统时钟为准，校正硬件时钟

练习  
1、显示当前时间，格式：2016-06-18 10:20:30
```bash
[root@CentOS6-1 ~]#date +"%F %T"
2018-09-22 22:07:25
```

2、显示前天是星期几
```bash
[root@CentOS6-1 ~]#date -d "-2 day" +%w
4
```
3、设置当前日期为2019-08-0706:05:10
```bash
[root@CentOS6-1 ~]#date 080706052019.10
Wed Aug  7 06:05:10 CST 2019
[root@CentOS6-1 ~]#date +"%F %T"
2019-08-07 06:05:24
```
同步时间
ntpdate xxx.xxx.xxx.xxx（其他主机IP）

### echo命令

echo会将输入的字符串送往标准输出。输出的字符串间以空白字符隔开, 并在最后加上换行号。

选项：  
- -E （默认）不支持\解释功能
- -n 不自动换行
- -e 启用\字符的解释功能

echo "$VAR_NAME” 变量会替换，弱引用
echo '$VAR_NAME’ 变量不会替换，强引用

' ' 弱引用，直接引用字符，不取变量值
" " 强引用，取变量值
` ` , $() 命令引用，把一个命令的输出打印给另一个命令的参数  

{ } 打印重复字符串  
```bash
[root@centos7-1 data]#echo file{1,3,5}
file1 file3 file5
[root@centos7-1 data]#echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z
[root@centos7-1 data]#echo {z..a}
z y x w v u t s r q p o n m l k j i h g f e d c b a
[root@centos7-1 data]#echo {aa..zz}
{aa..zz}
[root@centos7-1 data]#echo {aa,bb,cc,dd}
aa bb cc dd
[root@centos7-1 data]#echo {a,h,e,b,d}
a h e b d
```
### TAB 键  
命令补全：用户给定的字符串只有一条惟一对应的命令，直接补全，否则，再次Tab会给出列表  
路径补全：如果惟一：则直接补全，否则：再次Tab给出列表

### 命令行历史
登录shell时，会读取命令历史文件中记录下的命令~/.bash_history。  
登录进shell后新执行的命令只会记录在缓存中；这些命令会在用户退出shell时“追加”至命令历史文件中。

重复执行前一条命令的方法：
- 按上方向键，并回车执行
- 按!!，并回车执行
- 输入!-1，并回车执行
- 按Ctrl+p，并回车执行

!:0 执行前一条命令（除去参数）  
!n 执行history命令输出对应序号n的命令  
!-n 执行history历史中倒数第n条命令
!string 重复前一个以“string”开头的命令  
!?string 重复前一个包含“string”的命令  
!string:p 仅打印命令历史，而不执行  
!$:p 打印输出!$ （上一条命令的最后一个参数）的内容  
!*:p打印输出!*（上一条命令的所有参数）的内容  
^string删除上一条命令中的第一个string  
^string1^string2将上一条命令中的第一个string1替换为string2  
!:gs/string1/string2将上一条命令中所有的string1都替换为string2  
ctrl-r来在命令历史中搜索命令  
（reverse-i-search）`’：  
Ctrl+g：从历史搜索模式退出  
要重新调用前一个命令中最后一个参数  
- !$ 表示
- Esc, .（点击Esc键后松开，然后点击. 键）
- Alt+ .（按住Alt键的同时点击. 键）

#### 相关环境变量
HISTSIZE：命令历史记录的条数  
HISTFILE：指定历史文件，默认为~/.bash_history  
HISTFILESIZE：命令历史文件记录历史的条数    
HISTTIMEFORMAT=“%F %T “ 显示时间    
HISTIGNORE=“str1:str2*:… “ 忽略str1命令，str2开头的历史  
控制命令历史的记录方式：  
环境变量：HISTCONTROL  
ignoredups默认，忽略重复的命令，连续且相同为“重复”  
ignorespace忽略所有以空白开头的命令  
ignoreboth相当于ignoredups, ignorespace的组合  
erasedups删除重复命令  

### bash的快捷键
Ctrl + c终止命令
Ctrl + d退出
Ctrl + l清屏，相当于clear命令  
Ctrl + o执行当前命令，并重新显示本命令   
Ctrl + s阻止屏幕输出，锁定  
Ctrl + q允许屏幕输出  
Ctrl + c终止命令  
Ctrl + z挂起命令  
Ctrl + a光标移到命令行首，相当于Home  
Ctrl + e光标移到命令行尾，相当于End   
Ctrl + f光标向右移动一个字符  
Ctrl + b光标向左移动一个字符  
Alt + f光标向右移动一个单词尾  
Alt + b光标向左移动一个单词首  
Ctrl + xx光标在命令行首和光标之间移动  
Ctrl + u从光标处删除至命令行首  
Ctrl + k从光标处删除至命令行尾  
Alt + r 删除当前整行  
Ctrl + w从光标处向左删除至单词首   
Alt + d从光标处向右删除至单词尾  
Ctrl + d删除光标处的一个字符  
Ctrl + h删除光标前的一个字符  
Ctrl + y将删除的字符粘贴至光标后  
Alt + c从光标处开始向右更改为首字母大写的单词  
Alt + u从光标处开始，将右边一个单词更改为大写  
Alt + l从光标处开始，将右边一个单词更改为小写  
Ctrl + t交换光标处和之前的字符位置  
Alt + t交换光标处和之前的单词位置  
Alt + N提示输入指定字符后，重复显示该字符N次  
注意：Alt组合快捷键经常和其它软件冲突

### 其他命令
startx 切换GUI模式，不提示登录  
init 5 切换GUI模式，提示登录  
init 3 切换CLI模式  
init 0 关机  
init 6 重启  
runlevel 查看当前登录类型
Linux 中 Ctrl+Alt+[F1-F6] 本地控制台都是字符界面,F7 是图形界面。  
不过字符界面下面，有的时候 Ctrl 可以不按，Alt + Fx 就可以切换了。

lscpu 查看CPU信息
```bash
[root@centos7-1 data]#lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    1
Core(s) per socket:    2
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 142
Model name:            Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz
Stepping:              10
CPU MHz:               1800.001
CPU max MHz:           0.0000
CPU min MHz:           0.0000
BogoMIPS:              3600.00
Hypervisor vendor:     VMware
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              6144K
NUMA node0 CPU(s):     0,1
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc aperfmperf eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch epb fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid rdseed adx smap xsaveopt xsavec xgetbv1 dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp
```

lsblk,df 查看硬盘分区
```bash
[root@centos7-1 data]#lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  200G  0 disk 
├─sda1   8:1    0    1G  0 part /boot
├─sda2   8:2    0   50G  0 part /
├─sda3   8:3    0   30G  0 part /data
├─sda4   8:4    0    1K  0 part 
└─sda5   8:5    0    4G  0 part [SWAP]
sr0     11:0    1  8.8G  0 rom  /run/media/root/CentOS 7 x86_64
[root@centos7-1 data]#df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G  3.9G   47G   8% /
devtmpfs        896M     0  896M   0% /dev
tmpfs           911M     0  911M   0% /dev/shm
tmpfs           911M   10M  901M   2% /run
tmpfs           911M     0  911M   0% /sys/fs/cgroup
/dev/sda3        30G   33M   30G   1% /data
/dev/sda1      1014M  165M  850M  17% /boot
tmpfs           183M  8.0K  183M   1% /run/user/0
/dev/sr0        8.8G  8.8G     0 100% /run/media/root/CentOS 7 x86_64
```
mii-tool 查看网卡信息
```bash
[root@centos7-1 data]#mii-tool ens33
ens33: negotiated 1000baseT-FD flow-control, link ok
```
free 查看内存信息
```bash
[root@centos7-1 data]#free -h
              total        used        free      shared  buff/cache   available
Mem:           1.8G        228M        667M         10M        926M        1.3G
Swap:          4.0G        264K        4.0G
```
lsb_release -a 查看centos版本（CentOS6）
```bash
[root@CentOS6-1 ~]#lsb_release -a
LSB Version:	:base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	CentOS
Description:	CentOS release 6.10 (Final)
Release:	6.10
Codename:	Final
```
cat /etc/centos-release 查看centos版本
```bash
（CentOS6）
[root@CentOS6-1 ~]#cat /etc/centos-release
CentOS release 6.10 (Final)
（CentOS7）
[root@centos7-1 data]#cat /etc/centos-release
CentOS Linux release 7.5.1804 (Core) 
```
ifconfig 查看本机IP
```bash
[root@centos7-1 data]#ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.64.130  netmask 255.255.255.0  broadcast 192.168.64.255
        inet6 fe80::85ad:5a28:117d:9b02  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:7b:c3:98  txqueuelen 1000  (Ethernet)
        RX packets 3914  bytes 1339666 (1.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1327  bytes 134071 (130.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
tty 查看终端设备
```bash
[root@centos7-1 data]#tty
/dev/pts/0
```
sleep 延时命令  
`sleep 100` 延时100s

w 系统当前所有的登录会话及所做的操作
```bash
[root@centos7-1 data]#w
 11:11:50 up  2:27,  2 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.64.1     08:52    6.00s  0.61s  0.09s w
```
who 系统当前所有的登录会话
```bash
[root@centos7-1 data]#who
root     pts/0        2018-09-23 08:52 (192.168.64.1)
[root@centos7-1 data]#who am i
root     pts/0        2018-09-23 08:52 (192.168.64.1)
[root@centos7-1 data]#who is a
root     pts/0        2018-09-23 08:52 (192.168.64.1)

```
whoami 显示当前登录有效用户
```bash
[root@centos7-1 data]#whoami
root
```

whatis 简要说明命令的功能
```bash
[root@centos7-1 data]#whatis issue
issue (5)            - prelogin message and identification file
[root@centos7-1 data]#whatis ls
ls (1)               - list directory contents
ls (1p)              - list directory contents
```
du 查看分区容量
```bssh
[root@centos7-1 data]#du -sh /boot
133M	/boot
```
ps aux 列出系统中当前运行的进程
```bash

[root@centos7-1 data]#ps aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.3 210244  6988 ?        Ss   08:44   0:09 /usr/lib/systemd/systemd --swit
root          2  0.0  0.0      0     0 ?        S    08:44   0:00 [kthreadd]
root          3  0.0  0.0      0     0 ?        S    08:44   0:00 [ksoftirqd/0]
root          5  0.0  0.0      0     0 ?        S<   08:44   0:00 [kworker/0:0H]
root          7  0.0  0.0      0     0 ?        S    08:44   0:00 [migration/0]
```

pwd 查看当前路径
```bash
[root@centos7-1 data]#pwd
/data
```
ls 查看当前路径下的文件和文件夹
ll 查看当前路径下文件和文件夹的详细信息
```bash
[root@centos7-1 ~]#ls
anaconda-ks.cfg  Documents  initial-setup-ks.cfg  Pictures  Templates
Desktop          Downloads  Music                 Public    Videos
[root@centos7-1 ~]#ll
total 8
-rw-------. 1 root root 1883 Sep 19 13:31 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Sep 23 10:22 Desktop
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Documents
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Downloads
-rw-r--r--. 1 root root 1914 Sep 19 13:33 initial-setup-ks.cfg
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Music
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Pictures
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Public
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Templates
drwxr-xr-x. 2 root root    6 Sep 19 13:33 Videos
```
source 或. 使配置文件生效
`source .bashrc` 或 `. .bashrc`

rz 接收文件，sz 发送文件

iconv -l 显示系统中的字符集
iconv -f gb2312 w.txt -o l.txt 把GB2312编码转换为Unicode编码
```bash
[root@centos7-1 data]#rz

[root@centos7-1 data]#ls
win.txt
[root@centos7-1 data]#cat win.txt 
í¸羌Խ
[root@centos7-1 data]#iconv -f gb2312 win.txt -o lin.txt
[root@centos7-1 data]#ls
lin.txt  win.txt
[root@centos7-1 data]#cat lin.txt 
马哥教育
```
hostname 查看主机名
id -u 查看当前登录用户的UID
hexdump -C win.txt 显示文件二进制代码
```bash
[root@centos7-1 data]#hexdump -C w.txt
00000000  d6 d0 0d 0a                                       |....|
00000004
[root@centos7-1 data]#hexdump -C l.txt
00000000  e4 b8 ad 0d 0a                                    |.....|
00000005
```
`nmcli connection modify ens33 connection.autoconnect yes` 系统启动后网卡自动连接，（CentOS7）

## 命令帮助
内部命令：help COMMAND 或 man bash  
外部命令：：
1. COMMAND --help
COMMAND -h
2. 使用手册(manual)
man COMMAND
3. 信息页
info COMMAND
4. 程序自身的帮助文档
README
INSTALL
ChangeLog
5. 程序官方文档
官方站点：Documentation
7. 发行版的官方文档
8. Google

### --help和-h选项
date--help  
Usage:date[OPTION]...[+FORMAT]or: date[-u|--utc|--universal][MMDDhhmm[[CC]YY][.ss]]  
[]表示可选项  
CAPS或<>表示变化的数据  
...表示一个列表  
x |y| z的意思是“x或y或z“  
-abc的意思是-a -b –c  
{ } 表示分组

### man帮助
man帮助分为如下章节：
1. 用户命令
2. 系统调用
3. C库调用
4. 设备文件及特殊文件
5. 配置文件格式
6. 游戏
7. 杂项
8. 管理类的命令
9. Linux 内核API

#### 段落说明
- NAME 名称及简要说明
- SYNOPSIS 用法格式说明
- []可选内容
- <> 必选内容
- a|b二选一
- { }分组
- ...同一内容可出现多次
- DESCRIPTION 详细说明
- OPTIONS 选项说明
- EXAMPLES 示例
- FILES 相关文件
- AUTHOR 作者
- COPYRIGHT版本信息
- REPORTING BUGS bug信息
- SEE ALSO 其它帮助参考

man命令的操作方法：使用less命令实现  
space, ^v, ^f, ^F: 向文件尾翻屏  
b, ^b: 向文件首部翻屏  
d, ^d: 向文件尾部翻半屏  
u, ^u: 向文件首部翻半屏  
RETURN, ^N, e, ^E or j or ^J: 向文件尾部翻一行  y or ^Y or ^P or k or ^K：向文件首部翻一行  
q: 退出  
#：跳转至第#行  
1G: 回到文件首部  
G：翻至文件尾部  

搜索关键字  
/KEYWORD:
以KEYWORD指定的字符串为关键字，从当前位置向文件尾部搜索；不区分字符大小写；  
n: 下一个
N：上一个  
?KEYWORD:
以KEYWORD指定的字符串为关键字，从当前位置向文件首部搜索；不区分字符大小写；  
n: 跟搜索命令同方向，下一个
N：跟搜索命令反方向，上一个

练习
1、在本机字符终端登录时，除显示原有信息外，再显示当前登录终端号，主机名和当前时间
```bash
[root@centos7-1 ~]#nano /etc/issue
  GNU nano 2.3.1                   File: /etc/issue                                              

\S
Kernel \r on an \m
\n
\l
\d \t
```
2、今天18：30自动关机，并提示用户
```bash
[root@CentOS6-1 ~]#shutdown -P 18:30 Server will poweroff at 14:57,please save the data.
```

## SCREEN 命令
**screen** 是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

### 简单用法
创建新screen会话  
screen –S [SESSION]  
加入screen会话  
screen –x [SESSION]  
退出并关闭screen会话  
exit  
剥离当前screen会话  
Ctrl+a,d  
显示所有已经打开的screen会话  
screen -ls  
恢复某screen会话  
screen -r [SESSION]

screen 的详细用法请参照：[screen 命令详解](http://man.linuxde.net/screen)
