# Shell

## shell内置功能

### shell基础

#### 命令提示符（可修改）

* `#`, root用户
* `$`, 普通用户
* 变量`PS1`为命令提示符格式，使用`echo $PS1`查看
* 变量`PS2`为命令换行提示符，使用`echo $PS2`查看

#### 当前用户默认shell

```bash
$SHELL
echo $SHELL
```

#### 当前系统支持的所有shell

```bash
/etc/shells
cat /etc/shells
```



#### tab命令补全

* 命令中第一组字符 + Tab # 命令补全
* 命令中第二组字符 + Tab # 文件补全
* 补全插件：

```bash
yum install bash-completion # 参数补全（影响文件补全）  
yum erase bash-completion  
yum install bash-completion-extras # 参数补全（影响文件补全）  
yum install python-django-bash-completion # django命令补全  
```

#### shell快捷键

```bash
Ctrl + l # 清屏，同clear
Ctrl + c # 终止当前命令
Ctrl + z # 挂起命令，使用fg恢复，使用bg查看
Ctrl + r # 搜索history历史命令
Ctrl + d # EOF, End of File, End Of Input, exit

Shift + PageUp / PageDown # bash terminal 翻页

ctrl + a # 行首，同Home
ctrl + e # 行尾，同End

# linux 特色，删除时剪切，可使用Ctrl + y 粘贴
ctrl + u # 删除光标前所有内容，backspace增强
ctrl + k # 删除光标后所有内容，del增强

Ctrl + Alt + F1 - F6 # 在tty之间切换
Ctrl + Alt + F7 # 切换到图形界面
Ctrl + Alt + F1 # Centos7切换到图形界面
```

### shell快捷指令

```bash
!! # 执行上一条命令
!str # 执行上一条str开头的指令
```







### type 判断是shell内建命令还是外部程序

```shell
type man # /usr/bin/man
type type # shell

# type -t 返回标准type类型
type -t command 

alias # 别名
keyword # shell保留字
function # shell函数
builtin # shell内置
file # 磁盘文件
'' # 空，没有找到

# type -p 获取file类型的路径，相当于 which command
type -p command
```



## shell 脚本编程

### 注释

```bash
# 单行注释
# 使用井号

# 多行注释方法1（非主流，改方法利用重定向符号将后续内容读入，但不做任何操作，作为注释）
<< BLOCK
注释行1
注释行2
BLOCK

:<<EOF
注释行1
注释行2
EOF

# 多行注释方法2（有问题，利用冒号，执行一条字符串作为命令，但是其中语句可能会被执行
# centos7下无效
:'
注释行1
注释行2
`echo abc` # 会引发错误
'
```



### 数据类型

* **shell中所有变量都是字符串，没有数值概念**

#### 字符串

```bash
 # 获取字符串长度
 var="abcd"
 echo ${#var}  # 4
 
 # 提取字符串子串
 var="abcd"
 echo ${var:0:4} # 从index 0提取4个长度
 
 # 查找子串
 var="abcd"
 echo $(expr index $var "c") # 3
```

#### 布尔值

* bash中有布尔值，true/false，不等于0/1

```bash
# bash中，true和false不等于0/1，虽然输出结果为0/1
true; echo $? # 0
false; echo $? # 1

true && true; echo $? # 0
true && false; echo $? # 1
false && true; echo $? # 1
false && false; echo $? # 1

true || true; echo $? # 1
true || false; echo $? # 1
false || true; echo $? # 1
false || false; echo $? # 0
```

#### 数组

* bash支持一位数组，不支持多位数组
* bash使用下面提取元素，从0开始

```bash
# bash中使用小括号定义数组，数组使用分隔

# 定义数组1
array1=(1 2 3 4)

# 定义数组2
array2=(
5
6
7
8
)

# 定义数组3
array3[0]=9
array3[1]=10
array3[2]=11
array3[3]=12

# 使用数组
# 使用数组需要使用大括号

# 提取特定元素
echo ${array1[0]}
echo ${array1[1]}
echo ${array1[2]}
echo ${array1[3]}

# 提取所有元素
echo ${array2[*]}
echo ${array2[@]}

# 获取元素长度
echo ${#array3[*]}
echo ${#array3[@]}

```

