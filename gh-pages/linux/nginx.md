# Ningx #
### **Ning的介绍** ###

> Nginx是俄罗斯人编写的十分轻量级的HTTP服务器,Nginx，它的发音为“engine X”，是一个高性能的HTTP和反向代理服务器，同时也是一个IMAP/POP3/SMTP 代理服务器。Nginx是由俄罗斯人 Igor Sysoev为俄罗斯访问量第二的 Rambler.ru站点开发的，它已经在该站点运行超过两年半了。Igor Sysoev在建立的项目时，使用基于BSD许可。自Nginx 发布来，Nginx 已经因为它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

### **Nginx的有点** ###
1. **高并发连接：**官方测试能够支撑5万并发连接，在实际生产环境中跑到2～3万并发连接数。
2. **内存消耗少：**在3万并发连接下，开启的10个Nginx 进程才消耗150M内存（15M*10=150M）。
3. **配置文件非常简单：**风格跟程序一样通俗易懂。
4. **成本低廉：**Nginx为开源软件，可以免费使用。而购买F5 BIG-IP、NetScaler等硬件负载均衡交换机则需要十多万至几十万人民币。
5. **支持Rewrite重写规则：**能够根据域名、URL的不同，将 HTTP 请求分到不同的后端服务器群组。
6. **内置的健康检查功能：**如果 Nginx Proxy 后端的某台 Web 服务器宕机了，不会影响前端访问。
7. **节省带宽：**支持 GZIP 压缩，可以添加浏览器本地缓存的 Header 头。
8. **稳定性高：**用于反向代理，宕机的概率微乎其微。
### **Nginx和Apache的区别**  ###

> Nginx和Apache一样，都是一个HTTP服务器软件，功能实现上都采用模块化结构设计，都支持通用的语言接口，如PHP、Perl、Python等，同时还支持正、反向代理，虚拟主机，URL重写，压缩传输，SSL加密传输等。它们之间最大的差别是Apache处理速度很慢，且占用很多内存资源，而Nginx却恰恰相反；在功能实现上，Apache的所有模块都支持动、静态编译，而Nginx模块都是静态编译的，同时，Apache对Fcgi支持不好，而Nginx对Fcgi的支持非常的好；最重要的是，在处理连接方式上，Nginx支持epoll，而Apache却不支持；在大小上，Nginx安装包仅仅有几百K，和Nginx比起来Apache绝对是庞然大物。在了解了Nginx和Apache之间的异同点后基本知道了Nginx作为HTTP服务器的优势所在。

