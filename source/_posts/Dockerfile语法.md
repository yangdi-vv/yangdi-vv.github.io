---
title: Dockerfile 基本语法
categories:
- docker
- DockerFile
tags:
- Dockerfile
---

## 指令
- FROM
> 指定一个镜像作为构建自定义镜像的基础，构建的镜像在此基础上进行修改，例如
```shell
FROM java:8
FROM apline
...
```

- MAINTAINET
> 指定维护者信息，当前已更新换为使用LABEL
```shell
LABEL maintainer="name" version="1.0"
```

- EXPOSE 
> 告诉 Docker 服务，容器需要暴露的端口号
```shell
EXPOSE <port1> [<port2> ...]
```
> - 帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；
> - 在使用随机端口映射时，也就是 docker run -P （大写的 P）时，会自动随机映射 EXPOSE 的端口（而 -p 参数指定具体的映射端口）

- RUN
> 为构建的镜像指定要运行的命令行命令，而这些命令是在 docker build 的时候执行的
```shell

RUN <command>

RUN ["executable", "param1", "param2", ...]

```
> - shell 格式将在终端中运行命令，command 则为终端操作的 shell 命令；
> - exec 格式使用 exec 命令执行，可以指定其他终端，例如：RUN ["/bin/bash", "-c", "echo hello"]；
> - 每条 RUN 指令将在当前镜像的基础上执行指定命令，并提交为新的镜像，而 Dockerfile 的指令每执行一次都会在 docker 上新建一层，过多无意义的层，会造成镜像膨胀过大，所以建议将多个命令合并到同一个 RUN 指令上



- ENV
> 设置自定义的环境变量，容器中可使用
```shell
ENV env1 val1
ENV env1=val1 env2=val2

RUN echo $env1
```

- ADD
> 上下文目录或者文件复制到容器的指定目录，类似于COPY
```shell
ADD ${source} ${target}
```
> - 在执行 <源文件> 为 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，会自动复制并解压到 <目标路径>
> - 在不解压的前提下，无法复制 tar 压缩文件，会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢

- COPY
> 上下文目录或者文件复制到容器的指定目录，类似于ADD
```shell
COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
```
> - [--chown=<user>:<group>] 可选参数，用于改变复制到容器内文件的拥有者和所属组
> - 源文件或目录（以 Dockerfile 的上下文环境为相对路径），可以使用通配符，但通配符要满足 Go 的 filepath.Match 规则
> - 容器内的路径，如果路径不存在的话，会自动创建

- CMD
> 为启动的容器指定要运行的命令，类似于 RUN 指令，但 CMD 运行程序的时间是在 docker run 时执行的，命令运行结束，容器也就结束
```shell
# 1、shell 命令格式
CMD <shell command>

# 2、exec 命令格式：推荐 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 

# 3、该写法是为 ENTRYPOINT 指令指定的程序提供默认参数
CMD ["<param1>","<param2>",...]  
```
> - 每个 Dockerfile 只能有一条 CMD 命令，如果指定了多条 CMD 命令，只有最后一条会被执行；
> - 如果 docker run 命令行参数中指定了要运行的程序命令，则会覆盖CMD 指令指定的程序命令。

- ENTRYPOINT
> 指定容器启动时执行的命令
```
FROM nginx

ENTRYPOINT ["nginx", "-c"]    # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参 

```
> - 即使 docker run 的命令行参数指定了要运行的程序命令，ENTRYPOINT 指令的程序命令也不会被覆盖，并且这些命令行参数会被当作参数传送给 ENTRYPOINT 指令指定的程序
> - 但是，如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 ENTRYPOINT 指令指定的程序


- VOLUME
> 定义挂载匿名数据卷目录。在启动容器时如果忘记挂载数据卷，会自动挂载到匿名卷
```shell
VOLUME <path>
VOLUME ["<path1>", "<path2>", ...]
```
> 在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点


- USER
>

- WORKDIR
> 指定工作目录。用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在（WORKDIR 指定的工作目录，必须是提前创建好的）
> 构建镜像过程中的，每一个 RUN 命令都是新建的一层，只有通过 WORKDIR 创建的目录才会一直存在
```shell
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
### 最终路径为：/a/b/c
```


- ONBUILD
> 创建的镜像作为其他镜像的基础镜像时使用的指令
```shell
ONBUILD [INSTRUCTION]

示例 name: my-node
...
FROM node:slim
RUN mkdir /app
WORKDIR /app
ONBUILD COPY ./package.json /app
ONBUILD RUN [ "npm", "install" ]
ONBUILD COPY . /app/
CMD [ "npm", "start" ]

...

使用 name: use-node
...
FROM my-node
相当于
COPY ./package.json /app
RUN [ "npm", "install" ]
COPY . /app/
CMD [ "npm", "start" ]
...

```

- HEALTHCHECK
> 指定某个程序或者指令来监控 docker 容器服务的运行状态
```shell
# 设置检查容器健康状况的命令
HEALTHCHECK [选项] CMD <命令>

# 如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令
HEALTHCHECK NONE

# 这边 CMD 后面跟随的命令使用，可以参考 CMD 的用法
HEALTHCHECK [选项] CMD <命令>

```

## Dockerfile编写优化
- .dockerignore 文件
> 可以忽略一些不需要的文件，加快构建速度，减小镜像大小

- 多个Run 合并成一个， 同样变化频率的指令也放在一起
```shell
FROM ubuntu

RUN apt-get update && apt-get install -y nodejs  
ADD . /app  
RUN cd /app && npm install

CMD npm start
```

- 基础镜像不要使用latest
> latest不同时间可能指向不同的镜像，最后使用确定的版本或者没有其它疑虑的情况下使用latest

- RUN后删除不必要的文件
> 多余的压缩包和APK的安装包使用之后可以删除，减少镜像体积

- 选择合适的基础镜像
> 最好的还是使用apline,没有必要选择标准版本的ubuntu
```shell
FROM ubuntu
FROM node:7-alpine
```
> - alpine 是一个极小化的 Linux 发行版，非常适合作为基础镜像；
> - apk 是 Alpine 的包管理工具，它与 apt-get 有些不同，但是非常容易上手。另外，它还有一些非常有用的特性，比如 no-cache 和 --virtual 选项，它们都可以帮助我们减少镜像的大小；

- ENTRYPOINT
> ENTRYPOINT 指令并不是必须的，因为它会增加复杂度。ENTRYPOINT 是一个脚本，它会默认执行，并且将指定的命令作为其参数，它通常用于构建可执行的 Docker 镜像
```shell
指定脚本
ENTRYPOINT ["./entrypoint.sh"] 
执行
docker run -it demo /bin/bash
```

- 合理调整 COPY 与 RUN 的顺序
> 我们应该把变化最少的部分放在 Dockerfile 的前面，这样可以充分利用镜像缓存

- 设置默认的环境变量、映射端口和数据卷
```shell
# 设置环境变量
ENV MEDIA_DIR=/media \  
    APP_PORT=3000
# 挂载数据卷
VOLUME $MEDIA_DIR 
# 暴露映射端口
EXPOSE $APP_PORT
```
- 使用 LABEL 设置镜像元数据
> 使用 LABEL 指令，可以为镜像设置元数据，例如镜像创建者或者镜像说明。旧版的 Dockerfile 语法使用 MAINTAINER 指令指定镜像创建者，但是它已经被弃用了：
