---
title: MAC下安装Nginx
urlname: ifranm
date: 2019-08-04 12:40:17 +0800
tags: [Nginx]
categories: [其它]
comments: true
---

### 使用工具工具 [Homebrew](https://brew.sh/index_zh-cn.html)

安装完成后打开终端

<!-- more -->

#### 查看 nginx 信息

```bash
brew search nginx
```

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602937472305-4c2b53db-c95b-4e1a-a749-8fa8ab9f9d67.jpeg#align=left&display=inline&height=96&margin=%5Bobject%20Object%5D&originHeight=96&originWidth=772&size=0&status=done&style=none&width=772)

如果显示对号，证明已经安装过了。否则执行安装命令

```bash
brew install nginx
```

#### 安装完成

主页的文件在/usr/local/var/www 文件夹下
对应的配置文件地址在/usr/local/etc/nginx/nginx.conf

#### 运行 nginx

```bash
nginx
```

#### 重新启动 nginx

```bash
nginx -s reload
```

#### 浏览器查看 `http://localhost:8080`
