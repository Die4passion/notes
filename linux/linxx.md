
# Linux 3日速成 {#home}

>  注：本文linux版本为centos 6.9


>目录:
>[day1](#day1) `目录、文件、用户`

>[day2](#day2) `vim、权限、命令、任务调度、文件查找、网络配置、远程登录`

>[day3](#day3) `搭建配置ftp、安装配置lamp`

### *day1*{#day1}

	Start now：

##### 目录结构

>+ &emsp;/bin &emsp;二进制可执行文件&emsp;`所有用户`
+ &emsp;/sbin &emsp;二进制可执行文件&emsp;`root用户`
+ &emsp;/boot &emsp;系统启动核心文件
+ /dev  设备目录
+ /home  用户主目录
+ /root   *root* 用户主目录
+ &emsp;/etc &emsp; 系统配置文件
- &emsp;/etc/passwd &emsp; 用户信息文件
- &emsp;/etc/group &emsp; 用户组信息文件
+ &emsp;/mnt &emsp; 挂载目录
+ &emsp;/lib &emsp; 系统共享动态链接库
+ &emsp;/var &emsp; 存放经常发生变化的文件
  &emsp;&emsp;`如缓存,登陆文件，mysql数据库的文件等`
+ &emsp;/proc 　/sys &emsp; 虚拟目录，内存映射，可以访问内存中的系统信息
+ &emsp;/usr &emsp;存放应用程序和文件

#####目录操作

>+ mkdir &emsp;创建目录
+ mkdir -p &emsp;递归创建目录
+ rmdir &emsp;删除空目录
+ rmdir -p &emsp;递归删除空目录 
+ rm -r &emsp;删除目录+内容(有确认选择)
+ rm -rf &emsp;删除目录和内容，无提示
+ rm file &emsp;删除文件
+ mv dir1 dir2 &emsp;dir1改名和移动为dir2
+ cp -r dir1 dir2  复制
  &emsp;&emsp;&emsp;*如果是目录这复制dir1到dir2下*
  &emsp;&emsp;&emsp;*如果是文件则复制后更名为dir2*
+ cd ~ 或 cd / 或 cd    切换到用户主目录
+ cd - 切换到上次目录

#####文件操作

>+ touch &emsp;创建一个空文件
+ cat &emsp;查看文件内容
+ more less &emsp;多页显示内容
+ head|tail -n   从前或后显示n条
+ tail -f &emsp;动态显示文件内容
+ wc &emsp;统计文件内容行、句、字符数
+ 添加内容到文件
  &emsp;&emsp;`> 覆盖写    >> 追加写`

#####用户和组操作

>+ useradd &emsp;添加用户
    + -g &emsp;组id
    + -d &emsp;用户主目录
    + -u &emsp;用户id
`eg: useradd -g 502 -d /shabi -u 502 dashabi`<br><br>
+ usermod  修改用户
    + -l &emsp;修改用户名<br><br>
+ userdel &emsp;删除用户(信息）
    + -r  同时删除用户目录<br><br>
+ groupadd  添加组
+ groupmod &emsp;修改组
    + -n &emsp;新的组名&emsp;旧组名 
+ groupdel 删除组(有用户的组无法删除)

>+ passwd &emsp;修改当前用户的密码
+ passwd &emsp;用户名&emsp;修改指定用户的密码(root特权)
+ /etc/passwd   用户信息
+ /etc/group    组信息


###*day2*{#day2}

---

#####vi和vim使用

>+  命令模式
+ &emsp;尾行模式
+ 插入模式

尾行==>>插入

>  - a   光标往后移动一格
>  - i   什么事都没发生
>  - o   当前行后插入一行空白行
>  - s   删除当前光标位置的字符


尾行模式：

>+ :set nu &emsp;显示行号
+ :set nonu | :set nu! &emsp;不显示行号
+ :w &emsp;保存文档
+ :q  退出
+ :q!  强制退出不保存
+ :wq!  强制保存后退出
+ :x  保存并退出，没修改就直接退出
+ :n  跳转到n行
+ :s/xx/yy/  替换光标所在行的第一个xx为yy
+ :s/xx/yy/g &emsp;替换一行的全部xx为yy
+ :%s/xx/yy/g &emsp;替换全部xx为yy
+ :%s表示所有行,8s表示第8行,8,10s表示第8-10行
+ :syntax on/off  打开 | 关闭 语法高亮

命令模式

>+ h、j、k、l &emsp;左、下、上、右
+ dd &emsp;删除当前行
+ ndd &emsp;删除当前行往下n行
+ x 或 delete &emsp; 删除单个字符
+ yy &emsp;复制当前行
+ nyy &emsp;复制当前n行
+ p &emsp; 粘贴
    + u &emsp;撤销操作
    + . &emsp;重复执行
    + J &emsp;连接上下行为一行
    + r &emsp; 单个字符替换
+ ZZ &emsp;退出编辑，=:x

#####文件权限

>+ chmod -R 741 dir &emsp; 递归修改dir目录下的所有内容权限
+ chmod u+rx  &emsp;给所属用户加上rx权限
+ chmod g-w &emsp; 给用户组去掉w权限
+ chmod o=,g+w &emsp;其他用户权限为空，给当前组加w权限
+ chmod a=rw &emsp;讲所有用户权限设为rw
+ chmod 000 777 640 ...

#####修改文件所属用户和组

>+ chown username filename 修改文件所属用户
+ chgrp groupname filename 修改文件所属组
+ chown username.groupname filename 同时修改
+ chown .groupname filename 只修改组
+ chown -R username.groupname dir 递归修改

#####系统常用命令

>+ grep 关键字 路径  将文件中含有该关键字的行的内容显示出来
+ top &emsp; 任务管理器
+ ps -A &emsp;查看系统所有进程
+ free -m &emsp;查看内容使用情况
+ du 路径 &emsp;查看目录或文件占用磁盘空间
+ date &emsp;查看系统当前日期时间
+ date -s &emsp;设置系统时间
+ df [-lh] &emsp;查看系统分区
+ kill -9 pid &emsp;杀死指定pid号进程
+ ifconfig &emsp;查看网络配置

#####文件查找

>+ whereis &emsp;主要用于搜索系统命令和配置文件
>  `eg:     whereis passwd`
+ locate &emsp;速度快但不能实时搜索，如需实时搜索则先`updatedb`
+ find &emsp;实时搜索，速度慢，参数多，精确搜索用这个

#####任务调度指令

>+ crontab -e(编辑) -l(查看) -r(删除)
>   文件位置：/var/spool/cron/
>    调试信息:/var/spool/mail

#####网络配置

> 1. 配置虚拟机网卡
>    `虚拟机配置文件修改网络适配器为桥接模式`
  2. 修改linux网络配置

```
//查看网络配置
ifconfig
//修改网络配置文件
vim /etc/sysconfig/network-scripts/ifcfg-eth0
//修改为
ONBOOT=yes
BOOTPROTO=static
IPADDR=172.16.17.138
NETMASK=255.255.255.0
GATEWAY=127.16.17.1
//重启网卡
service network restart
```

1. 使用Xshell(或putty)远程登录linux


### *day3*{#day3}

    安装开发环境
---

##### 搭建ftp服务器

> 1. 关闭selinux  重启centos
>    `vim  /etc/selinux/config`
  1. 挂载光驱 
     `mkdir /mnt/sb
     mount /dev/cdrom  /mnt/sb`
     1. 从系统光盘安装vsftpd 
        `rpm  -ivh  /mnt/sb/Packages/vsftpf-2.2.2-24.e16.i686.rpm`
     2. 配置linux防火墙
        `setup  -->防火墙配置-->定制-->ftp-->退出`
     3. 启动ftp服务
        `service vsftpf start`
     4. 使用ftp客户端软件(winscp、filezilla、flashfxp)

##### ftp服务器配置

>+ 使用root用户登陆ftp

```
    vim /etc/vsftpd/ftpusers
    vim /etc/vsftpd/user_list
    注释掉里面的root用户(实际中不建议这样做)
    //重启vsftpd服务
    service vsftpd restart
```

+ 设置 白名单 或 黑名单

```
    vim /etc/vsftpd/vsftpd.conf
    //修改内容
        userlist_enable=YES
        userlist_file=/etc/vsftpd/user_list
        userlist_deny=NO
    //userlist_enable=YES表示启用userlist文件来管理ftp用户登录
    //userlist_file=xxx指定userlist文件的路径(默认使用当前目录)
    //userlist_deny=NO：user_list是一个白名单，该文件中的用户允许登录
    //userlist_deny=YES(默认)：user_list是一个黑名单，该文件中的用户被拒绝登录
```

+ 只允许访问用户主目录

```
    vim /etc/vsftpd/vsftpd.conf
    //修改内容(去掉注释)
        chroot_list_enable=YES
        chroot_list_file=/etc/vsftpd/chroot_list
    //将要限制的用户添加进chroot_list
```

````
    //限定用户test不能telnet，只能ftp
    usermod -s /sbin/nologin neo
````

#####linux安装软件

---

三种安装方式：

>1. rpm: 以软件包的形式安装

```
    rpm -q   软件名称        //查询软件是否已经安装
    rpm -ivh 软件包路径      //安装软件
    rpm -e    软件名称       //卸载软件
    rpm -qa | grep 软件名称  //利用管道查询软件安装情况
```

2. 源码编译安装

````
    ./configure     //配置软件的安装，可以设置路径和参数
    make            //编译  源码编译成二进制代码
    make install    //把二进制代码复制到相应的目录
````

3. yum安装(类似apt-get)

````
    yum install vsftpd   //要求联网
````

压缩和解压文件

>+ 压缩  `tar  cf` 打包文件名 要打包的文件1 文件2
+ 解压   `tar xf/xvf`    解压文件

##### 安装编译软件

>使用光盘作用yum源

````
    cd /etc/yum.repos.d/
    //干掉光盘安装
    mv CentOS-Base.repo CentOS-Base.repo.bak
    //编辑媒体源配置文件
    vim CentOS-Media.repo
    //在里面添加光盘挂载目录
    baseurl=file:///mnt/sb
    gpgcheck=0
    enabled=1
````

重新挂载光驱
`mkdir /mnt/sb
mount /dev/cdrom  /mnt/sb`

>使用yum安装gcc软件

```
    cd /mnt/sb
    ls
    yum -y install gcc
```

使用yum安装gcc++软件

```
    yum -y install gcc-c++
```

#####将服务设置为开机自启

````
    //查看开机服务启动情况：
    chkconfig --list   
    //添加一个开机启动服务：
    chkconfig --add  servicename 
    //删除一个服务
    chkconfig --del  servicename 
    //设置服务开机启动
    chkconfig --level 2345 servicename on
````
####LAMP编译环境搭建

```
    yum -y install gcc 
    yum install gcc-c++
```

#####Apache安装

````
安装 zlib
[root@localhost lamp]# tar -xf zlib-1.2.5.tar.gz
[root@localhost lamp]# cd zlib-1.2.5
[root@localhost zlib-1.2.5]# ./configure
[root@localhost zlib-1.2.5]# make && make install

安装 apr
[root@localhost zlib-1.2.5]# cd ..
[root@localhost lamp]# tar -xf apr-1.5.2.tar.gz
[root@localhost lamp]# cd apr-1.5.2
[root@localhost apr-1.5.2]# ./configure --prefix=/usr/local/apr
[root@localhost apr-1.5.2]# make
[root@localhost apr-1.5.2]# make install安装 apr-iconv
[root@localhost apr-1.5.2]# cd ..
[root@localhost lamp]# tar -xf apr-iconv-1.2.1.tar.gz
[root@localhost lamp]# cd apr-iconv-1.2.1
[root@localhost apr-iconv-1.2.1]# ./configure --prefix=/usr/local/apr-iconv --withapr=/usr/local/apr/
[root@localhost apr-iconv-1.2.1]# make && make install

安装 apr-util
[root@localhost apr-iconv-1.2.1]# cd ..
[root@localhost lamp]# tar -xf apr-util-1.5.4.tar.gz
[root@localhost lamp]# cd apr-util-1.5.4
[root@localhost apr-util-1.5.4]# ./configure --prefix=/usr/local/apr-util/ --withapr=/usr/local/apr/ --with-apr-iconv=/usr/local/apr-iconv/bin/apr-iconv
[root@localhost apr-util-1.5.4]# make && make install

安装 pcre
[root@localhost apr-util-1.5.4]# cd ..
[root@localhost lamp]# tar -xf pcre-8.35.tar.gz
[root@localhost lamp]# cd pcre-8.35
[root@localhost pcre-8.35]# ./configure --prefix=/usr/local/pcre && make && make
install

安装 httpd
[root@localhost pcre-8.35]# cd ..
[root@localhost lamp]# tar xf httpd-2.4.17.tar.gz
[root@localhost lamp]# cd httpd-2.4.17
[root@localhost httpd-2.4.17]# ./configure --prefix=/usr/local/lamp/apache2 \> --enable-modules=all \
   --enable-so \
   --with-apr=/usr/local/apr/ \
   --with-apr-util=/usr/local/apr-util/ \
   --with-pcre=/usr/local/pcre/
[root@localhost httpd-2.4.17]# make && make install
兄弟们，可以出去抽根烟，要等一会儿了。。。。。。
````

测试Apache
`ps -A \ grep httpd`
开启防火墙
`setup` 定制 www
重启防火墙`service iptables restart` 

#####PHP安装

````
安装 xml
[root@localhost httpd-2.4.17]# cd ..
[root@localhost lamp]# tar xf libxml2-2.7.2.tar.gz
[root@localhost lamp]# cd libxml2-2.7.2
[root@localhost libxml2-2.7.2]# ./configure[root@localhost libxml2-2.7.2]# make && make install

安装 jpeg
[root@localhost libxml2-2.7.2]# cd ..
[root@localhost jpeg-8b]# tar xf jpegsrc.v8b.tar.gz
[root@localhost jpeg-8b]# cd jpeg-8b/
[root@localhost jpeg-8b]# ./configure --prefix=/usr/local/jpeg --enable-shared --
enable-static
[root@localhost jpeg-8b]# make && make install

安装 png
[root@localhost jpeg-8b]# cd ..
[root@localhost lamp]# tar xf libpng-1.4.3.tar.gz
[root@localhost lamp]# cd libpng-1.4.3
[root@localhost libpng-1.4.3]# ./configure --prefix=/usr/local/png --enable-shared -
-enable-static
[root@localhost libpng-1.4.3]# make && make install

安装 freetype 字体库
[root@localhost libpng-1.4.3]# cd ..
[root@localhost lamp]# tar xf freetype-2.4.1.tar.gz
[root@localhost lamp]# cd freetype-2.4.1
[root@localhost freetype-2.4.1]# ./configure --prefix=/usr/local/freetype --enableshared
[root@localhost freetype-2.4.1]# make && make install

安装 gd
[root@localhost freetype-2.4.1]# cd ..
[root@localhost lamp]# tar xf libgd-2.1.1.tar.gz
[root@localhost lamp]# cd libgd-2.1.1[root@localhost libgd-2.1.1]# ./configure --prefix=/usr/local/gd --withjpeg=/usr/local/jpeg/ --with-png=/usr/local/png --with-zlib --withfreetype=/usr/local/freetype
[root@localhost libgd-2.1.1]# make && make install

安装 bison
[root@localhost gd-2.0.35]# rpm -ivh /mnt/sb/Packages/bison-2.4.1-
5.el6.i686.rpm

安装 libmcrypt
[root@localhost gd-2.0.35]# cd ..
[root@localhost lamp]# tar xf libmcrypt-2.5.7.tar.gz
[root@localhost lamp]# cd libmcrypt-2.5.7
[root@localhost libmcrypt-2.5.7]# ./configure --disable-posix-threads
[root@localhost libmcrypt-2.5.7]# make && make install

[root@localhost mcrypt-2.6.8]# export LD_LIBRARY_PATH=/usr/local/lib

[root@localhost libmcrypt-2.5.7]# cd ..
[root@localhost lamp]# tar xf mcrypt-2.6.8.tar.gz
[root@localhost lamp]# cd mcrypt-2.6.8
[root@localhost mcrypt-2.6.8]# ./configure --with-libmcrypt-prefix=/usr/local

[root@localhost mcrypt-2.6.8]# cd ../
[root@localhost lamp]# tar xf mhash-0.9.9.9.tar.gz
[root@localhost lamp]# cd mhash-0.9.9.9
[root@localhost mhash-0.9.9.9]# ./configure && make && make install
[root@localhost mhash-0.9.9.9]# cd ../mcrypt-2.6.8
[root@localhost mcrypt-2.6.8]# ./configure --with-libmcrypt-prefix=/usr/local
[root@localhost mcrypt-2.6.8]# make && make install

安装 autoconf
[root@localhost mcrypt-2.6.8]# cd ..
[root@localhost lamp]# tar xf autoconf-2.69.tar.gz
[root@localhost lamp]# cd autoconf-2.69
[root@localhost autoconf-2.69]# ./configure && make && make install

安装 libiconv
[root@localhost autoconf-2.69]# cd ..
[root@localhost lamp]# tar xf libiconv-1.14.tar.gz
c[root@localhost lamp]# cd libiconv-1.14
[root@localhost libiconv-1.14]# ./configure --prefix=/usr/local/libiconv && make && make install

安装 libXpm-devel
[root@localhost lamp]# yum install libXpm-devel
[root@localhost lamp]# rpm -ql libXpm-devel安装 php
[root@localhost libiconv-1.14]# cd ..
[root@localhost lamp]# tar xvf php-5.6.24.tar.gz
[root@localhost lamp]# cd php-5.6.24
[root@localhost php-5.6.24]# ./configure --prefix=/usr/local/lamp/php \
> --with-apxs2=/usr/local/lamp/apache2/bin/apxs \
> --with-mysql=mysqlnd \
> --with-pdo-mysql=mysqlnd \
> --with-mysqli=mysqlnd \
> --with-freetype-dir=/usr/local/freetype \
> --with-gd=/usr/local/gd/ \
> --with-zlib \
> --with-jpeg-dir=/usr/local/jpeg \
> --with-png-dir=/usr/local/png \
> --enable-mbstring=all \
> --enable-mbregex \
> --enable-shared \
> --with-mcrypt=/usr/local/libmcrypt/ \
> --with-iconv=/usr/local/libiconv \
> --with-libxml-dir=/usr/local \
> --with-xpm-dir=/usr/lib
[root@localhost php-5.6.24]# make
[root@localhost php-5.6.24]# make install
不开心、不开心、等了这么久。。。。。。
````
拷贝 PHP 配置文件
```
[root@localhost php-5.6.24]# cp php.ini-development
/usr/local/lamp/php/lib/php.ini
```
让 Apache 支持 php
```
[root@localhost php-5.6.24]# vim /usr/local/lamp/apache2/conf/httpd.conf
AddHandler application/x-httpd-php .php
```
重启
```
[root@localhost php-5.6.24]# killall httpd
[root@localhost php-5.6.24]# /usr/local/lamp/apache2/bin/apachectl
```

#####安装MYSQL
````
安装依赖

安装 ncurses-devel
[root@localhost php-5.6.24]# rpm -ivh /mnt/cdrom/Packages/ncurses-devel-5.7-
4.20090207.el6.i686.rpm安装 

cmake
[root@localhost mysql-5.6.25]# rpm -ivh /mnt/cdrom/Packages/cmake-2.8.12.2-
4.el6.i686.rpm

安装 MySQL
[root@localhost php-5.6.24]# cd ..
[root@localhost lamp]# tar xvf mysql-5.6.25.tar.gz
[root@localhost lamp]# cd mysql-5.6.25
[root@localhost mysql-5.6.25]# cmake - \
DCMAKE_INSTALL_PREFIX=/usr/local/lamp/mysql -
DMYSQL_UNIX_ADDR=/tmp/mysql.sock
-DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -
DWITH_EXTRA_CHARSETS:STRING=utf8,gbk \
-DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1
-DWITH_READLINE=1 -DENABLED_LOCAL_INFILE=1 \
-DMYSQL_DATADIR=/usr/local/lamp/mysql/data -DMYSQL_USER=mysql
[root@localhost mysql-5.6.25]# make && make install
客官、不要着急哦。。。。。 。 这么久。。。。。。。 这应该是最漫长的等待
了。。。。。。滚犊子。。。。。。
````

添加用户

`[root@localhost mysql-5.6.25]# useradd -s /sbin/nologin -r user`

修改目录权限，将 data 文件夹所有者改为 mysql

`[root@localhost mysql-5.6.25]# chown user.group /usr/local/lamp/mysql/data -R`

复制配置文件

`[root@localhost mysql-5.6.25]# cp support-files/my-default.cnf /etc/my.cnf`

创建mysql测试数据库和系统数据库

````
    [root@localhost mysql-5.6.25]# cd /usr/local/lamp/mysql
    [root@localhost mysql]# /usr/local/lamp/mysql/scripts/mysql_install_db --user=mysql --datadir=/usr/local/lamp/mysql/data
    测试
    [root@localhost mysql]# /usr/local/lamp/mysql/bin/mysqld_safe
    [root@localhost mysql]# /usr/local/lamp/mysql/bin/mysql -uroot
    mysql> show databases;
````

修改密码

`mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('password');`

创建允许远程登录的用户

````
    mysql> CREATE USER 'dashabi'@'%' IDENTIFIED BY '123456';
    mysql> select password('123456');
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY PASSWORD
'xxxxxx';    //xxxxx为加密后的密码
````
防火墙中开启3306端口

````
    setup
    防火墙配置->定制->转发->添加
    端口：3306
    协议：tcp
    service iptables restart
````
查看防火墙配置
`vim /etc/sysconfig/iptables`

设置lamp自启
````
    让mysql开机自启动
        1、chkconfig --add mysqld
        2、chkconfig mysqld on
    让vsftpd开机自启动
        chkconfig --level 35 vsftpd on
    让apache开机自启动
        cp /usr/local/lamp/apache2/bin/apachectl /etc/init.d/httpd
        vim /etc/init.d/httpd
    在 #!/bin/sh  下添加  
        #chkconfig:35 85 15
        #description:Start and stop the Apache HTTP Server
        chkconfig –add httpd
        chkconfig httpd on

````

道路虽被淹没，但 向下就是天空...

```php
    /**
     * 如有不解之处 或 错误的地方
     * 请q 465335731联系我
     * 
     *      write by  PHP0217   杨航
     */
    defined('PHP_IS_EASY') or define('PHP_IS_EASY', 'php是世界上最好的语言');
    for ($i=1; $i < 100000; $i++) { 
        $code = mt_rand(0, 9999);
        if ($code === 1){
            echo $i.'.'.PHP_IS_EASY.'<br>';
        } else {
            echo 'ni shi zui bang de<br>';
        }      
    }
            
```

[back to top](#home)


    