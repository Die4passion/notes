### shell script

```shell
#!/bin/bash
# do something
```

### run shell script

```shell
sh script.sh

or


chmod a+x script.sh
./script.sh
# 会读取首行的解释器。执行
```

### cmd

```shell
cmd1; cmd2

or 


cmd1
cmd2
```

### echo

echo 的功能正如其名，就是基于标准输出打印一段文本

```shell
echo "welcome to bash"
echo welcome to bash
```

使用不带引号的echo时，无法显示分号

使用单引号echo时，bash不会对单引号中的变量求值`'$var'`

echo 中转移换行符

默认情况，echo将换行标志追加到文本末尾，可以忽略结尾换行符

```shell
echo -n 'test\n'
```

对字符串进行转义

```shell
echo -e '1\t2\t3'
```

打印彩色输出

```shell
文字颜色码
    重置0
    黑色30
    红色31
    绿色32
    黄色33
    蓝色34
    洋红35
    青色36
    白色37

echo -e "\e[1;31m This is red test \e[0m"


背景颜色码
    重置0
    黑色40
    红色41
    绿色42
    黄色43
    蓝色44
    洋红45
    青色46
    白色47

echo -e "\e[1;42m Green Background \e[0m"

```

### printf

可以格式化字符串，使用参数同c中printf一样

```shell
printf "hello world"
```

默认不会加换行符，需要手动添加

```shell
printf "%-5s %-10s %-4.2f\n" 3 jeff 77.564
```

### 环境变量和变量

bash中，每个变量的值都是字符串，无论你给变量赋值时是否使用引号，值都会以字符串的形式存储

环境变量

查看所有与此终端进程相关的环境变量

```shell
env
```

查看某个进程的环境变量

```shell
cat /proc/$PID/environ
```

变量赋值

```shell
var=value
var='the value'
var="the $PARAM"

echo $var
echo ${var}

var=value 非变量赋值是相等操作
```

环境变量

```shell
未在当前进程中定义，而是从父进程中继承而来的变量
export 设置环境变量，之后，从当前shell 执行的任何程序都会继承这个变量


export PYTHONPATH=$PYTHONPATH:/home/ken/workspace
```

常用的环境变量

```shell
PATH 查找可执行文件路径，通常定义在/etc/environment or /etc/profile or ~/.bashrc

修改： export PATH=$PATH:/new/path/

HOME
PWD
USER
UID
SHELL
```

获取字符串长度

```shell
length=${#var}
```

识别当前shell版本

```shell
echo $SHELL
    /bin/bash
echo $0
    bash
```

检查是否为超级用户 or 普通用户

```shell
# root的UID=0

if [ $UID -ne 0 ]
then
    echo "not root user"
else 
    echo "root"
fi
```

修改bash的提示字符

```shell
设置PS1变量
\u用户名
\h主机名
\w当前工作目录

```

### shell数学运算

整数运算

let

```shell
no1=4
no2=5
let result=no1+no2

let no1++
let no2--
let no1+=7
let no2-=7
```

expr(少用)

```shell
result=`expr 3 + 4`
result=$(expr $no1 + 5)
```

其他方法

```shell
result=$[ no1 + no2 ]
result=$[ $no + 5 ]

result=$(( no1 + 5 ))
```

浮点数

```shell
echo "4 * 0.56" | bc
# 设定精度
echo "scale=2;3/8" | bc
# 进制转换
echo "obase=2;100" | bc
# 平方
echo "10^10" | bc
# 平根方
echo "sqrt(100)" | bc
```

### 命令状态

当命令成功完成，返回0

发生错误并退出，返回非0

可以从`$?`中获取`echo $?`

### 文件描述符和重定向

文件描述符：与文件输入/输出相关联的整数，用来跟踪已打开的文件

```shell
0 stdin 标准输入
1 stdout 标准输出
2 stderr 标准错误
```

重定向到文件

```shell
# 清空文件写入新内容
echo "test" > temp.txt
# 追加
echo "test" >> temp.txt


# > 等价于 1>
# >> 等价于 1>>
```

输出分离或合并

```shell
# 分离
cmd 2>stderr.txt 1>stdout.txt
# 合并
cmd >output.txt 2>&1
or 
cmd &> output.txt
```

扔到垃圾桶

```shell
/dev/null 特殊设备文件，接收到的任何数据都会被丢弃(位桶/黑洞)

只有标准错误
cmd 2>/dev/null


标准输出和标准错误
cmd >/dev/null 2>&1
```

同时输出到终端和文件

```shell
cmd | tee file1

tee默认覆盖，可以-a选项追加
cmd | tee -a file1
```

将stdinn作为命令参数

```shell
cmd1 | cmd2 | cmd3 -
```

将文件重定向到命令

```shell
cmd < file
```

自定义文件描述符

```shell
使用文件描述符3打开并读取文件
exec 3<input.txt
cat <&3


使用文件描述符4进行写入
exec 4>output.txt
echo newline >&4
```

### cat

cat, concatenate(拼接)

"cat" 代表了连结（Concatenation），连接两个或者更多文本文件或者以标准输出形式打印文件的内容

一般格式

```shell
cat file1 file2 file3

从管道中读取
OUTPUT_FROM_SOME_CMDS | cat

echo "test" | cat - file1
```

压缩空白行，多个连续空行变成单个

```shell
cat -s file
```

配合tr移除空白行

```shell
# 连续多个\n -> \n
cat file | tr -s '\n'
```

加行号

```shell
cat -n file
```

显示制表符等

```shell
cat -T file

cat f > t
注意： “>>” 和 “>” 调用了追加符号。它们用来追加到文件里，而不是显示在标准输出上。
”>“符号会删除已存在的文件，然后创建一个新的文件。
所以因为安全的原因，建议使用”>>“，它会写入到文件中，而不死覆盖或者删除。
```

输入多行文字（CTRL + d 退出）

```shell
cat > test.txt
```

### 数组和关联数组

普通数组，整数作为数组索引，借助索引将多个独立的数据存储为一个集合(list)

关联数组，可以使用字符串作为索引(map)

数组

定义

```shell
array_var=(1 2 3 4 5)
or
array_var[0]="test1"
array_var[3]="test3"
```

读取

```shell
echo ${array_var[0]}
```

以清单形式打印

```shell
echo ${array_var[*]}
echo ${array_var[@]}
```

长度

```shell
echo ${#array_var[*]}
```

获取索引列表

```shell
echo ${!array_var[*]}
```

关联数组

```shell
declare -A ass_array

内嵌索引-值
ass_array=([index1]=value1 [index2]=value2)

独立
ass_array[index3]=value3

echo ${ass_array[index1]}
```

### alias

alias 是一个系统自建的shell命令，允许你为名字比较长的或者经常使用的命令指定别名。

```shell
alias new_command='command seq'
unalias new_command

使用原生命令
\new_command
```

### date

"date"命令使用标准的输出打印当前日期和时间，也可以深入设置

读取日期

```shell
date
```

时间戳

```shell
date +%s
```

日期转换为时间戳

```shell
date --date "Thu Nov 18 08:07:21 IST 2010" +%s
```

日期格式化

```shell
星期    %a Sat
        %A Saturdy
月      %b Nov
        %B November
日      %d 31
固定日期格式   mm/dd/yy    %D
年      %y 10
        %Y 2010
小时       %I/%H  08
分钟     %M    33
秒        %S    10
纳秒     %N    696308515
Unix纪元时  %s
```

格式化

```shell
date "+%Y %B %d"

date +%Y-%m-%d
输出： 2011-07-28
date +"%Y-%m-%d %H:%M:%S"
```

