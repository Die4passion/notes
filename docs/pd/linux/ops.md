## else

uuid = universally unique identifier

## Unix Fundamentals 101

### 基本操作

`ctrl + f(orward)`  `ctrl + b(ackward)` 上下翻页

`~/.bash_profile`   linux环境变量配置

`ps aux | grep xxx`  查看进程 基本用这个

`top`    分析cpu占用   进去按m 切换内存显示方式

`df -h`     查看磁盘信息                 -h是 human

`df -h .`  查看当前磁盘信息

`du`        查看文件大小

`find . -name 'opsschool' -type d`     查找文件(d)或路径(f)  可正则

`lsof -i`  lists open files -internet

`cat file1 file2 > file3`    合并文件

`cut -f2,3  file`       表格(tab)文件 分列显示   -d 自定义分隔符  

`eg: -cut -f1 -d' '     -cut -f2 -d|`

`awk -F '-' '{print $1, $2}' file`    特定列

`awk 'NR==2' students.txt`            特定行

`awk 'length($0) > 20' file`          指定行长度

`awk -F '-' '{sum+=$2} END {print "Average age = ",sum/NR} students.txt` 算平均年龄

`sort`   排序

 `wc` 查看文件字节数等信息

> `crontab -e`

| 1    | 2    | 3            | 4     | 5         |
|:---- |:---- |:------------ |:----- |:--------- |
| 分    | 时    | day of month | month | dayofweek |
| 0-59 | 0-23 | 1-31         | 1-12  | 0-6       |

 eg:

```bash
 # .---------------- minute (0 - 59)
 # |   .------------- hour (0 - 23)s
 # |   |   .---------- day of month (1 - 31)
 # |   |   |   .------- month (1 - 12) OR jan,feb,mar,apr 
 ...
 # |   |   |   |  .----- day of week (0 - 7) (Sunday=0 or 
 7)  OR sun,mon,tue,wed,thu,fri,sat
 # |   |   |   |  |
 # *   *   *   *  *  command to be executed
```

 eg:

- `* * * * *`: every minutes
- `0 * * * *`: every hour, on the hour
- `0 0 * * *`: every day at midnight server time
- `0 0 * * 0`: every Sunday at midnight server time
- `*/10 * * * *`: every ten minutes
- `0 */4 * * *`: every four hours, on the hour

## Unix Fundamentals 201

`strace`    跟踪系统调用和信号    `参数 -c -T -e -o -i`  很多  需要看文档用

`initctl`   ubuntu自带   centos需要`yum install upstart`

`dstat`    `yum install`   通用系统资源统计工具   `dstat -m 5 10`
`nethogs`   `yum install`   网络流量监控工具
`sar`                  系统状态统计工具               `str -A`

## Text Editing 101

### vim 操作

- `h` 左 `j` 下 `k` 上 `l` 右  
- `w`   向前一个单词
- `b`   向后一个单词
- `0`   跳到行开头
- `^`   跳到行开头不为空格的位置
- `$`   跳到行末
- `/some text`  搜索 `n` 下一个 `N` 上一个

移动光标

| 按键                  | 移动光标                           |
| ------------------- | ------------------------------ |
| l or 右箭头            | 向右移动一个字符                       |
| h or 左箭头            | 向左移动一个字符                       |
| j or 下箭头            | 向下移动一行                         |
| k or 上箭头            | 向上移动一行                         |
| 0 (零按键)             | 移动到当前行的行首。                     |
| ^                   | 移动到当前行的第一个非空字符。                |
| $                   | 移动到当前行的末尾。                     |
| w                   | 移动到下一个单词或标点符号的开头。              |
| W                   | 移动到下一个单词的开头，忽略标点符号。            |
| b                   | 移动到上一个单词或标点符号的开头。              |
| B                   | 移动到上一个单词的开头，忽略标点符号。            |
| Ctrl-f or Page Down | 向下翻一页                          |
| Ctrl-b or Page Up   | 向上翻一页                          |
| numberG             | 移动到第 number 行。例如，1G 移动到文件的第一行。 |
| G                   | 移动到文件末尾。                       |

