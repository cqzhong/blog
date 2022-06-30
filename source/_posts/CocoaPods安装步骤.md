---
title: CocoaPods安装步骤
urlname: cg3tc4
date: 2019-01-20 11:46:52 +0800
tags: [iOS,CocoaPods]
categories: [移动端]
---

### [CocoaPods Blog](https://blog.cocoapods.org)

#### 安装步骤

- 更新系统 Ruby 环境

```ruby
# 这一步骤需要科学上网
sudo gem update --system
# 查看已安装的 Ruby 版本（最新版本：3.0.6，截止20200430）
gem -v
```

<!-- more -->

- 替换镜像源

```ruby
//1、移除源
sudo gem sources -r http://rubygems.org
//或者
gem sources --remove https://rubygems.org/

//2、添加源
sudo gem sources -a https://rubygems.org
//或者
gem sources -add https://gems.ruby-china.com/

//3、检查源是否被替换
gem sources -l
```

- 安装 cocoapods

1、安装 cocoapods 的预览版本

```ruby
sudo gem install -n /usr/local/bin cocoapods --pre
```

2、安装 cocoapods 到指定目录

```ruby
sudo gem install -n /usr/local/bin cocoapods
// 或者
sudo gem install cocoapods -n /usr/local/bin
```

#### 卸载

- 卸载

```ruby
sudo gem uninstall cocoapods
```

- 查看安装列表

```ruby
gem list --local | grep cocoapods
```

- Demo

```ruby
Select gem to uninstall:
 1. cocoapods-1.2.0.beta.3
 2. cocoapods-1.2.0
 3. cocoapods-1.2.1.beta.1
 4. All versions
> 4
Successfully uninstalled cocoapods-1.2.0.beta.3
Successfully uninstalled cocoapods-1.2.0
Remove executables:
pod, sandbox-pod

in addition to the gem? [Yn]  y
Removing pod
Removing sandbox-pod
Successfully uninstalled cocoapods-1.2.0
macminideMac-mini:~ macmini$ gem list --local | grep cocoapods
cocoapods-core (1.2.0)
cocoapods-deintegrate (1.0.1)
cocoapods-downloader (1.1.3)
cocoapods-plugins (1.0.0)
cocoapods-search (1.0.0)
cocoapods-stats (1.0.0)
cocoapods-trunk (1.1.2)
cocoapods-try (1.1.0)
macminideMac-mini:~ macmini$ sudo gem uninstall cocoapods-core
Successfully uninstalled cocoapods-core-1.2.0


ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-core
Successfully uninstalled cocoapods-core-1.9.1
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-deintegrate
Successfully uninstalled cocoapods-deintegrate-1.0.4
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-downloader
Successfully uninstalled cocoapods-downloader-1.3.0
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-plugins
Successfully uninstalled cocoapods-plugins-1.0.0
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-search
Successfully uninstalled cocoapods-search-1.0.0
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-stats
Successfully uninstalled cocoapods-stats-1.1.0
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-trunk
Successfully uninstalled cocoapods-trunk-1.4.1
ggzj@ggzjdeMacBook-Pro ~ % sudo gem uninstall cocoapods-try
Successfully uninstalled cocoapods-try-1.1.0
```
