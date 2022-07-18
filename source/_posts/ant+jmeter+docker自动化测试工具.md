---
title: 制作自动化测试镜像（ant+jmeter+docker）
categories:
- docker
tags:
- docker
---

### 一： 安装 docker
- 1、MAC、windows 建议安装 docker desktop,便于管理和操作docker
> 下载地址 [docker desktop](https://www.docker.com/products/docker-desktop/)

- 2、Linux 安装
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

### 下载安装包
- 1：jmeter安装包
> 下载地址 [docker desktop](https://jmeter.apache.org/download_jmeter.cgi) tgz格式安装包

- 1：ant安装包
> 下载地址 [docker desktop](https://ant.apache.org/bindownload.cgi) tar格式安装包

### 准备Dockerfile
> ant 支持邮件发送及jmeter支持shell命令均需要插件支持，部分jmeter文件夹下jar插件也需要复制到ant 文件夹下，如下所示
```shell
FROM openjdk:8-jre-alpine

ENV http_proxy ""
ENV https_proxy ""

# workdir
RUN mkdir /jmeterdocker
RUN mkdir -p /jmeterdocker/test
RUN mkdir -p /jmeterdocker/jar
RUN mkdir -p /jmeterdocker/xsl
RUN mkdir -p /jmeterdocker/bin
RUN mkdir -p /jmeterdocker/packages
RUN mkdir -p /jmeterdocker/test/input/jmx
RUN mkdir -p /jmeterdocker/test/input/testdata
RUN mkdir -p /jmeterdocker/test/report/html
RUN mkdir -p /jmeterdocker/test/report/jtl
RUN mkdir -p /jmeterdocker/test/report/outputdata

# env
ENV ANT_HOME=/jmeterdocker/apache-ant-1.9.16
ENV PATH=${PATH}:${ANT_HOME}/bin
ENV JMETER_HOME=/jmeterdocker/apache-jmeter-5.5
ENV JMETER_PATH=${JMETER_HOME}/bin:${PATH}
ENV PATH=${JMETER_HOME}/bin:${PATH}

# dependences
COPY /packages/ /jmeterdocker/packages/
COPY /jar/ /jmeterdocker/jar/
COPY /xsl/ /jmeterdocker/xsl/
COPY /build.xml /jmeterdocker/test
COPY /bin/ /jmeterdocker/bin

# decompression
RUN cd /jmeterdocker  \
    && tar zxvf packages/apache-jmeter-5.5.tgz  \
    && tar zxvf packages/apache-ant-1.9.16-bin.tar.gz \
    && cp jar/*.jar /jmeterdocker/apache-ant-1.9.16/lib \
    && cp jar/ext/* /jmeterdocker/apache-jmeter-5.5/lib/ext \
    && cp jar/lib/* /jmeterdocker/apache-jmeter-5.5/lib \
    && cp xsl/* /jmeterdocker/apache-jmeter-5.5/extras \
    && cp bin/* /jmeterdocker/apache-jmeter-5.5/bin \
    && rm -rf packages \
    && rm -rf jar
```

### ant build.xml配置
> 以下xml文件为以jmeter-results-detail-report_30.xsl为基础优化修改而来，添加详细接口请求及返回信息
```shell
<?xml version="1.0" encoding="UTF8"?>
<project name="ant-jmeter" default="all" basedir=".">
    <!--properties-->
    <property name="jmeterPath" value="/jmeterdocker/apache-jmeter-5.5"/>
    <property name="ant.home" value="/jmeterdocker/apache-ant-1.9.16"/>
    <property name="mail_host" value="smtp.163.com"/>
    <property name="mail_port" value="25"/>
    <property name="username" value="18705511619@163.com"/>
    <property name="password" value="TRPVPJSZTCXIXRUQ"/>
    <property environment="env"/>
    <property name="mail_from" value="18705511619@163.com"/>
    <property name="mail_to" value="${env.JMETER_EMAIL}"/>
    <property name="mailsubject" value="jmeter"/>
    <property name="message" value="AI平台测试报告"/>

    <!--timestamp-->
    <tstamp><format property="time" pattern="yyyyMMddhhmm" /></tstamp>

    <!-- jmeter环境配置 -->
    <property name="jmeter.home" value="${jmeterPath}"/>

    <!-- jtl测试结果存放路径 -->
    <property name="jmeter.result.jtl.dir" value="/jmeterdocker/test/report/jtl"/>

    <!-- html测试结果存放路径 -->
    <property name="jmeter.result.html.dir" value="/jmeterdocker/test/report/html"/>
    <property name="htmlReportNameSummary" value="TestReport"/>
    <property name="jmeter.result.jtlName" value="${jmeter.result.jtl.dir}/${htmlReportNameSummary}${time}.jtl"/>
    <property name="jmeter.result.htmlName" value="${jmeter.result.html.dir}/${htmlReportNameSummary}${time}.html"/>

    <!--run order-->
    <target name="all">
        <antcall target="clear"/>
        <antcall target="test"/>
        <antcall target="report"/>
        <antcall target="sendEmail"/>
    </target>

    <!--删除历史文件-->
    <target name="clear">
        <!-- 每次执行前先删除清空jtl和html文件夹，释放空间 -->
        <echo message="释放空间,清空jtl和html文件夹中 ..."></echo>
        <delete dir="${meter.result.jtl.dir}" />
        <delete dir="${jmeter.result.html.dir}" />
        <!-- 删除单个文件-->
        <!--<delete file="C:\Users\Administrator\Desktop\ant\ant_jmeter_test_report.rar" />-->
        <sleep seconds="2"/>
        <echo message="释放完成"></echo>
    </target>

    <!-- 执行接口测试 -->
    <target name="test">
        <echo message="执行接口自动化测试用例"></echo>
        <taskdef name="jmeter" classname="org.programmerplanet.ant.taskdefs.jmeter.JMeterTask" />
        <jmeter jmeterhome="${jmeter.home}" resultlog="${jmeter.result.jtlName}">
            <testplans dir="/jmeterdocker/test/input/jmx" includes="*/*.jmx" />
            <property name="jmeter.save.saveservice.output_format" value="xml" />
        </jmeter>
    </target>

    <!-- 转换执行结果-->
    <path id="xslt.classpath">
        <fileset dir="${jmeter.home}/lib" includes="xalan*.jar" />
        <fileset dir="${jmeter.home}/lib" includes="serializer*.jar" />
    </path>
    <target name="report">
        <echo message="生成接口自动化测试报告"></echo>
        <tstamp> <format property="report.datestamp" pattern="yyyy/MM/dd HH:mm" /></tstamp>
        <xslt classpathref="xslt.classpath"
              force="true"
              in="${jmeter.result.jtlName}"
              out="${jmeter.result.htmlName}"
              style="${jmeter.home}/extras/jmeter-results-detail-report_30.xsl">
            <param name="titleReport" expression="${mailsubject}${time}"/>
            <param name="dateReport" expression="${report.datestamp}"/>
        </xslt>

        <!-- 因为上面生成报告的时候，不会将相关的图片也一起拷贝至目标目录，所以，需要手动拷贝 -->
        <copy file="${jmeter.home}/extras/expand.png" tofile="${jmeter.result.html.dir}/expand.png" />
        <copy file="${jmeter.home}/extras/collapse.png" tofile="${jmeter.result.html.dir}/collapse.png" />
    </target>

    <!--获取报告HTML贴到邮件正文  发送邮件-->
    <target name="sendEmail">
        <!--获取HTML-->
        <echo message="加载文件"></echo>
        <loadfile property="html" srcFile="${jmeter.result.html.dir}/${htmlReportNameSummary}${time}.html" encoding="UTF-8">
            <filterchain>
                <expandproperties />
            </filterchain>
        </loadfile>

        <!--邮件配置-->
        <path id="lib_classpath">
            <fileset dir="${ant.home}/lib">
                <include name="mail*.jar" />
                <include name="activation*.jar" />
                <include name="commons-email*.jar" />
                <include name="ant-contrib*.jar" />
            </fileset>
        </path>

        <echo>发送测试报告</echo>
        <mail mailhost="${mail_host}"
              ssl="false"
              user="${username}"
              password="${password}"
              mailport="${mail_port}"
              subject="${mailsubject}"
              messagemimetype="text/html"
              tolist="${mail_to}"  >
            <from address="${mail_from}" />
            <to address="${mail_to}" />

            <attachments>
                <fileset dir="${jmeter.result.html.dir}">
                    <include name="${htmlReportNameSummary}${time}.html" />
                    <include name="collapse.png" />
                    <include name="expand.png" />
                </fileset>
            </attachments>

            <message>
                ${message}：
                ${html}
            </message>
        </mail>
    </target>

</project>
```

### 制作镜像
> Dockerfile文件夹下执行命令
```shell
docker build -t ${name} .
```

### 启动容器
```shell
docker run -itd --privileged=true --name ${containerName} --rm -v /Users/xxx/jmx_git/workflow:/jmeterdocker/test/input/jmx/workflow ai/test
```

### 进入容器
```shell
docker exec -it ${容器ID} /bin/sh
```

### 测试导出报告
> 可使用邮箱发送报告，也可直接在容器中查看报告html /jmeterdocker/test/report/html
```shell
cd /jmeterdocker/test && export JMETER_EMAIL=${邮箱地址: 推荐使用企业微信邮箱地址} && ant
```

### 完整Git地址
> [GIT 地址](https://github.com/yangdi-vv/jmeter-test)