### 变量

* 变量命名与其他语言相同，由字母、下划线开头，可以包含数字，不能使用shell关键字作为变量名
* **使用未定义的变量不报错，值为空**
* bash变量区分大小写

#### 声明变量

```bash
# 使用等号声明变量
# 注意：等号两边不能有空格，有空格时需要使用单引号或者双引号
var=value
var='value' # 此为原样输出最佳实践
var="value" # 此为变量定义最佳实践

# 修改变量的值与定义变量相同，不会报错
var=old
var=new
```

#### 使用变量

```bash
# 使用花括号帮助解释器识别变量边界，可省略
var
${var}
$varabc # 不使用花括号会识别为变量varabc，未定义的变量值为空
${var}abc # 使用花括号会导致变量var与字符串abc拼接
```

#### 字符串拼接

```bash
"$var"append
${var}append
```

#### 变量类型

1. **局部变量：在脚本或命令中定义，仅在当前shell实例中有效，其他shell中不能访问的局部变量**
2. **环境变量：所有程序，包括shell启动的程序，都能访问的变量**
3. shell变量：shell为了正常运行，使用的变量，包含环境变量和局部变量

#### 声明为只读变量

```bash
var='old'
readonly var
var='new' # NAME: This variable is read only
```

#### 删除变量

```bash
unset var
```

#### 引号区别

```bash
# 不加引号

# 1.转译符号斜杠无效
# 2.字符串中不能有空格（变量声明时识别为字符串结束，echo时识别为下一个字符串）
# 3.字符串中空格只
echo \a # a
echo "\a" # \a
echo 111  222   333 # 111 222 333
echo "111  222   333" # 111  222   333，空格被保留

# 单引号
# 引号里内容会原封不动显示
a=abc
echo $a # abc
echo \$a # $a，\转译符号解转译
echo "$a" # abc
echo '$a' # $a

# 双引号
# 双引号中特殊符号“斜杠”，“空格”，“$”，“`”会被解析
# 换言之，双引号支持变量和转译

# 反引号
反引号中内容被执行，用于获取执行结果
```

#### 获取命令执行结果

```bash
`反引号`
$(command)

d1=`date` 
d2=$(date) # 此为将命令结果赋值的最佳实践，反引号容易误读为单引号
```

#### 特殊变量

```bash
$$ # 当前进程的ID，echo $$
$0 # 当前脚本的文件名，**使用source执行时，不是文件名，是bash**
$1 # 第1个参数
$5 # 第5个参数
$# # 参数个数，不含$0
$* # 所有参数，被双引号包含时，输出"$1 $2 $3 $n"
$@ # 所有参数，被双引号包含时，输出"$1" "$2" "$3" "$n"
$? # 上个命令的状态，或函数的返回值
~ # 家目录

# "$*" 循环1次
echo "print each param from \"\$*\""
for var in "$*"
do
    echo "$var"
done

# "$@" 循环4次
echo "print each param from \"\$@\""
for var in "$@"
do
    echo "$var"
done
```

#### 转义字符

```bash
\\ # 反斜杠
\a # 警报
\b # 退格（删除）
\f # 换页
\n # 换行
\r # 回车
\t # 水平制表符
\v # 垂直制表符

echo "\a" # 默认echo不解释转义字符
echo -e "" # 使用-e参数解释转义字符
```

### 运算符

* 原生bash不支持数学运算，只能通过命令awk和expr实现

```bash
var=`expr 2 + 2`
# 1、数字、符号、expr中间均需要空格分隔，英文bash按空格识别参数
# 2、使用反引号获取结果，结果为字符串
# 3、乘号前必须加反引号转译，避免识别为通配符
```

#### 算数运算符

| 运算符 | 说明                                          | 举例                          |
| ------ | --------------------------------------------- | ----------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 10。    |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a。  |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |


#### 关系运算符

* 关系运算符只支持数字，不支持字符串

| 运算符 | 说明                                                  | 举例                       |
| ------ | ----------------------------------------------------- | -------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 true。  |
| -ne    | 检测两个数是否相等，不相等返回 true。                 | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大等于右边的，如果是，则返回 true。   | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

#### 布尔运算符

```bash
if true; then echo "T"; else echo "F"; fi # T
if false; then echo "T"; else echo "F"; fi # F

