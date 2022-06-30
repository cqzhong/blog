---
title: iOS录音lame使用
urlname: kge9wa
date: 2020-05-30 19:00:00 +0800
tags: [iOS,lame]
categories: [移动端]
---

lame 静态库 libmp3lame.a 编译

<!-- more -->

#### 1、[lame 历史版本](http://lame.sourceforge.net)

#### 2、[lame-ios-build](https://github.com/kewlbear/lame-ios-build)

#### 3、下载解压

- 下载 lame 的最新版本解压到桌面的一个文件夹里例如 lame。
- 下载得到 lame-build.sh 文件拷贝到上一步解压好的文件夹里（lame）

#### 4、打开 built-lame.sh 就可以看到我们将要修改的地方

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602936209601-688b910e-a703-491f-ac3e-ea621e7e6cf7.jpeg#align=left&display=inline&height=427&margin=%5Bobject%20Object%5D&originHeight=427&originWidth=655&size=0&status=done&style=none&width=655)

#### 5、修改后

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602936208029-2eef7963-7cab-4383-bf60-dba86a983368.jpeg#align=left&display=inline&height=427&margin=%5Bobject%20Object%5D&originHeight=427&originWidth=655&size=0&status=done&style=none&width=655)

#### 6、编译导出

- 打开终端 进入第一步的 lame 文件夹下  cd /Users/电脑名/Desktop/lame
- 成功进入 lame 文件下之后 输入   chmod 777 build-lame.sh
- 最后输入 ./build-lame.sh
- 找到 lame 文件夹下面的 fat-lame 文件夹里面的 libmp3lame.a 和 lame.h   这就是重新编译出来的静态库了
