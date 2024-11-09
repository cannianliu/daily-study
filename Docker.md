# Docker

> 官网：https://www.docker.com/  
> 文档地址：https://docs.docker.com/  
> 仓库地址：https://hub.docker.com/  

基于 Go 语言开发
## 概述

**开发即运维**

环境配置非常麻烦，每一个机器都要部署环境（集群Redis、ES、Hadoop...），费时费力。

**Docker实现打包部署上线，一套流程做完：**

java -- jar（环境） -- 打包项目带上环境（镜像）-- Docker仓库 -- 下载发布的镜像 -- 直接运行即可

**隔离**：Docker核心思想，打包装箱，每个箱子是互相隔离的。

2013年，Docker开源！

2014年，Docker 1.0发布！

容器技术出来之前，都是使用虚拟机技术

> 虚拟机过于笨重，启动慢；docker小巧，秒级启动

**虚拟机技术缺点：**

- 资源占用十分多
- 冗余步骤多
- 启动慢

**Docker与虚拟机技术的不同：**

- 传统虚拟机，虚拟出一个硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内部，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响

**DevOps（开发、运维）**

**1、应用更快速的交付和部署**

**2、更便捷的升级和扩缩容**

**3、更简单的系统运维**

**4、更高效的计算资源利用**

## 安装

### 基本组成

**镜像（image）：**

docker镜像好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像---> run---> tomcat01容器（提供服务），通过这个镜像可以创建多个容器（最终服务运行或项目运行就是在容器中的）。

**容器（container）：**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建

启动，停止，删除等基本命令

目前可以把这个容器理解为一个建议的linux系统

**仓库（repository）：**

仓库是存放镜像的地方

仓库分为公有仓库和私有仓库

Docker Hub（默认是国外的）

国内如阿里云等，都有容器服务器（需要配置镜像加速）

### 安装步骤

> linux环境查看

```shell
# 系统内核为 3.10 以上
[root@cannian ~]# uname -r
3.10.0-1160.25.1.el7.x86_64
```

```shell
# 系统信息
[root@cannian ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> 卸载旧版本

```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

> 安装需要的安装包

```shell
yum install -y yum-utils
```

> 设置镜像的仓库

```shell
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 阿里云
    # https://download.docker.com/linux/centos/docker-ce.repo 
    # 默认是国外的，需要改成国内的镜像
```

> 更新yum软件包索引

```shell
yum makecache fast
```

> 安装docker引擎  ce：社区版；ee：企业版

```shell
yum install docker-ce docker-ce-cli containerd.io
```

> 启动docker

```shell
systemctl start docker
docker version # 查看docker版本信息
```

> 测试docker引擎是否正确运行

```shell
docker run hello-world
```

> 查看已下载的hello-world镜像

```shell
[root@cannian ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   7 months ago   13.3kB
```

> 卸载docker

```shell
# 卸载依赖包
yum remove docker-ce docker-ce-cli containerd.io
# 删除资源
rm -rf /var/lib/docker
rm -rf /var/lib/containerd

# /var/lib/docker docker的默认工作路径
```

### 阿里云镜像加速

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://vrrdfkvm.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## HelloWorld执行流程

```shell
docker run hello-world
```

![[image-20211009093535722.png]]
### 底层原理

Docker是一个C/S结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问

DockerServer接收到DockerClient的指令，就会执行这个指令

**Docker为什么比VM快？**

- Docker有比虚拟机更少的抽象层
- Docker利用的是宿主机的内核，虚拟机则需要GuestOS

## 常用命令

### 帮助命令

```shell
docker version         # 显示docker的版本信息
docker info            # 显示docker的系统信息，包括镜像和容器的数量
docker commond --help  # 帮助命令
```
### 镜像命令

**docker images**	查看所有本地的镜像信息

```shell
[root@cannian ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   7 months ago   13.3kB

# 参数解释
REPOSITORY  镜像的仓库源
TAG         镜像的标签
IMAGE ID    镜像的id
CREATED     镜像的创建时间
SIZE        镜像的大小

# 可选项
-a --all       # 列出所有镜像
-q --quiet     # 只显示镜像的id
```

**docker search**	搜索镜像

