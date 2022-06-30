---
title: Cocoapods 之组件化工具
urlname: skbf9i
date: 2019-10-12 12:58:06 +0800
tags: [iOS]
categories: [移动端]
---

#### 一、Cocoapods 简介

每种语言发展到一个阶段，就会出现相应的依赖管理工具，例如 Java 语言的 Maven，nodejs 的 npm。随 着 iOS 开发者的增多，业界也出现了了为 iOS 程序提供依赖管理的工具，它的名字叫做:CocoaPods。
CocoaPods 项目是使用 Ruby 编写，源码在 Github 上管理。该项目开始于 2011 年 8 月 12 日，经过多年年发展， 现在已经成为 iOS 开发事实上的依赖管理标准工具。开发 iOS 项目不可避免地要使用第三方开源库， CocoaPods 的出现使得我们可以节省设置和更新第三方开源库的时间。

<!-- more -->

1.[Cocoapods 官方手册](https://guides.cocoapods.org)
2.Cocoapods 是什么?

```
Cocoapods 是 Swift 和 Objective-C Cocoa 项目依赖管理器。类似于Java 语言的 Maven， nodejs 的 npm。
```

3.Cocoapods 可以做什么?

```
1. 集成三方项目，可以减少 Xcode 的繁琐配置，更加方便的管理项⽬依赖，解决项目依赖冲突问题。 2. 持续构建应用，减少重复编码⼯工作。
3. 企业可以搭建自己的私有仓库，方便对企业内部公共组件的管理。
4. 可以对不同编译环境下进行三方库依赖配置。(后续 Podfile 讲解中有举例例说明)
```

4.如何学习 Cocoapods ?

```
建议一:网上查找解决方案;
建议二:查看学习 Cocoapods 源码;(简单演示)
```

#### 二、Cocoapods 安装

1.替换 Mac OS 自带 ruby 版本，推荐使用 RVM 管理 ruby 版本。【推
荐】

```bash
说明: 如果不使用 rvm 管理 ruby 版本，请自行替换系统 ruby 版本。 如果使用系统ruby，会因系统版本过低，会导致 cocoapods 无法升级到最新版本。
1. 安装 RVM `\curl -sSL https://get.rvm.io | bash -s stable` 2. 安装 ruby 2.5.1 `rvm install 2.5.1`
3. 默认使用 2.5.1 `rvm use 2.5.1 --default`
```

2.替换 gem 镜像源

```bash
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```

3.更新、升级 gem

```bash
sudo gem update --system
```

4.安装 Cocoapods

```bash
sudo gem install cocoapods
```

5.替换 podspec 文件托管地址从 github 切换到国内 oschina 【可选】

```bash
// 【先删除，再添加，再更新】
pod repo remove master
pod repo add master http://git.oschina.net/akuandev/Specs.git pod repo add master https://gitcafe.com/akuandev/Specs.git pod repo update
```

6.设置 pod 仓库

```bash
pod setup
```

7.测试 【如果有版本号，则说明已经安装成功】

```bash
pod --version
```

8.利用 cocoapods 来安装第三方框架

```
1. 进入要安装框架的项目的 .xcodeproj 同级文件夹 2. 在该文件夹中新建一个文件 Podfile
3. 在文件中告诉 cocoapods 需要安装的框架信息
   a.该框架支持的平台 b.适用的iOS版本
   c.框架的名称
   d.框架的版本
```

9.常用命令介绍

```
pod install # 安装三方库
pod update # 更新三方库，会根据 Podfile 指定的版本号更新，如果没有指定版本号，会更新至目前最新版本 pod install --no-repo-update # 安装三方库，不更新本地 Repo
pod update --no-repo-update # 更新三方库，不更新本地 Repo
pod cache clean [NAME] # 清除本地三方库缓存
pod cache clean --all # 清除所有本地三方库缓存
pod outdated # 列列举当前 Podfile.lock 中版本过低的三方库
```

#### 三、Podfile

[官方文档](https://guides.cocoapods.org/syntax/podfile.html)

1.简介

```
Podfile 是用于 Cocoapods 找出所有需要下载 Pod 的描述文件，是一个描述一个或多个Xcode项目的目标的依赖 关系的规范。
Podfile 本质上是一个ruby文件，可在里面编写一些基本的 ruby 函数
```

2.创建

```
1. 进入需要使用 Cocoapods 管理的 .xcodeproj 项目目录下。
2. 执行 pod init 命令，Cocoapods 会自动创建 Podfile 文件，并初始化一些必要参数
```

3.版本管理

```
= 0.1 # 版本号指定为 0.1
> 0.1 # 版本号大于 0.1
>= 0.1 # 版本号大于等于 0.1 < 0.1 # 版本号小于 0.1
<= 0.1 # 版本号小于等于 0.1
~> 0.1.2 # 版本号在 0.1.2 至 0.2 之间，并且每次更新都会取 0.1.2 ~ 0.2.0 之间的最新版本，一般使用该 方式管理版本(三方库再不更新API的情况下，一般不会升级大版本。如果有API的改变，必要时需要使用 = 0.1 )
```

4.编译环境配置

```
1. 在 Podfile 中，可以指定对对应的编译环境进⾏行行配置依赖。(编译环境需提前在xcode中配置)
举例例: 在 Debug, ADHoc 环境下，需要集成 Bugtags 来进⾏行行调试，但是在 Release 发布环境下，需要移
除 Bugtags。
pod 'Bugtags', :configurations => ['Debug', 'ADHoc'] # 多个环境
pod 'Bugtags', :configuration => 'Debug' # 单个环境
```

5.指定搜索源 source

```
cocoapods 会在指定的 source 路路径下检索该三方库
pod 'PonyDebugger', :source => 'https://github.com/CocoaPods/Specs.git'
```

6.指定依赖三方库的子项目。(可以去除不必要的依赖)

```
pod 'QueryKit/Attribute'
pod 'QueryKit', :subspecs => ['Attribute', 'QuerySet']
```

7.依赖本地库。(主要用于项目组件化管理)

```
pod 'AFNetworking', :path => '~/Documents/AFNetworking'
```

8.依赖 git 仓库下的指定资源

```
pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git' # master pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :branch => 'dev' # dev分支
pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :tag => '0.7.0' # 0.7.0 tag 版本
pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :commit => '082f8319af' # 指定提交版本
```

9.指定依赖 podspec 文件

```
会去指定的 podspec 文件查找源文件。(后续会对 podspec 文件进⾏行行讲解)
  pod 'JSONKit', :podspec => 'https://example.com/JSONKit.podspec'
```

10.plugin cocoapods 插件

```
可以在 cocoapods 中使用插件，具体插件用发可参照使用的插件本身介绍。 命令介绍:
1. pod plugins list # 检索当前能用的插件
2. pod plugins search xxx # 搜索指定插件
3. pod plugins installed # 列列举当前已经安装的插件
```

11.eg:

```
platform :ios, '9.0' # 代表平台是 ios，版本是 9.0 inhibit_all_warnings! # 忽略所有 Pods 项目内的警告 use_frameworks! # 使用 framework 进行三方库构建
# cocoapods-keys 插件，可自动生成项目依赖，加密部分参数
# pod install 时会让输入各种参数配置，在项目中可以直接使用 # 例如友盟，极光，等三方平台的 AppKey 可以使用该方式进行管理 plugin 'cocoapods-keys', {
    :target => "TestPlugin",
    :keys => [
      "ArtsyAPIClientSecret",
      "ArtsyAPIClientKey",
      "HockeyProductionSecret",
      "HockeyBetaSecret",
      "MixpanelProductionAPIClientKey",
]}
target 'MyApp' do # 项目的 Target 名称
pod 'ObjectiveSugar', '~> 0.5' # 代表项目 MyApp 依赖 版本号在 0.5 ~ 0.x 之间的
ObjectiveSugar 三方库
pod 'SSZipArchive', :inhibit_warnings => false # 不忽略警告
target "MyAppTests" do # 测试 Target
inherit! :search_paths # 继承父项目 MyApp 的所有 search_paths，在此项目内，可以使用
ObjectiveSugar
pod 'OCMock', '~> 2.0.1' # 对该 Target 唯一使用的三方库
end end
# 此方法是在执行 pod install 之前，会调用的方法，允许在下载三方代码之后但在安装之前对Pod进行任何更改。 pre_install do |installer|
installer.pods_project.targets.each do |target| puts "#{target.name}"
end end
# 此方法允许在生成的 Xcode 项目写入磁盘之前对其进行任何最后更改。 post_install do |installer|
installer.pods_project.targets.each do |target| puts "#{target.name}"
end end
```

#### 四、Podspec 文件介绍

1.简介

```
podspec 文件规范描述了了 Pod 库的一个版本。它包括有关应从何处获取源，要使用的文件，要应⽤的构建设置以及其 他常规元数据(如名称，版本和说明)的详细信息。
```

2.创建

```
方法一:直接拷⻉现有的 podspec 文件进⾏修改 方法二:使用 pod 命令 `pod spec create xxx` 创建
```

3.podspec 内字段含义介绍

```
 #### ==========配置============ ####
  s.name         = "Demo" # 必须和文件名保持一致
  s.version      = "0.0.1" # 版本号
  s.summary      = "LLBPayManager"  # 简单描述
  s.description  = <<-DESC          # 详细描述
                   LLBPayManager
                   DESC

  s.homepage     = "http://gitlab.langlib.io/clientlib_ios/LLBPayManager.git" # 主页
  s.license      = "MIT"  # 许可协议
  s.author             = { "Minlison" => "yuanhang@langlib.com" } # 作者，可多个
  s.platform     = :ios, "8.0" # 平台 tvos, ios, osx
  s.ios.deployment_target = '8.0' # 与platform 中的版本号相同
  s.source       = { :git => "http://gitlab.langlib.io/clientlib_ios/LLBPayManager.git", :tag => "#{s.version}" } # 源文件存放地址 git, http, svn ,支持 tag, commit, sha1 算法匹配
  s.documentation_url = "http://gitlab.langlib.io/clientlib_ios/LLBPayManager.git/LLBPayManager/docs/index.html" # 文档
  s.requires_arc = true # 是否是 ARC
  # s.requires_arc = "Classes/Arc" # arc 的文件
  # s.requires_arc = ["Classes/Arc","Classes/Arc.mm"] # arc 文件数组
  s.default_subspec = "Default" # 默认子模块
  s.default_subspecs = "Default" # 默认子模块
  s.static_framework = true # 是否使用静态 framework
  s.swift_version = "4.2" # swift 版本号
  s.cocoapods_version = ">= 1.5.3" # cocoapods 版本号
  s.social_media_url = "https://twitter.com/cocoapods" # 视频
  s.screenshot = "http://dl.dropbox.com/u/378729/MBProgressHUD/1.png" # 截图
  # s.screenshots = [ "http://dl.dropbox.com/u/378729/MBProgressHUD/1.png"] # 截图
  s.prepare_command = "ruby build_files.rb" # 安装该库之前执行的脚本
  s.deprecated = false # 是否已经废弃
  s.ios.framework = "CFNetwork" # 依赖系统 framework ，可以指定平台
  # s.frameworks = "CoreData" # 依赖系统framework，全部平台
  s.weak_framework = "UserNotifications" # 弱链接，比如说一个项目同时兼容iOS6和iOS7，但某一个framework只在iOS7上有，这时候如果用强链接，那么在iOS7上运行就会crash，使用weak_frameworks可以避免这种情况。
  # s.weak_frameworks = ["UserNotifications", "SafariServices"]
  s.ios.vendored_frameworks = "A.framework" # 自定义framework 只针对iOS平台
  # s.vendored_frameworks = "A.framework", "B.framework" # 自定义framework ，全部平台
  s.ios.library = "xml2" # 系统静态库，可以指定平台
  # s.libraries = "xml2", 'z' # 系统静态库数组，全部平台
  s.ios.vendored_library = "libTest.a" # 自定义静态库 只针对iOS平台
  # s.vendored_libraries = "libTest1.a", "libTest2.a" # 自定义静态库，全部平台
  s.compiler_flags = '-DOS_OBJECT_USE_OBJC=0', '-Wno-format' # 编译参数，在BuildSetting 中可设置
  s.xcconfig = { 'OTHER_LDFLAGS' => '-lObjC' }  # 针对整个项目
  s.pod_target_xcconfig = { 'OTHER_LDFLAGS' => '-lObjC' } # pod 的 target 项目配置
  s.user_target_xcconfig = { 'MY_SUBSPEC' => 'YES' } # 主工程的 target 配置
  s.prefix_header_contents = "#import <UIKit/UIKit.h>", "#import <Foundation/Foundation.h>" # pch 文件内容
  s.prefix_header_file = 'iphone/include/prefix.pch' # pch 文件
  s.module_name = 'Demo' # swift 模块名
  s.header_dir = 'Header' # 头文件存放位置，主要目的是为项目中引用头文件时使用 <> 找不到报错，React 中就有用到
  s.header_mappings_dir = 'src/include' # 头文件文件夹目录结构
  s.script_phase = { :name => 'Hello World', :script => 'echo "Hello World"' } # 编译时执行的脚本
  # s.script_phases = [ { :name => 'Hello World', :script => 'echo "Hello World"' } ]
  s.source_files = "Classes/**/*.{h,m}" # 源文件路径，是只相对 podspec 文件所在文件夹的路径，匹配正则
  s.public_header_files = "Headers/Plublic/*.h" # 公开头文件
  s.private_header_files = "Headers/Private/*.h" # 私有头文件，不会对外暴露
  s.ios.resource_bundle = {"Box" => "Image/*.png"} # bundle 资源文件
  # s.resource_bundles = {"Box" => ["Image/*.png","Text/*.txt"], "OtherBox" => ["Image1/*.png"]} # bundle 资源文件
  s.resource = "Resources/HockeySDK.bundle" # 资源文件，支持所有文件，bundle，imageasset，png, jpg, gif, ttf 等
  s.resources = ["image/*.png","font/*.ttf"]
  s.ios.exclude_files = 'Classes/osx' # 不包含文件，多用于跨平台
  # s.exclude_files = 'Classes/**/unused.{h,m}'
  s.preserve_path = "aa.txt" # 受保护的文件，下载以后，不会被删除, Cocoapods 默认会删除 podspec 没有引用到的文件， 多用与静态库，framework，资源文件等
  s.module_map = 'source/module.modulemap' # modulemap 文件，Cocoapods 默认会自己创建
  s.subspec "Demo1" do |ss|  # 上述属性大多数子模块都可使用
      ss.dependency "JSONKit" # 依赖，此处可以是公开库，私有库，本地库（前提是在主工程安装）
      ss.subspec "Demo11" do |sss|
        ss.dependency "AFNetworking", "~> 1.0.0"
      end
  end