设置日期和时间

```shell
date -s "格式化日期字符串"

date -s "21 June 2009 11:01:22"
```

延时

```shell
sleep number_of_seconds
```

两天后及两天前

```shell
date -d '2 days' +%Y%m%d
date -d '2 days ago' +%Y%m%d
```

某一天的几天前

```shell
TODAY=`date +%Y%m%d`
DAY_1_AGO=`date -d "$TODAY 1 days ago" +%Y%m%d`
```

时间戳日期转换

```shell
date -d @1193144433
date -d @1193144433 "+%Y-%m-%d %T"

反向：
date -d "2007-10-23 15:00:23" "+%s"
```

赋值给变量

```shell
DATE=$(date +%Y%m%d)
DATE=`date +%Y%m%d`
```

### 调试脚本

打印出所执行的每一行命令

```shell
bash -X script.sh
sh -x script.sh
```

在脚本中设置开关

```shell
set -x 在执行时显示参数和命令
set +x 关闭调试
set -v 当命令进行读取时显示输入
set +v 禁止打印输入
```

直接修改脚本

```shell
#!/bin/bash -xv
```

### 函数和参数

定义函数

```shell
function fname()
{
    statements;
}
or
fname()
{
    statements;
}
```

调用

```shell
fname;
传参
fname arg1 arg2;
```

接收参数

```shell
$1 第一个参数
$2 第二个参数
$n 第n个参数


"$@"被扩展成 "$1" "$2" "$3"
"$*"扩展成"$1c$2c$3",其中C是IFS第一个字符


"$@"使用最多，$*将所有的参数当作单个字符串
```

bash支持递归

导出函数，可以作用到子进程中

```shell
export -f fname
```

函数及命令返回值

```shell
cmd;
echo $?


退出状态，成功退出，状态为0；否则非0


cmd；

if [ $? -eq 0 ]
then
    echo "success"
else
    echo "fail"
fi
```

### 管道

前一个命令的输出作为后一个命令的输入

```shell
cmd1 | cmd2 | cmd3
```

### 读取命令输出

```shell
cmd_output=$(COMMANDS)
or
反引用
cmd_output=`COMMANDS`
```

### 子sell subshell

子shell本身是独立进程，不会对当前shell有任何影响

```shell
pwd;
(cd /bin; ls)
pwd #同上一个pwd
```

保留空格和换行符

```shell
out=$(cat text.txt)
echo $out # 丢失所有换行符

out="$(cat text.txt)"
echo $out # 保留

cat a
1
2
3
echo $(cat a)
123
echo "$(cat a)"
1
2
3
```

### read

read，用于从键盘或标准输入中读取文本

读取n个字符存入变量

```shell
read -n number_of_chars variable_name
```

不回显的方式读取密码

```shell
read -s var
```

显示提示信息

```shell
read -p "Enter input:" var
```

限时输入

```shell
read -t timeout var
```

设置界定符

```shell
read -d delim_char var
read -d ":" var
hello:
```

### 字段分隔符和迭代器

内部字段分隔符，Internal Field Separator，IFS

IFS默认为空白字符（换行符，制表符，空格）

```shell
data="name,sex,rollno"
oldIFS=$IFS
IFS=,
for item in $data
do
    echo $item
done

IFS=$oldIFS
```

### 循环

for循环

```shell
echo {1..50}

for i in {a..z}; do actions; done;

or 

for(( i=0; i<10; i++ ))
{
    commands;
}
```

while循环

```shell
while condition
do
    commands;
done
```

until循环

```shell
until condition
do
    commands;
done
```

### 比较和测试

if条件

```shell
if condition
then 
    commands;
elif condition
then
    commands;
else
    commands;
fi
```

逻辑运算符进行简化，短路运算更简洁

```shell
[ condition ] && action;
[ condition ] || action;
```

算术比较

```shell
-gt 大于
-lt 小于
-ge 大于等于
-le 小于等于
-ne 不等于
-eq 等于

注意[]和操作数之间的空格
[ $var -eq 0 ]

and

[ $var -ne 0 -a $var2 -ge 2 ]

or 

[ $var -ne 0 -o $var2 -ge 2 ]
```

文件测试

```shell
[ -f $file_var ] 正常文件路径或文件名
[ -x $var ] 可执行
-d 目录
-e 存在
-c 字符设备文件
-b 块设备文件
-w 可写
-r 可读
-L 符号链接
```

字符串比较

```shell
[[ $str1 = $str2 ]]
[[ $str1 == $str2 ]]

[[ $str1 != $str2 ]]

[[ $str1 > $str2 ]]
[[ $str1 < $str2 ]]

[[ -z $str1 ]] 空
[[ -n $str1 ]] 非空

if [[ -n $str1 ]] && [[ -z $str2 ]]
then 
    commands;
fi
```

### find

搜索指定目录下的文件，从开始于父目录，然后搜索子目录

基本

```shell
find_base_path

# 打印文件和目录列表
find . -print # 默认 \n 分割文件名
```

文件名

```shell
find path -name "*.txt" -print
          -iname 忽略大小写

多个条件 or
find . \( -name "*.txt" -o -name "*.py" \)  
```

文件路径

```shell
通配符
find /home/users -paths "*slynux" -print

正则 
find . -regex ".*\(\.py\|\.sh\)$"
       -iregex 忽略大小写
```

否定参数

```shell
find . ! -name "*.txt" -print
```

根据文件类型

```shell
find . -type d print
f 普通文件
l 符号链接
d 目录
c 字符设备
b 块设备
s 套接字
p Fifo
```

设定目录深度

```shell
find . -maxdepth 1 -type f print
find . -maxdepth 2 -type f print
```

根据文件时间搜索

```shell
计量单位 天
-atime 最近一次访问时间
-mtime 最后一次被修改时间
-ctime 文件元数据，最近一次修改时间

find . -type f -atime -7 -print # 最近7天内被访问的
find . -type f -atime 7 -print # 恰好7天前
                     +7 -print # 超过7天

计量单位 分钟
-amin 访问时间
-mmin 修改时间
-cmin 变化时间

find . -type f -amin + 7 -print # 访问时间超过7分钟的

find . -type f -newer file.txt -print 
# 用于比较时间戳的参考文化，比参考文件更新的文件
```

基于文件大小的搜索

```shell
find . -type f -size +2k
+ 大于 - 小于 无符号，恰好相等

b 块
c 字节
w 字
k 千字节
M 兆字节
G G字节
```

删除匹配的文件

```shell
find . -type f -name "*.swap" -delete
注意：-delete位置一定是最后
```

文件权限及所有权

```shell
find . -type f -perm 644 -print
find . -type f -user slynux -print
```

执行命令或动作(最强大的命令)

```shell
find . -type f -user root -exec chown slynux {} \;
find . -type f -exec cp {} OLD \;
find . -iname "abx.txt" -exec md5sum {} \;

{}将被替换成对应文件名
exec 无法结合多个命令，可以将多个命令放入脚本，调用之
```

跳过指定目录

```shell
find . \( -name ".git" -prune \) -name '*.txt'
```

### xargs

将标准输入数据转化成命令行参数

将stdinn接收到的数据重新格式化，再将其作为参数传给其他命令

多行输入转化成单行输出

```shell
cat example.txt | xargs # 空格替换掉\n
```

切成多行，每行n个参数

```shell
cat example.txt | xargs -n 3
```

可以指定分隔符

```shell
echo "aaaXbbbXccc" | xargs -d 'X'
```

将参数传递给脚本(类似循环)