# 注意，条件判断时方括号等同于test，
# if [ true ]; then echo "T"; else echo "F"; fi
if ! true; then echo "T"; else echo "F"; fi # F
if ! false; then echo "T"; else echo "F"; fi # T

# and
# 注意，-a 是 test的参数，所以必须要价方括号
if [ true -a false ]; then echo "T"; else echo "F"; fi
if [ true -o false ]; then echo "T"; else echo "F"; fi

# 示例
a=10
b=20

# and
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a -lt 100 -a $b -gt 15 : returns true"
else
   echo "$a -lt 100 -a $b -gt 15 : returns false"
fi

# or
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : returns true"
else
   echo "$a -lt 100 -o $b -gt 100 : returns false"
fi

# or
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : returns true"
else
   echo "$a -lt 100 -o $b -gt 100 : returns false"
fi
```

| 运算符 | 说明                                                |
| ------ | --------------------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 |
| -o     | 或运算，有一个表达式为 true 则返回 true。           |
| -a     | 与运算，两个表达式都为 true 才返回 true。           |

#### 字符串运算符

```bash
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
    echo "a等于b"
else
    echo "a不等于b"
fi

empty=""
if [ $empty ]
then
    echo "非空"
else
    echo "空"
fi
```

| 运算符 | 说明                                      |
| ------ | ----------------------------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。   |
| !=     | 检测两个字符串是否相等，不相等返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。     |
| -n     | 检测字符串长度是否为0，不为0返回 true。   |
| str    | 检测字符串是否为空，不为空返回 true。     |

#### 文件测试运算符

* 检测Unix文件的各种属性
* linux文件系统中所有设备都是文件，可以使用ls -l查看

* 文件类型：

```bash
- # 常规文件
c # 特殊档案
p # 命名管道
b # 块设备
s # 套接字
d # 目录
l # 链接
```

* 判断文件类型示例：

```bash
#!/bin/sh

file="/etc/rc.local"

# 测试文件是否有读权限
# 中括号和-r直接需有空格
if [ -r $file ]
then
	echo "文件有读权限"
else
	echo "文件没有读权限"
fi
```



| 操作符  | 说明                                                         |
| ------- | :----------------------------------------------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  |
| -p file | 检测文件是否是具名管道，如果是，则返回 true。                |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            |
| -r file | 检测文件是否可读，如果是，则返回 true。                      |
| -w file | 检测文件是否可写，如果是，则返回 true。                      |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          |

### shell脚本

#### 脚本解释器标记

* `#!` 用于标记使用的解释器
* `#!/bin/bash`

#### 执行脚本

* 使用source是在本shell进程中运行脚本，运行完之后，所有变量在当前shell有效
* 其他方法在新shell进程中运行脚本，运行完之后，所有变量在当前shell无效，除非export

```bash
# 执行方法1：在新shell中运行脚本

# 为脚本增加执行权限，默认没有权限
# 必须加./符号表示在当前目录下文件，不然bash解释器会在环境变量$PATH中寻找可执行程序
chmod +x ./test.sh
./test.sh # 相对路径执行
/root/test.sh # 绝对路径执行
test.sh # 添加环境变量，或放在$PATH任意目录下

# 使用解释器程序运行脚本
/bin/bash test.sh
sh test.sh
bash test.sh

# 执行方法2：在本shell中运行脚本（依次执行每行命令）
source test.sh
source ~/.bashrc

# 可以使用.代替source
. test.sh
```

#### 设置返回值

```bash
exit 0  
```

#### 获得输入

```bash
# 读取用户输入到变量
read input

# 带提示读取输入到变量
read -p "Please input: " input  

# 为读取内容设置默认值
var=${input:-"default"}  
```







### 流程控制

#### break

* 终止循环
* break n，可以跳出n层循环

#### continue

* 跳过本次循环
* continue n，可以跳过第n层本次循环

#### if

