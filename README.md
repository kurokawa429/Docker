

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

- `docker version`：查看版本信息

##### (2) 查看docker信息

- `docker info`：查看docker信息

##### (3) docker帮助命令

- `docker --help`：帮助命令

### 2. docker镜像命令

##### (1) 列出所有镜像

- `docker images`：列出本地主机上的镜像，镜像由镜像名:tag唯一标记，tag可认为是版本号
- `docker images -a`：列出本地所有的镜像（含中间映像层）
- `docker images -q`：只显示镜像的id
- `docker images --digests`：显示镜像的摘要信息
- `docker images --no-trunc`：显示完整的镜像信息

选项说明：

````yam
REPOSITORY：表示镜像的仓库源
TAG：镜像的标签
IMAGE ID：镜像ID
CREATED：镜像创建时间
SIZE：镜像大小
````

##### (2) 在DockerHub搜索镜像

- `docker search 镜像名`：在DockerHub上搜索某个镜像
- `docker search -s 30 tomcat`：列出starts数不小于30的镜像
- `docker search --no-trunc 镜像名`：显示完整的镜像描述
- `docker --automated 镜像名`：只列出automated build类型的镜像

##### (3) 下载镜像

- `docker pull 镜像名`：下载镜像
- `docker pull 镜像名:TAG`：下载指定TAG的镜像，不加TAG默认为latest

##### (4) 删除未在使用的镜像

- `docker rmi 镜像名`：删除未在使用镜像，若在使用则不能删除，默认删除latest的
- `docker rmi -f 镜像名`：强制删除
- `docker rmi -f 镜像名1:TAG 镜像名2:TAG`：删除多个
- `docker rmi -f $(docker images -qa)`：删除全部



### 3. docker容器命令

以CentOS镜像为例演示，先用`docker pull centos`命令下载相应镜像

##### (1) 新建并启动容器

- `docker run [options] 镜像名 [command] [arg...]`：新建并启动容器

- `docker run --name=容器新名字`：为容器指定一个名称

- `docker run -i 镜像名`：以交互模式运行容器，通常与-t同时使用

- `docker run -t 镜像名`：为容器重新分配一个伪输入终端，通常与-i同时使用

- `docker run -d 镜像名`：后台运行容器，并返回容器id，即启动守护式容器

- `docker run -P 镜像名`： 随机端口映射

- `docker run -p 镜像名`： 指定端口映射，有以下四种格式

  ````yaml
  ip:hostPort:containerPort
  ip::containerPort
  hostPort:containerPort
  containerPort
  ````

##### (2) 列出正在运行的容器

- `docker ps`：列出当前所有正在运行的容器

##### (3) 列出历史上运行过的容器

- `docker ps -a`：列出当前所有正在运行的容器+历史上运行过的容器
- `docker ps -l`：显示最近创建的容器
- `docker ps -n 数字`：显示最近创建的n个容器
- `docker ps -q`：静默模式，只显示容器编号
- `docker ps --no-trunc`：不截断输出

##### (4) 退出容器

- `exit`：容器停止退出
- `Ctrl+P+Q`：容器不停止退出

##### (5) 启动/重启/停止容器

- `docker start 容器名`：启动容器
- `docker restart 容器名`：重启容器
- `docker stop 容器名`：停止容器，类似于电脑关机
- `docker kill 容器名`：强制停止，类似于电脑拔电源关机

##### (6) 删除容器

- `docker rm 容器名`：删除已停止的容器，若未停止则不删除
- `docker rm -f 容器名`：停止并删除容器
- `docker rm -f $(docker ps -aq)`：删除所有容器

##### (7) 后台运行容器

- `docker run -d 容器名`：启动守护式容器，运行在后台，用docker ps查看看不到，因为已经退出了。docker容器后台运行，必须有一个前台进程

##### (8) 容器日志相关

- `docker logs [options] 容器名`：查看容器日志
- `docker logs -t 容器名`：加入时间戳

- `docker logs -f 容器名`：跟随最新的日志打印（动态打印）
- `docker logs --tail 数字 容器名`：显示最后多少条

##### (9) 查看容器内的进程

- `docker top 容器名`：查看容器内的进程

##### (10) 查看容器内部细节

- `docker inspect 容器名`：查看容器内部细节，返回是json串

##### (11) 进入正在运行的容器并以命令行交互

- `docker exec -it 容器名 bash命令`：在容器中打开新的终端，并可以启动新的进程
- `docker exec -it 容器名 ls -l /tmp`：不进入容器，直接拿到`ls -l /tmp`命令的执行结果，等价于使用`docker exec -it 容器名 /bin/bash`先进入bash，再执行`ls -l /tmp`得到结果

- `docker attach 容器名`：直接进入容器启动命令的终端，不会启动新的进程

##### (12) 拷贝容器内容到本机

- `docker cp 容器名:容器中文件的路径 本机路径`：从容器内拷贝文件到本机上
