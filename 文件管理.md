# 文件管理
文件和目录被组织成一个单根倒置的树结构。  
文件系统从根目录下开始，用“/”表示。  
根文件系统（rootfs）  
文件名称严格区分大小写  
以.开头的文件为隐藏文件  
路径以“/”分隔  
文件有两类数据：元数据：metadata，数据：data  
文件系统分层结构：LSB Linux Standard Base  
[FHS: Filesystem Hierarchy Standard](http://www.pathname.com/fhs/)

## 文件名规则
文件名最长255个字符  
包括路径在内文件名称最长4095个字节  
文件类型可通过颜色区分：  
蓝色--目录，绿色--可执行文件，红色--压缩文件，浅蓝色--链接文件，白色--普通文件，粉色--socket文件  
除了“/”和NUL，所有字符都有效  
在标准Linux 文件系统（如ext4）中，文件名称大小写敏感。在Windows 文件系统（NTFS）中，文件名称大小写不敏感。  
注意：FAT文件系统，Liunx可以识别，但不区分大小写

## 文件系统结构
![目录结构](https://user-images.githubusercontent.com/43201545/45925669-7966cc80-bf4c-11e8-8f62-82d828bc095a.jpg)

/boot：引导文件存放目录，内核文件（vmlinuz）、引导加载器（bootloader,grub），初始化文件initramfs(initrd)  

/bin：供所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的程序  

/sbin：供系统管理使用的工具程序，不能关联至独立分区，OS启动即会用到的程序  

/dev：存储特殊文件或设备文件，设备有两种类型：字符设备（线性设备），块设备（随机设备）  

/etc：系统程序的配置文件，只能为静态文件  

/home：普通用户家目录的集中位置：一般每个普通用户的家目录默认为此目录下与用户名同名的子目录，
/home/USERNAME；（可选目录）  

/root：管理员的家目录，（可选目录）  

/lib：为系统启动或根文件系统上的应用程序（/bin,/sbin等）提供共享库，以及为内核提供内核模块（/lib/module 用于存储内核模块的目录） 

/lib64：64位系统特有的存放64位共享库的路径  

/media： 便携式设备挂载点，cdrom，floppy等  

/mnt：其他文件系统的临时挂载点  

/opt：附加应用程序的安装位置，（可选路径）  

/srv：当前主机为服务提供的数据  

/tmp：为那些会产生临时文件的程序提供的，用于存储临时文件的目录，可供所有用户执行写入操作，有特殊权限  

/usr：universal shared, read-only data  
- bin: 保证系统拥有完整功能而提供的应用程序  
- sbin:  
- lib：32位使用  
- lib64：只存在64位系统  
- include: C程序的头文件(header files)  
- share：结构化独立的数据，例如doc, man等  
- local：第三方应用程序的安装位置  
- bin, sbin, lib, lib64, etc, share  
- X11R6：X-Window程序的安装位置  
- src：程序源码文件的存储位置  

/var: variable data files  
- cache: 应用程序缓存数据目录  
- lib: 应用程序状态信息数据  
- local：专用于为/usr/local下的应用程序存储可变数据；  
- lock: 锁文件  
- log: 日志目录及文件  
- opt: 专用于为/opt下的应用程序存储可变数据；  
- run: 运行中的进程相关数据,通常用于存储进程pid文件  
- spool: 应用程序数据池  
- tmp: 保存系统两次重启之间产生的临时数据  

/proc：基于内存的虚拟文件系统，用于为内核及进程存储其相关信息：它们多为内核参数  

sys：sysfs虚拟文件系统提供了一种比proc 更为理想的访问内核数据的途径；其主要作用在于为管理Linux设备提供一种统一模型的接口

/selinux: security enhanced Linux，selinux相关的安全策略等信息的存储位置

应用程序的组成部分  
二进制程序：/bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin  
库文件：/lib, /lib64, /usr/lib, /usr/lib64, /usr/local/lib, /usr/local/lib64  
配置文件：/etc, /etc/DIRECTORY, /usr/local/etc  
帮助文件：/usr/share/man, /usr/share/doc, /usr/local/share/man, /usr/local/share/doc  

## 文件类型
- -：普通文件
- d: 目录文件
- b: 块设备
- c: 字符设备
- l: 符号链接文件
- p: 管道文件pipe
- s: 套接字文件socket

CentOS7开始/bin,/sbin,/lib,/lib64 这些目录已经变成了软连接
```bash
[root@centos7-1 ~]#ll /bin
lrwxrwxrwx. 1 root root 7 Sep 19 13:21 /bin -> usr/bin
[root@centos7-1 ~]#ll /sbin
lrwxrwxrwx. 1 root root 8 Sep 19 13:21 /sbin -> usr/sbin
[root@centos7-1 ~]#ll /lib
lrwxrwxrwx. 1 root root 7 Sep 19 13:21 /lib -> usr/lib
[root@centos7-1 ~]#ll /lib64
lrwxrwxrwx. 1 root root 9 Sep 19 13:21 /lib64 -> usr/lib64
```
pwd: printing working directory  
-P 显示真实物理路径  
-L 显示链接路径（默认）
```bash
[root@centos7-1 ~]#pwd -P
/root
[root@centos7-1 ~]#pwd -L
/root
[root@centos7-1 ~]#cd /bin
[root@centos7-1 bin]#pwd
/bin
[root@centos7-1 bin]#pwd -P
/usr/bin
[root@centos7-1 bin]#cd /usr/bin
[root@centos7-1 bin]#pwd 
/usr/bin
```
## 绝对路径和相对路径
**绝对路径**
- 以正斜杠开始
- 完整的文件的位置路径
- 可用于任何想指定一个文件名的时候

**相对路径名**
- 不以斜线开始
- 指定相对于当前工作目录或某目录的位置
- 可以作为一个简短的形式指定一个文件名

基名：basename

`[root@centos7-1 ~]#basename /etc/profile.d/env.sh
env.sh
`

目录名：dirname

`[root@centos7-1 ~]#dirname /etc/profile.d/env.sh
/etc/profile.d
`
## 特殊文件
/dev/zero 一个特殊的文件，当你读它的时候，它会提供无限的空字符(NULL,ASCLL NUL,0X00)  
/dev/null 它丢弃一切写入其中的数据，但报告写入操作成功，常被称为位桶(bit bucket)或者黑洞(black hole)  
/dev/random 产生随机数的设备

## 基础命令
__cd 改变目录__

使用绝对或相对路径：  
cd/home/wang/  
cdhome/wang  
切换至父目录：cd..  
切换至当前用户主目录：cd  
切换至以前的工作目录：cd-
```bash
[root@centos7-1 log]#cd /home/sjj
[root@centos7-1 sjj]#cd ..
[root@centos7-1 home]#ls
sjj
[root@centos7-1 home]#cd
[root@centos7-1 ~]#ls
anaconda-ks.cfg  Documents  initial-setup-ks.cfg  Pictures  Templates
Desktop          Downloads  Music                 Public    Videos
[root@centos7-1 ~]#cd -
/home
[root@centos7-1 home]#ls
sjj
```
__touch 创建文件__
```bash
[root@centos7-1 data]#touch '$abc'   #创建$abc文件
[root@centos7-1 data]#ls
$abc
[root@centos7-1 data]#touch /data/-a #创建-a文件
[root@centos7-1 data]#touch ./-b     #创建-b文件
[root@centos7-1 data]#touch -- -c    #创建-c文件
[root@centos7-1 data]#ls
-a  $abc  -b  -c
```
__ls 列出目录内容__
用法：ls [options] [files_or_dirs]  
示例:  
ls -a包含隐藏文件  
ls -l显示额外的信息  
ls -R目录递归通过  
ls -ld目录和符号链接信息  
ls -1 文件分行显示  
ls –S 按从大到小排序  
ls –t 按mtime排序  
ls –u 配合-t选项，显示并按atime从新到旧排序  
ls –U 按目录存放顺序显示  
ls –X 按文件后缀排序

ls 命令列出的文件名默认顺序为 数字，小写字母，大写字母