```bash
# hjkl

# 2w 向前移动两个单词

# 3e 向前移动到第 3 个单词的末尾

# 0 移动到行首

# $ 当前行的末尾

# gg 文件第一行

# G 文件最后一行

# 行号+G 指定行

# <ctrl>+o 跳转回之前的位置

# <ctrl>+i 返回跳转之前的位置
```

删除

| 命令   | 删除的文本                |
| ---- | -------------------- |
| x    | 当前字符                 |
| 3x   | 当前字符及其后的两个字符。        |
| dd   | 当前行。                 |
| 5dd  | 当前行及随后的四行文本。         |
| dW   | 从光标位置开始到下一个单词的开头。    |
| d$   | 从光标位置开始到当前行的行尾。      |
| d0   | 从光标位置开始到当前行的行首。      |
| d^   | 从光标位置开始到文本行的第一个非空字符。 |
| dG   | 从当前行到文件的末尾。          |
| d20G | 从当前行到文件的第20行。        |

```bash
# x 删除当前字符

# dw 删除至当前单词末尾

# de 删除至当前单词末尾，包括当前字符

# d$ 删除至当前行尾

# dd 删除整行

# 2dd 删除两行
```

修改

```bash
# i 插入文本

# A 当前行末尾添加

# r 替换当前字符

# o 打开新的一行并进入插入模式
```

撤销

```bash
# u 撤销

# <ctrl>+r 取消撤销
```

复制粘贴剪切

| 命令   | 复制的内容                |
| ---- | -------------------- |
| yy   | 当前行。                 |
| 5yy  | 当前行及随后的四行文本。         |
| yW   | 从当前光标位置到下一个单词的开头。    |
| y$   | 从当前光标位置到当前行的末尾。      |
| y0   | 从当前光标位置到行首。          |
| y^   | 从当前光标位置到文本行的第一个非空字符。 |
| yG   | 从当前行到文件末尾。           |
| y20G | 从当前行到文件的第20行。        |

```bash
# v 进入可视模式

# y 复制

# p 粘贴

# yy 复制当前行

# dd 剪切当前行
```

状态

```bash
# <ctrl>+g 显示当前行以及文件信息
```

查找

```bash
# / 正向查找（n：继续查找，N：相反方向继续查找）

# ？ 逆向查找

# % 查找配对的 {，[，(

# :set ic 忽略大小写

# :set noic 取消忽略大小写

# :set hls 匹配项高亮显示

# :set is 显示部分匹配
```

替换

```bash
# :s/old/new 替换该行第一个匹配串

# :s/old/new/g 替换全行的匹配串

# :%s/old/new/g 替换整个文件的匹配串
```

执行外部命令

```bash
# :!shell 执行外部命令
```

## Security 101

`d   rwx  r-x  r-x`    第一个字母`d`表示文件夹, `-`表示不是文件夹

rwx  read write execute               第一个3个   文件夹所有者权限   第二个  特定用户组的用户的权限   第3个 其他用户的权限

## Troubleshooting

- `top, vmstat, iostat, systat, sar, mpstat`   查看本机问题
- `tcpdump, ngrep`      查看网络问题

## Networking 201

> 查看进程占用端口

```bash
netstat -lnp|grep 88
```

### ping

`-i` 改变源ip

`-s` 设置数据包大小

```bash
user@opsschool ~$ ping -M do -s 1473 upstream-host
PING local-host (local-host) 1473(1501) bytes of data.
From upstream-host icmp_seq=1 Frag needed and DF set (mtu = 1500)
From upstream-host icmp_seq=1 Frag needed and DF set (mtu = 1500)
From upstream-host icmp_seq=1 Frag needed and DF set (mtu = 1500)
```

### telnet  可测试特定端口

