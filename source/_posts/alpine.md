---
title: alpine 是什么
categories:
- docker
- linux
tags:
- linux
---

## alpine是什么

> 我们使用Dockerfile构建镜像时，通常镜像比较庞大，想要减小体积就需要一款比较精简的linux版本，alpine就是精简版的linux发新版本


## 特点
- 小巧： 基于Musl linc和busybox, 与busybox一样小巧，最小的docker镜像只有5Mb
- 安全： 面向安全的轻量发行版本
- 简单： 使用APK安装包管理工具
- 适合容器使用： 适合作为容器的基础镜像

## APK包管理工具
- apk update
> 更新镜像源列表

- apk add
> - apk add name 添加安装包
> - apk add name --no-cache 不使用本地镜像源缓存 添加安装包

- apk info
> apk info name 查询安装包信息

- apk upgrade
> - apk upgrade 升级所有包
> - apk upgrade name 升级指定包

- apk del
> apk del name 删除指定包
