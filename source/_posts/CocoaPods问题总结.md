---
title: CocoaPods问题总结
urlname: gs9bfr
date: 2019-01-21 11:00:00 +0800
tags: [iOS,CocoaPods]
categories: [移动端]
---

#### 解决错误信息[!] Unable to find a pod with name, author, summary, or description matching

- 1、检查本地有没有 repos

```bash
1、~/.cocoapods/repos
```

<!-- more -->

- 2、如果本地没有

```bash
1、删除现有基于 git 的 repo。
pod repo remove master

2、添加
pod repo add master https://github.com/CocoaPods/Specs.git

3、删除搜索缓存
rm ~/Library/Caches/CocoaPods/search_index.json

4、搜索第三方库
pod search afnetworking
```

- 3、如果本地有

```bash
// 1、更新本地repo
pod repo update

// 2、删除搜索缓存
rm ~/Library/Caches/CocoaPods/search_index.json

// 3、搜索第三方库
pod search afnetworking
```

#### 搜索类库失败的解决办法 2

```
1、执行pod setup
其实在你安装CocoaPods执行pod install时， 系统会默认操作pod setup，然而由于中国强大的墙可能会pod setup不成功。这时就需要手动执行pod setup指令，如下：
终端输入：pod setup
会出现Setting up CocoaPods master repo，稍等几十秒，最底下会输出Setup completed。说明执行pod setup成功。
如果pod search操作还是搜索失败，如下：
终端输入：pod search AFNetworking
输出：Unable to find a pod with name, author, summary, or descriptionmatching 'AFNetworking' 这时就需要继续下面的步骤了。
2、删除~/Library/Caches/CocoaPods目录下的search_index.json文件
pod setup成功后，依然不能pod search，是因为之前你执行pod search生成了search_index.json，此时需要删掉。
终端输入：rm ~/Library/Caches/CocoaPods/search_index.json
删除成功后，再执行pod search。
3、执行pod search
终端输入：pod search afnetworking(不区分大小写)
输出：Creating search index for spec repo 'master'.. Done!，稍等片刻······就会出现所有带有afnetworking字段的类库。
```

#### 如果更新完成后还是无法获取到最新的库，那就是本地仓库没更新

- 更新本地仓库，本地仓库完成后，即可搜索到指定的第三方库

```bash
pod repo update
```

#### 私有库刚提交，但是搜索不到的问题

```bash
pod search 私有库 --simple
```

#### 错误：You don't have write permissions for the /usr/bin directory. 解决方案

```bash
sudo gem install cocoapods --pre -n /usr/local/bin
```

#### 卸载错误

```bash
sudo gem uninstall -n /usr/local/bin cocoapods
```
