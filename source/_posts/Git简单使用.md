---
title: Git简单使用
urlname: dykyhv
date: 2018-01-08 11:46:52 +0800
tags: [git]
categories: [其它]
---

#### 简易的命令行入门教程

- Git 全局设置:

```bash
git config --global user.name "name"
git config --global user.email "email@163.com"
```

<!-- more -->

- 创建 git 仓库:

```bash
mkdir Privatefile
cd Privatefile
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:yao11.com/Privatefile.git
git push -u origin master
```

- 已有项目?

```bash
cd existing_git_repo
git remote add origin git@gitee.com:yao11.com/Privatefile.git
git push -u origin master
```

- Git 全局设置

```bash
git config --global user.name "cqz"
git config --global user.email "cqz@ttsing.com"
```

- 创建新版本库

```bash
git clone git@gitlab.ttsing.com:ios/CDFont.git
cd CDFont
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

- 已存在的文件夹

```bash
cd existing_folder
git init
git remote add origin git@gitlab.ttsing.com:ios/CDFont.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

- 已存在的 Git 版本库

```bash
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab.ttsing.com:ios/CDFont.git
git push -u origin --all
git push -u origin --tags
```

##### 如何从码云 GIT 导入到 GitHubDeskTop 桌面工具。

- 先用命令行切换到本地的目录
- 使用 git clone 码云 GIT 地址 命令将项目克隆到本地。
- 在 GitHub Desktop 上添加(Add)本地项目(local path)。
- 在 GitHub Desktop 上尽情地提交、同步吧！

##### 如何将本地的项目上传到码云 GIT。

- 选择本地目录，在 GitHub Desktop 上添加本地项目。
- 在码云 GIT 上新建项目。
- 命令行使用 git remote add origin GIT 地址 将本地项目与码云 GIT 项目建立关系。
- 先使用命令 git pull origin master 同步代码。
- 使用命令 git push origin master 将本地代码推送到远程项目。
- 在 GitHub Desktop 上尽情地提交、同步吧！

##### 建立链接命令

```bash
ssh-add

ssh -T git@git.oschina.net
ssh -T git@gitlab.ttsing.net
```

#### 第一步：成生 SSH 密钥

```bash
打开终端命令工具，输入命令：ssh-keygen -t rsa -C "xxx@gmail.com"

注意ssh-keygen没有空格。屏幕输出：

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/xxx/.ssh/id_rsa):xxx

在上方输入生成的密钥文件名，xxx，屏幕输出：

Enter passphrase (empty for no passphrase): 输入密码

Enter same passphrase again: 确认密码

Your identification has been saved in xxx.

Your public key has been saved in xxx.pub.

The key fingerprint is:

25:fd:01:00:89:98:49:bf:2e:ac:32:2e:d2:5d:bf:98 xxx@gmail.com

The key's randomart image is:

+--[ RSA 2048]----+

| ..+ ..o...      |

|  +.. .  . .     |

|    .   . o .    |

|     .   o . .   |

|    .   S   .    |

| . .  .          |

| .o... .         |

|=....  o.        |

|*o    E ..       |

+-----------------+

屏幕提示生成密钥文件成功，保存在/Users/xxx。
```

#### 第二步：把 xxx.pub 中的内容加入 git[@osc ](/osc) 的 SSH 密钥中

#### 第三步：添加 SSH 并连接

输入命令：ssh-add ~/xxx

~/xxx 是刚刚生成的密钥文件路径，屏幕输出：

Enter passphrase for /Users/xxx/xxx:输入密码

Identity added: /Users/xxx /xxx (/Users/xxx /xxx)

输入命令 ssh -T [git@git.oschina.net](mailto:git@git.oschina.net)，屏幕输出：

The authenticity of host 'git.oschina.net (58.215.179.44)' can't be established.

RSA key fingerprint is 14:b8:b8:0b:c2:b2:5e:ae:f2:21:f8:18:4d:3a:be:fc.

Are you sure you want to continue connecting (yes/no)? yes（输入 yes），屏幕输出：

Warning: Permanently added 'git.oschina.net,58.215.179.44' (RSA) to the list of known hosts.

Welcome to Git[@OSC ](/OSC) , 老左!

#### 错误解决

- git pull origin master --allow-unrelated-histories
- git pull --rebase origin master
- git push -u origin master

#### 回退为上次的版本

```bash
Last login: Thu Dec  6 19:32:45 on ttys000
cao:~ caoqingzhong$ cd /Users/caoqingzhong/Desktop/GitLab/FamousTeacher/CDProgramme
cao:CDProgramme caoqingzhong$ git reset --hard FETCH_HEAD

HEAD is now at ae6c1138 删除 .DS_Store
cao:CDProgramme caoqingzhong$
```