```shell
cat args.txt | xargs -n 1 ./cecho.sh

./cecho.sh -p arg1 1
需要变更
cat args.txt | xargs -I {} ./cecho.sh -p {} 1
```

find 与 xargs 组合

```shell
find .-type f -name "*.txt" -print | xargs rm -rf
```

其他

```shell
cat file | ( while read arg; do cat $arg; done )
cat file | xargs -I {} cat {}
```

### tr

tr可以对来自标准输入的字符串进行替换，删除以及压缩(**translate**, 可以将一组字符变为另一组字符)

tr 只能通过stdinn,无法通过其他命令进行接收参数

格式

```shell
tr [options] source-char-set replace-char-set
```

选项

```shell
-c 取source-char-set 补集,通常与-d/-s配合
-d 删除字source-char-set中所列的字符
-s 浓缩重复字符，连续多个变成一个
```

字符替换

```shell
cat /proc/12501/environ | tr '\0' '\n'
```

大小写替换

```shell
echo "HELLO" | tr 'A-Z' 'a-z'
cat text | tr '\t' ' '
```

删除字符

```shell
echo "hello 123 world 456" | tr -d '0-9'
```

字符集补集

```bash
echo "hello 1 char 2" | tr -d -c '0-9' #删除非0-9 
```

压缩字符

连续的重复字符

```shell
echo "GNU is       not UNIX" | tr -s ' '
```

字符类

```shell
alnum 字母和数字
alpha 字母
cntrl 控制字符
digit 数字
graph 图形字符
lower 小写字母
print 可打印字符
punct 标点符号
space 空白字符
upper 大写字母
xdigit 十六进制字符

tr '[:lower:]' '[:upper:]'
```

### md5sum

32个字符的十六进制串

```shell
md5sum filename
md5sum filename1 filename2
```

`md5sum`就是计算和检验MD5信息签名。`md checksum`（通常叫做哈希）使用匹配或验证文件的文件的完整性，因为文件可能因为传输错误，磁盘错误或者无恶意的干扰等原因而发生改变。

单向散列

```bash
md5sum file
sha1sum file
```

### sha1sum

40个字符的十六进制串

```shell
sha1sum file
```

### 对目录进行校验

需安装md5deep软件包

```shell
md5deep/sha1deep
md5deep -rl dirname
        r 递归 ，l 相对路径
```

### sort

语法

```shell
sort [options] [file(s)]

-c 检查是否已经排序
-u 丢弃所有具有相同键值的记录

-b 忽略开头空白
-d 字典序
-g 一般数值，以浮点数类型比较字段，仅支持gnu
-i 忽略无法打印的字符

-k 定义排序键值字段
-n 以整数类型比较字段
-r 倒转
-o 输出到指定文件
```

排序

```shell
sort file1 > file1.sorted
sort -o file1.sorted file1
```

按字数，要明确

```shell
sort -n file1
```

逆序

```shell
sort -r file
```

测试一个文件是否已经被拍过序

```shell
sort -c file
if [ $? -eq 0 ];
then 
    echo "success"
fi
```

合并两个排过序的文件，并不需要对合并后的文件进行再排序

```shell
sort -m sorted1 sorted2
```

根据键或者列排序（按照哪一个列）

```shell
sort -k 1 data
```

限定特定范围内一组字符

```shell
key=char4-char8
sort -k 2,3 data
sort -k 2.4,5.6 file
第2个字段的第4个字符开始比较，直到第5个字段的第5个字符
```

忽略前导空白及字段序排序

```shell
sort -bd unsorted.txt
```

去重

```shell
sort a.txt | uniq
sort -u a.txt
```

### uniq

用法

```shell
uniq file
```

只显示未重复的记录

```shell
uniq -u file
```

找出重复的行

```shell
uniq -d file
-s 可指定跳过前N个字符
-w 指定用于比较的最大字符数
```

统计各行出现的次数

```shell
uniq -c file
```

### tempfile

只有再基于Debian的发布版本才有(Ubuntu/Debian)

```shell
temp_file=$(tempfile)
等同
temp_file="/tmp/file-$RANDOM"

# $$为进程id
temp_file="/tmp/var.$$"
```

### split

按大小分割文件，单位k(kb)，M, G, c(byte), w(word)

```shell
split -b 10k data.file
```

-d 数字后缀 -a 后缀长度

```shell
split -b 10k data.file -d -a 4
```

分割后指定文件名前缀

```shell
split -b 10k data.file file_prefix
设后缀置格式
split -b 10k date.file -d -a 4 file_predix
```

根据行数进行分割

```shell
split -l 10 data
```

气扩展时csplit，可根据文件特性切分，关注

### bash 变量匹配切分

sample.jpg

```shell
file_jpg="sample.jpg"

从右向左匹配
${file_jpg%.*}
#sample

从左向右匹配
${file_jpg#.*}
#jpg

% # 属于非贪婪
%% ## 属于贪婪
```

贪婪 非贪婪

```shell
var=hack.fun.book.txt
${var%.*} #hack.fun.book
${var%%.*} #hack

${var#.*} #fun.book.txt
${var##.*} #txt
```

### expect

实现自动化

```shell
spawn ./ineractive.sh
expect "Enter the number:"
send "1\n"
expect "Enter name:"
send "hello\n"
expect eof

spawn 指定需要自动化的命令
expect 提供需要等待的消息
send 发送消息
expect eof 指明命令交互结束
```

### comm

两个文件之间比较，输出三列

```shell
onlyA \t onlyB \t bothAB

comm A B -1 -2 # 删除第一第二列

可以得到A^B A-B B-A
```

### mkdir

"mkdir" (Make directoty) 命令再命名路径下创建新的目录。然而如果目录以及存在，那么他就会返回一个错误信息”不能创建文件夹，文件夹已经存在“

(cannot create folder, folder already exists)

```shell
mkdir dirpath

mkdir -p dirpath1/dirpath2
# 一次多个目录
mkdir -p /home/user/{test,test1,test2}
```

注意： 目录只能在用户拥有写权限的目录下才能创建

### ls

ls命令是列出目录内容（List Directory Contents）的意思。运行他就是列出文件夹里的内容，可能是文件也可能是文件夹

ls 文件的内容关系

```shell
- 普通目录
d 目录
c 字符设备
b 块设备
l 符号链接
s 套接字
p 管道

文件权限序列
rwx
rwS setuid(S),特殊权限，出现在x的位置，允许用户以其拥有者的权限来执行文件，
即使这个可执行文件是由其他用户运行的


目录
r,允许读取目录中文件和子目录列表
w,允许在目录中创建或删除文件或目录
x,指明是否可以访问目录中的文件和子目录
rwt/rwT 粘滞位，只有创建该目录的用户才能删除目录中的文件，即使用户和其他用户也有写
权限，典型例子/tmp，写保护
```

查看目录

```shell
ls -d */
ls -F | grep "/$"
ls -l | grep "^d"
find . - type d maxdepth 1 -print
```

其他

```shell
ls -l  命令以详情模式(long listing fashion)列出文件夹的内容
ls -a  命令会列出文件夹里的所有内容，包括以“.”开头的隐藏文件
```

### chmod

设置文件权限

chmod 命令就是改变文件的模式位。

chmod会根据要求的模式来改变每个所给的文件，文件夹，脚本等等的文件模式（权限）。

设置权限

```shell
user group others all
u    g     o      a

chmod u=rwx g=rw 0=r filename
chmod u+x filename
chmod a+x filename #所有

chmod a-x filename
chmod 764 filename

#设置粘滞位
chmod a+t dirname

#递归改变
chmod 777 . -R
```

