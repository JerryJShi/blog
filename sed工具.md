# SED 工具
## SED 工具介绍
### SED （Stream Editor） 行编辑器
SED 是一种基于流的编辑工具，每次只能处理一行内容。它的工作原理是，每次读入一行，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区中的内容打印到屏幕。SED 命令默认打印当前读入的行。之后再读入下一行，执行下一次循环。如果没有使诸如‘D’的特殊命令，那会在两个循环之间清空模式空间，但不会清空保留空间。不断重复上述操作，直到文件末尾。文件内容本身并没有改变，只是打印了出来，除非你使用重定向存储输出。

SED 工具不用打开文件交互操作，通过命令行直接对文件进行过滤，修改，实现了自动编辑一个或多个文件，简化对文件的反复操作，可以编写脚本。

[参考：http://www.gnu.org/software/sed/manual/sed.html](http://www.gnu.org/software/sed/manual/sed.html)

## SED 工具用法
用法：sed [OPTION]... {script-only-if-no-other-script} [input-file]...

### 常用选项：
-n 不输出模式空间内容到屏幕  
-e 多点编辑  
-f /PATH/SCRIPT_FILE 从指定文件中读取编辑脚本  
-r 支持使用扩展正则表达式  
-i 备份文件并原处编辑  

### 地址定界：
1. 不给定地址：全文处理
2. 单地址：#：指定的行；$：最后一行；/pattern/：被模式所能够匹配到的每一行
3. 地址范围：#,#; #,+#; /pat1/,/pat2/; #,/pat1/
4. ~ 步进：1~2 奇数行；2~2 偶数行

注意：给定地址，或模式匹配后，要跟编辑命令，否则会报丢失命令

### 编辑命令：
- d 删除模式空间的行，并立即启用下一轮循环
```
sed '/^#/d' /etc/fstab
```
- p 打印当前模式空间内容，追加到默认输出之后
```
sed '2p' /etc/passwd
```
- a [\] text 在指定行后面追加文本，支持使用\n实现多行追加
```
sed ''/root/a\superman' /etc/passwd
sed -i.bak '/^root/a\  admin line' passwd   #追加空格开头的字符串，用\ 做分界
```
- i [\] text 在行前面插入文本
```
sed '/root/i\superman' /etc/passwd
```
- c [\] text 替换行为单行或多行
```
sed '/root/c\superman' /etc/passwd
```
- w /path/file 保存模式匹配的行至指定文件
```
sed -n '/^UUID/w /data/dir1/ft1' /etc/fstab
```
- r /path/file 读取指定文件的文本至模式空间中匹配到的行后
```
sed -n '/^UUID/p;$r /data/dir1/grub' /etc/fstab
```
- = 为模式空间中的行打印行号
```
sed -n '/^$/=' /etc/fstab
```
- ！模式空间中匹配行取反处理
```
sed -n '/^#/!p' /etc/fstab
```
### 查找替换
支持使用三种格式 
- s/// 
- s@@@ 
- s###
替换标记：
- g 行内全部替换
- p 显示替换成功的行
- w /PATH/FILE 将替换成功的行保存至文件中
```
    sed -n 's/root/&superman/p' /etc/passwd  #单词后插入字符串  
    sed -n 's/root/superman&/p' /etc/passwd  #单词前插入字符串  
```
sed 的特殊用法'''$str''' 用来引用变量
```
str=net.ifnames=0;sed 's/\(^GRUB_CMDLINE_LINUX=.*\)"/\1 '''$str'''"/' /etc/default/grub
```
文件中的小写字母替换为大写字母
```
sed -r 's/[[:alpha:]]/\u&/g' /data/fstab.bak
```
文件中的大写字母替换为小写字母
```
sed -r 's/[[:alpha:]]/\l&/g' /data/fstab.bak
```
\u 表示大写，\l 表示小写

## SED 工具的高级用法
高级编辑命令：
- P：打印模式空间开端至\n内容，并追加到默认输出之前
- h：把模式空间中的内容覆盖至保持空间中
- H：把模式空间中的内容追加至保持空间中
- g：从保持空间取出内容覆盖至模式空间
- G：从保持空间取出内容追加至模式空间
- x：把模式空间中的内容与保持空间中的内容进行互换
- n：读取匹配到的行的下一行覆盖至模式空间
- N：读取匹配到的行的下一行追加至模式空间
- d：删除模式空间中的行
- D：如果模式空间包含换行符，则删除直到第一个换行符的模式空间中的文本，并不会读取新的输入行，而使用合成的模式空间重新启动循环。如果模式空间不包含换行符，则会像发出d命令那样启动正常的新循环。

sed示例
- sed -n 'n;p' FILE  显示偶数行
```bash
[root@centos7-1 data]#seq 10 |sed -n 'n;p'
2
4
6
8
10
```
- sed '1!G;h;$!d' FILE  逆序显示行
```bash
[root@centos7-1 data]#seq 10 |sed '1!G;h;$!d'
10
9
8
7
6
5
4
3
2
1
```
- sed 'N;D'FILE  显示最后一行
```bash
[root@centos7-1 data]#seq 10 |sed 'N;D'
10
```
- sed '$!N;$!D' FILE  显示最后两行
```bash
[root@centos7-1 data]#seq 10 |sed '$!N;$!D'
9
10
```
- sed '$!d' FILE  显示最后一行
```bash
[root@centos7-1 data]#seq 10 |sed '$!d'
10
```
- sed 'G' FILE  奇数行之后追加一个空行
```bash
[root@centos7-1 data]#seq 10 |sed 'G'
1

2

3

4

5

6

7

8

9

10

```
- sed 'g' FILE  显示相应文本行数的空行
```bash
[root@centos7-1 data]#seq 10 |sed 'g'










```
- sed '/^$/d;G' FILE  每行文本下面显示一个空行
```bash
[root@centos7-1 dir1]#cat f1
UUID=a9e2060d-678e-47bd-b2ce-98a73101f123 /                       xfs     defaults        0 0
UUID=a1fee27e-f745-415e-a406-c02e51f62ef1 /boot                   xfs     defaults        0 0

UUID=c9ae8656-7635-476c-b8f3-f372f0997cac /data                   xfs     defaults        0 0

UUID=8193094f-203d-4682-800f-486bd5bc7540 swap                    swap    defaults        0 0
[root@centos7-1 dir1]#sed '/^$/d;G' f1
UUID=a9e2060d-678e-47bd-b2ce-98a73101f123 /                       xfs     defaults        0 0

UUID=a1fee27e-f745-415e-a406-c02e51f62ef1 /boot                   xfs     defaults        0 0

UUID=c9ae8656-7635-476c-b8f3-f372f0997cac /data                   xfs     defaults        0 0

UUID=8193094f-203d-4682-800f-486bd5bc7540 swap                    swap    defaults        0 0

```
- sed 'n;d' FILE  显示奇数行
```bash
[root@centos7-1 dir1]#seq 10 |sed 'n;d'
1
3
5
7
9
[root@cento
```
- sed -n '1!G;h;$p' FILE  逆序显示行
```bash
[root@centos7-1 dir1]#seq 10 |sed -n '1!G;h;$p'
10
9
8
7
6
5
4
3
2
1
```

打印奇数行
```
seq 10 | sed -n '1~2p'
seq 10 | sed '1~2d'
seq 10 | sed 'n;d'
```
打印偶数行
```
seq 10 | sed -n '2~2p'
seq 10 | sed '2~2!d'
seq 10 | sed -n 'n;p'
```
倒序显示
```
seq 10 | tac
seq 10 | sed '1!G;h;$!d'
seq 10 | sed -n '1!G;h;$p'
```
## 练习
1、删除centos7系统/etc/grub2.cfg文件中所有以空白开头的行行首的空白字符
```
sed  's/^[[:space:]]\+\(.*\)/\1/' /etc/grub2.cfg
```
2、删除/etc/fstab文件中所有以#开头，后面至少跟一个空白字符的行的行首的#
和空白字符
```
sed 's/^# \+\(.*\)/\1/' /etc/fstab
```
3、在centos6系统/root/install.log每一行行首增加#号
```
sed 's/.*/#&/' /root/install.log
```
4、在/etc/fstab文件中不以#开头的行的行首增加#号
```
sed 's/^[^#]\(.*\)/#\1/' /etc/fstab
```
5、处理/etc/fstab路径,使用sed命令取出其目录名和基名
```
echo /etc/fstab/ | sed 's@\(.\+\)/\(.\+\)/\?@\1@'
echo /etc/fstab/ | sed 's@\(.\+\)/\(.\+\)/\?@\2@'
```
6、利用sed 取出ifconfig命令中本机的IPv4地址
```
ifconfig eth0 |sed -nr '2s/^.+inet [[:alpha:]:]*(.*)  [[:alpha:]]+.*  .*/\1/p'
ifconfig eth0 |sed -nr '2s/^.+ [[:alpha:]:]*(.*)  [[:alpha:]]+.*  .*/\1/p'
ifconfig eth0 |sed -n 's/^ *inet [[:alpha:]:]*\([0-9.]\+\)  .\+  .*/\1/p'
ifconfig eth0 |sed -r '2!d;s/.*inet (addr:)?//;s/ (.*)//'
```
7、统计centos安装光盘中Package目录下的所有rpm文件的以.分隔倒数第二个
字段的重复次数
```
ls /misc/cd/Packages/ | sed -r 's/.*\.(.*)\.rpm$/\1/' |sort |uniq -c |sort -n
```
8、统计/etc/init.d/functions文件中每个单词的出现次数，并排序（用grep和
sed两种方法分别实现）
```
grep -o "\<[[:alpha:]_]\+\>" /etc/init.d/functions |sort |uniq -c |sort -n
grep -ow "[[:alpha:]_]\+" /etc/init.d/functions |sort |uniq -c |sort -n
cat /etc/init.d/functions |sed -nr 's/\<[[:alpha:]_]+\>/\n&\n/gp' |sed -r '/\<[[:alpha:]_]+\>/!d' |sort |uniq -c |sort -n
```
9、将文本文件的n和n+1行合并为一行，n为奇数行
```
seq 10 | sed 'N;s/\n//'
seq 10 | xargs -n2
```
