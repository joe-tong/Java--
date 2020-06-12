## Docker简介

![](/assets/docker.png)

Docker是一个开源的容器引擎，它有助于更快地交付应用。 Docker可将应用程序和基础设施层隔离，并且能将基础设施当作程 序一样进行管理。使用 Docker可更快地打包、测试以及部署应用程序，并可以缩短从编写到部署运行代码的周期。

### Docker的优点如下：

#### 1、简化程序

Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚 拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大 优势，过去需要用数天乃至数周的 任务，在Docker容器的处理下，只需要数秒就能完成。

#### 2、避免选择恐惧症

如果你有选择恐惧症，还是资深患者。Docker 帮你 打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配 置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、 消息队列等等都可以打包成一个镜像部署。

#### 3、节省开支

一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。 Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。

## Docker的架构

* ![](/assets/docker-const.png)
* **Docker daemon（ Docker守护进程）**

Docker daemon是一个运行在宿主机（ DOCKER-HOST）的后台进程。可通过 Docker客户端与之通信。

* Client（ Docker客户端）

Docker客户端是 Docker的用户界面，它可以接受用户命令和配置标识，并与 Docker daemon通信。图中， docker buil

等都是 Docker的相关命令。

* **Images（ Docker镜像）**

Docker镜像是一个只读模板，它包含创建 Docker容器的说明。

它和系统安装光盘有点像

，使用系统安装光盘可以安装系

统，同理，使用Docker镜像可以运行 Docker镜像中的程序。

* **Container（容器）**

容器是镜像的可运行实例。

镜像和容器的关系有点类似于面向对象中，类和对象的关系

。可通过 Docker API或者 CLI命令来

启停、移动、删除容器。

* **Registry**

Docker Registry是一个集中存储与分发镜像的服务。构建完 Docker镜像后，就可在当前宿主机上运行。但如果想要在其他

机器上运行这个镜像，就需要手动复制。此时可借助 Docker Registry来避免镜像的手动复制。

一个 Docker Registry可包含多个 Docker仓库，每个仓库可包含多个镜像标签，每个标签对应一个 Docker镜像。这跟

Maven的仓库有点类似，如果把 Docker Registry比作 Maven仓库的话，那么 Docker仓库就可理解为某jar包的路径，而

镜像标签则可理解为jar包的版本号。

Docker Registry可分为公有Docker Registry和私有Docker Registry。 最常⽤的Docker Registry莫过于官⽅的Docker

Hub， 这也是默认的Docker Registry。 Docker Hub上存放着⼤量优秀的镜像， 我们可使⽤Docker命令下载并使⽤。

## Docker 的安装

Docker 是一个开源的商业产品，有两个版本：社区版（Community Edition，缩写为 CE）和企业版（Enterprise Edition，缩

写为 EE）。企业版包含了一些收费服务，个人开发者一般用不到。下面的介绍都针对社区版。

Docker CE 的安装请参考官方文档，

我们这里以CentOS为例：

1、Docker 要求 CentOS 系统的内核版本高于 3.10

通过 uname -r 命令查看你当前的内核版本

```
uname ‐ r
```

2、使用 root 权限登录 Centos。确保 yum 包更新到最新。

```
yum ‐ y update
```

3、卸载旧版本\(如果安装过旧版本的话\)

```
yum remove docker docker‐common docker‐selinux docker‐engine
```

4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```
yum install ‐y yum‐utils device‐mapper‐persistent‐data lvm2
```

5、设置yum源，并

更新 yum 的包索引

```
yum‐config‐manager ‐‐add‐repo http://mirrors.aliyun.com/docker‐ce/linux/centos/docker‐ce.repo

yum makecache fast
```

6、可以查看所有仓库中所有docker版本，并选择特定版本安装

```
yum list docker‐ce ‐‐showduplicates | sort‐r
```

7、安装docker

```
yum ‐y install docker‐ce‐18.03.1.ce #这是指定版本安装

yum ‐y install docker‐ce #这是安装最新稳定版
```

8、启动并加入开机启动

```
systemctl start docker

systemctl enable docker
```

9、验证安装是否成功\(有client和service两部分表示docker安装启动都成功了\)

```
docker version
```

10、卸载docker

```
yum ‐y remove docker‐engine
```

## Docker常用命令

### 镜像相关命令

#### 1、搜索镜像

可使用 docker search命令搜索存放在 Docker Hub中的镜像。执行该命令后， Docker就会在Docker Hub中搜索含有 java这

个关键词的镜像仓库。

```
docker search java
```

以上列表包含五列，含义如下：

- NAME:镜像仓库名称。

- DESCRIPTION:镜像仓库描述。

- STARS：镜像仓库收藏数，表示该镜像仓库的受欢迎程度，类似于 GitHub的 stars0

- OFFICAL:表示是否为官方仓库，该列标记为\[0K\]的镜像均由各软件的官方项目组创建和维护。

- AUTOMATED：表示是否是自动构建的镜像仓库。

#### 2、下载镜像

使用命令docker pull命令即可从 Docker Registry上下载镜像，执行该命令后，Docker会从 Docker Hub中的 java仓

新版本的 Java镜像。如果要下载指定版本则在java后面加冒号指定版本，例如：docker pull java:8

```
docker pull java:8
```

#### 3、列出镜像

使用 docker images命令即可列出已下载的镜像

```
docker images
```

#### 4、删除本地镜像

使用 docker rmi命令即可删除指定镜像，强制删除加 -f

```
docker rmi java 
```

删除所有镜像

```
docker rmi $(docker images‐q)
```

### 容器相关命令

#### 1、新建并启动容器

使用以下docker run命令即可新建并启动一个容器，该命令是最常用的命令，它有很多选项，下面将列举一些常用的选项。

-d选项：表示后台运行

-P选项：随机端口映射

-p选项：指定端口映射，有以下四种格式。

-- ip:hostPort:containerPort

-- ip::containerPort

-- hostPort:containerPort

-- containerPort

--net选项：指定网络模式，该选项有以下可选参数：

--net=bridge:

默认选项

，表示连接到默认的网桥。

--net=host:容器使用宿主机的网络。

--net=container:NAME-or-ID：告诉 Docker让新建的容器使用已有容器的网络配置。

--net=none：不配置该容器的网络，用户可自定义网络配置。

```
docker run ‐d ‐p 91:80 nginx
```

这样就能启动一个 Nginx容器。在本例中，为 docker run添加了两个参数，含义如下：

-d 后台运行

-p 宿主机端口:容器端口 \#开放容器端口到宿主机端口

访问 http://Docker宿主机 IP:91/，将会看到nginx的主界面如下：





