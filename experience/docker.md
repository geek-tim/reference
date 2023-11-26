# 全栈之路 docker 从0到1

什么是docker(WHAT：抽象概念)
---

### 概念
<!--rehype:wrap-class=col-span-3-->
Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。



Docker 包括三个基本概念:
- `镜像（Image）`：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
- `容器（Container）`：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
- `仓库（Repository）`：仓库可看成一个代码控制中心，用来保存镜像。

Docker的主要目标是：”Build ,Ship And Run Any App,AnyWhere“,通过对应组件的封装、分发、部署、运行等生命周期的管理，使用户的APP(可以是一个WEB APP 或数据库应用)以及运行环境能够做到“一次镜像，处处运行“  

怎么使用(HOW:怎么玩)
---
<!--rehype:body-class=cols-6-->

### 命令分组一
<!--rehype:wrap-class=col-span-2-->

```sh
docker --help      #帮助
docker container   #管理容器
docker image       #管理镜像
docker network     #管理网络
docker create      #创建一个新容器 
docker exec        #在容器中执行一条命令
docker kill        #杀死一个或多个正在运行的容器    
docker attach      #介入到一个正在运行的容器
docker push        #推送一个镜像或仓库到 registry
docker pull        #拉取一个镜像或仓库到 registry
docker ps          #列出所有容器
docker rename      #重命名一个容器
docker rm          #删除一个或多个容器
docker start       #启动一个或多个已经停止运行的容器
```
### 命令分组二
<!--rehype:wrap-class=col-span-2-->

```sh
docker restart     #重新启动一个或多个容器
docker stop        #停止一个或多个正在运行的容器
docker top         #显示一个容器内的所有进程
docker unpause     #恢复一个或多个容器内所有被暂停的进程
docker cp          #在本地文件系统与容器中复制 文件/文件夹
docker run         #在一个新的容器中执行一条命令
docker stats       #显示一个容器的实时资源占用
#
docker images      #列出镜像
docker search      #在 Docker Hub 中搜索镜像
docker rmi         #删除一个或多个镜像
```

### 服务管理
<!--rehype:wrap-class=col-span-2-->

```sh
service docker start       # 启动docker服务，守护进程
service docker stop        # 停止docker服务
service docker status      # 查看docker服务状态
chkconfig docker on        # 设置为开机启动
sytemctl start docker 	   # 启动docker
sytemctl stop docker 	   # 停止docker
sytemctl restart docker    # 重启docker
sytemctl status docker 	   # 查看docker状态
sytemctl enable docker 	   # 开机启动docker
```

### 镜像管理
<!--rehype:wrap-class=col-span-3-->

#### 常用命令

```sh
docker pull centos:latest  # 从docker.io中下载centos镜像到本地
docker images              # 查看已下载的镜像
docker rmi [image_id]      # 删除镜像，指定镜像id

# 删除所有镜像
# none 默认为 docker.io
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)

# 连接进行进入命令行模式，exit命令退出。
docker run -t -i nginx:latest /bin/bash

```


#### 创建镜像
我们可以通过以下两种方式对镜像进行更改。
- 从已经创建的容器中更新镜像，并且提交这个镜像
    ```sh
    # 通过容器创建镜像
    docker commit -m="First Docker" -a="geek-tim" a6b0a6cfdacf geek-tim/nginx:v1.2.1
    ```
- 使用 Dockerfile 指令来创建一个新的镜像
    ```sh
    # 通过Dockerfile创建镜像
    FROM node:8.4
    COPY . /app
    WORKDIR /app
    RUN npm install --registry=https://registry.npm.taobao.org
    EXPOSE 3000
    ```

#### 发布自己的镜像
- 1.在Docker 注册账户，发布的镜像都在这个页面里展示
- 2.将上面做的镜像nginx，起个新的名字nginx-test
    ```sh
    $ docker tag geek-tim/nginx:v1.2.1 geek-tim/nginx-test:lastest
    ```
- 3.登录docker
    ```sh
    $ docker login
    ```
- 4.上传nginx-test镜像
    ```sh
    $ docker push geek-tim/nginx-test:lastest
    ```
