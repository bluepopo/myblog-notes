【Docker】尚硅谷核心

# 一、Docker的组成

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711202704.png" alt="image-20200711202702266" style="zoom:100%;" />

## 1.1 镜像



Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。 

## 1.2 容器

Docker 利用容器（Container）独立运行的一个或一组应用。容器是用镜像创建的运行实例。

它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台。

可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。

容器的定义和镜像几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

## 1.3 仓库

仓库（Repository）是集中存放镜像文件的场所。
仓库(Repository)和仓库注册服务器（Registry）是有区别的。仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。

仓库分为公开仓库（Public）和私有仓库（Private）两种形式。
最大的公开仓库是 Docker Hub(https://hub.docker.com/)，存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云 、网易云 等





# 二、Docker安装

Docker支持以下的CentOS版本：
CentOS 7 (64-bit)
CentOS 6.5 (64-bit) 或更高的版本

## 2.1 CentOS 6 安装

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711210444.png" alt="image-20200711210443439" style="zoom:100%;" />



查看已安装的CentOS版本信息（CentOS6.8有，CentOS7无该命令）

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110029.png" alt="image-20200711202210387" style="zoom:100%;" />





1. yum install -y epel-release

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711203015.png" alt="image-20200711203013520" style="zoom:100%;" />

2. yum install -y docker-io

No package docker-io available.
错误：无须任何处理,解决方案 [No package docker-io available](https://www.cnblogs.com/jianshuai520/p/11898851.html)





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110038.png" alt="image-20200711203641634" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711204414.png" alt="image-20200711204413429" style="zoom:100%;" />







## 2.2 CentOS 7 安装



[CentOS安装教程：](https://www.cnblogs.com/unicornam/archive/2019/10/16/11687021.html)

官方文档：[https://docs.docker.com/engine/install/centos/]()

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110046.png" alt="image-20200711205751599" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711213836.png" alt="image-20200711213835097" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110054.png" alt="image-20200711213353046" style="zoom:100%;" />

## 2.4 CentOS 8 安装

[菜鸟教程：CentOS Docker 安装](https://www.runoob.com/docker/centos-docker-install.html)

**1. 卸载旧版本**

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



使用 Docker 仓库进行安装

在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

**2. 设置仓库**

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

```
$ sudo yum install -y yum-utils device-mapper-persistent-data  lvm2
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712224514.png" alt="image-20200712224513289" style="zoom:100%;" />



使用以下命令来设置稳定的仓库。可以选择国内的一些源地址：阿里云

```
$ sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712224556.png" alt="image-20200712224555026" style="zoom:100%;" />



**3. 安装 Docker Engine-Community**

安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：

```properties
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712224741.png" alt="image-20200712224740540" style="zoom:100%;" />

<font color=red>出现错误：docker-ce-3：19.xxxxx要求 containerd.io 版本大于 1.2.2-3，但是这里并没有提供者支持安装</font>

使用up主提供的安装方法安装一个 1.2.6版本的

```properties
yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712225317.png" alt="image-20200712225316368" style="zoom:100%;" />



安装好出错步骤后，继续上一步未完成的

```properties
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712225948.png" alt="image-20200712225947299" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712232219.png" alt="image-20200712230135353" style="zoom:100%;" />

我这里是直接docker安装成功了

<font color=red>但是在这一步，up主出现事务错误</font>

<font color=red>解决方案是 删除 podman，移除后再执行上一步未完成命令</font>

```properties
yum remove podman-manpages-1.4.2-5.module_el8.1.0+237+63e26edc.noarch
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712232253.png" alt="image-20200712230241300" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712232307.png" alt="image-20200712230523262" style="zoom:100%;" />



**安装完成后启动docker**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712232357.png" alt="image-20200712230826400" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712232317.png" alt="image-20200712231407914" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712233437.png" alt="image-20200712232939337" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712233443.png" alt="image-20200712230648591" style="zoom:100%;" />

## 2.3 阿里云镜像加速



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110102.png" alt="image-20200711214654957" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711220901.png" alt="image-20200711220900381" style="zoom:100%;" />

使用命令

- 配置阿里云镜像
  - cat /etc/sysconfig/docker
  - other_args="--registry-mirror=https://2ot5mo2t.mirror.aliyuncs.com"

- 重启docker：
  - service docker restart

- 查看运行的docker进程
  - ps -ef|grep docker

```json
{
"registry-mirrors":["https://2ot5mo2t.mirror.aliyuncs.com"]

}
```





Hello-World怎么运行呢？

使用命令：==docker run hello-world==

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711225349.png" alt="image-20200711225347837" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110115.png" alt="image-20200711225931748" style="zoom:100%;" />

## 2.4 Docker底层原理

Docker是一个Client-Server结构的系统，==Docker守护进程==运行在主机上， 然后通过==Socket连接==从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。` 容器，是一个运行时环境，就是我们前面说到的集装箱。`

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711233624.png" alt="image-20200711233623567" style="zoom:100%;" />



为什么Docker比较比VM快?

(1) docker有着比虚拟机更少的抽象层。由亍docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。

(2)docker利用的是宿主机的内核,而不需要Guest OS。因此,当新建一个容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引寻、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载Guest OS,返个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了返个过程,因此新建一个docker容器只需要几秒钟。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711233849.png" alt="image-20200711233847278" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711233901.png" alt="image-20200711233859661" style="zoom:100%;" />

# 三、Docker命令



## 

## 3.1 帮助命令：

- docker version

- docker info

- docker --help





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110126.png" alt="image-20200711233522336" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110131.png" alt="image-20200711234051059" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110137.png" alt="image-20200711234428627" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110144.png" alt="image-20200711234504242" style="zoom:80%;" />

## 3.2 镜像命令

### ==docker images==

列出本地主机上的镜像

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110151.png" alt="image-20200711235312157" style="zoom:100%;" />

各个选项说明:

- REPOSITORY：表示镜像的仓库源

- TAG：镜像的标签

- IMAGE ID：镜像ID

- CREATED：镜像创建时间

- SIZE：镜像大小

  

-  同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。
  如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200711235534.png" alt="image-20200711235533030" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110200.png" alt="image-20200711235915544" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712000109.png" alt="image-20200712000107752" style="zoom:100%;" />



### ==docker search==

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110209.png" alt="image-20200712000250115" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110217.png" alt="image-20200712000502618" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110223.png" alt="image-20200712000734740" style="zoom:100%;" />



### ==docker pull== 

拉取某个XXX镜像名字

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712001143.png" alt="image-20200712001141614" style="zoom:100%;" />





### ==docker rmi==

删除 某个XXX镜像名字ID

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712001605.png" alt="image-20200712001603260" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712110231.png" alt="image-20200712002140759" style="zoom:100%;" />

## 3.3 容器命令

有镜像才能创建容器，这是根本前提(下载一个CentOS镜像演示)：docker pull centos

### docker run ..IMAGE ..

==**新建**并启动容器==

==docker run [OPTIONS] IMAGE [COMMAND] [ARG...]==

- OPTIONS说明（常用）：有些是一个减号，有些是两个减号

> --name="容器新名字": 为容器指定一个名称；
> -d: 后台运行容器，并返回容器ID，也即启动守护式容器；
> -i：以交互模式运行容器，通常与 -t 同时使用；
> -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
> -P: 随机端口映射；
> -p: 指定端口映射，有以下四种格式
>       ip:hostPort:containerPort
>       ip::containerPort
>       hostPort:containerPort
>       containerPort

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712113021.png" alt="image-20200712113019977" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712111904.png" alt="image-20200712111902255" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712112219.png" alt="image-20200712112218666" style="zoom:100%;" />





### docker ps



==docker ps [OPTIONS]==

列出当前所有正在运行的容器

> OPTIONS说明（常用）：
>
> -a :	列出当前所有正在运行的容器+历史上运行过的
> -l :		显示最近创建的容器。
> -n：	显示最近n个创建的容器。
> -q :	静默模式，只显示容器编号。
> --no-trunc : 不截断输出。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164715.png" alt="image-20200712113906507" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712114325.png" alt="image-20200712114323899" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164710.png" alt="image-20200712114558849" style="zoom:100%;" />



### 退出容器&启动重启

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712115220.png" alt="image-20200712115218272" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164614.png" alt="image-20200712115129333" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712115647.png" alt="image-20200712115556847" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164609.png" alt="image-20200712115824305" style="zoom:100%;" />



### docker rm 

==docker rm 容器ID==

一次性删除多个容器

**docker rm -f $(docker ps -a -q)** 

 `-f表示强制删除，后面写已停止容器的id或者是所有容器的id`

`docker ps -a -q | xargs docker rm `

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712120057.png" alt="image-20200712120055869" style="zoom:100%;" />





## 3.4 重要

### docker run -d 容器名

==docker run -d 容器名==

启动守护式容器

#使用镜像centos:latest以`后台模式`启动一个容器:  <font color=red>**docker run -d centos**</font>

问题：然后docker ps -a 进行查看, 会发现容器已经退出
很重要的要说明的一点: Docker容器后台运行,就必须有一个`前台进程.`
容器运行的命令如果不是那些一直挂起的命令（比如运行top，tail），就是会自动退出的。

这个是docker的机制问题,比如你的web容器,我们以nginx为例，正常情况下,我们配置启动服务只需要启动响应的service即可。例如
`service nginx start`
但是,这样做,nginx为后台进程模式运行,就导致docker前台没有运行的应用,
**这样的容器后台启动后,会立即自杀因为他觉得他没事可做了.**
**所以，最佳的解决方案是,将你要运行的程序以前台进程的形式运行**



### docker logs

==docker logs -f -t --tail 容器ID==

docker run -d centos /bin/sh -c "while true;do echo hello zzyy;sleep 2;done"

创建centos容器并作为守护线程启动，但是它在后台并不安静，会每隔2s一直打印该日志信息  `hello zzyy`

*   -t ：是加入时间戳
*   -f ：跟随最新的日志打印，不断更新追加
*   --tail 10 ：显示最后10条日志

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712154012.png" alt="image-20200712154010109" style="zoom:100%;" />



### docker top

==docker top 容器ID==

查看容器内运行的进程

### docker inspect 

==docker inspect 容器ID==

查看容器内部细节

### docker exec& docker attach

==”进入“ 正在运行的容器并以命令行交互==

docker exec -it 容器ID bashShell

重新进入docker attach 容器ID



上述两个区别

- exec 是在容器中打开新的终端，并且可以启动新的进程

- attach 直接进入容器启动命令的终端，不会启动新的进程

```java
//注意 docker exec -t 10b9a3588ab9 ls -l /tmp /bin/bash 命令，是会进入容器的
```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164316.png" alt="image-20200712155228013" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712155910.png" alt="image-20200712155908844" style="zoom:100%;" />



### docker cp

从容器内拷贝文件到主机上

==docker cp  容器ID: 容器内路径 目的主机路径==



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164306.png" alt="image-20200712160722334" style="zoom:100%;" />





## 3.4 命令总结

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712160810.png" alt="image-20200712160808897" style="zoom:100%;" />

> 

```properties
attach    Attach to a running container                 # 当前 shell 下 attach 连接指定运行镜像
build     Build an image from a Dockerfile              # 通过 Dockerfile 定制镜像
commit    Create a new image from a container changes   # 提交当前容器为新的镜像
cp        Copy files/folders from the containers filesystem to the host path   #从容器中拷贝指定文件或者目录到宿主机中
create    Create a new container                        # 创建一个新的容器，同 run，但不启动容器
diff      Inspect changes on a container's filesystem   # 查看 docker 容器变化
events    Get real time events from the server          # 从 docker 服务获取容器实时事件
exec      Run a command in an existing container        # 在已存在的容器上运行命令
export    Stream the contents of a container as a tar archive   # 导出容器的内容流作为一个 tar 归档文件[对应 import ]
history   Show the history of an image                  # 展示一个镜像形成历史
images    List images                                   # 列出系统当前镜像
import    Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export]
info      Display system-wide information               # 显示系统相关信息
inspect   Return low-level information on a container   # 查看容器详细信息
kill      Kill a running container                      # kill 指定 docker 容器
load      Load an image from a tar archive              # 从一个 tar 包中加载一个镜像[对应 save]
login     Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器
logout    Log out from a Docker registry server          # 从当前 Docker registry 退出
logs      Fetch the logs of a container                 # 输出当前容器日志信息
port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口
pause     Pause all processes within a container        # 暂停容器
ps        List containers                               # 列出容器列表
pull      Pull an image or a repository from the docker registry server   # 从docker镜像源服务器拉取指定镜像或者库镜像
push      Push an image or a repository to the docker registry server    # 推送指定镜像或者库镜像至docker源服务器
restart   Restart a running container                   # 重启运行的容器
rm        Remove one or more containers                 # 移除一个或者多个容器
rmi       Remove one or more images             # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]
run       Run a command in a new container              # 创建一个新的容器并运行一个命令
save      Save an image to a tar archive                # 保存一个镜像为一个 tar 包[对应 load]
search    Search for an image on the Docker Hub         # 在 docker hub 中搜索镜像
start     Start a stopped containers                    # 启动容器
stop      Stop a running containers                     # 停止容器
tag       Tag an image into a repository                # 给源中镜像打标签
top       Lookup the running processes of a container   # 查看容器中运行的进程信息
unpause   Unpause a paused container                    # 取消暂停容器
version   Show the docker version information           # 查看 docker 版本号
wait      Block until a container stops, then print its exit code   # 截取容器停止时的退出状态值
 
```





# 四、Docker镜像

## 4.1 是什么

镜像是一种轻量级、可执行的独立软件包，用来`打包软件运行环境`和`基于运行环境开发`的软件，<font color=red>它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。</font>>



**UnionFS（联合文件系统）**：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。`镜像可以通过分层来进行继承`，基于`基础镜像`（没有父镜像），可以制作各种具体的应用镜像。

<font color=blue> 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录. </font>





 **Docker镜像加载原理：**
   docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，`在Docker镜像的最底层是bootfs`。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。 

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712163342.png" alt="image-20200712163341417" style="zoom:100%;" />

 **平时我们安装进虚拟机的CentOS都是好几个G，为什么docker这里才200M？？**

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。



**分层的镜像,为什么 Docker 镜像要采用这种分层结构呢**

以我们的pull为例，在下载的过程中我们可以看到docker的镜像好像是在一层一层的在下载

分成的最大的一个好处就是 - 共享资源

比如：有多个镜像都从相同的 base 镜像构建而来，那么宿主机只需在磁盘上保存一份base镜像，
同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。



## 4.2 镜像的特点

1. 分层的镜像

2. Docker镜像都是只读的
   1. 可写层只是顶层的容器，也就是我们之前用命令常操作的
   2. - 当容器启动时，一个新的可写层被加载到镜像的顶部。
        这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”，镜像层是不可写的。



## 4.3 tomcat案例演示

实现要求：

 在docker上启动一个tomcat容器，通过CentOS的搜狐浏览器访问tomcat

将该tomcat容器中的/webapps/doc文件删除

commit更改后的容器，形成一个新的镜像

运行新的镜像，以该镜像运行的tomcat容器都没有 /docs 文件

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164255.png" alt="image-20200712171040911" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712170239.png" alt="image-20200712170238060" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712170937.png" alt="image-20200712170935755" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712171230.png" alt="image-20200712171229436" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712173851.png" alt="image-20200712171757315" style="zoom:100%;" />



# 五、Docker容器数据卷



## 5.1 是什么


先来看看Docker的理念：
*  将运用与运行的环境打包形成容器运行 ，运行可以伴随着容器，但是我们对数据的要求希望是持久化的
*  容器之间希望有可能共享数据

Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据做为镜像的一部分保存下来，
那么<font color=red>当容器删除后，数据自然也就没有了。为了能保存数据在docker中我们使用卷。</font>

> 一句话：有点类似我们Redis里面的rdb和aof文件









##  5.2 能干嘛

 卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File System提供一些用于持续存储或共享数据的特性：

 卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷

特点：
1：数据卷可在容器之间共享或重用数据
2：卷中的更改可以直接生效
3：数据卷中的更改不会包含在镜像的更新中
4：数据卷的生命周期一直持续到没有容器使用它为止



总之：

容器的持久化

容器间继承+共享数据





## 5.3 容器内添加数据卷

### 直接命令添加



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712205921.png" alt="image-20200712205920594" style="zoom:100%;" />



1.  命令：docker run -it -v /宿主机目录:/容器内目录 centos /bin/bash

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712205949.png" alt="image-20200712180407718" style="zoom:100%;" />

2. 查看数据卷是否挂载成功

docker inspect 容器ID

Volums 数据卷，可以看到容器中的目录和宿主机中的目录绑定了

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713164237.png" alt="image-20200712181335163" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712200546.png" alt="image-20200712200545516" style="zoom:100%;" />

3. 容器和宿主机之间数据共享

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712210002.png" alt="image-20200712181407753" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713163600.png" alt="image-20200712201452733" style="zoom:100%;" />



4. 容器停止退出后，主机修改后数据是否同步

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713163540.png" alt="image-20200712181446714" style="zoom:100%;" />



命令(带权限) ：  docker run -it -v /宿主机绝对路径目录:/容器内目录:==ro== 镜像名

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713163034.png" alt="image-20200712181539101" style="zoom:100%;" />





### DockerFile添加

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161821.png" alt="image-20200712210216389" style="zoom:100%;" />

1.根目录下新建mydocker文件夹并进入



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161717.png" alt="image-20200712211413551" style="zoom:100%;" />



2. 可在Dockerfile中使用==VOLUME指令==来给镜像添加一个或多个数据卷

   ```yml
   VOLUME["/dataVolumeContainer","/dataVolumeContainer2","/dataVolumeContainer3"]
   ```

   说明：

   出于可移植和分享的考虑，用-v 主机目录:容器目录这种方法不能够直接在Dockerfile中实现。
   由于宿主机目录是依赖于特定宿主机的，并不能够保证在所有的宿主机上都存在这样的特定目录，所以DockerFile中不指定。

3. File构建

volume test /；由下面的DockerFile文件中编写的模板，

4. 之后通过docker build 命令构建一个新的镜像，这个通过模板构建出的镜像与容器，一出生就自带了两个目录，即 "/dataVolumeContainer1","/dataVolumeContainer2"

```
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,--------success1"
CMD /bin/bash
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712212417.png" alt="image-20200712212211599" style="zoom:100%;" />



```properties
docker build -f /mydocker/Dockerfile -t zzyy/centos
# 会以宿主机下的 /mydocker/Dockerfile 文件为模板，构建一个新的镜像，名称为zzyy/centos
```





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161705.png" alt="image-20200712213859963" style="zoom:100%;" />



5. run容器

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712214900.png" alt="image-20200712214859169" style="zoom:67%;" />



6. 通过上述步骤，容器内的卷目录地址已经知道，对应的主机目录地址哪？？

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712215343.png" alt="image-20200712215342161" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161652.png" alt="image-20200712215441850" style="zoom:100%;" />





ps。报错解决

```properties
Docker挂载主机目录Docker访问出现cannot open directory .: Permission denied
解决办法：在挂载目录后多加一个--privileged=true参数即可
```



## 5.4 容器共享数据卷

**是什么**：命名的容器挂载数据卷，其它容器通过挂载这个(父容器)实现数据共享，挂载数据卷的容器，称之为数据卷容器

简言之，就是zzyy/cento镜像容器已经挂在了两个数据卷，其他镜像容器继承 zzyy/centos 的话就可以继承这两个数据卷了。



总体概述：

- 以上一步新建的镜像zzyy/centos为模板并运行容器dc01/dc02/dc03

- 它们已经具有容器卷 /dataVolumeContainer1、/dataVolumeContainer2



容器间传递共享(--volumes-from)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712220224.png" alt="image-20200712220223201" style="zoom:100%;" />

1. 先启动一个父容器dc01，并在dataVolumeContainer2新增一些内容

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712221555.png" alt="image-20200712220448758" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712221744.png" alt="image-20200712220515085" style="zoom:100%;" />



2. dc02/dc03继承自dc01，dc02/dc03分别在dataVolumeContainer2各自新增内容

```properties
docker run -it --name dc02 --volumes-from dc01 zzyy/centos
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712221716.png" alt="image-20200712220727681" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712220950.png" alt="image-20200712220949212" style="zoom:100%;" />

3. 回到dc01可以看到02/03各自添加的都能共享了

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712221153.png" alt="image-20200712221151409" style="zoom:100%;" />





4. 删除dc01，dc02修改后dc03可否访问



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712222409.png" alt="image-20200712222407622" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712222423.png" alt="image-20200712222422610" style="zoom:100%;" />

==结论：容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止==

5. 删除dc02后dc03可否访问。新建dc04继承dc03后再删除dc03。

答：都是可访问的。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200712222735.png" alt="image-20200712222734654" style="zoom:100%;" />





# 六、Dockerfile解析

https://hub.docker.com/



## 6.1 是什么

Dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。

构建三步骤

1. 编写Dockerfile文件

2. docker build

3. docker run



那么，DockerFile这个文件到底长什么样子？

以我们熟悉的CentOS为例 



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713113411.png" alt="image-20200713111345992" style="zoom:100%;" />



## 6.2 dockerFile构建过程解析

<font color=blue>Dockerfile内容基础知识</font>

1：每条保留字指令都必须为大写字母且后面要跟随至少一个参数, 不能为空

2：指令按照从上到下，顺序执行

3：#表示注释

4：每条指令都会创建一个新的镜像层，并对镜像进行提交

<font color=blue>Docker执行Dockerfile的大致流程</font>

（1）docker从基础镜像运行一个容器

（2）执行一条指令并对容器作出修改

（3）执行类似docker commit的操作提交一个新的镜像层

（4）docker再基于刚提交的镜像运行一个新容器

（5）执行dockerfile中的下一条指令直到所有指令都执行完成

<font color=blue>总结：</font>

从应用软件的角度来看，Dockerfile、Docker镜像与Docker容器分别代表软件的三个不同阶段，
*  Dockerfile是软件的原材料
*  Docker镜像是软件的交付品
*  Docker容器则可以认为是软件的运行态。
**Dockerfile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可，合力充当Docker体系的基石。**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161351.png" alt="image-20200713113840729" style="zoom:100%;" />

1. Dockerfile，需要定义一个Dockerfile，Dockerfile定义了进程需要的一切东西。Dockerfile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程`(当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制)`等等;

2. Docker镜像，在用Dockerfile定义一个文件之后，`docker build`时会产生一个Docker镜像，当运行 Docker镜像时，会真正开始提供服务;

3. Docker容器，容器是直接提供服务的。

 

## 6.3 DockerFile体系结构(保留字指令)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125757.png" alt="image-20200713115720306" style="zoom:100%;" />

FROM ： 基础镜像，当前新镜像是基于哪个镜像的

MAINTAINER :镜像维护者的姓名和邮箱地址

RUN : 容器构建时需要运行的命令

EXPOSE : 当前容器对外暴露出的端口

WORKDIR :  指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

ENV : 用来在构建镜像过程中设置环境变量

> ENV MY_PATH /usr/mytest
> environment这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；
> 也可以在其它指令中直接使用这些环境变量，
>
> 比如：WORKDIR $MY_PATH

ADD : 将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包

COPY : 类似ADD，拷贝文件和目录到镜像中。
将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置

> - COPY src dest
>
> - COPY ["src", "dest"]

VOLUME : 容器数据卷，用于数据保存和持久化工作

CMD : 指定一个容器启动时要运行的命令 

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125751.png" alt="image-20200713121140680" style="zoom:100%;" />

Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

ENTRYPOINT : 指定一个容器启动时要运行的命令,ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数

ONBUILD : 当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713121421.png" alt="image-20200713121419872" style="zoom:100%;" />

## 6.4 案例1 :自定义镜像mycentos

==Base镜像(scratch)==

Docker Hub 中 99% 的镜像都是通过在 base 镜像中安装和配置需要的软件构建出来的

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713121722.png" alt="image-20200713121721455" style="zoom:100%;" />



==自定义镜像mycentos==

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713122221.png" alt="image-20200713122220329" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161147.png" alt="image-20200713122126825" style="zoom:100%;" />

1. <font color=blue>Hub默认CentOS镜像什么情况</font>

假设要求自定义mycentos目的使我们自己的镜像具备如下：
         登陆后可以落脚到的默认路径
         有vim编辑器
         查看网络配置ifconfig支持

2. <font color=blue>准备编写DockerFile文件</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161141.png" alt="image-20200713122537795" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713124946.png" alt="image-20200713124945019" style="zoom:100%;" />

```
FROM centos
MAINTAINER zzyy<zzyy167@126.com>
 
ENV MYPATH /usr/local
WORKDIR $MYPATH
 
RUN yum -y install vim
RUN yum -y install net-tools
 
EXPOSE 80
 
CMD echo $MYPATH
CMD echo "success--------------ok"
CMD /bin/bash
```

3. <font color=blue>构建,docker build -t 新镜像名字:TAG .</font>



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713125354.png" alt="image-20200713125352853" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161133.png" alt="image-20200713125259989" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161126.png" alt="image-20200713125501016" style="zoom:100%;" />

<font color=blue>4. 运行,docker run -it 新镜像名字:TAG </font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713130107.png" alt="image-20200713130106304" style="zoom:100%;" />

<font color=blue>5. 列出镜像的变更历史 ,docker history 镜像名</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713130252.png" alt="image-20200713130250908" style="zoom:100%;" />





## 6.5 案例2 ：CMD/ENTRYPOINT 镜像案例

都是指定一个容器启动时要运行的命令

区别

==CMD==

Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

Case  tomcat的讲解演示  docker run -it -p 8888:8080 tomcat ls -l



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713132442.png" alt="image-20200713131306176" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713132452.png" alt="image-20200713131716496" style="zoom:100%;" />



==ENTRYPOINT== 

docker run 之后的参数会被当做参数传递给 ENTRYPOINT，之后形成新的命令组合

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161119.png" alt="image-20200713130601283" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713133541.png" alt="image-20200713133540057" style="zoom:100%;" />

```properties
FROM centos
RUN yum install -y curl
ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713134013.png" alt="image-20200713134012446" style="zoom:100%;" />



## 6.6 案例3：ONBUILD



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161111.png" alt="image-20200713134434961" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161105.png" alt="image-20200713135003407" style="zoom:100%;" />





## 6.7 案例4：自定义tomcat

==ADD 复制且解压缩==

==COPY 复制==

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713135625.png" alt="image-20200713135624123" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161057.png" alt="image-20200713135640303" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125826.png" alt="image-20200713135648341" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161050.png" alt="image-20200713140909653" style="zoom:100%;" />



```properties
FROM         centos
MAINTAINER    zzyy<zzyybs@126.com>
#把宿主机当前上下文的c.txt拷贝到容器/usr/local/路径下
COPY c.txt /usr/local/cincontainer.txt
#把java与tomcat添加到容器中
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.8.tar.gz /usr/local/
#安装vim编辑器
RUN yum -y install vim
#设置工作访问时候的WORKDIR路径，登录落脚点
ENV MYPATH /usr/local
WORKDIR $MYPATH
#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_171
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.8
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.8
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#容器运行时监听的端口
EXPOSE  8080
#启动时运行tomcat
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.8/bin/startup.sh" ]
# CMD ["/usr/local/apache-tomcat-9.0.8/bin/catalina.sh","run"]
CMD /usr/local/apache-tomcat-9.0.8/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.8/bin/logs/catalina.out
 
```

run 运行该容器，并追加参数，添加数据卷完成宿主机与该容器的数据共享

```properties
docker run -d -p 9080:8080 --name myt9 
-v /zzyyuse/mydockerfile/tomcat9/test:/usr/local/apache-tomcat-9.0.8/webapps/test 
-v /zzyyuse/mydockerfile/tomcat9/tomcat9logs/:/usr/local/apache-tomcat-9.0.8/logs --privileged=true zzyytomcat9
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713141116.png" alt="image-20200713141115575" style="zoom:100%;" />

> 备注：Docker挂载主机目录Docker访问出现cannot open directory .: Permission denied
> 解决办法：在挂载目录后多加一个--privileged=true参数即可

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713141149.png" alt="image-20200713141147593" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713144230.png" alt="image-20200713144229014" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713144241.png" alt="image-20200713144239294" style="zoom:100%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://java.sun.com/xml/ns/javaee"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
  id="WebApp_ID" version="2.5">
  
  <display-name>test</display-name>
 
</web-app>

```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
  </head>
  <body>
    -----------welcome------------
    <%="i am in docker tomcat self "%>
    <br>
    <br>
    <% System.out.println("=============docker tomcat self");%>
  </body>
</html>
 

```





## 6.8 小总结

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713144421.png" alt="image-20200713144420048" style="zoom:100%;" />





# 七、Docker常用安装

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713144914.png" alt="image-20200713144912886" style="zoom:100%;" />





## 7.1 安装 Tomcat

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713145006.png" alt="image-20200713145005466" style="zoom:100%;" />

## 7.2 安装 MySQL

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713145116.png" alt="image-20200713145115006" style="zoom:100%;" />

```
docker pull daocloud.io/library/mysql:5.7
docker run -p 3306:3306 --name mysql 
docker exec -it MySQL运行成功后的容器ID     /bin/bash
mysql -uroot -p
create database db01;
use db01;
create table tb_book(id int not null primary key ,name varchar(20));
insert into tb_book (001,docker to study);
select * from tb_book;
exit

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713145519.png" alt="image-20200713145517712" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713152055.png" alt="image-20200713152054257" style="zoom:100%;" />

```properties
docker run -p 3306:3306 --name mysql -v /zzyyuse/mysql/conf:/etc/mysql/conf.d -v /zzyyuse/mysql/logs:/logs -v /zzyyuse/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d daocloud.io/library/mysql:5.7
 
# 命令说明：
-p 12345:3306：将主机的12345端口映射到docker容器的3306端口。
--name mysql：运行服务名字
-v /zzyyuse/mysql/conf:/etc/mysql/conf.d ：将主机/zzyyuse/mysql录下的conf/my.cnf 挂载到容器的 /etc/mysql/conf.d
-v /zzyyuse/mysql/logs:/logs：将主机/zzyyuse/mysql目录下的 logs 目录挂载到容器的 /logs。
-v /zzyyuse/mysql/data:/var/lib/mysql ：将主机/zzyyuse/mysql目录下的data目录挂载到容器的 /var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=123456：初始化 root 用户的密码。
-d mysql:5.6 : 后台程序运行mysql5.6
```

```properties
docker exec -it MySQL运行成功后的容器ID     /bin/bash
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713152304.png" alt="image-20200713152300997" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161037.png" alt="image-20200713153133253" style="zoom:100%;" />





外部Win10也来连接运行在dokcer上的mysql服务

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713152338.png" alt="image-20200713152326333" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713161005.png" alt="image-20200713161003621" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713160923.png" alt="image-20200713160922248" style="zoom:100%;" />





数据备分测试：

```
docker exec myql服务容器ID sh -c ' exec mysqldump --all-databases -uroot -p"123456" ' > /zzyyuse/all-databases.sql
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713153822.png" alt="image-20200713153820603" style="zoom:100%;" />





## 7.3 安装 Redis

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713171006.png" alt="image-20200713171004556" style="zoom:100%;" />

```properties
docker run -p 6379:6379 -v /zzyyuse/myredis/data:/data -v /zzyyuse/myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf  -d redis:3.2 redis-server /usr/local/etc/redis/redis.conf --appendonly yes

# 主机的redis.conf文件与容器中的redis.conf文件共享
# -d 以后台守护进程启动容器
#-- appendonly yes 
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713193537.png" alt="image-20200713171153589" style="zoom:100%;" />



在主机/zzyyuse/myredis/conf/redis.conf目录下新建redis.conf文件
vim /zzyyuse/myredis/conf/redis.conf/redis.conf

```properties
# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf
 
# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
################################## INCLUDES ###################################
 
# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf
 
################################## NETWORK #####################################
 
# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only into
# the IPv4 lookback interface address (this means Redis will be able to
# accept connections only from clients running into the same computer it
# is running).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#bind 127.0.0.1
 
# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode yes
 
# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379
 
# TCP listen() backlog.
#
# In high requests-per-second environments you need an high backlog in order
# to avoid slow clients connections issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlog
# in order to get the desired effect.
tcp-backlog 511
 
# Unix socket.
#
# Specify the path for the Unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700
 
# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0
 
# TCP keepalive.
#
# If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
# of communication. This is useful for two reasons:
#
# 1) Detect dead peers.
# 2) Take the connection alive from the point of view of network
#    equipment in the middle.
#
# On Linux, the specified value (in seconds) is the period used to send ACKs.
# Note that to close the connection the double of the time is needed.
# On other kernels the period depends on the kernel configuration.
#
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300
 
################################# GENERAL #####################################
 
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
#daemonize no
 
# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised no
 
# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/run/redis_6379.pid
 
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice
 
# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""
 
# To enable logging to the system logger, just set 'syslog-enabled' to yes,
# and optionally update the other syslog parameters to suit your needs.
# syslog-enabled no
 
# Specify the syslog identity.
# syslog-ident redis
 
# Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
# syslog-facility local0
 
# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16
 
################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""
 
save 120 1
save 300 10
save 60 10000
 
# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes
 
# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes
 
# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes
 
# The filename where to dump the DB
dbfilename dump.rdb
 
# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./
 
################################# REPLICATION #################################
 
# Master-Slave replication. Use slaveof to make a Redis instance a copy of
# another Redis server. A few things to understand ASAP about Redis replication.
#
# 1) Redis replication is asynchronous, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of slaves.
# 2) Redis slaves are able to perform a partial resynchronization with the
#    master if the replication link is lost for a relatively small amount of
#    time. You may want to configure the replication backlog size (see the next
#    sections of this file) with a sensible value depending on your needs.
# 3) Replication is automatic and does not need user intervention. After a
#    network partition slaves automatically try to reconnect to masters
#    and resynchronize with them.
#
# slaveof <masterip> <masterport>
 
# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the slave to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the slave request.
#
# masterauth <master-password>
 
# When a slave loses its connection with the master, or when the replication
# is still in progress, the slave can act in two different ways:
#
# 1) if slave-serve-stale-data is set to 'yes' (the default) the slave will
#    still reply to client requests, possibly with out of date data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) if slave-serve-stale-data is set to 'no' the slave will reply with
#    an error "SYNC with master in progress" to all the kind of commands
#    but to INFO and SLAVEOF.
#
slave-serve-stale-data yes
 
# You can configure a slave instance to accept writes or not. Writing against
# a slave instance may be useful to store some ephemeral data (because data
# written on a slave will be easily deleted after resync with the master) but
# may also cause problems if clients are writing to it because of a
# misconfiguration.
#
# Since Redis 2.6 by default slaves are read-only.
#
# Note: read only slaves are not designed to be exposed to untrusted clients
# on the internet. It's just a protection layer against misuse of the instance.
# Still a read only slave exports by default all the administrative commands
# such as CONFIG, DEBUG, and so forth. To a limited extent you can improve
# security of read only slaves using 'rename-command' to shadow all the
# administrative / dangerous commands.
slave-read-only yes
 
# Replication SYNC strategy: disk or socket.
#
# -------------------------------------------------------
# WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
# -------------------------------------------------------
#
# New slaves and reconnecting slaves that are not able to continue the replication
# process just receiving differences, need to do what is called a "full
# synchronization". An RDB file is transmitted from the master to the slaves.
# The transmission can happen in two different ways:
#
# 1) Disk-backed: The Redis master creates a new process that writes the RDB
#                 file on disk. Later the file is transferred by the parent
#                 process to the slaves incrementally.
# 2) Diskless: The Redis master creates a new process that directly writes the
#              RDB file to slave sockets, without touching the disk at all.
#
# With disk-backed replication, while the RDB file is generated, more slaves
# can be queued and served with the RDB file as soon as the current child producing
# the RDB file finishes its work. With diskless replication instead once
# the transfer starts, new slaves arriving will be queued and a new transfer
# will start when the current one terminates.
#
# When diskless replication is used, the master waits a configurable amount of
# time (in seconds) before starting the transfer in the hope that multiple slaves
# will arrive and the transfer can be parallelized.
#
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
repl-diskless-sync no
 
# When diskless replication is enabled, it is possible to configure the delay
# the server waits in order to spawn the child that transfers the RDB via socket
# to the slaves.
#
# This is important since once the transfer starts, it is not possible to serve
# new slaves arriving, that will be queued for the next RDB transfer, so the server
# waits a delay in order to let more slaves arrive.
#
# The delay is specified in seconds, and by default is 5 seconds. To disable
# it entirely just set it to 0 seconds and the transfer will start ASAP.
repl-diskless-sync-delay 5
 
# Slaves send PINGs to server in a predefined interval. It's possible to change
# this interval with the repl_ping_slave_period option. The default value is 10
# seconds.
#
# repl-ping-slave-period 10
 
# The following option sets the replication timeout for:
#
# 1) Bulk transfer I/O during SYNC, from the point of view of slave.
# 2) Master timeout from the point of view of slaves (data, pings).
# 3) Slave timeout from the point of view of masters (REPLCONF ACK pings).
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-slave-period otherwise a timeout will be detected
# every time there is low traffic between the master and the slave.
#
# repl-timeout 60
 
# Disable TCP_NODELAY on the slave socket after SYNC?
#
# If you select "yes" Redis will use a smaller number of TCP packets and
# less bandwidth to send data to slaves. But this can add a delay for
# the data to appear on the slave side, up to 40 milliseconds with
# Linux kernels using a default configuration.
#
# If you select "no" the delay for data to appear on the slave side will
# be reduced but more bandwidth will be used for replication.
#
# By default we optimize for low latency, but in very high traffic conditions
# or when the master and slaves are many hops away, turning this to "yes" may
# be a good idea.
repl-disable-tcp-nodelay no
 
# Set the replication backlog size. The backlog is a buffer that accumulates
# slave data when slaves are disconnected for some time, so that when a slave
# wants to reconnect again, often a full resync is not needed, but a partial
# resync is enough, just passing the portion of data the slave missed while
# disconnected.
#
# The bigger the replication backlog, the longer the time the slave can be
# disconnected and later be able to perform a partial resynchronization.
#
# The backlog is only allocated once there is at least a slave connected.
#
# repl-backlog-size 1mb
 
# After a master has no longer connected slaves for some time, the backlog
# will be freed. The following option configures the amount of seconds that
# need to elapse, starting from the time the last slave disconnected, for
# the backlog buffer to be freed.
#
# A value of 0 means to never release the backlog.
#
# repl-backlog-ttl 3600
 
# The slave priority is an integer number published by Redis in the INFO output.
# It is used by Redis Sentinel in order to select a slave to promote into a
# master if the master is no longer working correctly.
#
# A slave with a low priority number is considered better for promotion, so
# for instance if there are three slaves with priority 10, 100, 25 Sentinel will
# pick the one with priority 10, that is the lowest.
#
# However a special priority of 0 marks the slave as not able to perform the
# role of master, so a slave with priority of 0 will never be selected by
# Redis Sentinel for promotion.
#
# By default the priority is 100.
slave-priority 100
 
# It is possible for a master to stop accepting writes if there are less than
# N slaves connected, having a lag less or equal than M seconds.
#
# The N slaves need to be in "online" state.
#
# The lag in seconds, that must be <= the specified value, is calculated from
# the last ping received from the slave, that is usually sent every second.
#
# This option does not GUARANTEE that N replicas will accept the write, but
# will limit the window of exposure for lost writes in case not enough slaves
# are available, to the specified number of seconds.
#
# For example to require at least 3 slaves with a lag <= 10 seconds use:
#
# min-slaves-to-write 3
# min-slaves-max-lag 10
#
# Setting one or the other to 0 disables the feature.
#
# By default min-slaves-to-write is set to 0 (feature disabled) and
# min-slaves-max-lag is set to 10.
 
# A Redis master is able to list the address and port of the attached
# slaves in different ways. For example the "INFO replication" section
# offers this information, which is used, among other tools, by
# Redis Sentinel in order to discover slave instances.
# Another place where this info is available is in the output of the
# "ROLE" command of a masteer.
#
# The listed IP and address normally reported by a slave is obtained
# in the following way:
#
#   IP: The address is auto detected by checking the peer address
#   of the socket used by the slave to connect with the master.
#
#   Port: The port is communicated by the slave during the replication
#   handshake, and is normally the port that the slave is using to
#   list for connections.
#
# However when port forwarding or Network Address Translation (NAT) is
# used, the slave may be actually reachable via different IP and port
# pairs. The following two options can be used by a slave in order to
# report to its master a specific set of IP and port, so that both INFO
# and ROLE will report those values.
#
# There is no need to use both the options if you need to override just
# the port or the IP address.
#
# slave-announce-ip 5.5.5.5
# slave-announce-port 1234
 
################################## SECURITY ###################################
 
# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared
 
# Command renaming.
#
# It is possible to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# hard to guess so that it will still be available for internal-use tools
# but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
# rename-command CONFIG ""
#
# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to slaves may cause problems.
 
################################### LIMITS ####################################
 
# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# maxclients 10000
 
# Don't use more memory than the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU cache, or to set
# a hard memory limit for an instance (using the 'noeviction' policy).
#
# WARNING: If you have slaves attached to an instance with maxmemory on,
# the size of the output buffers needed to feed the slaves are subtracted
# from the used memory count, so that network problems / resyncs will
# not trigger a loop where keys are evicted, and in turn the output
# buffer of slaves is full with DELs of keys evicted triggering the deletion
# of more keys, and so forth until the database is completely emptied.
#
# In short... if you have slaves attached it is suggested that you set a lower
# limit for maxmemory so that there is some free RAM on the system for slave
# output buffers (but this is not needed if the policy is 'noeviction').
#
# maxmemory <bytes>
 
# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> remove the key with an expire set using an LRU algorithm
# allkeys-lru -> remove any key according to the LRU algorithm
# volatile-random -> remove a random key with an expire set
# allkeys-random -> remove a random key, any key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# noeviction -> don't expire at all, just return an error on write operations
#
# Note: with any of the above policies, Redis will return an error on write
#       operations, when there are no suitable keys for eviction.
#
#       At the date of writing these commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# The default is:
#
# maxmemory-policy noeviction
 
# LRU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can tune it for speed or
# accuracy. For default Redis will check five keys and pick the one that was
# used less recently, you can change the sample size using the following
# configuration directive.
#
# The default of 5 produces good enough results. 10 Approximates very closely
# true LRU but costs a bit more CPU. 3 is very fast but not very accurate.
#
# maxmemory-samples 5
 
############################## APPEND ONLY MODE ###############################
 
# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.
 
appendonly no
 
# The name of the append only file (default: "appendonly.aof")
 
appendfilename "appendonly.aof"
 
# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".
 
# appendfsync always
appendfsync everysec
# appendfsync no
 
# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.
 
no-appendfsync-on-rewrite no
 
# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.
 
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
 
# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes
 
################################ LUA SCRIPTING  ###############################
 
# Max execution time of a Lua script in milliseconds.
#
# If the maximum execution time is reached Redis will log that a script is
# still in execution after the maximum allowed time and will start to
# reply to queries with an error.
#
# When a long running script exceeds the maximum execution time only the
# SCRIPT KILL and SHUTDOWN NOSAVE commands are available. The first can be
# used to stop a script that did not yet called write commands. The second
# is the only way to shut down the server in the case a write command was
# already issued by the script but the user doesn't want to wait for the natural
# termination of the script.
#
# Set it to 0 or a negative value for unlimited execution without warnings.
lua-time-limit 5000
 
################################ REDIS CLUSTER  ###############################
#
# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
# WARNING EXPERIMENTAL: Redis Cluster is considered to be stable code, however
# in order to mark it as "mature" we need to wait for a non trivial percentage
# of users to deploy it in production.
# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
#
# Normal Redis instances can't be part of a Redis Cluster; only nodes that are
# started as cluster nodes can. In order to start a Redis instance as a
# cluster node enable the cluster support uncommenting the following:
#
# cluster-enabled yes
 
# Every cluster node has a cluster configuration file. This file is not
# intended to be edited by hand. It is created and updated by Redis nodes.
# Every Redis Cluster node requires a different cluster configuration file.
# Make sure that instances running in the same system do not have
# overlapping cluster configuration file names.
#
# cluster-config-file nodes-6379.conf
 
# Cluster node timeout is the amount of milliseconds a node must be unreachable
# for it to be considered in failure state.
# Most other internal time limits are multiple of the node timeout.
#
# cluster-node-timeout 15000
 
# A slave of a failing master will avoid to start a failover if its data
# looks too old.
#
# There is no simple way for a slave to actually have a exact measure of
# its "data age", so the following two checks are performed:
#
# 1) If there are multiple slaves able to failover, they exchange messages
#    in order to try to give an advantage to the slave with the best
#    replication offset (more data from the master processed).
#    Slaves will try to get their rank by offset, and apply to the start
#    of the failover a delay proportional to their rank.
#
# 2) Every single slave computes the time of the last interaction with
#    its master. This can be the last ping or command received (if the master
#    is still in the "connected" state), or the time that elapsed since the
#    disconnection with the master (if the replication link is currently down).
#    If the last interaction is too old, the slave will not try to failover
#    at all.
#
# The point "2" can be tuned by user. Specifically a slave will not perform
# the failover if, since the last interaction with the master, the time
# elapsed is greater than:
#
#   (node-timeout * slave-validity-factor) + repl-ping-slave-period
#
# So for example if node-timeout is 30 seconds, and the slave-validity-factor
# is 10, and assuming a default repl-ping-slave-period of 10 seconds, the
# slave will not try to failover if it was not able to talk with the master
# for longer than 310 seconds.
#
# A large slave-validity-factor may allow slaves with too old data to failover
# a master, while a too small value may prevent the cluster from being able to
# elect a slave at all.
#
# For maximum availability, it is possible to set the slave-validity-factor
# to a value of 0, which means, that slaves will always try to failover the
# master regardless of the last time they interacted with the master.
# (However they'll always try to apply a delay proportional to their
# offset rank).
#
# Zero is the only value able to guarantee that when all the partitions heal
# the cluster will always be able to continue.
#
# cluster-slave-validity-factor 10
 
# Cluster slaves are able to migrate to orphaned masters, that are masters
# that are left without working slaves. This improves the cluster ability
# to resist to failures as otherwise an orphaned master can't be failed over
# in case of failure if it has no working slaves.
#
# Slaves migrate to orphaned masters only if there are still at least a
# given number of other working slaves for their old master. This number
# is the "migration barrier". A migration barrier of 1 means that a slave
# will migrate only if there is at least 1 other working slave for its master
# and so forth. It usually reflects the number of slaves you want for every
# master in your cluster.
#
# Default is 1 (slaves migrate only if their masters remain with at least
# one slave). To disable migration just set it to a very large value.
# A value of 0 can be set but is useful only for debugging and dangerous
# in production.
#
# cluster-migration-barrier 1
 
# By default Redis Cluster nodes stop accepting queries if they detect there
# is at least an hash slot uncovered (no available node is serving it).
# This way if the cluster is partially down (for example a range of hash slots
# are no longer covered) all the cluster becomes, eventually, unavailable.
# It automatically returns available as soon as all the slots are covered again.
#
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes
 
# In order to setup your cluster make sure to read the documentation
# available at http://redis.io web site.
 
################################## SLOW LOG ###################################
 
# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.
 
# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000
 
# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 128
 
################################ LATENCY MONITOR ##############################
 
# The Redis latency monitoring subsystem samples different operations
# at runtime in order to collect data related to possible sources of
# latency of a Redis instance.
#
# Via the LATENCY command this information is available to the user that can
# print graphs and obtain reports.
#
# The system only logs operations that were performed in a time equal or
# greater than the amount of milliseconds specified via the
# latency-monitor-threshold configuration directive. When its value is set
# to zero, the latency monitor is turned off.
#
# By default latency monitoring is disabled since it is mostly not needed
# if you don't have latency issues, and collecting data has a performance
# impact, that while very small, can be measured under big load. Latency
# monitoring can easily be enabled at runtime using the command
# "CONFIG SET latency-monitor-threshold <milliseconds>" if needed.
latency-monitor-threshold 0
 
############################# EVENT NOTIFICATION ##############################
 
# Redis can notify Pub/Sub clients about events happening in the key space.
# This feature is documented at http://redis.io/topics/notifications
#
# For instance if keyspace events notification is enabled, and a client
# performs a DEL operation on key "foo" stored in the Database 0, two
# messages will be published via Pub/Sub:
#
# PUBLISH __keyspace@0__:foo del
# PUBLISH __keyevent@0__:del foo
#
# It is possible to select the events that Redis will notify among a set
# of classes. Every class is identified by a single character:
#
#  K     Keyspace events, published with __keyspace@<db>__ prefix.
#  E     Keyevent events, published with __keyevent@<db>__ prefix.
#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
#  $     String commands
#  l     List commands
#  s     Set commands
#  h     Hash commands
#  z     Sorted set commands
#  x     Expired events (events generated every time a key expires)
#  e     Evicted events (events generated when a key is evicted for maxmemory)
#  A     Alias for g$lshzxe, so that the "AKE" string means all the events.
#
#  The "notify-keyspace-events" takes as argument a string that is composed
#  of zero or multiple characters. The empty string means that notifications
#  are disabled.
#
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
#
#  By default all notifications are disabled because most users don't need
#  this feature and the feature has some overhead. Note that if you don't
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events ""
 
############################### ADVANCED CONFIG ###############################
 
# Hashes are encoded using a memory efficient data structure when they have a
# small number of entries, and the biggest entry does not exceed a given
# threshold. These thresholds can be configured using the following directives.
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
 
# Lists are also encoded in a special way to save a lot of space.
# The number of entries allowed per internal list node can be specified
# as a fixed maximum size or a maximum number of elements.
# For a fixed maximum size, use -5 through -1, meaning:
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
# Positive numbers mean store up to _exactly_ that number of elements
# per list node.
# The highest performing option is usually -2 (8 Kb size) or -1 (4 Kb size),
# but if your use case is unique, adjust the settings as necessary.
list-max-ziplist-size -2
 
# Lists may also be compressed.
# Compress depth is the number of quicklist ziplist nodes from *each* side of
# the list to *exclude* from compression.  The head and tail of the list
# are always uncompressed for fast push/pop operations.  Settings are:
# 0: disable all list compression
# 1: depth 1 means "don't start compressing until after 1 node into the list,
#    going from either the head or tail"
#    So: [head]->node->node->...->node->[tail]
#    [head], [tail] will always be uncompressed; inner nodes will compress.
# 2: [head]->[next]->node->node->...->node->[prev]->[tail]
#    2 here means: don't compress head or head->next or tail->prev or tail,
#    but compress all nodes between them.
# 3: [head]->[next]->[next]->node->node->...->node->[prev]->[prev]->[tail]
# etc.
list-compress-depth 0
 
# Sets have a special encoding in just one case: when a set is composed
# of just strings that happen to be integers in radix 10 in the range
# of 64 bit signed integers.
# The following configuration setting sets the limit in the size of the
# set in order to use this special memory saving encoding.
set-max-intset-entries 512
 
# Similarly to hashes and lists, sorted sets are also specially encoded in
# order to save a lot of space. This encoding is only used when the length and
# elements of a sorted set are below the following limits:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
 
# HyperLogLog sparse representation bytes limit. The limit includes the
# 16 bytes header. When an HyperLogLog using the sparse representation crosses
# this limit, it is converted into the dense representation.
#
# A value greater than 16000 is totally useless, since at that point the
# dense representation is more memory efficient.
#
# The suggested value is ~ 3000 in order to have the benefits of
# the space efficient encoding without slowing down too much PFADD,
# which is O(N) with the sparse encoding. The value can be raised to
# ~ 10000 when CPU is not a concern, but space is, and the data set is
# composed of many HyperLogLogs with cardinality in the 0 - 15000 range.
hll-sparse-max-bytes 3000
 
# Active rehashing uses 1 millisecond every 100 milliseconds of CPU time in
# order to help rehashing the main Redis hash table (the one mapping top-level
# keys to values). The hash table implementation Redis uses (see dict.c)
# performs a lazy rehashing: the more operation you run into a hash table
# that is rehashing, the more rehashing "steps" are performed, so if the
# server is idle the rehashing is never complete and some more memory is used
# by the hash table.
#
# The default is to use this millisecond 10 times every second in order to
# actively rehash the main dictionaries, freeing memory when possible.
#
# If unsure:
# use "activerehashing no" if you have hard latency requirements and it is
# not a good thing in your environment that Redis can reply from time to time
# to queries with 2 milliseconds delay.
#
# use "activerehashing yes" if you don't have such hard requirements but
# want to free memory asap when possible.
activerehashing yes
 
# The client output buffer limits can be used to force disconnection of clients
# that are not reading data from the server fast enough for some reason (a
# common reason is that a Pub/Sub client can't consume messages as fast as the
# publisher can produce them).
#
# The limit can be set differently for the three different classes of clients:
#
# normal -> normal clients including MONITOR clients
# slave  -> slave clients
# pubsub -> clients subscribed to at least one pubsub channel or pattern
#
# The syntax of every client-output-buffer-limit directive is the following:
#
# client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>
#
# A client is immediately disconnected once the hard limit is reached, or if
# the soft limit is reached and remains reached for the specified number of
# seconds (continuously).
# So for instance if the hard limit is 32 megabytes and the soft limit is
# 16 megabytes / 10 seconds, the client will get disconnected immediately
# if the size of the output buffers reach 32 megabytes, but will also get
# disconnected if the client reaches 16 megabytes and continuously overcomes
# the limit for 10 seconds.
#
# By default normal clients are not limited because they don't receive data
# without asking (in a push way), but just after a request, so only
# asynchronous clients may create a scenario where data is requested faster
# than it can read.
#
# Instead there is a default limit for pubsub and slave clients, since
# subscribers and slaves receive data in a push fashion.
#
# Both the hard or the soft limit can be disabled by setting them to zero.
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
 
# Redis calls an internal function to perform many background tasks, like
# closing connections of clients in timeout, purging expired keys that are
# never requested, and so forth.
#
# Not all tasks are performed with the same frequency, but Redis checks for
# tasks to perform according to the specified "hz" value.
#
# By default "hz" is set to 10. Raising the value will use more CPU when
# Redis is idle, but at the same time will make Redis more responsive when
# there are many keys expiring at the same time, and timeouts may be
# handled with more precision.
#
# The range is between 1 and 500, however a value over 100 is usually not
# a good idea. Most users should use the default of 10 and raise this up to
# 100 only in environments where very low latency is required.
hz 10
 
# When a child rewrites the AOF file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
aof-rewrite-incremental-fsync yes

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713172428.png" alt="image-20200713172427620" style="zoom:100%;" />





测试redis-cli连接上来

```
docker exec -it 运行着Rediis服务的容器ID redis-cli
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713172928.png" alt="image-20200713172926280" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713173424.png" alt="image-20200713173326783" style="zoom:100%;" />



# 八、本地镜像push

## 8.1 本地镜像发布到阿里云流程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713181209.png" alt="image-20200713181208054" style="zoom:100%;" />

##  镜像的生成方法

前面的DockerFile

从容器创建一个新的镜像
docker commit [OPTIONS] 容器ID [REPOSITORY[:TAG]]

> OPTIONS说明：
> -a :提交的镜像作者；
> -m :提交时的说明文字；

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713193531.png" alt="image-20200713181609750" style="zoom:100%;" />

## 8.2 将本地镜像推送到阿里云

访问 dev.aliyun.com

1. 本地镜像素材原型
2. 阿里云开发者平台 https://dev.aliyun.com/search.html



3. 创建仓库镜像 ，命名空间，仓库名称

4. 将镜像推送到 registry ，

5. 公有云可以查询到

6. 查看详情

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713182843.png" alt="image-20200713182701925" style="zoom:100%;" />



/zzyybuy是在阿里云上创建的镜像仓库名

```properties
3. 将镜像推送到Registry
$ sudo docker login --username=bluepopopo registry.cn-beijing.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-beijing.aliyuncs.com/zytowork/zytest:[镜像版本号]
$ sudo docker push registry.cn-beijing.aliyuncs.com/zytowork/zytest:[镜像版本号]properties
请根据实际镜像信息替换示例中的[ImageId]和[镜像版本号]参数。
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713183610.png" alt="image-20200713183609141" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713193346.png" alt="image-20200713183619498" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200713192847.png" alt="image-20200713192845898" style="zoom:100%;" />