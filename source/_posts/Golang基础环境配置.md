---
title: Golang基础环境配置
date: 2024-11-10 23:47:18
tags:
  - Golang
categories: 
  - 运维开发
  - 云原生
---
## 1.Go环境安装

这阵子因为以后工作的原因，所以开始了go语言的学习之旅，工欲善其事必先利其器，首先就得把go语言环境搭建完成

这里我看视屏是IT大地老师的课程:https://www.bilibili.com/video/BV14T4y1g7h9/?spm_id_from=333.337.search-card.all.click

马哥教育也一块汇总到此

### 1.1 Windows下Go语言的安装

> 下载Go

因为go语言的官网经常打不开，所以我就找了一个 [镜像网站](https://studygolang.com/dl)，里面有很多版本的Go语言，选择自己合适的，比如我的是Windows电脑，所以我选中里面的Windows版本的

![image-20200718111751694](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20200718111751694.png)

下载完成是一个安装文件，我们需要进行安装，同时需要注意的就是安装目录，因为事后还需要配置环境变量，下面是安装成功后的图片

![image-20200718111822269](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20200718111822269.png)

> 配置环境变量

根据windows系统在查找可执行程序的原理，可以将Go所在路径定义到环境变量中，让系统帮我们去找运行的执行程序，这样在任何目录下都可以执行go指令，需要配置的环境变量有：

| 环境变量 | 说明              |
| -------- | ----------------- |
| GOROOT   | 指定SDK的安装目录 |
| Path     | 添加SDK的/binmulu |
| GOPATH   | 工作目录          |

首先我们需要打开我们的环境变量，然后添加上GOROOT

![image-20200718151418230](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20200718151418230.png)

然后我们在PATH上添加我们的bin目录

![image-20200718151503318](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20200718151503318.png)

添加完成后，我们输入下面的命令，查看是否配置成功

```sh
go version
```

![image-20240510113522780](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20240510113522780.png)

### 1.2 linux下Go语言的安装

这里我使用的是windows的wsl2子系统，安装方法通用

> 1、找到[linux](https://so.csdn.net/so/search?q=linux&spm=1001.2101.3001.7020) 版本go包 （[Downloads - The Go Programming Language](https://golang.google.cn/dl/)）

这里可以直接去官网下载后再上传到服务器

> 这里直接使用wget 拉取下载

```sh
wget https://dl.google.com/go/go1.22.3.linux-amd64.tar.gz
```

> 2、解压到/usr/local （官方推荐）

```sh
tar -zxvf go1.22.3.linux-amd64.tar.gz -C /usr/local
```

> 3、添加到环境变量

```sh
# 习惯用vim，没有的话可以用命令`sudo apt-get install vim`安装一个
vim /etc/profile
# 在最后一行添加
# Golang 环境变量设置
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
# 保存退出后source一下（vim 的使用方法可以自己搜索一下）
source /etc/profile
```

> 4、查看是否安装成功

![image-20240510114728267](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20240510114728267.png)

### 1.3 Visual Studio Code 安装

官网：[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

> 安装go插件

![image-20240510115058408](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20240510115058408.png)

> 解决VSCode安装Go tools失败的问题

**测试环境**

```sh
hj@DESKTOP-FBLS07J:~$ go version
go version go1.22.3 linux/amd64
```

安装Go后，打开VS Code，按照提示安装了微软官方的GO插件。但在安装go tools时，出现了下面的一大堆错误（日志）。

> 主要提示以下错误

```powershell
The “gopls“ command is not available. Use “go get -v golang.org/x/tools/cmd/gopls“ to install.解决
```

**解决方案**

```sh
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
```

![image-20240509194134014](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20240509194134014.png)

设置完成后重启VS Code，按照提示安装即可。

参考https://l2m2.top/2020/05/26/2020-05-26-fix-golang-tools-failed-on-vscode/