```bash
# 单行if
if true; then echo "T"; fi # "T"
if false; then echo "T"; fi # ""

# 多行if
if [ expression ]
then
	command
fi

# 单行if else
if true; then echo "T"; else echo "F"; fi # T
if false; then echo "T"; else echo "F"; fi # F

# 多行 if else
if [ expression ]
then
	command
else
	command
fi

# 单行 if elif fi
var=0
if [ 0 = $var ]; then echo "0"; elif [ 1 = $var ]; then echo "1"; else echo "other"; fi

# 多行 if elif fi
if [ 0 = $var ]
then
	echo "0"
elif [ 1 = $var ]
then
	echo "1"
else
	echo "other"
fi
```



#### while

* 单行循环

```bash
while true; do echo 1; sleep 1; done
```

* 多行循环

```bash
while true
do
    echo 1
    sleep 1
done
```

#### for in

```bash
# 迭代列表
for x in 'a' 'b' 'c'
do
echo $x
done

# 迭代序列
for x in $(seq 3):
do
echo $x
done

# 迭代当前目录所有文件
for file in *
do
echo $file
done
```

#### for

```bash
for (( x=1; x<=5; x=x+1 ))
do
echo $x
done
```

#### case

```bash
case $var in
    1) 
    	command1
	    command2
    	;;
	2)
		command3
		command4
		;;
	*)
		command5
		command6
		;;
esac

# 示例1：读取输入
read -p "请输入1-3:" num
case $num in
    1) echo "一"
    ;;
    2) echo "二"
    ;;
    3) echo "三"
    ;;
    *) echo "您输入错误，$num"
    ;;
esac

# 示例2：判断参数
case $1 in
    -a)
    echo "参数-a 处理 $2"
    ;;
    -b)
    echo "参数-b 处理 $2"
    ;;
    *)
    echo "usage: -[a|b] param2"
    ;;
esac

# ./test.sh -a 3
# 参数-a 处理 3
# ./test.sh -b 3
# 参数-b 处理 3
# ./test.sh -c 3
# usage: -[a|b] param2
```

#### until

```bash
until [ condition ]
do
done
```



### 函数

#### 定义函数

* 定义函数必须在使用函数前

```bash
# 定义函数方法1，无关键字
funname() {
	command1
	command2
}

# 定义函数方法2，使用关键字function
function funcname() {
	command1
	command2
}
```

#### 调用函数及获取函数返回值

* 1、shell函数返回值只能是整数，一般0表示成功
* 2、使用return语句显式返回值
* 3、没有显式返回值，则最后一条命令的返回值就是返回值

```bash
# 示例1
say_hello () {
    echo "Hello world!"
}

say_hello # 调用函数


# 示例2
sum () {
    read -p "Input number1:" number1
    read -p "Input number2:" number2
    return $(( $number1 + $number2 ))
}

sum
echo "Sum is $?" # 使用$? 获取返回值
```

#### 删除函数

* 与删除变量同样使用unset，但需要加上.f参数

```bash
unset .f funcname
```

#### 函数的参数

* 函数获取参数与使用脚本参数相同
* 函数输入参数与调用脚本参数相同
* 第十个参数开始不能使用$1获取，只能使用${10}获取

```bash
#!/bin/bash

sum () {
    echo "param1 $1"
    echo "param2 $2"
    echo "param all $*"
    echo "param all $@"
    echo "param count $#"

    tmp=0
    for param in $@
    do
        tmp=$(expr $tmp + $param)
    done

    return $tmp
}

sum `seq 5`
# sum $(seq 1000)，会引起错误

echo "sum is $?"
```









## shell基础

### .bash_history

记录这次登录之前的command  

### wildcard 通配符

略

### type
type command #显示是内建指令or外部指令  
type -t command #测试命令类型是file alias builtin？  
type -p command #显示外部指令path  
type -a command #显示命令搜索顺序，显示所有command命令和alias  

### 命令换行，转译Enter
\Enter


### 声明变量
=  
等号两边不能有空格  
等号右边内容有空格时，需要使用' or "  
""内的$var会被调用  
''内的$var会保留$  
可以使用\转译Enter,$,\,space,\,'等  

### 调用变量
$var  
${var}  

### 获取命令执行结果
`command`  
$(command)  

### 变量内容累加
"$var"append  
${var}append  

### unset
取消变量  
unset var  

### 变量跨进程引用  
export #显示所有环境变量  

