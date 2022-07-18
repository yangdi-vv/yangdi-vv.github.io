---
title: docker 操作指令
categories:
- docker
tags: 
- docker
---

### docker是什么
> Docker是一个虚拟化平台（[官网](https://www.docker.com/)）可以通过虚拟化方式，为应用提供可运行的容器。基于这种方式，可更快地打包、测试以及部署应用程序。

- 镜像
> Docker 镜像是一个特殊的文件系统（[官网](https://hub.docker.com/)），镜像可以打包应用的运行环境以及应用程序，可以通过 Docker 启动这个镜像，进而将应用程序在一个容器中启动运行起来。在 Docker 镜像中，操作系统是高度精简的，镜像中的操作系统还不包含内核，容器都是共享所在的宿主机的内核。所以有时会说容器仅包含必要的操作系统（通常只有操作系统文件和文件系统对象），容器中查看到的 Linux 内核版本与宿主机一致。假如现在理解镜像有些抽象，可以暂时先将其理解为一个安装程序。

- 容器
> Docker容器可以将其理解为一个运行镜像的载体，镜像（Image）和容器（Container）的关系，就像是光盘和光驱。容器基于镜像创建、启动，然后运行镜像的中的文件。我们常常说使用镜像打包应用程序，使用 Docker 发布、部署应用程序，那么当你的应用成功在 Docker 上运行时，称这个应用就是容器化应用( containerized applications)。

### docker 安装
#### windows/mac环境下
> 建议直接下载docker Desktop， 图形化界面，方面使用

#### linux环境下
- 方式一
```shell
yum install -y yum-utils vim  // 安装基础工具
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo //添加软件源信息
yum -y install docker-ce   --allowerasing  //必须添加--allowerasing不然会报错，是因为centos8中的默认podman和docker冲突不能共存，要先卸载podman才能安装docker,centos7就不存在这个问题
systemctl enable docker --now  //启动docker并设置为开机自启
[root@localhost ~]# docker --version //查看docker版本 yum默认安装是最新版本
Docker version 20.10.12, build e91ed57

#因为docker默认是从国外服务器拉取镜像的，很慢的！！所以一定要配置镜像加速器!
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://x31x8hjg.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

- 方式二： 二进制包安装
```shell
#下载二进制安装包（因为我可以连接互联网，不能连接互联网的可以在自己电脑下载好在上传）
[root@localhost ~]# wget https://download.docker.com/linux/static/stable/x86_64/docker-20.10.9.tgz
#解压压缩包
[root@localhost ~]# tar -zxvf docker-20.10.9.tgz
[root@localhost ~]# ls
anaconda-ks.cfg  docker  docker-20.10.9.tgz
#将解压docker目录下的所有文件移动到/usr/bin下
[root@localhost ~]# mv docker/* /usr/bin
#以systemd的方式管理docker
cat > /usr/lib/systemd/system/docker.service << EOF
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target
 
[Service]
Type=notify
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s
 
[Install]
WantedBy=multi-user.target
EOF
 
#配置镜像加速器
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://x31x8hjg.mirror.aliyuncs.com"]
}
EOF
#启动docker并设置开机自启
systemctl daemon-reload
systemctl start docker
systemctl enable docker
#检查docker是否安装成功
[root@localhost ~]# docker run hello-world
 
Hello from Docker!
This message shows that your installation appears to be working correctly.
 
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
 
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
 
Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/
 
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
#直到出现以上才证明安装成功！！
```

### docker 基础操作指令

docker相关的命令：

- 查看docker容器版本
```shell
docker version
```

- 查看docker容器信息
```shell

```
> docker info

- 查看docker容器帮助
```shell
docker -help
```

- 列出本地images
```shell
docker images
```

- 含中间映像层
```shell
docker images -a
```

- 只显示镜像ID
- ```shell
docker images -q
```

- 含中间映像层
```shell
Docker images -qa
```

- 显示镜像摘要信息(DIGEST列)
```shell
docker images --digests
```

- 显示镜像完整信息
```shell
docker images --no-trunc
```

- 显示指定镜像的历史创建；参数：-H 镜像大小和日期，默认为true；--no-trunc 显示完整的提交记录；-q 仅列出提交记录ID
```shell
docker history -H redis
```

- 搜索仓库MySQL镜像
```shell
docker search mysql
```

-  --filter=stars=600：只显示 starts>=600 的镜像
```shell
docker search --filter=stars=600 mysql
```

-  --no-trunc 显示镜像完整 DESCRIPTION 描述
```shell
docker search --no-trunc mysql
```

-  --automated ：只列出 AUTOMATED=OK 的镜像
```shell
docker search  --automated mysql
```

- 下载Redis官方最新镜像，相当于：docker pull redis:latest
```shell
docker pull redis
```

- 下载仓库所有Redis镜像
```shell
Docker pull -a redis
```

- 下载私人仓库镜像
```shell
docker pull bitnami/redis
```

- 单个镜像删除，相当于：docker rmi redis:latest
```shell
docker rmi redis
docker rmi 镜像ID
```

- 强制删除(针对基于镜像有运行的容器进程)
```shell
docker rmi -f redis
```

- 多个镜像删除，不同镜像间以空格间隔
```shell
docker rmi -f redis tomcat nginx
```

- 删除本地全部镜像
```shell
docker rmi -f $(docker images -q)
```

- 新建并启动容器，参数：-i 以交互模式运行容器；
```shell
-t ##为容器重新分配一个伪输入终端；
--name 为容器指定一个名称
docker run -i -t --name mycentos
```

- 后台启动容器，参数：-d 已守护方式启动容器
```shell
docker run -d mycentos
```
> 注意：此时使用"docker ps -a"会发现容器已经退出。这是docker的机制：要使Docker容器后台运行，就必须有一个前台进程。解决方案：将你要运行的程序以前台进程的形式运行。

- 启动一个或多个已经被停止的容器
```shell
docker start redis
```

- 重启容器
```shell
docker restart redis
docker restart 容器ID
```

- top支持 ps 命令参数，
> 格式：docker top [OPTIONS] CONTAINER [ps OPTIONS]

- 列出redis容器中运行进程
```shell
docker top redis #
```

- 查看所有运行容器的进程信息
```shell
for i in `docker ps |grep Up|awk '{print $1}'`;do echo \ &&docker top $i; done
```


- 查看redis容器日志，默认参数
```shell
docker logs rabbitmq
```

- 查看redis容器日志，参数：
> -f 跟踪日志输出；

> -t 显示时间戳；

> --tail 仅列出最新N条容器日志；

> docker logs -f -t --tail=20 redis

- 查看容器redis从2019年05月21日后的最新10条日志。
```shell
docker logs --since="2019-05-21" --tail=10 redis
```

- 使用run方式在创建时进入
```shell
docker run -it centos /bin/bash
```

- 关闭容器并退出
```shell
exit
```

- 直接进入centos 容器启动命令的终端，不会启动新进程，多个attach连接共享容器屏幕，参数：--sig-proxy=false 确保CTRL-D或CTRL-C不会关闭容器
```shell
docker attach --sig-proxy=false centos
```

- 在 centos 容器中打开新的交互模式终端，可以启动新进程，参数：
> -i 即使没有附加也保持STDIN 打开；

> -t 分配一个伪终端

> docker exec -i -t centos /bin/bash

- 以交互模式在容器中执行命令，结果返回到当前终端屏幕
```shell
docker _exec_ -i -t centos ls -l /tmp
```

- 以分离模式在容器中执行命令，程序后台运行，结果不会反馈到当前终端 
```shell
docker exec -d centos touch cache.txt
```

- 查看正在运行的容器
```shell
docker ps
```

- 查看正在运行的容器的ID
```shell
docker ps -q
```

- 查看正在运行+历史运行过的容器
```shell
docker ps -a
```

- 显示运行容器总文件大小
```shell
docker ps -s
```

- 显示最近创建容器
```shell
docker ps -l
```

- 显示最近创建的3个容器
```shell
docker ps -n 3
```

- 不截断输出
```shell
docker ps --no-trunc
```

- 获取镜像redis的元信息
```shell
docker inspect redis
```

- 获取正在运行的容器redis的 IP 
```shell
docker inspect --format="{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}" redis
```
- 停止一个运行中的容器
```shell
docker stop redis
```

- 杀掉一个运行中的容器
```shell
docker kill redis
```

- 删除一个已停止的容器
```shell
docker rm redis
docker rm 容器ID
```

- 删除一个运行中的容器
```shell
docker rm -f containerID
```

- 删除多个容器
```shell
docker rm -f $(docker ps -a -q) docker ps -a -q | xargs docker rm
```

-  -l 移除容器间的网络连接，连接名为 db
```shell
docker rm -l db
```

-  -v 删除容器，并删除容器挂载的数据卷
```shell
docker rm -v redis
```

- 基于当前redis容器创建一个新的镜像；参数：
> -a 提交的镜像作者；

> -c 使用Dockerfile指令来创建镜像；

> -m :提交时的说明文字；

> -p :在commit时，将容器暂停

> docker commit -a="DeepInThought" -m="my redis" [容器ID] myredis:v1.1

- 将容器中的文件copy至本地路径
```shell
docker cp 容器ID:/[container_path] [local_path]
```

- 将主机文件copy至容器目录
```shell
docker cp [local_path] 容器ID:/[container_path]/
```

- 将主机文件copy至容器，目录重命名为[container_path](注意与非重命名copy的区别)
```shell
docker cp [local_path] 容器ID:/[container_path]
```