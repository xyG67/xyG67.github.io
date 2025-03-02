---
layout: post
title: "Install Golang"
date: 2022-04-04
---

1. 下载并安装
https://go.dev/dl/

对于 Windows：
- 下载 .msi 文件后，双击安装。
- 安装过程中，按照指引完成安装，确保将 Go 添加到环境变量中。
  
对于 macOS：
- 下载 .pkg 文件后，双击打开并遵循安装指引完成安装。
  
对于 Linux：
- 可以使用命令行下载并解压：
  
```  
wget https://golang.org/dl/go1.20.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.20.1.linux-amd64.tar.gz
```

添加环境变量，编辑你的 ~/.bash_profile 或 ~/.bashrc 文件，添加以下行：

```
export PATH=$PATH:/usr/local/go/bin
```

应用更改：
```
source ~/.bash_profile  # 或 source ~/.bashrc
```

2. After Intall
   
export GOPATH=$HOME/go
同样将其添加到 ~/.bash_profile 或 ~/.bashrc 文件中，并重新加载。

或者

在GOPATH外

```
go mod init {name}
go run {file name}
```
