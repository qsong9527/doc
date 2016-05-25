# Docker使用入门
## 1. Docker简介
### 1.1 什么是Docker
<p><b>Docker 是一个开源的应用容器引擎。</b><br/>
让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上（目前Docker已经退出基于windows与Mac的Beta版本）。即将服务于服务所需的依赖打包，便于维护、复制与移植。</p>

<p><b>Docker 是一个开源的应用容器引擎。</b><br/>
让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上（目前Docker已经退出基于windows与Mac的Beta版本）。即将服务于服务所需的依赖打包，便于维护、复制与移植。</p>

<p><font color="red"><b>最后，如果上面的实在不原理理解，你可以理解Docker为一个用了新颖方式实现的超轻量级虚拟机</b></font><br/>

### 1.2 为啥需要容器？
要知道为啥需要容器，那么就应该先知道应用容器长什么样子。一个做好的应用容器长得就好像一个装好了一组特定应用的虚拟机一样。比如我现在想用MySQL那我就找个装好MySQL的容器，运行起来，那么我就可以使用 MySQL了。

为什么不直接装个 MySQL不就好了，何必还需要这个容器这么诡异的概念？话是这么说，可是你要真装MySQL的话可能要再装一堆依赖库，根据你的操作系统平台和版本进行设置，有时候还要从源代码编译报出一堆莫名其妙的错误，可不是这么好装。而且万一你机器挂了，所有的东西都要重新来，可能还要把配置在重新弄一遍。但是有了容器，你就相当于有了一个可以运行起来的虚拟机，只要你能运行容器，MySQL的配置就全省了。而且一旦你想换台机器，直接把这个容器端起来，再放到另一个机器就好了。硬件，操作系统，运行环境什么的都不需要考虑了。

### 1.3 Docker能带来什么好处
一个很大的好处就是可以保证线下的开发环境、测试环境和线上的生产环境一致。利用容器可以实现开发的时候直接在容器里开发，提测的时候把整个容器当前的镜像提交给测试，测试好了后开发在当前镜像的基础上进行Bug修复，直到测试通过后，在将经过测试的容器直接上线就好了。这样，通过容器，整个开发、测试和生产环境可以保持高度的一致。

### 1.4 为啥不直接用VM
那么既然容器和 VM 这么类似为啥不直接用 VM 还要整出个容器这么个概念来呢？Docker 容器相对于 VM 有以下几个优点：

* 启动速度快，容器通常在一秒内可以启动，而VM通常要更久
* 资源利用率高，一台普通 PC 可以跑上千个容器，你跑上千个 VM 试试
* 性能开销小， VM 通常需要额外的 CPU 和内存来完成 OS 的功能，这一部分占据了额外的资源

为啥相似的功能在性能上会有如此巨大的差距呢，其实这和他们的设计的理念是相关的。 VM 的设计图如下：<br/>
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/vm_struct.jpg)

VM 的 Hypervisor 需要实现对硬件的虚拟化，并且还要搭载自己的操作系统，自然在启动速度和资源利用率以及性能上有比较大的开销。而 Docker 的设计图是这样的： <br/>
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/docker_struct.jpg)

Docker 几乎就没有什么虚拟化的东西，并且直接复用了 Host 主机的 OS，在 Docker Engine 层面实现了调度和隔离重量一下子就降低了好几个档次。

### 1.9 Docker相关网络资料整理
[1. 史上最全的Docker资料集萃](http://special.csdncms.csdn.net/BeDocker/)

## 2. Docker安装与配置
### 2.1 Docker安装
#### 2.1.1 基于linux的Docker安装
<hr/>
#### 2.1.1.1Docker 安装的系统需求
安装linux版Docker，系统必须满足如下两个条件：

* <font color="red">必须是64-bit系统
* 系统内容必须是3.10及以上版本</font>

Linux系统内核获取：

```
$uname -a
输出：
Linux xxxxx.hostname 3.10.0-123.el7.x86_64 #1 SMP Mon Jun 30 12:09:22 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux`
```

其中可以看出该主机的系统内核信息为3.10.0-123.el7.x86_64，即为3.10.0的64bit系统版本，符合docker安装系统要求。
#### 2.1.2.2 安装docker
安装docker-io

```
$yum -y install docker-io
```

升级docker-io

```
$yum -y update docker-io
```

启动docker进行

```
$service docker start
```

将docker设置为开启启动

```
$chkconfig docker on
```


## 3. Docker基本使用

### 3.1 下载一个基础容器，修改并保存为新的容器

#### 3.1.1 下载一个预建立的镜像
在官方的Docker Hub上下载一个与预建立的ubuntu镜像

```
$docker pull ubuntu
```

下载完成后，可以通过如下命令查看本地docker镜像列表

```
$docker images
```

#### 3.1.2 启动下载镜像
通过docker run命令运行一个临时的本地的image的交互shell

```
#通过run -i -t 启动一个镜像的伪终端
$docker run -i -t ubuntu /bin/bash
#使用CTRL-p + CTRL-q 退出伪终端
```

通过docker run 命令运行一个后台image,并长执行一个工作进程

```
#开启一个Docker后端进程，并将容器ID赋值给JOB变量
$JOB=$(sudo docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done") 
```

通过docker ps命令查看当前后端运行的容器

```
$docker ps
```

通过docker logs命令查看运行容器的日志

```
#logs $job_id, job_id可以通过ps命令获取
$docker logs $JOB
```

通过docker kill命令终止容器运行

```
$docker kill $JOB
```

#### 3.1.3 提交保存的容器的状态

docker可以将本地运行容器的变更进行镜像保存，当保存容器的时候，仅仅只是保存现在容器和创建容器时候的不同状态（as a diff）。

```
#commit 仅针对与运行中的镜像，即容器进行diff保存，保存结果为另一个新的镜像
$docker commit <container_id> <new_image_name>
```