注意：对于系统管理员和用户来说，这个命令是最有用的命令之一了。在多用户环境或者服务器上，对于某个用户，如果设置了文件不可访问，那么这个命令就可以解决，如果设置了错误的权限，那么也就提供未授权的访问。

### chown

每个文件都属于一个用户组和一个用户

chown 命令用来改变文件的所有权，所以仅仅用来管理和提供文件的用户和用户组授权。

改变所有权

```shell
chown user.group filename
```

递归

```shell
chown -R user.group .
```

每次都以其他用户身份执行（允许其他用户以文件所有者的身份来执行）

```shell
chmod +s executable_file

chown root.root executable_file
chmod +s executable_file
./executable_file
```

### chattr

创建不可修改文件

```shell
chattr +i file
```

一旦被设置为不可修改，任何用户包括超级用户都不能删除该文件，除非其不可修改的数学被移除

```shell
chattr -i file
```

### touch

touch 命令代表了将文件的访问和修改时间更新为当前时间。

touch 命令只会在文件不存在的时候才会创建它（空白文件）。如果文件已经存在了，他会更新时间戳，但是并不会改变文件的内容。

空白文件

```shell
touch filename

for name {1..100}.txt
do
    touch $name
done
```

修改文件访问时间

```shell
touch -a "Fri Jun 25 20:50:14 IST 1999" filename
touch - #修改文件内容的修改时间
```

修改文件或目录的时间戳（YYMMDDhhmm）

```shell
touch -t 0712250000 file
```

注意：touch 可以用来在用户拥有写权限的目录下创建不存在的文件

### ln

建立软连接

```shell
ln -s target symbolic_link_name
```

如果目的路径已经存在，而没有指定-f标志，ln命令不会创建新的链接，而是向标准错误写一条诊断消息并继续链接剩下的 SourceFiles。

-f 促使 ln 命令替换掉任何已经存在的目的路径

### readlink

读取链接对应的真实路径

```shell
readlink web 

readlink ~/.vim
/Users/ken/github/k-vim
```

### file

通过查看文件内容来找出特定类型的文件

打印文件类型信息

```shell
file filename
```

打印不包含文件名在内

```shell
file -b filename
```

e.g.

```shell
file /etc/passwd
/etc/passwd: ASCII text

file -b /etc/passwd
ASCII text
```

读文件

```shell
while read line;
done 
    something
done < filename
```

### diff

生成文件差异

非一体化

```shell
diff version1.txt version2.txt
```

一体化，可读性更好

```shell
diff -u version.txt
```

使用patch将命令应用于任意一个文件

```shell
diff -u version1.txt version2.txt > version.patch
patch -p1 version1.txt < version.patch
```

递归作用于目录

```shell
diff -Naur directory1 directory2

-N 所有缺失的文件作为空文件
-a 所有文件视为文本文件
-u 一体化输出
-r 递归遍历
```

### head

前10行打印

```shell
head file
```

前n行

```shell
head -n 4 file
```

扣除最后N行之外的所有行

```shell
head -n -5 file
```

### tail

最后10行

```shell
tail file
```

打印最后5行

```shell
tail -n 5 file
tail -5 file
```

扣除前n行

```shell
tail -n +(N+1)
```

实时动态打印

```shell
tail -f growing_file
```

当某个给定进程结束后，tail 随之终结

```shell
tail -f file --PID $PID
```

### pushd/popd

将当前路径压入栈

```shell
pushed
```

压入某个路径

```shell
pushd /home/ken
```

查看当前路径列表

```shell
dirs
```

切换到某一个

```shell
# dirs从左到右编号 0 -
pushd +3
```

移除最近压入栈的路径并切换到下一个目录

```shell
popd
```

### cd

经常使用的“cd”命令代表了改变目录。它在终端中改变工作目录来执行，复制，移动，读，写等操作。

切换到上一目录

```shell
cd -
```

回到HOME目录

```shell
cd
cd ~
```

回到上一级目录

```shell
cd ..
```

### wc

World Count

统计行数

```shell
wc -l file
```

### tree

以图形化的树状结构打印文件和目录的结构，需要自行安装

```shell
tree ~/unixfile
```

重点标记出匹配某种样式的文件

```shell
tree  PATH -P "*.sh"
```

只标记符合样式之外的文件

```shell
tree path -I PATTERN
```

> **同时打印文件和目录大小**

```shell
tree -h
```

### grep

文本搜索工具，支持正则表达式和通配符

`grep`命令搜索指定文件中包含给定字符串或单词的行

基本用法

```shell
grep "match_pattern" file1 file2
```

使用颜色重点标记

```shell
grep word filename --color=auto
```

扩展型使用正则

```shell
grep -E "[a-z]+"
egrep "[a-z]+"
```

只输出匹配到的文本部分

```shell
grep -o word filename
```

除陪陪行外的所有行

```shell
grep -v word filename
```

统计匹配行数

```shell
grep -c 'tet' filename
```

打印出包含匹配字符串的行数

```shell
grep linux -n filename
```

打印样式匹配所位于的字符或字节的偏移

```shell
echo "gnu is not unix" | grep -b -o "not"
```

搜索多个文件，找出匹配文本位于哪个文件中

```shell
grep -l linux file1 file2
取反
grep -L
```

递归搜索目录

```shell
grep -R "text" dir
```

忽略大小写

```shell
grep -i "hello" filename
```

匹配多个样式

```shell
grep -e "pattern1" -e "patterns" file
```

运行匹配脚本

```shell
grep -f pattern_file source_file

pattern_file:
hello
cool
```

在搜索中包含、排除文件

```shell
grep --include *.{c,cpp} word file
```

排除

```shell
grep --exclude "Readme" filename
--exclude-dir
```

静默输出，用户判断(不会产生任何输出)

```shell
grep -q word file
if [ $? -eq 0 ]
```

打印匹配行之前，之后的行

```shell
grep -A 3 之后3行
grep -B 3 之前
grep -C 3 前后
```

使用行缓冲

```shell
在使用tail -f 命令时是可以即时看到文件的变化的，但是如果再加上一个grep命令，
可能看到的就不那么即时了，
因为grep命令在buffer写不满时就不输出，可以通过选项 --line-buffered 
来搞定，如：

tail -f file.txt | grep something --line-buffered
```

### cut

语法

```shell
cut -c list [  file ... ]
cut -f list [ -d delim ] [ file ... ]

-c list 以字符为主，作剪切操作
```

提取字段或列

```shell
# 第一列
cut -f1 filename

# 第二三列
cut -f2,3 filename
```

提取补集

```shell
cut -f1 --complement filename
```

指定字段分隔符

```shell
cut -d ";" -f2 filename
cut -d : -f 1,5 /etc/passwd
```

指定字符

```shell
-b 字节
-c 字符
-f 字段

cut -c1-5 filename
N-
N-M
-M

ls -l | cut -c 1-10
```

 指定输出分隔符

```shell
cut -c1-3,6-9 --output-delimiter ","
```

### join

语法

```shell
join [options] file1 file2

选项
-1 field1
-2 field2
-o file.field
-t seprator
```

例子

```shell
join file1 file2
```

### sed

sed(Stream editor)流编辑器，可以配合正则使用，进行替换等

sed替换语法

```shell
sed 's/pattern/replace_string/' file
```

将结果直接运用于源文件

```shell
-i 用于，直接修改源文件

替换第一个
sed -i 's/pattern/replace_string/' file

替换第二个
sed -i 's/pattern/replace_string/2' file

替换所有
sed -i 's/pattern/replace_string/g' file

从第N处开始替换
sed -i 's/pattern/replace_string/2g' file 
```

移除空白行

