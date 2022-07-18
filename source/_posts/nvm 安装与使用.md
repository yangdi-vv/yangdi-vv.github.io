---
title: nvm 安装与使用
categories:
- nvm 
- node
tags:
- nvm
- 工具
---

## 说明
> NVM 是一个nodeJS 的版本管理工具，可以切换当前可使用的nodeJS版本，以适应开发过程中对node版本切换的需求。
> 工作中有时会遇到不同项目使用的nodeJS版本不同的情况（一般安装前需要卸载已有的nodeJS ,有时候不卸载也能正常安装）


## NVM安装
### MAC 下安装
- 终端输入
```shell   
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```
- 安装后进入文件夹
```shell
cd ~/.nvm
```

- 输入以下命令新建文件
```shell
vi .bash_profile
```
- 并输入以上创建文件的内容
```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

### windows安装

- 下载地址
> https://github.com/coreybutler/nvm-windows/releases
> 可以选择nvm-setup的安装包，解压后直接点击安装即可

## NVM 常用操作指令

- nvm install {versionNumber}
> 制定安装某一版本nodeJS 

- nvm list
> 显示已安装的可用nodeJS 版本

- nvm on
> 启用nodeJS版本管理,默认on

- nvm off
> 禁用nodeJS版本管理

- nvm uninstall versionNumber
> 卸载某一版本nodeJS

- nvm use
> 使用某一版本nodeJS 

- nvm version
>显示当前运行的nvm版本