```shell
# 可选项
--filter=STARS=3000 # 通过收藏来过滤，只显示收藏3000以上的镜像
```

**docker pull**	下载镜像

```shell
# 可以指定版本，不写tag，默认使用latest版本
docker pull 镜像名[:tag]
# 使用分层下载，联合文件系统，是docker images的核心
# 签名信息
# 真实地址
```

**docker rmi**	删除镜像

```shell
docker rmi -f 镜像id  # 删除指定id的镜像
docker rmi -f 镜像id 镜像id ... # 删除多个镜像
docker rmi -f $(docker images -aq) # 删除全部镜像
```
### 容器命令

> **有了镜像才可以创建容器！**

**docker run**	新建容器并启动

```shell
docker run [可选参数] image

# 可选参数
--name="Name"  # 容器名字：tomcat01,tomcat02，用来区分容器
-d             # 后台方式运行
-it            # 使用交互的方式运行，进入容器查看内容
-p             # 指定容器的端口 -p 8080:8080
	-p ip:主机端口：容器端口
	-p 主机端口：容器端口  #常用
	-p 容器端口
	容器端口
-P 	           # 随机指定端口

# 启动并进入容器
docker run -it image /bin/bash

# 从容器中退回主机
exit
```

**docker ps**	列出容器信息

```shell
docker ps # 列出当前正在运行的容器

# 可选参数
-a            # 列出所有当前和历史运行的容器
-n=?          # 列出最近创建的n个容器
-q            # 只显示容器的id
```

**退出容器**

```shell
exit         # 停止容器运行并退出
Ctrl+P+Q     # 不停止容器并退出
```

**删除容器**

```shell
docker rm 容器id                 # 删除指定容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)   # 删除所有的容器
docker ps -a -q|xargs docker rm # 删除所有的容器
```

**启动和停止容器**

```shell
docker start 容器id       # 启动容器
docker restart 容器id     # 重启容器
docker stop 容器id        # 停止当前正在运行的容器
docker kill 容器id        # 强制停止正在运行的容器
```
### 其他常用命令

**后台启动容器**

```shell
docker run -d image 
# docker 容器使用后台运行，就必须有一个前台进程，docker 发现没有提供服务，就会自动停止
```

**查看日志**

```shell
docker logs -ft --tail n 容器id
# 显示最近n条日志

# 参数解释
-f            # 跟踪日志输出，动态显示
	--tail n  # 最近n行日志
-t            # 显示时间戳
```

**查看容器中的进程信息**

```shell
docker top 容器id

UID PID PPID 
```

**查看容器的元数据**

```shell
docker inspect 容器id
```

**进入当前正在运行的容器**

```shell
# 通常容器都是使用后台方式运行的，有时需要进入容器修改一些配置

docker exec -it 容器id /bin/bash(shell)

docker attach 容器id

# exec    进入容器后开启一个新的终端，可以在里面操作——常用
# attach  进入容器正在执行的终端，不会启动新的进程
```

**从容器内拷贝文件到主机上**

```shell
docker cp 容器id:容器内路径 目标主机路径
# 拷贝是一个手动过程，之后会使用 -v 卷的技术实现同步
```
### 小结

![[image-20211009145535533.png]]

