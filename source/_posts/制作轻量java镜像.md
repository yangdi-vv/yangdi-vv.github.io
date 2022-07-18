---
title: 制作轻量JAVA镜像
categories:
- java
- docker
tags: 
- java
---

### Dockerfile
> 标准版JAVA制作的基础镜像一般达到600MB以上，如无太多的要求可直接使用openjdk的jre版本
```shell
FROM openjdk:8-jre-alpine
```

### 制作镜像
> Dockerfile 文件夹下执行命令行
```shell
docker build -t dockerName .
```