```

#### 五、Cocoapods 私有仓库

1. 简介

```
Cocoapods 私有仓库，本质是一个git仓库，只用来存放 podspec 文件
```

2.作⽤

```
统一管理公司内部依赖库的版本号，并且可作为历史回归，可以让不同App使⽤不同的版本;
```

3.创建

```
1. 在gitlab，gitoschina，github等平台，创建一个公开(私有需要分配响应的权限)的git仓库; 2. 在本地使用 pod repo add ${REPO_NAME} ${SOURCE_URL} 添加私有仓库即可;
```

4.推送 podspec 文件到私有仓库

```
1. 在新建的podspec 文件夹下，使用 pod spec lint xxx.podspec 检验podspec文件格式是否正确;
2. 检验私有库是否编译成功，pod lib lint xxx.podspec
3. 推送到私有仓库 pod repo push ${REPO_NAME} xxx.podspec # 如果是公开库，需要配置 trunk (另 说)
```

```
小技巧:
某些时候，会遇到 podspec 文件依赖本地库，或者是依赖别的三方比较难下载和验证的时候，或者是没有包含
i386, x86_64 的静态库，就会一直报错;

这时候，如果能保证库本身没有问题，可以改本地 cocoapods 源码，绕过验证，直接推送到私有仓库
1. which pod 找到 pod 命令所在位置
2. pod --version 打印当前cocoapods 版本
3. 在pod 命令所在的上级文件夹，找 cocoapods指定版本的源码文件存放位置
4. 找到 lib/cocoapods/validator.rb 并找到函数 def validate ，在 @results = [] 下方直接
return true 即可
```

#### 六、几个问题

一、pod search xxx 查找不到，但是确定是有该库

```
解决办法: 删除 ~/Library/Caches/CocoaPods/search_index.json
```

二、pod install 不更新库文件

```
解决办法:删除需要更新文件的仓库缓存
1. pod cache clean xxx
2. 删除 ~/Library/Caches/CocoaPods/Pods 目录下的指定⼯工程的缓存

//查看所有spec文件的缓存，可以直接到路径下删除文件
pod cache list
//删除指定库的缓存文件
pod cache clean AFNetworking
//运行podfile文件但不更新本地spec文件
pod install --no-repo-update
```