<!--rehype:className=style-timeline-->


#### 镜像中安装软件
```sh
# 进入到centos7镜像系统
docker run -i -t centos:7 /bin/bash
yum update
yum install vim
```

### 容器管理
<!--rehype:wrap-class=col-span-3-->

#### 常用命令

```sh
# 列出本机正在运行的容器
docker container ls
# 列出本机所有容器，包括终止运行的容器
docker container ls --all
docker start [containerID/Names] # 启动容器
docker stop [containerID/Names]  # 停止容器
docker rm [containerID/Names]    # 删除容器
docker logs [containerID/Names]  # 查看日志
docker exec -it [containerID/Names] /bin/bash  # 进入容器

# 从正在运行的 Docker 容器里面，将文件拷贝到本机，注意后面有个【点】拷贝到当前目录
docker container cp [containID]:[/path/to/file] .

docker run centos echo "hello world"  # 在docker容器中运行hello world!
docker run centos yum install -y wget # 在docker容器中，安装wget软件
docker ps                             # 列出包括未运行的容器
docker ps -a                          # 查看所有容器(包括正在运行和已停止的)
docker logs my-nginx                  # 查看 my-nginx 容器日志

docker run -i -t centos /bin/bash     # 启动一个容器
docker inspect centos                 # 检查运行中的镜像
docker commit 8bd centos              # 保存对容器的修改
docker commit -m "n changed" my-nginx my-nginx-image # 使用已经存在的容器创建一个镜像

# 获取id为 44fc0f0582d9 的PID进程编号
docker inspect -f {{.State.Pid}} 44fc0f0582d9        

# 下载指定版本容器镜像
docker pull gitlab/gitlab-ce:11.2.3-ce.0
```

#### 容器服务管理
```sh
docker run -itd --name my-nginx2 nginx  # 通过nginx镜像，【创建】容器名为 my-nginx2 的容器
docker start my-nginx --restart=always  # 【启动策略】一个已经存在的容器启动添加策略
    # no - 容器不重启
    # on-failure - 容器推出状态非0时重启
    # always - 始终重启
docker start my-nginx             # 【启动】一个已经存在的容器
docker restart my-nginx           # 【重启】容器
docker stop my-nginx              # 【停止运行】一个容器
docker kill my-nginx              # 【杀死】一个运行中的容器
docker rename my-nginx new-nginx  # 【重命名】容器
docker rm new-nginx               # 【删除】容器
```

#### 进入容器
- 1.创建一个守护状态的 Docker 容器
    ```sh
    $ docker run -itd my-nginx /bin/bash
    ```
- 2.使用docker ps查看到该容器信息
    ```sh
    $ docker ps
    ```
- 3.使用docker exec命令进入一个已经在运行的容器
    ```sh
    $ docker exec -it 6bd0496da64f /bin/bash
    ```
- 4.exit命令退出
    ```sh
    $ exit
    ```
<!--rehype:className=style-timeline-->

#### 文件拷贝
从主机复制到容器 sudo docker cp host_path containerID:container_path
从容器复制到主机 sudo docker cp containerID:container_path host_path

实践（一）
---
### [一] docker环境搭建(安装)
```sh
yum install docker -y          #安装
systemctl start docker         #启动    
systemctl enable docker        #设置开机自启动
```

### [二] 本地镜像发布到阿里云流程

#### 2.1 镜像的生成方法
1、基于原有镜像commit
```sh
docker commit xxx xxx:1.1
```
2、基于dockerfile
```sh
docker commit -m="First Docker" -a="geek-tim" a6b0a6cfdacf geek-tim/nginx:v1.2.1
```
#### 2.2 镜像的推送到阿里云


#### 2.3 从阿里云拉取镜像到本地

### [三] 本地镜像发布到私有仓库
docker registry

实践（二）
---
与实际业务相结合

### 静态业务部署


### 基于 Docker 的 Node.js 应用容器化实践


## dockerfile

### 流程示意图
```
╭┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╮
┆  dockerfile   ┆
╰┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╯
       build
╭┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╮
┆     images    ┆
╰┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╯
       run
╭┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╮
┆   container   ┆
╰┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╯
```