### **安装Nginx服务器** ###
1. 下载nginx的windows版本
 [http://nginx.org/download/nginx-1.8.0.zip]()
1. **将压缩文件解压到非中文的目录下**
2. 3.Nginx的启动和停止（前提：确保apache关闭）<br>
**A.启动<br>**
进入解压的目录下执行startnginx.exe 命令<br>
启动后在浏览器中访问到以下界面说明安装启动成功<br>
**B.停止<br>**
nginx -s stop 快速关闭<br>
nginx -s quit 安全关闭<br>
nginx -s reload 重载配置文件<br>
nginx -s reopen 重新打开日志文件<br>
**C.配置重启和关闭的控制器台<br>**
1.拷贝nginx.bat文件到nginx文件夹下<br>
2.修改nginx.bat中配置nginx的安装路径<br>
> @ECHO OFF           配置文件的响应头<br>
> SET NGINX PATH=D:   指存在的根目录<br>
> SET NGINX_DIR=D:....  nginx.bat所在的绝对目录的目录<br>

注意: 该目录后必须以/结尾:<br>
4.使用nginx.bat控制nginx<br>

打开nginx.bat使用上面的参数可以很简便的操作nginx服务

### 在Linux上安装 ###
**1. 安装pcre<br>tar -zxvf pcre-8.32.tar.gz<br>**
cd pcre-8.32<br>
./configure--prefix=/usr/local/pcre<br>
make&&make install<br>
**2. 安装nginx<br>**
安装之后通过以下命令启动nginx<br>
./usr/local/nginx/sbin/nginx<br>
通过浏览器访问虚拟机的ip地址得到nginx页面说明安装成功!<br>

**3. 控制nginx<br>**
cd /usr/local/nginx/sbin<br>
./nginx -t<br>
启动操作<br>
./nginx -c /usr/local/nginx/nginx.conf<br>
-c参数指定了要加载的nginx配置文件路径. 默认为安装配置时指定的配置文件路径.<br>
停止操作<br>
停止操作是通过向nginx进程发送信号来进行的<br>
步骤1：查询nginx主进程号<br>
ps -ef | grep nginx<br>
在进程列表里面找master进程，它的编号就是主进程号了。<br>

步骤2：发送信号<br>
从容停止Nginx：<br>
kill -QUIT 主进程号<br>
快速停止Nginx：<br>
kill -TERM 主进程号<br>
强制停止Nginx：<br>
pkill -9 进程的名字<br>

另外，若在nginx.conf配置了pid文件存放路径则该文件存放的就是Nginx主进程号，如果没指定则放在nginx的logs目录下。有了pid文件，我们就不用先查询Nginx的主进程号，而直接向Nginx发送信号了，命令如下：<br>
kill -信号类型(QUIT| TERM)`cat /usr/local/nginx/logs/nginx.pid`<br>

**4. nginx安装服务**<br>
如果想通过以下命令控制nginx:<br>
service nginx start<br>
service nginx stop<br>
需要在linux中安装服务脚本:<br>
1.编辑并且正确配置该脚本中的变量.如下:<br>

----------

	#!/bin/bash
	# nginx     This shell script takes care of starting and stopping
	#           nginx
	#
	# chkconfig: - 13 68
	# description: nginx is a web server
	### BEGIN INIT INFO
	# Provides: $named
	# Short-Description: start|stop|status|restart|configtest 
	### END INIT INFO
	#variables
	NGINX_BIN="/usr/local/nginx/nginx"
	NGINX_CONF="/usr/local/nginx/nginx.conf"
	NGINX_PID="/usr/local/nginx/nginx.pid"
	NETSTAT="/bin/netstat"
	alter=$1
	prog=nginx
	#load system function
	. /etc/rc.d/init.d/functions
	#function:echo ok or error
	function if_no {
	if [ $2 == 0 ]; then
	echo -n $"$1 ${prog}:" && success && echo
	else
	echo -n $"$1 ${prog}:" && failure && echo
	fi
	}
	#start nginx
	function start {
	if [ -s ${NGINX_PID} ]; then
	echo "nginx already running" 
	else
	if [ `${NETSTAT} -tnpl | grep nginx | wc -l` -eq 0 ]; then
	rm -f ${NGINX_PID} 2>/dev/null
	${NGINX_BIN} -c ${NGINX_CONF} 
	if_no start $?
	        else
	${NETSTAT} -tnpl | grep nginx | awk '{ print $7}' | cut -d '/' -f 1 > ${NGINX_PID}
	if_no start $?
	fi
	fi
	}
	#stp nginx
	function stop {
	if [ -s ${NGINX_PID} ]; then
	cat ${NGINX_PID} | xargs kill -QUIT
	if_no stop $?
	else
	        if [ `${NETSTAT} -tnpl | grep nginx | wc -l` -eq 0 ]; then
	rm -f ${NGINX_PID} 2>/dev/null
	if_no stop 0
	else
	rm -f ${NGINX_PID} 2>/dev/null
	kill `${NETSTAT} -tnpl | grep nginx | awk '{ print $7}' | cut -d '/' -f 1`
	if_no stop $?
	fi
	fi
	}
	function restart {
	if [ -s ${NGINX_PID} ]; then
	cat ${NGINX_PID} | xargs kill -HUP
	if_no restart $?
	else
	stop
	sleep 1
	start
	fi
	}
	function status {
	${NETSTAT} -tnpl | grep nginx | grep LISTEN
	[ $? == 0 ] && echo "nginx is running" || echo "nginx is not running"
	}
	function configtest {
	${NGINX_BIN} -t
	}
	case $alter in
	start)
	start
	;;
	stop)
	stop
	;;
	restart)
	restart
	;;
	status)
	status
	;;
	configtest)
	configtest
	;;
	*)
	echo "use:${NGINX} {start|stop|restart|status|configtest}"
	;;
	esac

需要修改对应的路径<br>
NGINX_BIN="/usr/local/nginx/sbin/nginx";
NGINX_BIN="/usr/local/nginx/conf/nginx.conf";
NGINX_BIN="/usr/local/nginx/logs/nginx.pid";<br>
2.做以下的操作:<br>
拷脚本到/etc/init.d/<br>
cp nginx /etc/init.d/<br>
修改服务脚本的执行权限<br>
chmod 755 /etc/init.d/nginx<br>
nginx加入服务<br>
chkconfig --add nginx<br>
nginx 设置为开机启动<br>
/sbin/chkconfig nginx on<br>

3.服务器脚本支持以下功能<br>
service nginx 可以使用以下5个参数 例如service nginx start 开启nginx服务<br>
use:start：开启 stop 关闭 restart重启 status状态 configtest检测<br>

