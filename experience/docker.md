# 全栈之路 docker 从0到1

## 什么是docker(WHAT：抽象概念)
Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

Docker 包括三个基本概念:

**镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。

**容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

**仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

Docker的主要目标是：”Build ,Ship And Run Any App,AnyWhere“,通过对应组件的封装、分发、部署、运行等生命周期的管理，使用户的APP(可以是一个WEB APP 或数据库应用)以及运行环境能够做到“一次镜像，处处运行“  

## 怎么使用(HOW:怎么玩)

[参考](../docs/docker.md)
### [一] docker环境搭建(安装)

### [二] 本地镜像发布到阿里云流程

#### 2.1 镜像的生成方法
1、基于原有镜像commit
```
docker commit xxx xxx:1.1
```
2、基于dockerfile

#### 2.2 镜像的推送到阿里云
#### 2.3 从阿里云拉取镜像到本地

### [三] 本地镜像发布到私有仓库
docker registry

### docker 操作系统相关命令
<!--rehype:wrap-class=row-span-2-->

:- | :-
:- | :-
`sytemctl start docker `     | 启动docker
`sytemctl stop docker `     | 停止docker
`sytemctl restart docker `     | 重启docker
`sytemctl status docker `     | 查看docker状态
`sytemctl enable docker `     | 开机启动docker

## dockerfile(配置)

dockerfile =(build)=> docker 镜像 =(run)=> docker 容器
构建步骤
1、编写dockerfile
2、docker build

## 与实际业务结合(WHERE:使用场景)

### 静态业务部署


### 基于 Docker 的 Node.js 应用容器化实践

