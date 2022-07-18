---
title: homebrew 安装与使用
categories:
- homebrew
tags:
- homebrew
- 工具
---

## 说明
> homebrew是MAC下的一款软件管理工具，方便进行软件的安装
> 官网[https://brew.sh/index_zh-cn](https://brew.sh/index_zh-cn)
> 帮助文档[http://mirrors.ustc.edu.cn/help/brew.git.html#homebrew-linuxbrew](http://mirrors.ustc.edu.cn/help/brew.git.html#homebrew-linuxbrew)

## 安装
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 错误处理
- 可能会出现以下错误
> error: Not a valid ref: refs/remotes/origin/master 
### 需执行以下步骤后重新安装
  - 新建文件 uninstall.rb
  > vi uninstall.rb
  - 输入内容：
```shell
#!/usr/bin/ruby

STDERR.print <<EOS
Warning: The Ruby Homebrew uninstaller is now deprecated and has been rewritten in
Bash. Please migrate to the following command:
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"

EOS

Kernel.exec "/bin/bash", "-c", '/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"' + ' uninstall ' +  ARGV.join(" ")
```
- 执行文件
```shell
ruby uninstall.rb
```

## 常用指令
- brew install pageName
> 安装应用

- brew uninstall pageName
> 卸载软件

- brew info
> 应用信息

- brew list
> 已安装的应用

- brew search
> 查找可用软件

- brew update
> 升级 homebrew