```shell
attach    Attach to a running container                                  #当前shell下attach连接指定运行镜像
build     Build an image from a Dockerfile                               #通过Dockerfile定制镜像
commit    Create a new image from a containers changes                   #提交当前容器为新的镜像
cp        Copy files/folders from a container to a HOSTDIR or to STDOUT  #从容器中拷贝指定文件或者目录到宿主机中
create    Create a new container                                         #创建一个新的容器，同run 但不启动容器
diff      Inspect changes on a containers filesystem                     #查看docker容器变化
events    Get real time events from the server                           #从docker服务获取容器实时事件
exec      Run a command in a running container                           #在已存在的容器上运行命令
export    Export a containers filesystem as a tar archive                #导出容器的内容流作为一个tar归档文件(对应import)
history   Show the history of an image                                   #展示一个镜像形成历史
images    List images                                                    #列出系统当前镜像
import    Import the contents from a tarball to create a filesystem image  #从tar包中的内容创建一个新的文件系统映像(对应export)
info      Display system-wide information                                #显示系统相关信息
inspect   Return low-level information on a container or image           #查看容器详细信息
kill      Kill a running container                                       #kill指定docker容器
load      Load an image from a tar archive or STDIN                      #从一个tar包中加载一个镜像(对应save)
login     Register or log in to a Docker registry                        #注册或者登陆一个docker源服务器
logout    Log out from a Docker registry                                 #从当前Docker registry退出
logs      Fetch the logs of a container                                  #输出当前容器日志信息
pause     Pause all processes within a container                         #暂停容器
port      List port mappings or a specific mapping for the CONTAINER     #查看映射端口对应的容器内部源端口
ps        List containers                                                #列出容器列表
pull      Pull an image or a repository from a registry                  #从docker镜像源服务器拉取指定镜像或者库镜像
push      Push an image or a repository to a registry                    #推送指定镜像或者库镜像至docker源服务器
rename    Rename a container                                             #重命名容器
restart   Restart a running container                                    #重启运行的容器
rm        Remove one or more containers                                  #移除一个或者多个容器
rmi       Remove one or more images                                      #移除一个或多个镜像(无容器使用该镜像才可以删除，否则需要删除相关容器才可以继续或者-f强制删除)
run       Run a command in a new container                               #创建一个新的容器并运行一个命令
save      Save an image(s) to a tar archive                              #保存一个镜像为一个tar包(对应load)
search    Search the Docker Hub for images                               #在dockerhub中搜索镜像
start     Start one or more stopped containers                           #启动容器
stats     Display a live stream of container(s) resource usage statistics  #统计容器使用资源
stop      Stop a running container                                       #停止容器
tag       Tag an image into a repository                                 #给源中镜像打标签
top       Display the running processes of a container                   #查看容器中运行的进程信息
unpause   Unpause all processes within a container                       #取消暂停容器
version   Show the Docker version information                            #查看容器版本号
wait      Block until a container stops, then print its exit code        #截取容器停止时的退出状态值
```

## 可视化

- **portainer**——前期暂用
- **Rancher**(CI/CD)
### **portainer**

Docker图形化界面管理工具，提供一个后台面板供我们操作

```shell
docker run -d -p 8003:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```
## Docker镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件

应用打包成镜像之后，可以直接运行

**获得镜像：**
- 从远程仓库下载
- 朋友拷贝
- 自己制作镜像
### Docker镜像加载原理

> UnionFS（联合文件系统）

联合文件系统是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）。联合文件系统时docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

**下载镜像时一层层的文件就是这个！**

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统就是UnionFS

**bootfs**（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层就是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs

**rootfs**（root file system）在bootfs之上，包含的就是典型Linux系统中的 /dev,/proc,/bin,/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等

对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了，由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs

### 分层理解

所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容是，就会在当前的镜像层之上，创建新的镜像层

![[image-20211011112042699.png]]

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合

下图为一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层的文件7是文件5的一个更新版本

![[image-20211011112519974.png]]

这种情况下，上层镜像中的文件覆盖了下层镜像层中的文件，这样就使得文件的更新版本作为一个新镜像层添加到镜像中

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多层镜像对外展示为统一的文件系统

Linux上可用的存储引擎有AUFS、Overlay2、Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于Linux中对应的文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点

Docker在Windows上仅支持windowsfilter一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW

下图展示了于系统显示相同的三层镜像。所有镜像层堆叠并合并，对外提供统一的视图

![[image-20211011113846185.png]]

> **特点**

docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部

这一层就是我们通常说的**容器层**，容器之下的都叫镜像层
### commit镜像

```shell
docker commit  # 提交容器成为一个新的镜像

# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```
## 容器数据卷

如果数据都在容器中，那么我们容器删除，数据就会丢失！

——需求：**数据可持久化，将容器中产生的数据，同步到本地**

如MySQL数据可以存储在本地

> 注意：建议==不要将数据存在容器中！不要把数据库用docker部署！==

**数据卷技术：**——将容器内的目录，挂载到Linux上
- **容器的持久化和同步操作！容器间的数据共享！**

