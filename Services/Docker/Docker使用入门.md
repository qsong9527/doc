# Docker使用入门
@author:jy.zenist.song, crate:2016-5-25, LastEdited:2016-5-25

## 1. Docker安装
### 1.1 基于linux的Docker安装
#### 1.1.1 Docker安装的系统需求
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
#### 1.1.2 安装docker
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

<hr/>

## 2. Docker基本使用

### 2.1 下载一个预建立的镜像
在官方的Docker Hub上下载一个与预建立的ubuntu镜像

```
$docker pull ubuntu
```

下载完成后，可以通过如下命令查看本地docker镜像列表

```
$docker images
```

### 2.2 启动下载镜像
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

### 2.3 提交保存的容器的状态

docker可以将本地运行容器的变更进行镜像保存，当保存容器的时候，仅仅只是保存现在容器和创建容器时候的不同状态（as a diff）。

```
#commit 仅针对与运行中的镜像，即容器进行diff保存，保存结果为另一个新的镜像
$docker commit <container_id> <new_image_name>
```