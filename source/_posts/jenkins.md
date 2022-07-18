---
title: docker + jenkins + github 代码提交自动部署
categories:
- 部署
tags:
- jenkins
- docker
---

# jenkins + docker + webhook实现自动化部署前端项目

## 安装docker

- 安装
```bash
# 直接安装
yum install docker-ce
# 或者安装自己需要的版本
yum install docker-ce-17.12.0.ce
 ```

- 启动
```bash
# 启动
systemctl start docker
# 设置开机启动
systemctl enable docker
```

## 安装jenkins

- 安装
```bash
# 回车后会打印一行字符串，这个是jenkins密码,记下来
docker run --name jenkins -u root --rm -d -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean
 ```
- 安装后访问
```shell
http://{服务器地址}:8080
```

- jenkins安装插件
  > 访问地址后选择“安装Jenkins社区推荐的插件”

  ![选择插件](http://101.34.242.163/img_blog/jenkins_plugins.jpeg)

- 创建账户

  ![选择插件](http://101.34.242.163/img_blog/jenkins_login.jpeg)
  > 账户和密码要记住 

## 项目配置
- 全局工具配置
  ![ndoejs](http://101.34.242.163/img_blog/jenkins/jenkins_addPlugin.jpeg)
  > 点击NodeJS选项，添加NodeJS插件配置
- 新建任务
  > 点击创建任务
  
  ![选择选项](http://101.34.242.163/img_blog/jenkins/jenkins_create.jpeg)
  
  > 选择“自由风格”进行操作
  
  ![选择选项](http://101.34.242.163/img_blog/jenkins/jenkins_createType.jpeg)
- 配置git
  > 依次添加GIT仓库地址及凭据(部署时自动更新代码)
  
  ![配置git](http://101.34.242.163/img_blog/jenkins/jenkins_git.jpeg)

- 配置webhook
  > 先配置jenkins
  
  ![配置git](http://101.34.242.163/img_blog/jenkins/jenkins_webhook.jpeg)
  
  > 进入GitHub项目，点击setting -> 点击“webhook” -> "add webhook" -> 添加地址“http:{服务器IP}/webhook”

- 配置执行脚本shell
  
  ![执行脚本](http://101.34.242.163/img_blog/jenkins/jenkins_shell.jpeg)
  代码更新后自动在当前目录执行

## 添加前端访问
- 获取工作空间路径
  > 项目文件地址：/var/jenkins_home/workspace/{项目名称} 
- 安装nginx
  > yum install nginx -y
- 启动nginx
  > systemctl start nginx.service (启动nginx)
  
  > systemctl enable nginx.service (开机自启动)
  
  > systemctl reload nginx.service (修改nginx.conf文件后需重启)
  
- 设置nginx访问地址
```
    server {
        listen       9999;
        server_name  localhost;
        # Load configuration files for the default server block.
  
        location / {
            root   /var/jenkins_home/workspace/projectName/dist;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }
  
        location = /404.html {
  
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
  
        }
    }
```

- 访问服务器地址/域名
  
  > 即可在浏览器访问项目啦~