数据卷是经过特殊设计的目录，可以绕过容器使用的联合文件系统(Union File System)，为一个或多个容器提供访问。**数据卷本质上对应的是宿主机的文件目录**。

![[image-20211012113811762.png]]
### 数据卷使用方法

#### 命令参数

```shell
docker run -it -v 主机内目录:容器内目录
```

将容器删除后，挂载到本地的数据卷依旧不会丢失

**具名和匿名挂载**

```shell
# 匿名挂载 不指定主机内目录
-v 容器内目录 # 不常用

# 具名挂载 指定挂载的目录名字
-v 卷名:容器内目录 # 通过具名挂载可以方便的找到数据卷，常用

# 指定路径挂载
-v 主机内目录:容器内目录

# 查看挂载到的主机内目录
docker volume inspect 卷名
# 所有的docker容器内的卷，没有指定目录的情况下都是在：/var/lib/docker/volumes/xxx/_data
```

拓展：

```shell
# 通过 -v 容器内路径：ro rw 改变读写权限

docker run -d -p --name nginx-test -v juming-nginx:/etc/nginx:ro nginx
# ro 表示这个路径只能通过宿主机来操作，容器内部是无法操作的
```
#### Dockerfile

Dockerfile是用来构建docker镜像的构建文件，通过脚本可以生成镜像

```shell
# 创建dockerfile脚本文件，名字建议dockerfile
# 脚本简单示例
# 指令（全大写） 参数
FROM centos

VOLUME ["/volume01","/ volume02"] # 匿名挂载

CMD echo "---end---"
CMD /bin/bash
```

**生成镜像**

```shell
docker build -f dockerfile脚本文件路径 -t 镜像名:tag .
```

通常会构建自己的容器，因此这种方法会比较常用
### 数据卷容器

不想指定挂载的宿主机的目录，或者说，我们只想实现容器与容器之间的数据共享，这就需要用到数据卷容器。

顾名思义，数据卷容器就是挂载着数据卷的容器，其他容器通过挂载这个容器来实现数据共享。

**数据卷容器的生命周期一直持续到没有人使用为止！**

![[image-20211012111818233.png]]
## DockerFile

Dockerfile是用来构建docker镜像的命令参数脚本

**构建步骤**

- 编写一个dockerfile文件
- docker build 构建成为一个镜像
- docker run 运行镜像
- docker push 发布镜像（DockerHub、阿里云仓库）
### Dockerfile脚本

基础：

1、每个保留关键字（指令）都必须是大写

2、执行从上到下顺序执行

3、# 表示注释

4、每个指令都会创建一个新的镜像层，并提交

### dockerfile命令

```shell
FROM        # 基础镜像
LABEL       # 将元数据添加到镜像；LABEL <key>=<value> <key>=<value> ...
MAINTAINER  # 镜像是谁写的，姓名+邮箱；已启用，使用LABEL代替：LABEL maintainer="author@email"
RUN         # 镜像构建时需要运行的命令
ADD         # 编译镜像时复制文件到镜像中，会自动解压需要添加的压缩包
WORKDIR     # 镜像的工作目录
VOLUME      # 挂载的目录
EXPOSE      # 对外暴露的端口
CMD         # 指定这个镜像启动时的默认参数，可以被重写覆盖，只有最后一个命令会生效，可被替代
ENTRYPOINT  # 指定这个镜像初始化的时候要运行的命令，不可被重写覆盖，可以追加命令
ONBUILD     # 在继承的dockerfile中会自动运行onbuild的指令内容
COPY        # 将文件拷贝到镜像中
ENV         # 构建的时候设置环境变量
```

> Docker Hub中99%的镜像都是以 `FROM scratch`开始，然后配置需要的软件和配置来进行构建

**制作Tomcat镜像**

1、准备tomcat压缩包、jdk压缩包

2、编写dockerfile

```shell
FROM centos
LABEL maintainer="cannian@717762557"

ADD jdk-8u291-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.46.tar.gz /usr/local/

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_291
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.46
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.46
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_HOME/lib

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.46/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.46/logs/catalina.out  
```

3、构建镜像

```shell
# 构建脚本命名为 dockerfile ，build时可以不用指定构建脚本，会自动寻找dockerfile脚本文件
docker build -t imageName .
```

