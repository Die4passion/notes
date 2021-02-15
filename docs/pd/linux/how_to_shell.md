# 如何书写shell脚本

目的是快速写出需要的shell脚本

### 1. 一些概念

标准io

```bash
文件描述符
0 标准输入 默认键盘
1 标准输出 默认终端
2 标准错误 默认终端
```

重定向

```bash
> 输出重定向
>> 追加到输出重定向
< 输入重定向
<< 追加到输入重定向

ls -l > /tmp/a
cmd > /dev/null 2>&1 # 输出到垃圾桶
```

管道

```bash
# 前后连接两个命令

ls -l | grep test 
```

引号

```bash
双引号：可以除了字符$`\外的任何字符或字符串
单引号：忽略任何引用值，将引号里的所有字符作为一个字符串 $var 不能被解析
反引号：设置系统命令输出到变量
```

shell脚本识别三种基本命令：内建命令，shell函数和外部命令

基本的命令查找：shell会沿着查找路径$PTAH来寻找命令

```bash
echo $PATH

# 可以在.profile文件中修改
export PTAH=$PATH:$HOME/bin
```

`and/or`

```bash
expression1 && expression2 && expression3
只有前面一条命令执行成功，才执行下一条
expression1执行成功，才执行expression2

expression1 || expression2 || expression3
执行命令，直到有一条成功为止
```

### 2. shell脚本

首先声明使用bash(声明执行脚本解释器)

```bash
#!/bin/bash

# do something

exit 0/n
```

运行

```bash
sh xx.sh
bash xx.sh 
# 大部分情况下两个一样，某些命令只有bash有

or 

chmod u+x xx.sh
./xx.sh
```

调试

```bash
# 查看运行时,每个命令回显，执行之后回显
sh -x xx.sh

# 执行之前回显
sh -v xx.sh

# 检查语法错误，不执行
sh -n xx.sh

# 如果使用了未定义的变量，给出错误的信息
sh -u xx.sh

# 调试部分脚本
echo "Hello $USER,"
set -x
echo "Today is $(date %Y-%m-%d)"
set +x
```

判断执行结果

```bash
N=$?    #0 <=N <= 255

    0 无错误，正常执行结束
    非0 异常
    1-125 命令不成功退出
    126 命令成功，但文件无法执行
    127 命令找不到
    >128 命令因收到信号而死亡
```

获取目录名和文件名

```bash
# to find base directory
APP_ROOT=`dirname "$0"`

# to find the file name
filename=`basename "$filepath"`

# to find the file name without extension
filename=`basename "$filepath" .html`

eg:
BASEDIR=$(dirname $0)
cd $BASEDIR
CURRENT_DIR=`pwd`
```

日期

```bash
TODAY=`date +%Y%m%d`
DAY_1_AGO=`date -d "$TODAY 1 days ago" +%Y%m%d`

常用接受日期/使用默认日期处理

if [ -n "$1" ]
then 
    TODAY="$1"
else
    TODAY=`date +%Y%m%d`
fi
```

crontab调度

```bash
查看
crontab -l
编辑
crontab -e

格式
* * * * * command

字段    含义    范围
1      分钟    0-59
2      小时    0-23
3      日期    1-31
4      月份    1-12
5      星期    0-6


*任意时刻
n1,n2    分割，n1和n2
*/n     每隔n单位
n1-n2   时段，一个时间段内

0 */2 * * * 每隔两小时
20 7 * * * 每天7点20
0 1,5 * * * 每天1点和5点
* * * * *   每分钟
```
### 3.变量

1. 变量赋值

!> 注意等号两边必须不能包含空格

```bash
varname="value"
varname=`expression`
```

2. 分类

```bash
四种变量：环境变量，本地变量，位置变量，特定变量

环境变量可作用于所有子进程
本地变量在用户现在的shell 生命期的脚本中使用，仅存在于当前进程
位置变量：作为程序参数
特殊变量：特殊作用
```

3. 环境变量

```bash
# 设置
MYVAR="test"
expire MYVAR
or
export MYVAR="test"

# 只读
MYVAR=”test“
readonly MYVAR
or
readonly MYVAR="test"

# 显示
export -p 
env # 查看所有环境变量
$MYVAR # 获取