```bash
user@opsschool ~$ telnet yahoo.com 80
Trying 98.138.253.109...
Connected to yahoo.com.
Escape character is '^]'.

user@opsschool ~$ telnet yahoo.com 8000
Trying 98.138.253.109...
telnet: connect to address 98.138.253.109: Connection timed out
```

> ip,  ss,  traceroute, mtr, iftop, iperf, tcpdump 等命令/工具都是检测网络
>
> 都有很多选项和使用方法,具体自行google  
>
> traceroute 和mtr比较详细

## Common services

### dig + nsloopup(win)

`yum install bind-utils`

`dig xxxx.com`

> #### Tools: *Speaking http* with `telnet`  /`netcat(nc)`  /`curl`

`nc -l 192.168.0.1 1234 > output.log`

## Programming 101

`${var}`  使用变量

 `$(( ))` 进行运算

> 系统变量

```bash
# Home directory of the logged-on user. 用户主目录

$HOME

# Name of the computer you are currently logged on to  主机名

$HOSTNAME

# Present working directory    当前路径

$PWD

# Exit status of the previously executed script/command     上一个脚本执行状态

$?

# Number of arguments passed to the script      脚本传入的参赛数量

$#

# Value of the 1st argument passed to the script    第一个参数

${1}
```

## Logs 101

> 日志参数设置

`/etc/logrotate.conf`

eg:

```bash
# sample logrotate configuration file
# 通过gzip压缩备份文件
compress

/var/log/messages {
    # 最多保留5个备份,多了就覆盖

    rotate 5
    # 每周备份

    weekly
    # 执行备份之后的操作,一般重新打开

    postrotate
        # 不停止进程重新生成配置文件

        /sbin/killall -HUP syslogd
    endscript
}

# 两个日志文件同样的操作
"/var/log/httpd/access.log" /var/log/httpd/error.log {
    rotate 5
    # 超过5个备份发邮件

    mail foo@bar.org
    # 超过100k备份

    size=100k
    # 只执行一次脚本

    sharedscripts
    postrotate
         # 不停止进程重新生成配置文件

         /sbin/killall -HUP httpd
    endscript
}
```

## Application Components 201

> Message Queue Systems 消息队列

**一般满足以下条件:**

1. 进程或主机,低耦合

2. 多线程并发执行

3. 分发事件和信息,如数据更改,日志文件条目,监控,通知的统计信息

4. 数据流或文件流 单点或多点传输

5. 用面向事件的系统替换轮询机制

6. 网络层面的工作队列

7. 失败/升级方案的消息重新路由

8. 验证结果（最好的三个）

9. 异步远程调用

10. 网络互斥   *~~互斥锁用于保护临界区(同个进程不同线程均可访问的区域)*~~

11. 网络或分布式系统的消息总线

12. 消息持久化(很多时候需要,非必须)

**RabbitMQ is the most widely deployed open source message broker.**

## Load Balancing 负载均衡

> apache

`mod_proxy_balancer` 和  `mod_jk`

> nginx 自身支持
>
> HAProxy
>
> 硬件 BIG-IP

## Configuration Management 101

> 自动部署工具

| 名称        | 语言                  | 通信方式        | 配置文件格式              | 是否需要客户端 | 优点      | 缺点                |
| --------- | ------------------- | ----------- | ------------------- | ------- | ------- | ----------------- |
| SaltStack | python              | ZeroMQ 消息队列 | .sls (类似yml/docker) | 是       | 通信方式比较好 |                   |
| Ansible   | python              | ssh(可切换0mq) | yml                 | 否       | 自带UI不错  | 执行效率不高,文档不清楚      |
| Puppet    | ruby                | https       | .conf 类似nginx       | 是       | 多用于配置同步 | slave轮询master执行同步 |
| chef      | chef  (ruby和erlang) | docker      |                     | 否       | 文档好像还不错 |                   |

## Soft Skills 101