```shell
sed '/^$/d' file
```

已匹配字符串标记

```shell
引用匹配到的
sed 's/\w\+/[&]/g' filename
```

组合多个表达式

```shell
sed 'exp1' | sed 'exp2'
等价
sed 'exp1;exp2'
```

使用引用

```shell
sed "s/$text/HELLO/"
```

子串匹配标记（后向引用，最多9个）

```shell
sed 's/\([a-z]\+\)' \([a-z\]\+\)/\2 \1/' filename
```

保存到文件

```shell
sed 's/pattern/replacement/' -i outfile
```

使用其他分隔符

```shell
sed 's#/home/#/tmp/#'
```

### awk

基本结构

```shell
awk -F '-' 'BEGIN{statement} {statements} END{statements}' file
表达式中单引号可以换成双引号
BEGIN -> 每一行，执行statements，执行END
```

打印某一列

```shell
awk -F '-' '{print $0}' file # 全部
awk -F '-' '{print $2}' file # 第二列
```

print 拼接字符

```shell
awk '{var="v1"; var1="v2"; print var1"-"var2;}'
```

特殊变量

```shell
NR number of records,记录数
NF number of fields, 字段数
$0 当前行文本
$1 第一字段
$2 第二字段
$NF 最后一个字段

FILENAME 当前输入文件的名称
FNR 当前输入文件记录数
FS 字段分隔字符
OFS 输入字段分隔符, 默认" "
ORS 输出记录分隔符，默认"\n"
```

统计行数

```shell
awk 'END{print NF}'
```

将外部变量值传递给awk

```shell
awk -v VARIABLE=$VAR '{ print VARIABLE }'
awk '{print v1, v2}' v1=$var1 v2=$var2
```

读取行

```shell
seq 5 | awk '{ getline var; print var }'
```

进行行过滤

```shell
awk 'NR<5' # 行号小于5
awk 'NR==1,NR==4' # 行号在1到5之间
awk '/linux/' # 包含样式linux
awk '!/linux/' # 不包含
awk '$1 ~/jones/' # 第一个字段包含jones

tail file 
awk 'NR <= 10' file
```

设定分隔符

```shell
awk -F: '{ print $NF }' file
```

设定输出分隔符

```shell
awk -F: -v "OFS=-" '{print $1,$2}' /etc/passwd
```

打印空行

```shell
awk 'NF>0 {print $0}'
or
awk 'NF>0' #未指定action默认打印
```

print和printf

```shell
awk -F: '{print "User", $1, "is really", $5}' /etc/passwd
awk -F: '{printf "User %s is really %s\n", $1, $5}' /etc/passwd
```

awk中使用循环

```shell
for(i=0;i<10;i++) { print $i; }

for(i in array) { print array[i] }
```

内建函数

```shell
length(str)
index(str,search_str)
split(str,array,delimiter) # 用界定符生成一个字符串列表
substr(string, start, end) # 子串
sub(regex, replacement_str, str) #正则替换收割匹配位置
gsub(regex, replacement_str, string) # 最后一个匹配位置
match(string, regex) # 检查是否能够匹配字符串
tolower(string) #转小写
toupper(string) #转大写
```

写成脚本文件

```shell
BEGIN {}
pattern1 {action1}
pattern2 {action2}
END {}
```

### 文件迭代

读文件行

```shell
while read line;
do 
    echo $line;
done < file.txt
```

迭代每个单词

```shell
for word in $line;
do 
    echo $words;
done
```

迭代每一个字符

```shell
for ((i=0;i<${#word};i++))
do
    echo $(word:i:i);
done
```

### paste

按列合并文件

```shell
paste file1 file2 file3
```

指定分隔符

```shell
paste file1 file2 -d ','
```

### tac

逆序打印

```shell
tac file1 file2
```

### rev

接收一个文件或stdin作为输入，逆序打印每一行内容

```shell
echo "abc" | rev
```

### wget

Wget是用于非交互式（例如后台）下载文件的免费工具，支持HTTP，HTTPS，FTP协议和

HTTP代理（选项多，用法灵活）

一个用于文件下载的命令行工具

```shell
wget URL1 URL2
```

指定保存文件名

```shell
wget URL -O local.txt
```

指定日志，默认是到stdout

```shell
wget URL -O local.txt -o log.txt
```

指定重复尝试次数

```shell
wget -t 5 URL
```

下载限速

```shell
wget --limit-rate 20k url
```

指定限额

```shell
wget -Q 100m url
```

断点续传

```shell
wget -c URL

wget -c -t 100 -T 120 http://www.linux.com/xxxx.data

当文件特别大或者网络特别慢的时候，往往一个文件还没有下载完，连接就已经被切断，
此就需要断点续传。 wget的断点续传是自动的。

-c 选项的作用为断点续传。
-t 参数表示重试次数（例如重试100次， -t 100, 如果-t 0，表示无穷次直到成功）
-T 参数表示超时等待时间。例如-T 120，表示等待120秒连接不上就算超时
```

复制或镜像整个网站

```shell
wget --mirror example.admin.com
wget -r -N -l DEPTH URL
    递归，允许对文件使用时间戳，层级

wget -r -np -nd http://www.linux.com/packs/

-np 的作用是不遍历父目录
-nd 表示不在本机重新创建目录结构
```

访问需要认证的HTTP/FTP

```bash
wget --user username --password pass URL
```

post 请求

```bash
wget url -post-data "name=value" -O output.html
```

批量下载

```bash
wget -i downloads.txt # 将文件地址写入一个文件
```

用wget命令执行ftp下载

```bash
wget -m ftp://username:password@hostname
```

### curl

基本用法

```bash
curl url > index.html
curl url -o filename
```

不显示进度信息

```bash
curl URL --silent
```

将内容写入文件，而非标准输出

```bash
curl URL --silent -O
```

写入指定文件

```bash
curl URL --silent -o filename
```

显示进度条

```bash
curl url -o index.html --progress
```

断点续传

```bash
curl -C - URL 
```

设置参照页字符串

```bash
curl --referer Referer_URL target_URL
跳转到target_URL,其头部referer为Referer_url
```

设置cookie

```bash
curl url --cookie "user=slynux;pass=hack"

另存为一个文件
curl URL --cookie-jar cookie_file
```

设置用户代理

```bash
curl URL --user-agent "Mozilla/5.0"
头部信息
curl -H "Host: www.slynux.org" -H "Accept-languague: en" url
```

限定下载带宽

```bash
curl url --max-filesize bytes
超出限制的话，返回非0
```

进行认证

```bash
curl -u user:pass url
```

只打印头部信息，不下载远程文件

```bash
curl -I url
curl -head url
```

发送post请求

```bash
curl URL -d "va1=1&va2=2" 
         --data
```

### lynx

将网页以ascii字符形式下载

```bash
lynx -dump URL > webpage_as_text.txt
```

打印出网站的文本板块而非html

```bash
lynx -dump url
```

生成信息文件

```bash
lynx -traversal url
```

### tar

`tar`命令是磁带归档`Tape Archive`，对创建一些文件的归档和他们的解压很有用。

将多个文件和文件夹保存成单个文件，同时还能保留所有的文件属性

对文件进行归档

```bash
-c create file, 创建文件
-f specify filename, 指定文件名

tar cf output.tar file1 file2 file3
tar cf output.tar 

tar cvf output.tar *.txt
```

向归档中追加文件

```bash
tar rvf original.tar new_file
-r,追加
```

提取文件或文件夹

```bash
-x, exact

tar xf archive.tar

-C, 指定文件
tar xf archive.tar -C /path/to/extraction_directory

tar xvf archive.tar
```

