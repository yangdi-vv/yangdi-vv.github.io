---
title: MAC 下 jmeter安装使用
categories: 
- Jmeter
- Java
Tag:
- Jmeter
---

## 介绍
> JMeter最初设计用于web应用测试，后来扩展到其他领域，可用于测试静态和动态资源，也可灵活的使用JMeter断言进行测试。

## 安装
- 需先安装JAVA
> 下载地址 https://www.macv.com/mac/2515.html

- JAVA环境变量配置
```shell
export JAVA_8_HOME=/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
export JAVA_HOME=$JAVA_8_HOME
export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

- 安装jmeter
> https://jmeter.apache.org/download_jmeter.cgi
![安装包](http://101.34.242.163/img_blog/jmeter.png)

- 复制以下文件到文件夹/lib
[ApacheJMeter_ssh-1.2.0](http://101.34.242.163/img_blog/ApacheJMeter_ssh-1.2.0.jar)
[jsch-0.1.55](http://101.34.242.163/img_blog/jsch-0.1.55.jar)

## 使用
- 线程组
![](http://101.34.242.163/img_blog/jmeter1.png)


- 创建HTTP请求
![](http://101.34.242.163/img_blog/jmeter2.png)


- 创建结果树
![](http://101.34.242.163/img_blog/jmeter3.png)

- 正则表达式提取
> 可利用正则表达式提取cookie等数据

![](http://101.34.242.163/img_blog/jmeter4.png)