# 消除
unset MYVAR
```

4. 本地变量

```bash
# 设置
LOCAL_VAR="test"
or
LOCAL_VAR="test"
readonly LOCAL_VAR # 设置只读

# 还可以使用declare命令定义
```

5. 位置变量

```bash
$0 # 脚本名称
$# # 传递到脚本的参数个数
$$ # shell脚本运行当前进程ID
$? # 退出状态
$N # N>=1,第n个参数
```

6. 字符串处理

长度

```bash
# 字符串的长度
${#VARIABLE_NAME} 

if [ ${#authy_api_key} != 32 ]
then
    return $FAIL
fi
```

拼接字符串

```bash
echo "$x$y"
```

字符串切片

```bash
${变量名:起始:长度} # 得到子字符串

test='I love china'
echo ${test:5}
echo ${test:5:10}
```

字符串替换

```bash
# 一个"/" 表示替换第一个， "//" 表示替换所有
# 当查找中出现了:"/"  需要”\/“转义
$(变量/查找/替换值) 

echo ${str/foo/bar} # 首个
echo ${str//foo/bar} # 所有
```

正则匹配

```bash
if [[ $str =~ [0-9]+\.[0-9]+ ]]
```

7. 数值处理

自增

```bash
a=1
a=`expr a + 1`

or 

a=1
let a++
let a+=2
```

let

```bash
no1=4
no2=5
let result=no1+no2
```

expr

```bash
result=`expr 3 + 4`
result=$(expr $no1 + 5)
```

其他

```bash
result=$[ no1 + no2 ]
result=$[ $no + 5 ]

result=$(( no1 + 5 ))
```

浮点数

```bash
echo "4 * 0.56" | bc
# 设定进度
echo "scale=2;3/8" | bc
# 进制转换
echo "obase=2;100" | bc
# 平方
echo "sqrt(100)" | bc
```

### 4. 控制流

1. 条件测试

语法

```bash
test condition
[ condition ] # 注意两边加空格

$? # 获取判断结果，0表示 contition=true
```

条件测试中的逻辑

```bash
-a 与
-o 或
！  非
&&
||

if [ -n "$str" -a -f "$file" ]
if [ -n "$str" ] && [ -f "$file" ]
```

字符串测试

```bash
= # 两字符串相等
!= # 两字符串不等
-z # 空串 [zero]
-n # 非空串 [nozero]

[ -z "$EDITOR" ]
[ "$EDITOR" = "vi" ]
```

数值测试

```bash
-eq # 数值相等 (equal)
-ne # 不等 (not equal)
-gt # A>B (greter than)
-lt # A<B (lessthan)
-le # A<=B (less equal)
-ge # A>=B (greater equal)

N=130
[ "$N" -eq 130 ]
```

文件测试

```bash
-d # 目录
-f # 普通文件
-e # 文件存在
-z # 文件长度 = 0
-s # 文件长度 > 0，非空
-b # 块专用文件
-c # 字符专用文件
-L # 符号链接

-r # Readable (文件、目录 可读)
-w # Writable (文件、目录 可写)
-x # Executable (文件可执行，目录可浏览)

-g # 如果文件的 set-group-id 位被设置则结果为真
-u # 文件有 suid 的位置
```

2. 分支 `if-else`/case

`if-else`语法

```bash
if condition1
then
    # do thing a
elif condition2
then
    # do thing b
else
    # do thing c
fi

or

if condition
then
    # do something
fi
```

`case` 语法

```bash
case $VAR in
    1)
        echo "abc"
        ;;
    2|3|4)
        echo "def"
        ;;
    *)
        echo "last"
        ;;
esac
```

3. 循环`for/while/until`

`for` 语法

```bash
for VARIABLE in 1 2 3 4 5 .. N
do
    # something
done

for OUTPUT in $(Linux-Or-Unix-Command-Here)
do
    # something
done

for (( EXPR1; EXPR2; EXPR3 ))
do
    # something
done

# 例子

for i in 1 2 3 4 5;
do
    echo $i
done

for i in `seq 1 5`;
do
    echo $i
done

echo "Bash version"
for i in $(seq 1 2 20)
do
    echo "Welcome $i times"
done

# 无限循环
for (( ; ; ))
do
    echo "infinite loops [hit Ctrl+C to stop]"
done
```

`while`

```bash
while condition
do 
    # something
done

COUNTER=0
while [ $COUNTER -lt 5 ]
do
    COUNTER=`expr $COUNTER + 1`
    echo $COUNTER
done

# 无限循环
while [ 1 ]
do
    # something
done
```

`until`

```bash
# 执行命令，直到条件为真，至少执行一次
# 可用来做监控
# condition 每次都会去检查

until condition
do 
    # something
done
```

`break/continue`

```bash
break
# 允许跳出循环，通常在进行一些列处理后退出循环或case语句
# 若多重循环，可指定跳出的循环个数，如跳出两重循环 break 2

continue
# 不会跳出循环，只是跳过此循环步
# 命令是车光绪在本循环累忽略下面的语句，从循环开头执行
```

### 5. 函数

1. 定义函数

```bash
function func_name() {

}

func_name() {
    # so something
}
```

!> 注意： 函数名在脚本中必须唯一

!> 函数必须， 先定义，后使用

`return`

```bash
function equal() {
    return 1
}

equal
echo $? # got 1
```

2. 参数传递

```bash
function copyfile() {
    cp $1 $2
    return $?
}

# 调用

copyfile /tmp/a /tmp/b
or
result=`copyfile /tmp/a /tmp/b`
```

位置参数

```bash
$1 - $9  # 当参数超过10个时，需要使用${10}
$#  # 参数个数
$*  # 将所有参数视为一个字符串 = "$1 $2 .."
$@  # 将所有参数视为个体 = "$1" "$2" "$3" ..
```

3. 返回值和退出状态

```bash
# 返回值
function func_a() {
    return 1
}

result=`func_a`
if [ result != 0 ]
then
    echo "Error"
fi

# 退出状态
function func_b() {
    # do something
}

func_b
if [ $? -eq 0 ]
then
    echo "Success"
else
    echo "Error"
fi

# 更简洁
if func_b;
then 
    echo "Success"
else
    echo "Error"
fi

func_b $$ echo "Success" || echo "Error"
```

### 6. 高级

读文件

```bash
while read -r line;
do
    echo $line
done < file

# 保留首尾字符
while IFS = read -r line;
do
    echo $line
done
```

一些内置命令

```bash
:
# 空命令，类似python的pass

.
# 相当于source

\
# 用于跨行命令

echo
# 输出，类似println

exec
#

exit n
# 脚本以 n 作为退出码退出

export
# 设置或显示环境变量

expr
# 简单计算

let d=111
let d=$d+1;
echo $d
# 112

printf
# 格式化输出

return
# 函数返回

set

shift
# 所有参数变量左移一个位置

unset
# 中删除变量或函数
```

`Best Practice`：

```bash
使用$() 代替反引号
$(()) 代替expr运算符
```

`bash`

```bash
$((3 + 4))   # 而不需要expr 3 + 4 算术展开
/usr/{bin,local/bin} # 而不需要 /usr/bin /usr/local/bin
${str/src/dst} # 而不需要 echo $str | sed "s/$src/$dst"
```

更方便的语法

```bash
for (( expr1; expr2; expr3 ));
do
    commands;
done

for (( i = 0; i < 100; i++ ));
do
    ...
done

echo a{b,c,d}e ==> abe ace ade
```

表达式求值

```bash
$[]     # []中间可以加表达式 如： echo $[$a+$b]
$(())   # (())中间可以加表达式 如： total=$(( $a * $b )) 
```

正则

```bash
# bash的正则表达式
str='hello, world'
if [[ str=~ '\s+world$' ]];
then
    echo match!
fi

if echo "$str" | grep -E '[ ]+world$';
then
    echo match!
fi
```

获取软链接指向的真实文件名

```bash
# 有些系统没有这个命令

readlink /usr/bin/python
```

增加`debug`

```bash
fucntion debug() {
    if [[ $DEBUG ]]
    then
        echo ">>> $*"
    fi
}

# For any debug msg
debug "Trying to find config file"

# 还有来自于一些很酷的geeks的单行debug函数

function debug() { ((DEBUG)) && echo ">>> $*"; }
function debug() { [ "DEBUG" ] && echo ">>> $*"; }
```

将执行日志全部写到某个文件

```bash
exec >>"$LOGPATH"/xx.log.$TODAY 2>&1
# begin of code
```