提取指定文件

```bash
tar svf file.tar file1 file4
```

拼接两个归档文件

```bash
tar Af file1.tar file2.tar
# file2 合并到file1中
```

只有在文件内容修改时间更新`newer`，才进行添加

```bash
tar uvvf archive.tar filea
```

比较归档文件与文件系统中的内容

```bash
tar df archive.tar filename1 filename2
```

从归档文件中删除文件

```bash
tar f archive.tar --delete file1 file2
```

提取到某个目录

```bash
tar zxvf package.tar.gz -C new_dir
```

压缩归档文件

```bash
gzip/gunzip -> .gz
f.tar.gz     -z
tar      -czvf
tar      -xzvf

bzip/bunzip -> .bz2
f.tar.bz2 -j

f.tar.lzma --lzma
f.tar.lzo
```

从归档中排除部分文件

```bash
tar cf arch.tar * --exclude "*.txt"

cat list
    filea
    fileb

tar cf arch.tar * -X list
```

排除版本控制文件

```bash
tar --exclude-vcs czvvf source.tar.gz files
```

打印总字节数

```bash
tar -cf arc.tar * --exclude "*.txt" --totals
```

### cpio

**C**opies **f**iles **i**n and **o**ut of archives.

使用频率不高

归档，保留文件属性（权限、所有权等）

```bash
echo file1 file2 | cpio -ov > archive.cpio
-o 指定输出
-v 打印归档文件列表
```

列出cpio中的文件内容

```bashag-0-1erqof3k4ag-1-1erqof3k4
cpio -it < archive.cpio
-i 指定输入
-t 列出归档文件中的内容
```

### gzip

压缩，会删除源文件

```bash
gzip filename
# got filename.gz
```

解压

```bash
gunzip filename.gz
```

列出文件属性信息

```bash
gzip -l filename.gz
```

stdin读入文件并写出到stdout

```bash
cat file | gzip -c > file.gz
```

压缩归档文件

```bash
tar czvvf archive.tar.gz [files]
or
tar cvvf archive.tar.gz [files]
gzip archive.tar
```

指定压缩率

```bash
1-9,1最低，但速度最快
gzip -9 test.img
```

### zcat

无需解压缩，直接从.gz中提取内容

```bash
zcat test.gz
```

### bzip2

更大的压缩率

```bash
bzip2 filename
```

解压缩

```bash
bunzip2 filename.bz2
```

stdin到stdout

```bash
cat file > bzip2 -c > file.tar.bz2
```

压缩归档

```bash
tar cjvvf archive.tar.bz2 [files]
or
tar cvvf archive.tar [files]
bzip2 archive.tar
```

保留输入文件

```bash
bunzip2 test.bz2 -k
```

压缩率

```bash
bzip2 -9 test.imgag-0-1erqof3k4ag-1-1erqof3k4
```

### lzma

比`gzip/bzip2`更好的压缩率

压缩

```bash
lzma filename
```

解压

```bash
unlzma filename.lzma
```

stdin到stdout

```bash
cat file | lzma -c > file.lzma
```

创建归档

```bash
tar cavvf archive.tar.lzma [files]
    xavf
```

保留输入文件

```bash
lzma test.bz2 -k
```

压缩率

```bash
lzma -9 test.img
```

### zip

压缩

```bash
zip archive_name.zip [source_files/dirs]
```

对目录和文件进行递归操作

```bash
zip -r archive.zip folder1 file2
```

### base64

编码

```bash
base64 filename > outfile

cat file | base64 > outfile
```

解码

```bash
base64 -d file > outfile
```

### rsync

可以对位于不同位置的文件和目录进行备份，借助差异计算和压缩技术实现最小化数据传输量

要确保远端安装来openssh

从一个目录复制到另一个目录

```bash
rsync -av source_path dest_path
-a 进行归档 -v 打印细节
路径可以是本地，也可以是远端

eg:
rsync -av /home/test /home/backups/  #复制到backups目录下
rsync -av /home/test /home/backups   #创建backups目录，复制
```

备份到远程服务器

```bash
rsync -av source_path user@host:PATH
可以反向
```

改善传输速度

```shell
rsync -avz source destination
```

排除文件

```bash
rsync -avz source dest --exclude "*.txt"
                       --exclude-from FILEPATH

FILEPATH:
*.bak
```

更新备份时，删除不存在的文件

```bash
rsync -avz source dest --delete
```

### git

初始化目录

```bash
git init
```

配置用户信息

```bash
git config --global user.name "Die4passion"
git config --global user.email "465335731@qq.com"
```

加到远端

```bash
git remote add origin user@remotehost:/home/backup/backup.git
git push origin master
```

设置`github`代理

```bash
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
# 设置完成后, ~/.gitconfig 文件中会增加以下条目:
[http "https://github.com"]
    proxy = socks5://127.0.0.1:1080
```

添加

```bash
git add *
```

删除

```bash
git rm *.py
```

标记一个检查点

```bash
git commit -m "Commit message"
```

查看日志

```bash
git log
# 显示commit历史，以及每次commit发生变更的文件
git log --stat
# 搜索提交历史，根据关键词 （keyword是文件名，不必完全匹配）
git log -S [keyword]
# 显示某个commit之后的所有变动，每个commit占据一行
git log [tag] HEAD --pretty=format:%s
# 单行
git log --pretty=oneline
# 显示某个commit之后的所有变动，
# feature 时 commit信息里的内容
git log [tag] HEAD --grep feature
# 显示某个文件的历史版本，包括改文件名
git log --follow [file]
git whatchanged [file]
# 显示指定文件的每一次diff 【常用】
git log -p [file]
# 显示过去的n次提交
git log -n --pretty --oneline
# 查看远程分支合并图
git log --graph --pretty --oneline --abbrev-commit
# 显示所有提交过的用户，按提交次数排序
git shortlog -sn
# 显示指定文件是什么人什么时间修改过
git blame [file]
# 显示你写了多少行代码
git diff --shortstat "@{0 day ago}" # 今天
git diff --shortstat "@{1 day ago}" # 昨天和今天
git diff --shortstat "@{1 month ago}" # 这个月
```

回滚到某个版本

```bash
git checkout hashid [ filename ]
```

克隆

```bash
git clone url
```

查看版本库状态

```bash
git status -
```

回退到上个版本

```bash
git reset --hard HEAD^
```

回退到上上个版本

```bash
git reset --hard HEAD^^
```

回退到某个版本

```bash
git reset --hard 34732842
```

查看提交历史，确定需要的版本号

```bash
git reflog
```

创建分支，然后切换到该分支

```bash
git checkout -b dev
# 相当于
git branch dev && git checkout dev
```

列出当前所有分支

```bash
git branch
```

合并dev到master

```bash
git merge dev # 当前在master上
git merge --no-ff -m "msg" dev 
# --no-ff 表示禁用 fast forward
```

删除指定分支

```bash
git branch -d dev
```

查看分支的合并情况

```bash
git log --graph --pretty=oneline --abbrev-commit
```

存下工作现场，恢复现场后继续工作

```bash
git stash
```

查看有哪有stash

```bash
git stash list
```

应用stash内容

```bash
git stash apply
```

删除stash

```bash
git stash drop
```

回到现场的同时删除stash

```bash
git stash pop
```

一些笔记：

- git 支持多种协议，但通过ssh'支持的原生git协议速度最快

- 当手头工作没有完成时，先stash修复bug再stash pop回到现场

### dd

`Data Definition` 要注意参数顺序，错误的参数会损毁所有数据

