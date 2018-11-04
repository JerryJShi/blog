---
title: SHELL脚本编程进阶
date: 2018-11-04 22:50:51
tag: [shell,编程]
categories: 编程
---
# SHELL脚本编程进阶

## 流程控制语句
面向过程编程语言：
- 顺序执行
- 选择执行
- 循环执行

## if条件选择
    if COMMANDS; then
        COMMANDS
    [ elif COMMANDS; then COMMANDS; ]
        ... 
    [ else COMMANDS; ]
    fi

## case条件判断
    case 变量引用 in
    PATTERN)
        分支1
        ;;
    PATTERN)
        分支2
        ;;
    ...
    *)
        默认分支
        ;;
    esac

## for循环
    for NAME [in WORDS ... ] ; do 
        COMMANDS
    done

    for (( exp1; exp2; exp3 )); do
        COMMANDS
    done

((...))格式可以用于算术运算  
((...))也可以使bash shell实现C语言风格的变量操作  
i=10 ; i++  
控制变量初始化：仅在运行循环代码开始时执行一次
控制变量的变化：每轮循环结束会先进行控制变量修正运算，再做条件判断

### 列表生成方法
1. 直接给出列表
2. 整数列表
   1. {1..10}
   2. $(seq 2 2 10)
3. 返回列表的命令
4. 使用通配符
5. 变量引用; $@,$*

## while循环
```
while CONDITION; do
    循环体
done
```
CONDITION：循环控制条件；进入循环之前，先做一次判断；每一次循环之后再次做判断；条件为"true"，执行一次循环；条件为"false"，终止循环
CONDITION一般应该有循环控制变量，变量的值会在循环体中不断地修正

进入条件：CONDITION true

退出条件：CONDITION false

### 无限循环

    while true; do
        循环体
    done

### while循环遍历文件的每一行
    while read line; do 
        循环体
    done < /PATH/FROM/SOMEFILE
依次读取/PATH/FROM/SOMEFILE文件中的每一行，且将行赋值给变量line

```
#可以给read赋多个变量
[root@centos7-1 data]#echo "  magedu  wang "|while read name;do echo $name;done
magedu wang
[root@centos7-1 data]#echo "  magedu  wang "|while read name name2;do echo $name $name2;done
magedu wang
```

```
#命令执行效果不同
[root@centos7-1 data]#echo "mage"|read name;echo $name

[root@centos7-1 data]#echo "mage"|while read name;echo $name
> ^C
[root@centos7-1 data]#echo "mage"|while read name;do echo $name;done
mage
```
## until循环
    until COMMANDS; do
        循环体
    done 
进入条件：CONDITION false

退出条件：CONDITION true

### 无限循环
    until false; do
        循环体
    done

## select循环与菜单
select适合创建菜单，按数字顺序排列的菜单项显示在标准输出上，并显示PS3提示符，等待用户输入  
用户输入列表中的数字，执行相应的命令  
select是个无限循环，因此要用break命令退出循环，或者用exit命令终止脚本，也可以按CTRL+c退出循环  
select经常和case联合使用  
可以省略in list，此时使用位置参量

格式： select NAME [in WORDS ... ;] do COMMANDS; done 

select MENU in A B C D; do
echo $MENU
done
用户输入被保存在内置变量REPLY中，REPLY 是由PS3影响的

示例
```
vim menu.sh
PS3="Please input a number:"    #PS3 提示符
select MENU in A B C D E F quit; do
    case $REPLY in
    1|2)
        echo "A and B"
        ;;
    3|4)
        echo "C and D"
        ;;
    5|6)
        echo "E and F"
        ;;
    7)
        echo "Byebye"
        break                                                                                
        ;;
    *)
        echo "Input invalid"
    esac
done
```

## 循环控制语句continue
用于循环体中  
continue[n]：提前结束第N层的本轮循环，而直接进入下一轮判断；最内层为第1层
```
while CONDITION1; do
    COMMAND1
    ...
    if CONDITION2; then
        continue
    fi
    COMMANDn
    ...
done
```

