---
title: 码云和GitHub Desktop建立联系
urlname: vrqkrw
date: 2019-01-12 10:46:52 +0800
tags: [git]
categories: [其它]
---

### 电脑生成 ssh 密钥

##### 一：打开终端命令工具，输入命令：ssh-keygen -t rsa -C "这里写你邮箱"

<!-- more -->

注意 ssh-keygen 没有空格。屏幕输出：

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/xxx/.ssh/id_rsa):这里输入你的 ssh 密钥文件名

Enter passphrase (empty for no passphrase): 输入密码

Enter same passphrase again: 确认密码

屏幕提示生成密钥文件成功，保存在/Users/xxx 文件夹下。

#### 二：把 xxx.pub 中的内容加入 git[@osc ](/osc) 的 SSH 密钥中

#### 三：添加 SSH 并连接

输入命令：ssh-add ~/xxx
~/xxx 是刚刚生成的密钥文件路径，屏幕输出：

Enter passphrase for /Users/xxx/xxx:输入密码

Identity added: /Users/xxx /xxx (/Users/xxx /xxx)

输入命令 ssh -T [git@git.oschina.net](mailto:git@git.oschina.net) 回车
屏幕显示链接成功。

#### 四：打开 GitHub Desktop

选择 File -> clone reponsitory -> URL

输入项目地址，选择存放本地的项目路径。点击 clone

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602935484569-2c96cd8d-d6f9-4b7e-8d4c-970f65a56e91.png#align=left&display=inline&height=708&margin=%5Bobject%20Object%5D&originHeight=708&originWidth=1042&size=0&status=done&style=none&width=1042)

#### 五：在 GitHub Desktop 上尽情地提交、同步吧！

#### 六：和码云、gitlab、腾讯云开发者平台、CODING 建立链接的命令

ssh-add 密钥文件路径

ssh -T [git@git.oschina.net](mailto:git@git.oschina.net)

ssh -T [git@gitlab.xxxx.com](mailto:git@gitlab.xxxx.com)

ssh -T [git@github.com](mailto:git@github.com)

ssh -T [git@git.dev.tencent.com](mailto:git@git.dev.tencent.com)

ssh -T [git@git.e.coding.net](mailto:git@git.e.coding.net)