### Nginx的配置文件详解 ###
> Linux 下基本上每个服务都会有它的主配置文件，该文件会定义服务应该如果去运行，
> 使用些什么参数，启用些什么功能，相关会涉及到的一些操作文件在哪，所以主配置文件对服务是至关重要的。下面我们来分析一下 Nginx 的主配置文件。<br>
> Nginx的主配置文件默认情况下位于/usr/local/nginx/nginx.conf 以下为Nginx 

可以使用nginx.conf 文件

    #指定使用的用户
	#user  nobody;
	
	#启动进程,通常设置成和cpu的数量相等
	worker_processes  1;
	
	#全局错误日志
	#error_log  logs/error.log;
	#error_log  logs/error.log  notice;
	#error_log  logs/error.log  info;
	
	#PID文件--存放进程号的文件
	#pid        logs/nginx.pid;
	
	#工作模式及连接数上限
	events {
    #单个后台worker process进程的最大并发链接数
    worker_connections  1024;
    #并发总数是 worker_processes 和 worker_connections 的乘积

    #Nginx支持如下处理连接的方法（I/O复用方法），这些方法可以通过 use指令指定。  
    #use [ kqueue | rtsig | epoll | /dev/poll | select | poll ];  

    #use epoll;  #使用 epoll（linux2.6的    性能方式 ）  
    #select - 标准方法。 如果当前平台没有更有效的方法，它是编译时默认的方法。你可以使用配置参数 –with-select_module 和 –without-select_module 来启用或禁用这个模块。  
    #poll - 标准方法。 如果当前平台没有更有效的方法，它是编译时默认的方法。你可以使用配置参数–with-poll_module 和 –without-poll_module 来启用或禁用这个模块。  
    #kqueue -   效的方法，使用于 FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0 和 MacOS X. 使用双处理器的 MacOS X系统使用 kqueue可能会造成内核崩溃。  
    #epoll -  效的方法，使用于Linux内核2.6版本及以后的系统。在某些发行版本中，如 SuSE 8.2,有让2.4版本的内核支持epoll的补丁。  
    #rtsig - 可执行的实时信号，使用于Linux内核版本 2.2.19以后的系统。可是从Linux内核版本2.6.6-mm2开始，这个参数就不再使用了.  
    #/dev/poll -  效的方法，使用于 Solaris 7 11/99+, HP/UX 11.22+ (eventport), IRIX 6.5.15+ 和 Tru64 UNIX 5.1A+.  
    #eventport -  效的方法，使用于 Solaris 10. 为了防止出现内核崩溃的问题， 有必要安装 这个 安全补丁。  
	}
	
	
	http {
    #设定mime类型,类型由mime.type文件定义
    include       mime.types;
    default_type  application/octet-stream;
    
     #设定日志格式	
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，
    #对于普通应用，必须设为 on,
    #如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，
    #以平衡磁盘与网络I/O处理速度，降低系统的uptime.	
    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;
    
    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;

    #开启gzip压缩	
    #gzip  on;
    
    #设定虚拟主机配置	
    server {
        #侦听80端口
        listen       80;

	#定义使用 www.nginx.cn访问
        server_name  localhost;

	#定义服务器的默认网站根目录位置
        root html;

	#设置编码
        #charset koi8-r;
	
	#设定本虚拟主机的访问日志
        #access_log  logs/host.access.log  main;

	#默认请求
        location / {
	    root html;	
	    #定义首页索引文件的名称
            index  index.html index.htm;
        }

	# 定义错误提示页面
        #error_page  404              /404.html;
        #redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #PHP脚本转给apache处理
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        # 禁止访问 .htxxx 文件
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
}

1.nginx的配置文件结构<br>
nginx.conf由多个块组成，最外面的块是main，main包含events和http,http包含多个upstream和多个server，server又包含多个location：<br>
main（全局设置）、server（虚拟主机设置）、upstream（负载均衡服务器设置）和 location（URL匹配特定位置的设置）。<br>
- main块设置的指令将影响其他所有设置；<br>
- server块的指令主要用于指定主机和端口,以及网站路径；<br>
- upstream指令主要用于负载均衡，设置一系列的后端服务
- 器；<br>
- location块用于匹配网页位置。<br>

**这四者之间的关系式：server继承main，location继承server，upstream既不会继承其他设置也不会被继承。**<br>
在这四个部分当中，每个部分都包含若干指令，这些指令主要包含Nginx的主模块指令、事件模块指令、HTTP核心模块指令，同时每个部分还可以使用其他HTTP模块指令，例如Http SSL模块、HttpGzip Static模块和Http Addition模块等。<br>