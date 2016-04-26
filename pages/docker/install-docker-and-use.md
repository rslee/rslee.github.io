# docker 使用和安装

## 前言

docker是基于linux的lxc(linux 容器)技术发展过来的新型容器技术，特点是比VM轻，资源消耗少，而且是标准化的镜像，很方便就可以传输和发布到其他的机器上面，是现在paas和devops有力助手。目前软件开发渐渐开始往分布式方面发展，需要的机器是越来越多了。但是本人穷啊，穷则思变，所以自然就开始打起了docker的注意来。希望可以用docker完成如下几个目标:

1. 基于nginx+jetty+redis的高可用集成方案
2. 基于mysql的主备+1写3读方案
3. 基于hadoop的离线计算方案
4. 基于jstorm的实时计算方案
5. 基于spark的计算方案

安装环境:ubuntu 14.04 LTS

## docker安装

由于国情的原因，直接到docker的官网上面下载安装docker耗时长且不稳定。这里采用的是国内的paas提供商daocloud进行安装。使用前请先注册个账号

具体的安装步骤如下：

首先是切换到root账号
```
sudo su

curl -sSL https://get.daocloud.io/docker | sh
```
后面会自动执行安装过程。没有异常的话会提示类似如下的docker环境信息:
```
Reading package lists...
Building dependency tree...
Reading state information...
docker-engine is already the newest version.
0 upgraded, 0 newly installed, 0 to remove and 314 not upgraded.
+ sh -c docker version
Client:
 Version:      1.11.0
 API version:  1.23
 Go version:   go1.5.4
 Git commit:   4dc5990
 Built:        Wed Apr 13 18:34:23 2016
 OS/Arch:      linux/amd64

Server:
 Version:      1.11.0
 API version:  1.23
 Go version:   go1.5.4
 Git commit:   4dc5990
 Built:        Wed Apr 13 18:34:23 2016
 OS/Arch:      linux/amd64

If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!
```

检查docker服务有没有启动:
```
service docker status
```
我这边的输出是:
```
docker start/running, process 1527
```

## docker基本使用方法

安装好docker以后就可以开始进行相关的操作了。docker里面有几个概念要介绍下：
镜像：软件运行的基础环境，不过是只读的，只用于创建容器使用。
容器：软件实际运行的环境，容器通过镜像进行创建和运行，可以挂载外部的文件夹。通过进程和网络隔离，每个容器里面运行的软件不受外面的环境干扰。
私服：保存镜像的私有服务器

### 下载镜像

官方的下载镜像命令是：`docker pull 镜像名称`，考虑国情的原因，下载的命令存在变化：

首先是安装下载镜像的工具
```
curl -sSL https://get.daocloud.io/daomonit/install.sh | sh -s ffd82b06ef8c488fe8ebfbbc6fb25f0e7e1c76d7

```
注意 `ffd82b06ef8c488fe8ebfbbc6fb25f0e7e1c76d7` 这一段是网站随机分配的，和当前登陆的用户有关，请使用你自己的配置。
具体的页面可以到这里看看
[接入自有主机](https://dashboard.daocloud.io/nodes/new)

然后是下载镜像
```
dao pull ubuntu:15.04
```

下载完成后，执行如下命令可以查看已有的镜像
```
docker images
```

### 基于镜像创建容器

有了镜像以后，下一步就是在镜像的基础上面运行一个容器了。你可以执行如下命令：
```
docker run -it ubuntu:15.04 /bin/bash
```
然后你就可以看到类似如下的信息：
```
root@621bbe4bbee0:/# 
```
现在大家可以直接运行ps命令看一下，发现docker的容器的话，确实只运行了指定的程序，比较节省资源。

```
  PID TTY          TIME CMD
    1 ?        00:00:00 bash
   10 ?        00:00:00 ps
```

由于是新的镜像，里面没有我们需要的一些软件，我们先把基础的软件都安装一下。

首先是vim
```
apt install vim
```
由于没有更改apt的源，安装的过程可能会有点久，这个得耐心等待下。

安装好vim以后，就可以编辑apt的源：
```
vi /etc/apt/source.list
```

在http所在的域名前面加上`.cn`，切换到我天朝的域名。我这边基于ubuntu15.04版本的配置如下：
```

```
搞完执行下`apt update`命令，看是不是快多了？

这一步搞完了以后，执行`exit`命令就可以退出当前运行的容器

### 重新运行容器



### 提交更改到镜像

### 挂载本地磁盘/虚拟磁盘到容器

### 创建镜像指定虚拟的ip和端口

## 基于nginx+jetty+redis的高可用集成方案