4、启动镜像

```shell
docker run -d -p 8002:8080 --name mytomcat01 -v /home/dockerProgram/tomcat/test:/usr/local/apache-tomcat-9.0.46/webapps/test -v /home/dockerProgram/tomcat/logs/:/usr/local/apache-tomcat-9.0.46/logs mytomcat
```

5、访问测试

### **发布镜像到仓库**

> Docker Hub

登录到dockerhub

```shell
docker login -u username
```

登录成功后提交镜像

```shell
# docker上发布的镜像尽量带上版本号tag
docker push image:tag

# 镜像添加tag
docker tag 镜像id image:tag
```

> 阿里云镜像服务器

- 登录阿里云
- 找到容器镜像服务，创建个人实例
- 创建命名空间
- 创建镜像仓库
### 小结

![[image-20211018145556540.png]]
## Docker网络

`ip addr` 显示结果：
- lo：本地回环地址
- eth0：本地网络地址
- Docker0：docker网络地址

 **本地可以ping通docker容器内部，容器之间也可以互相ping通**

> 原理

1、每启动一个docker容器，docker就会给docker容器分配一个ip，只要安装了docker，就会有一个网卡docker0

veth-pair技术：一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相连，充当一个桥梁（桥接模式），连接各种虚拟网络设备

![[image-20211018155320590.png]]

2、tomcat01和tomcat02公用一个路由器——docker0

所有的容器在不指定网络的情况下，都是使用docker0进行路由的，docker会给容器分配一个默认的可用ip

容器stop后，就会移除veth-pair

### 自定义网络

```shell
docker network ls # 查看所有的docker网络
```

| 网络模式  | 配置                       | 说明                                                         |
| --------- | -------------------------- | ------------------------------------------------------------ |
| host      | --net=host                 | 容器和宿主机共享Network namespace                            |
| container | --net=container:NAME_or_ID | 容器和另外一个容器共享Network namespace。 kubernetes中的pod就是多个容器共享一个Network namespace。 |
| none      | --net=none                 | 容器有独立的Network namespace，但并没有对其进行任何网络设置，如分配veth pair 和网桥连接，配置IP等。 |
| bridge    | --net=bridge               | 桥接 （docker默认）                                          |

```shell
# 自定义网络
docker network create --drive bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

# 参数
--drive    # 网络模式
--subnet   # 子网
--gateway  # 网关
```

自定义的网络docker已经帮我们维护好了对应的关系，推荐平时使用自定义网络

好处：不同的集群使用不同的网络，保证集群是安全和健康的

###  网络连通

```shell
# 将容器和网络连通
docker network connect net container

# 本质是将容器加入到网络中，一个容器两个ip
```

假设需要跨网络，就需要使用该命令进行网络连通
## Redis集群部署实战

部署架构

![[image-20211018164354143.png]]

1、自定义网络

2、通过脚本创建6个redis配置