### env 环境变量
HOME #家目录~  
SHELL #shell类型  
HISTSIZE #history size  
MAIL # /var/spool/mail/`whoami`  
PATH  
LANG  
RANDOM #随机数 declare -i number=$RANDOM*10/32768 ; echo $number  

### set
显示所有变量包括env  
BASH #当前使用bash路径  
COLUMNS #列数  
HISTFILE #历史文件  
HISTFILESIZE #文件中最大记录数  
HISTSIZE #内存中最大记录数  
IFS=$'\t\n' #预设分隔符号  
LINES #目前终端最大行数  
PS1 #命令提示字符格式  
PS2 #续行命令提示符  
$ #当前shell所有的PID   
? #上一条命令的返回值  

### locale
locale -a #显示所有支持语言  
locale #显示当前语言配置  
*LANG与LC_ALL两个变量会被其他locale变量使用*   

### read
read var #读取输入到变量  
read -p "xx" var #提示字符  
read -t second var #等待n秒  

### declare / typeset # 默认bash变量都是字符串，不会进行计算
declare #读取所有变量和值 ==set  
declare -a var #array ${var[n]}  
declare -i #integer #只能进行整数运算  
declare -x # ==export  
declare -r #readonly #必须重新进入bash才能改变  

### ulimit # user limit 用户限制
ulimit -a #显示所有限制值

```
core file size                      (blocks, -c)                       0 #core dump file size, 0 is unlimit  
data seg size                     (kbytes, -d)         unlimited #最大内存分段  
scheduling priority            (-e)                                  0  
file size                               (blocks, -f)         unlimited #单一文件大小限制，一次登录中只能减小不能增加  
pending signals                 (-i)                             3911  
max locked memory         (kbytes, -l)                      64 #最大锁定内存量  
max memory size              (kbytes, -m)       unlimited  
open files                           (-n)                             1024 #开启文件数量限制  
pipe size                             (512 bytes, -p)                8  
POSIX message queues     (bytes, -q)             819200  
real-time priority                (-r)                                  0  
stack size                            (kbytes, -s)                8192  
cpu time                             (seconds, -t)      unlimited #最大CPU时间  
max user processes            (-u)                            3911 #最大进程数量  
virtual memory                   (kbytes, -v)        unlimited  
file locks                              (-x)                    unlimited  
```

ulimit -H #hard limit限制值  
ulimit -S #soft limit警告值  

