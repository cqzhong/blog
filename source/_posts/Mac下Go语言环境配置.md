---
title: Mac下Go语言环境配置
urlname: tg0d6q
date: 2019-11-03 11:58:06 +0800
tags: [Golang,Visual Studio Code]
categories: [后端]
---

一：插件
1、Go
2、添加 struct tag 的插件：gomodifytags

<!-- more -->

二、配置 Go 环境变量 GOPATH 和 GOBIN
　　（1）打开终端，cd ~
　　（2）查看是否有.bash_profile 文件：
　　　　  ls -all
　　（3）有则跳过此步，没有则：
　　　　 1）创建：touch .bash_profile
　　　　 2）编辑：open -e .bash_profile
　　　　 3）自定义 GOPATH 和 GOBIN 位置：
export GOPATH=/Users/hopkings/www/Go
export GOBIN=$GOPATH/bin
export PATH=$PATH:\$GOBIN
　　（4）编译：source .bash_profile
　　　\*查看 Go 环境变量：go env

三、安装环境失败
Installing github.com/mdempsky/gocodeFAILED
Installing github.com/ramya-rao-a/go-outlineFAILED
Installing github.com/acroca/go-symbolsFAILED
Installing golang.org/x/tools/cmd/guruFAILED
Installing golang.org/x/tools/cmd/gorenameFAILED
Installing github.com/stamblerre/gocodeFAILED
Installing github.com/sqs/goreturnsFAILED
Installing golang.org/x/lint/golintFAILED

8 tools failed to install.

解决方案：
1、设置 GOPATH ： /Users/用户 user/go
2、 在 /Users/用户 user/go/src 下创建 golang.org/x 文件夹
3、cd /Users/用户 user/go/src/golang.org/x

git clone [https://github.com/golang/tools.git](https://github.com/golang/tools.git) tools

git clone [https://github.com/golang/lint](https://github.com/golang/lint)

4、执行完以后，会多一个 tools 文件夹 、lint 文件夹

5、打开 vsCode 终端，切换到 终端，进入“%GOPATH”目录,执行
go install github.com/ramya-rao-a/go-outline

go install github.com/sqs/goreturns

6、或者直接在输出下面安装插件，成功

参考：[Mac 下安装与配置 Go 语言开发环境](https://www.cnblogs.com/hopkings/p/5809850.html)