```shell
# 1、自定义网络
docker network create redis --subnet 172.38.0.0/16

# 2、通过脚本创建6个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379 
bind 0.0.0.0
cluster-enabled yes 
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 3、启动redis容器
for port in $(seq 1 6); \
do \
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf; \
done

# 4、创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
 
# 5、连接上集群
redis-cli -c

# 查看node信息
127.0.0.1:6379> cluster nodes
2f5ad03619cda4d43e2c51820e9b1e9c5e025091 172.38.0.11:6379@16379 myself,master - 0 1634622240000 1 connected 0-5460
286b8bec8fed2c38623b02cfea4e37e8b47e8b19 172.38.0.13:6379@16379 master - 0 1634622240083 3 connected 10923-16383
cba765e87653cd449cf0beebe2888a087e67853c 172.38.0.12:6379@16379 master - 0 1634622242087 2 connected 5461-10922
73e8a1afb6e4762c449fdb68848aa4d89f1053f6 172.38.0.15:6379@16379 slave 2f5ad03619cda4d43e2c51820e9b1e9c5e025091 0 1634622241084 5 connected
0647b4c81ba000a5b79205f5b7b7dadfb3874bb7 172.38.0.14:6379@16379 slave 286b8bec8fed2c38623b02cfea4e37e8b47e8b19 0 1634622241000 4 connected
cc19c15b1e309ce9e8ff6b3ce602f6725bd40844 172.38.0.16:6379@16379 slave cba765e87653cd449cf0beebe2888a087e67853c 0 1634622242087 6 connected

# 6、stop redis-1后，redis-5会自动顶上去成为主节点
172.38.0.15:6379> cluster nodes
cc19c15b1e309ce9e8ff6b3ce602f6725bd40844 172.38.0.16:6379@16379 slave cba765e87653cd449cf0beebe2888a087e67853c 0 1634622760131 6 connected
73e8a1afb6e4762c449fdb68848aa4d89f1053f6 172.38.0.15:6379@16379 myself,master - 0 1634622758000 7 connected 0-5460
2f5ad03619cda4d43e2c51820e9b1e9c5e025091 172.38.0.11:6379@16379 master,fail - 1634622580992 1634622580685 1 connected
286b8bec8fed2c38623b02cfea4e37e8b47e8b19 172.38.0.13:6379@16379 master - 0 1634622760632 3 connected 10923-16383
0647b4c81ba000a5b79205f5b7b7dadfb3874bb7 172.38.0.14:6379@16379 slave 286b8bec8fed2c38623b02cfea4e37e8b47e8b19 0 1634622759629 4 connected
cba765e87653cd449cf0beebe2888a087e67853c 172.38.0.12:6379@16379 master - 0 1634622759128 2 connected 5461-10922
```

**SpringBoot微服务打包Docker镜像**

1、构建springboot项目

2、打包应用

3、编写dockerfile

4、构建镜像

5、发布镜像，拉取运行
## Docker Compose

> 官方介绍

定义运行多个容器的工具，使用YAML file配置文件，使用简单的命令启动复数服务，所有的环境都可以使用compose

Using Compose is basically a three-step process:（3个基本步骤）
1. Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.
2. Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.
3. Run `docker compose up` and the [Docker compose command](https://docs.docker.com/compose/cli-command/) starts and runs your entire app. You can alternatively run `docker-compose up` using the docker-compose binary.

**作用：批量容器编排**

**注意：Compose是Docker官方的开源项目，需要另行安装！**
### 安装

```shell
# 官网命令，外网下载速度太慢
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 换国内源
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m`  > /usr/local/bin/docker-compose

# 赋予文件权限
chmod +x docker-compose

# 查看是否安装成功
[root@cannian bin]# docker-compose version
docker-compose version 1.25.5, build 8a1c60f6
docker-py version: 4.1.0
CPython version: 3.7.5
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
```

**官网简单示例**：https://docs.docker.com/compose/gettingstarted/

1、应用app.py

2、Dockerfile 应用打包镜像脚本

3、Docker-compose.yaml文件（定义整个服务，需要的环境：web、redis）完整的上线服务

4、启动compose项目 docker-compose up（-d 后台运行）

```shell
# 启动成功结果
[root@cannian conf]# docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS         PORTS                    NAMES
69fe55b02cf2   composetest_web   "flask run"              10 seconds ago   Up 8 seconds   0.0.0.0:5000->5000/tcp   composetest_web_1
d308b6e6ece3   redis:alpine      "docker-entrypoint.s…"   10 seconds ago   Up 8 seconds   6379/tcp                 composetest_redis_1
[root@cannian conf]# curl localhost:5000
Hello World! I have been seen 1 times.
```

**默认的服务名**：文件名服务名_num（多个服务器下，num代表副本id）

实际的项目使用集群部署，服务不可能只有一个运行实例

**创建一个网络**：项目中的内容都在同一个网络下，可以通过域名访问

```shell
# 停止compose,在当前运行目录下
docker-compose down
Ctrl + C
```
### yaml编写规则

官方文档：https://docs.docker.com/compose/compose-file/compose-file-v3/

docker-compose 的核心

```yaml
# 3层
# 版本号，向下兼容
version: ""
# 服务
services:
	服务1 web:
	# 服务配置
	images
	build
	network
	...
	服务2 redis:
	...
# 其他配置 网络/卷、全局规则
volumes:
networks:
configs:
```

