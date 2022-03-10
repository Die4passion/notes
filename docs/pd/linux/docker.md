### 1. 安装

> 使用脚本自动安装

```bash
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
```

### 2. 使用镜像

> 获取镜像

```bash
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

> 运行镜像

```bash
$ docker run -it --rm \
    ubuntu:18.04 \
    bash

# -it：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。
# --rm：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。
# bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 bash。
```

> 列出镜像

```bash

$ docker image ls

$ docker system df

# 虚悬镜像
$ docker image ls -f dangling=true
# 删除
$ docker image prune

# 过滤显示
$ docker image ls -f since=mongo:3.2
$ docker image ls -f label=com.example.version=0.1 

# 只显示id
$ docker images -q
# 只包含镜像ID和仓库名：
$ docker image ls --format "{{.ID}}: {{.Repository}}"
# 以表格等距显示，并且有标题行，和默认一样，不过自己定义列：
$ docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

# 显示镜像摘要
$ docker image ls --digests
```

> 删除镜像

```bash
# <镜像> 可以是 镜像短 ID、镜像长 ID、镜像名 或者 镜像摘要。
$ docker image rm [选项] <镜像1> [<镜像2> ...]

# 以镜像摘要删除
$ docker image rm node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228
```

> 理解镜像构成

```bash
# 运行一个web服务
$ docker run --name webserver -d -p 80:80 nginx

# 进入容器 
$ docker exec -it webserver bash
root@3729b97e8226:/# echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
root@3729b97e8226:/# exit
exit

# 查看容器变化
$ docker diff webserver

# docker commit
$ docker commit [选项] <容器ID或容器名> [<仓库名>[:<标签>]]

# 例如
$ docker commit \
    --author "Tao Wang <twang2218@gmail.com>" \
    --message "修改了默认网页" \
    webserver \
    nginx:v2

# ps 少用commit命令,因为这样docker会变很臃肿,不好管理
```

> Dockerfile 定制镜像

```docker
# 创建dockerfile
$ mkdir mynginx
$ cd mynginx
$ touch Dockerfile

# 其内容为：

FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html

# 在dockerfile路径下
$ docker build -t nginx:v3 .

# docker build [选项] <上下文路径/URL/->

# 直接用 Git repo 进行构建
$ docker build https://github.com/twang2218/gitlab-ce-zh.git#:11.1
# 这行命令指定了构建所需的 Git repo，并且指定默认的 master 分支，构建目录为 /11.1/，然后 Docker 就会自己去 git clone 这个项目、切换到指定分支、并进入到指定目录后开始构建。

----------------------------
# Dockerfile文件相关

# 运行nginx服务
CMD ["nginx", "-g", "daemon off;"] # 后面不能接参数

# 后面可以接参数
ENTRYPOINT [ "curl", "-s", "https://ip.cn" ]

# ENV 定义环境变量
ENV VERSION=1.0 DEBUG=on \
    NAME="Happy Feet"

# docker build 中用 --build-arg <参数名>=<值> 覆盖 ARG

# VOLUME 定义匿名卷
```

> else

```bash
# Ctrl-p 或Ctrl-q 推出一个实例而不终止 
```
