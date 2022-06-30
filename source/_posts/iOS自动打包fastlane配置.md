---
title: iOS自动打包fastlane配置
urlname: wht82o
date: 2018-12-28 15:06:53 +0800
tags: [iOS,fastlane]
categories: [移动端]
---

- 检查是否安装 Xcode ：`xcode-select --install`
- `sudo gem install fastlane -NV` //出现错误，重新安装 ruby 即可解决：`brew install ruby`
  或者它安装：`gem install fastlane`

```
1、报错：     You don't have write permissions for the /usr/bin directory.
  解决：brew cask install fastlane 或者：sudo gem install cocoapods -n /usr/local/bin
```

<!-- more -->

- cd app 目录 `fastlane init`
- `fastlane add_plugin pgyer` //安装蒲公英插件
- 安装蒲公英插件出错时候

```bash
# 1、移除
gem sources --remove https://ruby.taobao.org/

# 2、添加
gem sources -a https://gems.ruby-china.org/

gem sources -a https://gems.ruby-china.com/

# 3、查看

gem sources -l

# 4、修改Gemfile文件
# 报错：（Installing plugin dependencies...Could not fetch specs from https://ruby.taobao.org/）
# 修改Gemfile为 https://gems.ruby-china.com/

source "https://gems.ruby-china.com/"

gem "fastlane"
```

- vim ./fastlane/Fastfile //查看 Fastfile 文件
- 安装错误：

```bash
# 查询Xcode路径
xcode-select -p
xcode-select -print-path
# 修改Xcode 路径：
sudo xcode-select --switch /Applications/Xcode.app/
sudo xcode-select --switch /Applications/你自己安装的路径
```

- desc "打包到 pgy"lane :test do |options|gym( clean:true, #打包前 clean 项目 export_method: "ad-hoc", #导出方式 scheme:"shangshaban", #scheme configuration: "Debug",#环境 output_directory:"./app",#ipa 的存放目录 output_name:get_build_number()#输出 ipa 的文件名为当前的 build 号 )#蒲公英的配置 替换为自己的 api_key 和 user_keypgyer(api_key: "xxxxxxx", user_key: "xxxxxx",update_description: options[:desc])end

---

在终端用 gem 命令的时候，时常遇到的问题：

墙墙墙
通过 gem source 查看你的当前的 gem 资源库位置，如果你的当前资源库的位置为： [https://rubygems.org/](https://rubygems.org/)，不好意思，你是无法安装成功的，因为这个资源库在国外，所以你需要安装的 githug 是无法下载成功的，那我们怎么办呢，其实方法很简单，修改当前资源库的位置：

1- gem sources -r [https://rubygems.org/](https://rubygems.org/) 删除原来的资源库位置
2- gem sources -a [https://ruby.taobao.org/](https://ruby.taobao.org/) 添加新的资源库位置
3- gem sources -u 更新资源库

目录权限不够
报错：
ERROR: While executing gem … (Gem::FilePermissionError) You don’t have write permissions for the /Library/Ruby/Gems/2.0.0 directory.
解决：
sudo chmod 777 /Library/Ruby/Gems/2.0.0 修改权限

gem 需要更新
报错：
ERROR: While executing gem … (Errno::EACCES)
Permission denied - /Library/Ruby/Gems/2.0.0/cache/i18n-0.7.0.gem
解决：
gem update –system

如果还是不行，又想用 gem 安装怎么办？sudo gem install….