## 循环控制语句break
用于循环体中  
break[N]：提前结束第N层循环，最内层为第1层
```
while CONDITION1; do
    COMMAND1
    ...
    if CONDITION2; then
        break
    fi
    COMMANDn
    ...
done
```

## shift命令
shift[n]  
用户将参量列表list左移指定次数，缺省为左移一次。  
参量列表list一旦被移动，最左端的哪个参数就从列表中删除。while循环遍历位置参量列表时，常用到shift
```
while [ $# -gt0 ] # or (( $# > 0 ))
    do
    echo $*
    shift
done
```

## 函数
函数是由若干条shell命令组成的语句块，实现代码重用和模块化编程

### 函数和shell脚本的区别
执行shell脚本相当于开启了一个shell子进程，程序在子shell中运行
shell函数在当前shell中运行，和当前shell进程平级。函数可以对shell中的变量进行修改。函数不能独立运行，而是shell程序的一部分

### 函数组成
函数组成部分：
- 函数名
- 函数体

### 定义函数
1. f_name (){...函数体...}
2. function f_name {...函数体...}
3. function f_name (){...函数体...}

### 查看函数定义
格式： declare -f function_name
```
[root@centos7-1 data]#declare -f is_digit
is_digit () 
{ 
    [[ $1 =~ [0-9]+ ]] && echo true || echo false
}
```

### 定义函数
1. 交互环境下定义函数
```
[root@centos7-1 data]#version(){ sed -nr 's/.* ([0-9]+)\..*/\1/p' /etc/centos-release; }
```
2. 在脚本中定义函数
```
vim functionsipaddr()
{
    [ "$1" ] || echo "Please input arguments!"
    while [ $# -gt 0 ]; do
        ifconfig $1 |sed -nr '2s/[^0-9]+([0-9.]+).*/\1/p'
        shift
    done
}
```
函数在使用前必须定义，因此应将函数定义放在脚本开始部分。

函数调用使用其函数名即可。

可以将经常使用的函数存入函数文件，然后将函数文件载入shell脚本中。

文件名可任意选取。

一旦函数文件载入shell，就可以在命令行或脚本中调用函数。

使用set命令查看所有定义的函数，其输出列表包括已经载入shell的所有函数。

若要改动函数，先用unset命令从shell中删除函数，改完后，在重新载入次文件。

### 载入函数
定位函数文件并载入shell的格式
- . filename
- source filename

### 删除函数
格式： unset function_name

### 环境函数
环境函数子进程也可以使用
声明： export -f function_name
查看： export -f 或 declare -xf

### 函数引用
$(function_name)
\`function_name\`

### 函数变量

#### 环境变量
当前shell及子shell进程有效

#### 本地变量，全局变量，普通变量
只在当前shell进程有效，对shell子进程无效，执行脚本会启动专用子shell进程

#### 局部变量
函数体内部有效  
函数中定义局部变量  
local NAME=VALUE
如果函数中有局部变量，且名称与本地变量同名，函数内部使用局部变量

### 函数参数
- 传递参数给函数：调用函数时，在函数名后面以空白分隔给定参数列表，“testfunc arg1 arg2”
- 函数体中，可使用$1,$2,...调用参数；还可以使用$@,$*,$#等特殊变量

### 函数递归
函数直接或间接调用自身  
求自然数n的阶乘 n!=1x2x3x...x(n-1)xn  
阶乘可以用递归方式实现 n!=nx(n-1)!, (n-1)!=(n-1)x(n-2)!...  
n!=nx(n-1)x(n-2)x...x2x1  
```
vim fact.sh
fact()
{
    if [ $1 -eq 1 ]; then
        echo 1
    else
        echo $[$1*$(fact $[$1-1])]
    fi
}
fact $1
```

### fork炸弹
fork炸弹是一种恶意程序，它的内部是一个不断在fork进程的无限循环，实质上是一个简单的递归程序。由于是递归程序，如果没有任何限制，这会导致这个程序迅速消耗尽系统里的所有资源。

函数实现：

`:(){:|:&};:`  
`bomb() { bomb | bomb& }; bomb`
