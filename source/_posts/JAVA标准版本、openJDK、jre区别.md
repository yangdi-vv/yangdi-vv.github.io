---
title: JDK 和 openjdk的区别
categories:
- docker
- java
tags:
- java
---

### Java SE 标准版本
> Oracle JDK Oracle JDK 由 Oracle 公司开发，该公司是 Sun 许可证，基于 Java 标准版规范实现。它以二进制产品的形式发布。它支持多种操作系统，如 Windows，Linux，Solaris，MacOS 等。它支持不同的平台，如 Intel 32 位和 64 位架构，ARM 架构和 SPARC。它完全基于 Java 编程语言。之后，该许可证宣布将根据 GPL（通用公共许可证）许可证发布。Oracle JDK 包含许多组件作为库形式的编程工具集合。


### openjdk
> 在JAVA SE基础上进行精简的版本，删除了JDK的一些未开源的部分，openjdk 的社区比 JDK的社区要活跃

### 区别
- 授权协议的不同
> openjdk采用GPL V2协议放出，而JDK则采用JRL放出。两者协议虽然都是开放源代码的，但是在使用上的不同在于GPL V2允许在商业上使用，而JRL只允许个人研究使用。

- OpenJDK不包含Deployment（部署）功能
> 部署的功能包括：Browser Plugin、Java Web Start、以及Java控制面板，这些功能在Openjdk中是找不到的。

- OpenJDK源代码不完整
> 这个很容易想到，在采用GPL协议的Openjdk中，sun jdk的一部分源代码因为产权的问题无法开放openjdk使用，其中最主要的部份就是JMX中的可选元件SNMP部份的代码。因此这些不能开放的源代码将它作成plug，以供OpenJDK编译时使用，你也可以选择不要使用plug。而Icedtea则为这些不完整的部分开发了相同功能的源代码(OpenJDK6)，促使OpenJDK更加完整。

- 部分源代码用开源代码替换
> 由于产权的问题，很多产权不是SUN的源代码被替换成一些功能相同的开源代码，比如说字体栅格化引擎，使用Free Type代替。

- openjdk只包含最精简的JDK
> OpenJDK不包含其他的软件包，比如Rhino Java DB JAXP……，并且可以分离的软件包也都是尽量的分离，但是这大多数都是自由软件，你可以自己下载加入。

- 不能使用Java商标
> 这个很容易理解，在安装openjdk的机器上，输入“java -version”显示的是openjdk，但是如果是使用Icedtea补丁的openjdk，显示的是java