### 变量内容删除
${var#key} #从左向右删除，删除最短匹配  
${var##key} #从左向右删除，删除最长匹配  
${var%key} #从右向左删除，删除最短匹配  
${var%%key} #从右向左删除，删除最长匹配  
${var/key1/key2｝ #将首个匹配的key1替换为key2  
${var//key1/key2｝ #将所有匹配的key1替换为key2  
表达式 str没有定义 str="" str已经定义且不等于""
var=${str-expr} var=expr var= var=$str
var=${str:-expr} var=expr var=expr var=$str
var=${str+expr} var= var=expr var=expr
var=${str:+expr} var= var= var=expr
var=${str=expr} str=exprvar=expr str不变var= str不变var=$str
var=${str:=expr} str=exprvar=expr str=exprvar=expr str不变var=$str
var=${str?expr} expr输出至stderr var= var=$str
var=${str:?expr} expr输出至stderr expr输出至stderr var=$str

### alias unalias

使用\转译还原alias  

### history
history n # 列出最近n条命令  
history -c # clean  
history -r # read histfile  
history -w # write histfile  
history -a # append write  
!n # 执行第n条指令  
!key # 搜索开头为key的指令执行  
!! # 执行上一条指令  
ctrl + r  

### bash欢迎信息
/etc/issue #bash欢迎信息  
/etc/issue.net #telnet欢迎信息  
/etc/motd #登录后显示的欢迎信息  

### /etc/profile # login shell 配置文件

#### 执行过程  

决定PATH MAIL USER HOSTNAME HISTSIZE umask  
执行/etc/profile.d*.sh  
执行/etc/profile.d/lang.sh -> /etc/locale.conf  
执行/usr/share/bash-completion/completions/*  

#### 执行顺序

* login shell按顺序读取第一个存在的配置文件 *  
  ~/.bash_profile # 读取~/.bashrc/bashrc->读取/etc/bashrc  
  ~/.bash_login  
  ~/.profile  

### source # 读取环境设置文件
source ~/.bashrc  
. ~/.bashrc #小数点读取环境设置文件  

### ~/.bashrc # non-login shell读取配置文件
读取/etc/bashrc 为CentOS特有，目的是执行login shell的profile和profild.d  

### ~/.bash_logout # 退出后执行的命令
略  

#### stty #s etting tty 快捷键配置

stty -a # list all  
stty keyword keyboard #^是ctrl  

intr # interrupt  
quit # quit signal  
erase # 向后删除  
kill # 删除在目前指令上的所有文字  
eof # End of file  
start # 重新启动输出  
stop # 停止输出  
susp # terminal stop  

#### set # bash tty设置
set -u #default no #未定义的变量显示错误信息  
set -v #default no #信息输出前，会先显示信息的原始内容  
set -x #default no #指令执行前，会显示指令内容，++指令内容  
set -h #default yes #history  
set -H #default yes #history  
set -m #default yes #manage  
set -B #default yes #设置[]  
set -C #default no #使用>输出到文件时，文件存在则不覆盖  
echo $- #显示所有set设定  

#### 其他terminal设置
/etc/inputrc  
/etc/DIR_COLORS*  
/usr/share/terminfo/*   

#### 通配符
星号* #任意n个字符  
? #任意1个字符  
[a,b] #括号内某个字符  
[a-b] #编码顺序内任意字符  
[^a] #排除字符  

#### 特殊符号
'#' # 注释  
\ # 转义字符  
| # pipo管道  
; # 命令分隔符  
~ # 家目录  
$ # 取变量  
& # 后台运行  
! # not  
/ # 路径分割  
大于号> # 替换  
两个大于号>> # 累加  
< << #  
'' # 不能置换变量  
"" # 置换变量  
`` # 先执行指令，再显示 ==$(cmd)  
() # 子shell起止  
{} # 命令区块  

#### 标准输入输出
stdin 0 < << #<<设置结束字符  
stdout 1 > >>  
stderr 2 2> 2>>  
两条流不能同时写入一个文件，所以 cmd >file 2>file是错误的  
正确的写法是 cmd >file 2>&1 或者 &>file  
2>/dev/null #丢弃错误输出  

#### 组合命令
cmd;cmd #顺序执行  
cmd1&&cmd2 #cmd1返回0则执行cmd2  
cmd1||cmd2 #cmd1返回1则执行cmd2  

#### cut
cut -d 'deviceby' -f fieldnum #取分段，从1开始计数  
cut -c n1-n2 #截取字符n1到n2  
cut -c n1- #截取字符n1到最末尾  
cut无法处理多个空白字符，使用awk  

#### sort
sort [-fbMnrtuk] file  
-f #忽略大小写  
-b #忽略行首空白  
-M #以英文月份排序  
-n #以数字排序  
-r #reverse  
-u #uniq 合并相同行  
-t #分隔符，default=tab  
-k #key field，以特定列排序  

#### uniq # 合并重复
-i # ignore case  
-c # count  

#### wc
-l # line count  
-w # English word count  
-m # 字符个数  

tee # 带显示的重定向 >  
-a # append >>  

#### tr
tr [a-z] [A-Z] # 按字符替换  
tr key1 key2 # 按词组替换  
tr -d key # delete  

#### col
col -x # 将tab替换为对等的space  
col -A # 显示所有空白字符  

#### join # 按行拼接两个文本。默认以空白分隔，比较每行首列，一样则合并
-t # 设置分隔符号  
-i # ignore case  
-1 # 设置第一个文件用第几列比较  
-2 # 设置第二个文件用第几列比较  

#### paste
paste -d ' ' file1 file2 #按行拼接，默认两行以tab分割，-d设置devide  

#### expand
expand -t n file #将一个tab以n个空格替换  

#### split
split -b [b|k|m] file newname #按大小分割文件  
split -l num file newname #按行分割文件  

### xargs # 给不支持管道的命令提供参数
cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -n 1 id #-n 设置每次读取n个参数  
cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -p -n 1 id #-p 互动模式，每次询问是否使用参数  
cut -d ':' -f 1 /etc/passwd | xargs -e'sync' -n 1 id #-eEOF 设置结束字符  
find /usr/sbin -perm /7000 | xargs ls -l == ls -l $(find /usr/sbin -perm /7000)   

#### 减号- # stdout stdin替代符

tar -cvf - /home | tar -xvf - -C /tmp/homeback #-f - 将打包结果输出到stdout #-f - 从stdin输入tarball  



### Shell 脚本基础

 



#### 整数运算
declare -i total=${num1}*${num2}  
total=$((${num1}*${num2}))  

+  

-  

*  /  
   %  

#### 浮点数运算

echo "1.1*2.2" | bc  


#### test
test -e file && echo "exist" || echo "not exist"  

##### 文件测试  

-e #exist  
-f #file  
-d #directory  
-b #block device  
-c #character device  
-S #Socket  
-p #pipe fifo  
-L #link  

##### 权限测试  

-r #read  
-w #write  
-x #execute  
-u #SUID  
-g #SGID  
-k #Sticky bit  
-s #非空白文档  

##### 文件比较

-nt #newer than  
-ot #older than  
-ef #equal(hard link)  

##### 数值比较

-eq #equal  
-ne #not equal  
-gt #greater than  
-lt #less than  
-ge #greater than or equal  
-le #less than or equal  

##### 字符串

test -z string #zero 空字符串  
test -n string #not 非空字符串 #可以省略 test string  
test str1 == str2  
test str1 != str2  

##### 多重条件

-a #and  
-o #or  
test ! option object #非  

#### 判断符号[]

[ -z "${HOME}" ]  
[ "$HOME" == "$MAIL" ] #注意全部加空格以和正则表达式区分  
变量还原注意两边加双引号"$HOME"避免还原后有空格  

#### 与或非

[] && [] #AND  
[] || [] #or  
[ -o ]  
[ -a ]  

#### 参数偏移
shift #偏移一个参数，删除第一个参数  
shift n #再偏移n个参数，删除前n个参数  



#### 函数参数输入

fname var1 var2  



### 随机数
check=$((${RANDOM}*${varnum}/32767+1))  

###  sh
-n #不执行，仅检查语法  
-v #执行前将script打印到屏幕上  
-x #将使用到的script内容显示出来  

### 判断命令是否存在

command -v foo >/dev/null 2>&1 #POSIX script  
type foo > /dev/null 2>&1     #consider built-ins & keywords  
hash foo 2 > /dev/null    #for bash  
which不是bash自带，不一定存在  





shell脚本
----------

### bash基础知识

```
home/.bash_history # 保存历史命令，仅当正常退出时保存


env # 环境变量
set # 系统预设变量
export var # 导出环境变量，所有子shell都可以使用
export # 导出所有变量
/etc/profile # 所有用户都是用的变量
/home/.bashrc # 某个用户使用的变量
a='b c' # 声明变量a，等号右侧遇见空白字符则终端，需要使用引号包括
a="b'c" # 使用双引号包含单引号内容
a=`pwd` # 反引号中的内容将会执行，a的值等于执行后的结果输出 
a="b"c # 只有双引号可以累加内容
"$a" # 双引号使用$符号展开变量内容
a='$a'c # 单引号中特殊字符$失效，不会展开内容
unset var # 取消变量

# 特殊字符
* # 匹配0-n个字符
? # 匹配1个字符
[] # 中括号中任意一个值 [123abc] [a-zA-Z] [0-9]
# 注释
\ # 转义字符
$ # 取变量 or 代表上条命令输出最后的内容

; # 顺序执行多条命令
cmd1&&cmd2 # cmd1执行成功后执行cmd2
cmd1||cmd2 # cmd1执行成功后cmd2不执行
```

### 脚本

```
# 声明变量
a=b # 等号左变是变量，等号两边不能加空格，等号右值有特殊字符时用引号包括

# 使用变量
$var
${var}

# echo
echo string string
\n # 换行，echo结束会自动换行
\r # return
\t # table
\v # 垂直制表符
\\ # 转译斜杠

# printf 
printf "fmt" var1 var2 var... # 复制c语言

# I/O重定向
< # 输入重定向
> # 输出重定向
2> # 错误重定向
>> # 追加重定向
| # 管道
/dev/null # 输出到此直接丢弃，作为输入文件结束
/dev/tty # 终端

```

