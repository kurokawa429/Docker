

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

##### (13) 导入和导出镜像

- `docker export 容器ID > 容器文件名.tar`：根据容器 ID 将镜像导出成一个文件
- `docker import - 容器名 < 容器文件名.tar`：导入镜像文件

##### (14) 本地的镜像推送到Docker Hub

- `docker login`：根据容器 ID 将镜像导出成一个文件

- `docker tag <本地镜像名称> <Docker Hub用户名>/<仓库名称>:<标签>`：根为本地镜像添加一个Docker Hub仓库名称和标签

  ex: docker tag <myubuntu> < kurokawa429>/<docker-learning>:<1.3>

- `docker push <Docker Hub用户名>/<仓库名称>:<标签>`：将镜像推送到Docker Hub

  ex: docker push  < kurokawa429>/<docker-learning>:<1.3>

## 三、Docker镜像

### 1. 联合文件系统（UnionFS）

UnionFS是一种分层、轻量级并且高性能的文件系统，**它支持对文件系统的修改作为一次提交来一层层的叠加**，同时可以将不同目录挂载到同一个虚拟文件系统下，UnionFS是docker镜像的基础，镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统该，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

### 2. docker镜像加载原理

docker的镜像实际上是由一层层的文件系统组成，这种层级的文件系统就是UnionFS

bootfs（boot文件系统）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动的时候会加载bootfs文件系统，在docker镜像的最底层是bootfs。这一层与Linux系统该是一样的，包含boot加载器和内核。当boot加载完成后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs（root文件系统），在bootfs之上，包含的是典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统该发行版，如Ubuntu、CentOS等。

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就行了。所以对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以共用bootfs。

### 3. 镜像的特点和优点

##### (1) 镜像的特点

docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部，这一层通常被称作“容器层”，“容器层”之下的为“镜像层”。

##### (2) 分层镜像的优点

使用分层镜像的优点是可以共享资源，比如有多个镜像都从相同的base镜像构建而来，那么宿主机上只需要保存一份base镜像，内存中也只需要加载一份base镜像，就可以为所有容器服务了。镜像的每一层都可以被共享。 以pull为例，在下载的过程中可以看到docker的镜像好像是在一层一层的在下载。

### 4. 镜像提交

##### (1) commit命令

- `docker commit -m="提交的描述信息" -a="作者" 容器名 要创建的目标镜像名:[TAG]`：提交容器副本使之成为一个新的镜像

## 四、容器数据卷

### 1. 容器数据卷

##### (1) 是什么

容器删除后数据自然也就没有了，所以用卷来保存数据。容器数据卷功能是持久化和数据共享。 卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File Syste提供一些用于持续存储或共享数据的特性。

**容器数据卷是一种特殊的文件或目录，可以在容器和主机之间共享数据，并且能够独立于容器的生命周期存在。**

##### (2) 容器数据卷的特点

- 数据卷可以在容器之间共享或重用数据
- 卷中的更改可以直接生效
- 数据卷中的更改不会包含在镜像的更新中
- 数据卷的生命周期一直持续到没有容器使用它为止

### 2. 容器内添加数据卷

##### (1) 直接命令添加

- `docker run -it -v /宿主机绝对路径:/容器内目录[:ro] 镜像名 .`：将宿主机的目录和容器内目录绑定

`:ro`时表示只读模式（容器只能读取宿主机的数据而无法修改）

`:rw`或者不写，表示读写模式

- `docker inspect 容器名`：查看数据卷是否挂载成功，其中Volumes里面有绑定的目录

## 五、分布式存储

##### (1) 是什么

分布式存储是指将数据存储在多个计算机节点上，以提高数据存储的可靠性、可扩展性和性能。

比如：

- Redis：一主多从
- MySQL：主从复制
- Apache Hadoop：一个开源的分布式存储和计算框架，用于处理大规模的数据集。
- Apache ZooKeeper：一个开源的分布式协调服务，用于协调和管理分布式系统中的各个组件，提高系统的可用性和可靠性。

##### (2) 常见的分布式存储算法

- 哈希取余算法：基本思想是将关键字（Key）进行哈希（Hash）运算，得到一个哈希值（Hash Value），然后将哈希值对存储空间大小取余，得到的余数作为该关键字在存储空间中的位置

- 一致性哈希算法：主要思想是将所有的节点和数据映射到一个圆环上，每个节点在圆环上占据一个位置，而数据则根据其哈希值映射到圆环上的位置

- **哈希槽算法**：主要思想是将数据分配到一个固定数量的哈希槽（Hash Slot）中，每个哈希槽对应一个节点，数据存储在对应的节点上

- **布隆过滤器**：空间效率非常高、可用于判定一个元素是否在一个集合中的概率型数据结构，它利用位数组和一些哈希函数来实现

##### (3) 3主3从Redis集群配置

具体案例就不演示了，在

https://www.bilibili.com/video/BV1gr4y1U7CY?p=45&spm_id_from=pageDriver&vd_source=a935d8bab752d7599ca90dbb21e90749

P45-P55中有详细演示

- 集群配置
- 集群读写
- 主从容错
- 主从扩容/缩容