可以用来转换和复制文件，大多数时间是用来复制iso文件(或任何其他文件)到一个usb设备(或其他任何地方)中去，所以可以用来制作USB启动器

语法说明

```bash
dd if=SOURCE of=TARGET bs=BLOCK_SIZE count=COUNT
# if/of  输入/输出文件或设备路径
# 块大小
# count 限制复制到目标的字节数

dd if=/dev/zero of=/dev/sda1
# 制作iso 从cdrom设备读取所有数据，创建iso文件
dd if=/dev/cdrom of=cdrom.iso 
```

备份恢复

```bash
dd if=/dev/sda1 of=x.img

dd if=x.img of=/dev/sda1
```

生成任意大小的文件

```shell
# 创建一个1M大小的文件 junk.data
# bs=2M count=2 则文件大小4M

dd if=/dev/zero of=junk.data bs=1M count=1
    输入文件       输出文件    块大小   复制块数


块大小单位
字节(1B)    c
字(2B)      w
块(512B)    b
千字节(1024B) k
兆字节(1024KB) M
吉字节(1024MB) G
```

### mount

`mount`是一个很重要的命令，用来挂载不能自动挂载的文件系统。你需要root权限挂载设备。在插入你的文件系统后

```bash
mount --bind /source /destination

首先运行“lsblk命令”，识别出你的设备，然后把分配的设备名记下来。
```

挂载镜像

```bash
mount -o loop file.img /mnt/mount_point
```

## 网络相关

### ifconfig

显示网络接口、子网掩码等详细信息

```bash
ifconfig
/sbin/ifconfig
```

打印某个特定网络接口

```bash
ifconfig iface_name

eg:
ifconfig en1

HWaddr    MAC地址
inet addr     ip地址
Bcast    广播地址
Mask     子网掩码
```

设置网络接口ip

```bash
ifconfig wlan0 192.168.0.80
```

dns

```bash
cat /etc/resolv.conf
host google.com # Dns查找
nslookup google.com # 更详细信息
```

修改`dns/host`

```bash
echo nameserver IP_ADDRESS >> /etc/resolv.conf

echo ip domain >> /etc/hosts
```

路由信息

```bash
显示路由表
route

以数字形式显示地址
route -n
```

设置默认网关

```bash
route add default gw 192.168.0.1 wlan0
```

`trace_route`，显示分组途径的所有网关的地址

```bash
traceroute google.com
```

### ping

基本

```bash
ping ADDRESS # 主机名，域名 或 ip
```

`ping`可以得到`RTT(Round Trip Time)`，分组从源到目的主机的往返时间，单位ms

限制发送分组数

```bash
ping ADDREDD -c COUNT
```

### fping

同时ping一组ip，而且响应非常快

```bash
fping -a ip1 ip2 -g
fping -a 192.160.1/24 -g
fping -a < ip.list

# -a 所有活动主机的ip
# -g 从IP/MASK生成的ip地址范围
```

进行dns查询

```bash
fping -a -d 2>/dev/null < ip.list
```

### lftp

基本用法

```bash
lftp username@ftphost
cd dir
# lcd 改变本地主机目录
mkdir
get/put
quit
```

### scp

`scp`是`secure copy`的缩写，是linux系统下基于ssh登录进行安全的远程文件拷贝命令。

可以在linux服务器之间复制文件和目录。

拷贝文件

```bash
scp filename user@remotehost:/home/pat

scp SOURCE DESTINATION
```

递归复制

```bash
scp -r dir1 user@remotehost:/home/backup
```

提高拷贝速度

```bash
scp -c arcfour -r -P20755 dir/ 192.168.2.*:/**/**/data/

# -c arcfour 这个算法没有加校验不保证完整性，慎用
# 内网1000M带宽，默认算法速度之恩达到30M/s
# 用 arcfour 可以达到 50~80M/s
```

### ssh

连接远程

```bash
ssh username@remote_host
ssh -p port username@remote_host
```

执行命令

```bash
ssh username@remote_host 'cmd1; cmd2' > stdout.txt 2>errors.txt
```

压缩功能

```bash
ssh -C user@hostname 'cmds'
```

打通ssh

- 创建ssh密钥

```bash
ssh-kengen -t rsa

# 公钥 ~/.ssh/id_rsa.pub
```

- 登录远端服务器，将公钥写入 `~/.ssh/authorized_keys`

### lsof

列出系统开发中开放端口及运行在端口上的服务

```bash
lsof -i 
```

配合grep，获取需要的信息

### netstat

查看开放端口和服务

```bash
netstat -tlnp
```

## 磁盘和系统

### du

`du = disk usage`

估计文件的空间占用。逐层统计文件（例如以递归方式）并输出摘要。

查看占用磁盘空间

```bash
du filename1 filename2
```

查看目录

```bash
du -a dir
```

以kb，mb或块为单位显示

```bash
du -h filename
```

显示总计情况

```bash
du -c filename
```

只显示合计

```bash
du -s filename
```

以特定单位打印

```bash
du -b/-k/-m/-B files
```

排除部分文件

```bash
du --exclude "*.txt" DIR
   --exclude-form EXCLUDE.txt DIR
```

指定最深层级

```bash
du --max-depth 2 DIR
```

指定目录最大的10个文件

```bash
du -ak S_DIR | sort -nrk 1 | head
```

### df

`df = disk free`

报告系统的磁盘使用情况。在跟踪磁盘使用情况方面对于普通用户和系统管理员都很有用。

`df`通过检查目录大小工资，但这一数值仅当文件关闭时才得到更新。

查看磁盘可用空间

```bash
df -h
```

### time

计算命令执行时间

```bash
time COMMAND

real 挂钟时间，从开始执行到结束的时间
user 进程花费在用户模式中的cpu时间，真正用于执行进程所花的时间
sys 进程花费在内核模式中的cpu时间
```

写入文件 (~~貌似废弃~~)

```bash
time -o output.txt COMMAND
time -a output.txt COMMAND # 追加
```

格式化输出(~~貌似废弃~~)

```bash
time -f "Time: %U" -a -o timing.log uname
real %e
user %U
sys %S
```

### who

获取当前用户登录信息

```bash
who / w
```

当前登陆主机的用户列表

```bash
users
```

### uptime

查看系统已经通电运行多长时间了

```bash
uptime # 也可以看到负载
```

### last

显示上次用户登录信息 ，前一次启动会话信息

```bash
last
```

获取单个用户

```bash
last USER
```

### watch

在终端中以固定间隔监视命令输出

```bash
# default 2s

# 5s
watch -n 5 ls
```

颜色标示

```bash
watch -d 'COMMAND'
```

## 进程和线程

### ps

ps命令给出真正运行的某个进程的状态，每个进程有特定的id成为PID。

ps命令主要查看系统中进程的状态

```bash
USER              PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
USER表示启动进程用户
PID表示进程标志号

%CPU表示运行该进程占用CPU的时间与该进程总的运行时间的比例
%MEM表示该进程占用内存和总内存的比例。

VSZ表示占用的虚拟内存大小，以KB为单位。
RSS为进程占用的物理内存值，以KB为单位。

TTY表示该进程建立时所对应的终端，"?"表示该进程不占用终端。
STAT表示进程的运行状态，包括以下几种代码：
    D，不可中断的睡眠；
    R，就绪（在可运行队列中）；
    S，睡眠；
    T，被跟踪或停止；
    Z，终止（僵死）的进程，Z不存在，但暂时无法消除；
    W，没有足够的内存分页可分配；<高优先序的进程；
    N，低优先序的进程；
    L，有内存分页分配并锁在内存体内（实时系统或I/O）。

START为进程开始时间。
TIME为执行的时间。
COMMAND是对应的命令名。
```

