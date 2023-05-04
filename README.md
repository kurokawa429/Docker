# Docker

## 一、Docker简介

### **1.简介**

Docker是一种开源的容器化平台，可以让开发者将应用程序及其依赖项打包成一个独立的、可移植的容器，从而实现跨平台、快速部署和可靠运行的应用程序。



### 2.**Docker与传统虚拟化方式的不同**

传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程
而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便
每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源



### 3.**Docker三要素**

docker三要素：镜像、仓库、容器

镜像就是模板，容器就是镜像的一个实例，docker利用容器独立运行一个或一组应用，可以把容器看作是一个简易版的Linux环境和运行在其中的应用程序，仓库是集中存放镜像的地方。



### 4. Docker的安装

##### (1) centOS安装docker

````yaml
yum install -y epel-release
yum install -y docker-io
# 安装后的配置文件：/etc/sysconfig/docker
service docker start # 启动docker后台服务
docker version # 查看版本，验证是否安装成功
````

##### (2) ubuntu安装docker

````yam
curl -sSL https://get.daocloud.io/docker | sh
````



##### (3) ubuntu安装docker

````yam
#运行hello world
docker run hello-world
````



## 二、Docker常用命令

### 1. docker帮助命令

##### (1) 查看docker版本

````yam
docker version：查看版本信息
````

##### (2) 查看docker信息

````yam
docker info：查看docker信息
````

##### (3) docker帮助命令

````yaml
docker --help：帮助命令
````

### 2. docker镜像命令

##### (1) 列出所有镜像

````yam
docker images：列出本地主机上的镜像，镜像由镜像名:tag唯一标记，tag可认为是版本号
    docker images -a：列出本地所有的镜像（含中间映像层）
    docker images -q：只显示镜像的id
    docker images --digests：显示镜像的摘要信息
    docker images --no-trunc：显示完整的镜像信息
````

选项说明：

````yam
REPOSITORY：表示镜像的仓库源
TAG：镜像的标签
IMAGE ID：镜像ID
CREATED：镜像创建时间
SIZE：镜像大小
````

##### (2) 在DockerHub搜索镜像

````yam
docker search 镜像名：在DockerHub上搜索某个镜像
    docker search -s 30 tomcat：列出starts数不小于30的镜像
    docker search --no-trunc 镜像名：显示完整的镜像描述
    docker --automated 镜像名：只列出automated build类型的镜像
````

##### (3) 下载镜像

````yaml
docker pull 镜像名：下载镜像
    docker pull 镜像名:TAG：下载指定TAG的镜像，不加TAG默认为latest
````

##### (4) 删除未在使用的镜像

````yam
docker rmi 镜像名：删除未在使用镜像，若在使用则不能删除，默认删除latest的
    docker rmi -f 镜像名：强制删除
    docker rmi -f 镜像名1:TAG 镜像名2:TAG：删除多个
    docker rmi -f $(docker images -qa)：删除全部
````

### 3. docker容器命令

以CentOS镜像为例演示，先用`docker pull centos`命令下载相应镜像