查看进程信息

```bash
# 当前终端
ps

PID TTY TIME CMD
PID 进程ID
TYY 终端
TIME 进程启动后过去的时间
CMD 进程对于的命令
```

查看所有进程

```bash
ps aux 
ps -ef
```

查看某个用户的所有进程

```bash
ps aux U root
```

命令格式

```bash
ps [OTHER OPTIONS] -o par1,par2,par3
ps -eo comm,pcpu | head
# pmem 内存使用率
# comm 可执行文件名
# etime 启动后度过的时间
```

设置升序降序

```bash
ps -eo comm,pcpu --sort -pcpu | head
# + 升序 - 降序
```

找出给定命令名对应进程ID

```bash
ps -C COMMAND_NAME
PS -c bash -o pid=
```

进程线程相关

```bash
ps -eLf --sort -nlwp | head
```

查看子进程数

```bash
ps axwef
```

!> 注意：当你要知道有哪些进程在运行或者需要知道想杀死的进程PID时ps命令很管用。你可以把它与`grep`何用来查询指定的输出结果，如：

```bash
ps -A | grep -i ssh
```

### pgrep

获取某个进程名对应进程id

```shell
pgrep gedit
```

`pgrep`只需要命令名的一部分，ps需要准确的全名

基本用法

```bash
pgrep bash
```

指定进程的用户

```bash
pgrep -u root,die4 COMMAND
```

返回匹配进程数

```bash
pgrep -c COMMAND
```

### top

查看占用cpu最多的进程列表

```bash
top
# 不查看僵尸进程
top -i 
```

### kill

kill是用来杀死已经无关紧要或者没有响应的进程，杀死一个进程需要知道进程的pid

列出可用信号

```bash
kill -l 
```

终止一个进程

```bash
kill PROCESS_ID_LIST
```

强杀进程

```bash
kill -9 PROCESS_ID
```

杀死一组命令

```bash
killall process_name
killall -9 process_name
```

### pkill

杀，接受进程名

```bash
pkill process_name
pkill -s SIGNAL process_name
```

### which

查找PATH下某个命令位置

```bash
which ls
```

### whereis

用来定位命令的二进制文件\资源\或者帮助页，举例来说，获得`ls`和`kill`命令的二进制文件/资源以及帮助页

```bash
whereis ls
whereis kill
```

类似`which`，多了命令手册位置，源代码位置

!> 当需要知道二进制文件保存位置时有用

### whatis

对命令的简短描述

### hostname

当前主机名

### uname

```bash
# 主机名 = hostname
uname -n
# 内核版本，硬件架构等
uname -a
# 内核发行版本
uname -r
# 主机类型 32/64
uname -m
# cpu相关信息
cat /proc/cpuinfo
# 内存信息
cat /proc/meminfo
```

### contab

格式

```bash
* * * * * cmd
# 从左到右 
# 分钟(0-59)
# 小时(0-23)
# 天(1-31)
# 月份(1-12)
# 工作日(0-6)

A,B A and B
*/C every C
```

查看

```bash
crontab -l
crontab -l -u die4
```

编辑

```bash
crontab -e
```

移除

```bash
crontab -r 
crontab -u root -r
```

可以在crontab中加入环境变量

### getopts

命令行参数处理

```bash
while getopts :f:vql opt
do 
    case $opt in
    f) file=$OPTARG
        ;;
    V) verbose=true
        ;;
    ....
```

### history

全部历史命令

```bash
history
```

!> 按`ctrl + R` 可以搜索补全执行过的命令

### sudo

`sudo(super user do)`命令允许授权用户执行超级用户或者其它用户的命令。通过在`sudoers`列表的安全策略来指定。

!> `sudo` 允许用户借用超级用户的权限，然而`su`命令实际上是允许用户以超级用户登录。所以`sudo`比`su`更安全。 并不建议使用`sudo`或者`su`来处理日常用途，因为它可能导致严重的错误如果你意外的做错了事，这就是为什么在linux社区流行一句话：

> “To err is human, but to really foul up everything, you need root password.” “人非圣贤孰能无过，但是拥有root密码就真的万劫不复了。”

### cal

`cal（Calender）`，它用来显示当前月份或者未来或者过去任何年份中的月份

```bash
cal 
cal 9 1752
cal jy # 一年的
```

### cp

“copy”就是复制。它会从一个地方复制一个文件到另外一个地方

```bash
cp file1 file2
cp -r dir1 dir2
```

快速备份一个文件

```bash
cp some_file_name{,.bkp}
```

!> `cp`在shell脚本中时最常用的一个命令，而且他可以使用通配符，来定制所需文件的复制

### mv

移动

### pwd

!> 这个命令并不会在脚本中经常使用，但是对于新手，当从连接到nux很久后在终端中迷失了路径，这绝对是救命稻草。

### free

```bash
free -m 
 total        used        free      shared  buff/cache   available
Mem:           7.9G        5.6G        2.1G         17M        223M        2.2G
Swap:           14G        162M         14G

```

显示剩余内存

```bash
free -m | awk '/[0-9]/{ print $4" MB" }'
```

### eval

```bash
eval "ls -l"
```

### basename

获取路径中文件部分

```bash
basename resolv.conf 
basename /etv/resolv.conf
```

### cmp

比较两个任意类型的文件并将结果输出至标准输出。如果两个文件相同，返回0: 如果不同，将显示不同的字节数和第一处不同的位置。

```bash
cmp file1 file2
diff file1 file2
```

### rm

标准移除命令。 可以用来删除文件和目录

```bash
rm file1 
rm -r fir1
rm -rf file_or_dir
```

!> `rm -rf`命令是一个破坏性的命令,假如你不小心删除一个错误的目录。 一旦你使用`rm -rf`删除一个目录,在目录中所有的文件包括目录本身会被永久的删除,所以使用这个命令要非常小心。

### service

`service`命令控制服务的启动、停止和重启，它让你能够不重启整个系统就可以让配置生效以开启、停止或者重启某个服务。

```bash
service nginx restart
```

注意：要想使用`service`命令，进程的脚本必须放在`/etc/init.d`，并且路径必须在指定的位置。 如果要运行`service apache2 start`实际上实在执行`service /etc/init.d/apache2 start`

### man

Man提供命令所有选项及用法的在线文档。几乎所有的命令都有它们的帮助页

```bash
man command
```

### passwd

这是一个很重要的命令，在终端中用来改变自己密码很有用。显然的，因为安全的原因，你需要知道当前的密码。

### gcc

gcc 是Linux环境下C语言的内建编译器。下面是一个简单的C程序，在桌面上保存为Hello.c，**记住必须要有‘.c‘扩展名**

```bash
gcc Hello.c
gcc -o Hello Hello.c
```

!> 编译C程序时，输出会自动保存到一个名为“a.out”的新文件，因此每次编译C程序 “a.out”都会被修改。 因此编译期间最好定义输出文件名.，这样就不会有覆盖输出文件的风险了。

### g++

g++是C++的内建编译器

```bash
g++ Add.cpp
g++ -o Add Add.cpp
```

### 关于 /dev/null

特别有用的特殊文件，位桶，传送到此文件的数据都会被系统丢弃。

### 语言及乱码

查看变量值

```bash
echo $LANG # 未设置任何LC_xxx时使用的默认值
echo $LC_ALL # 覆盖所有LC_XXX变量，总控开关
```

好的做法是，避免为任何LC_XXX变量赋值，使用LC_ALL和LANG来控制

!> 避免乱码：从编辑器到语言，再到系统，统一编码为UTF-8
