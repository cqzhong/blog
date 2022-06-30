---
title: Cocoapods 私有仓库
urlname: lorm1a
date: 2019-10-14 12:58:06 +0800
tags: [iOS,组件化,CocoaPods]
categories: [移动端]
---

### 一、创建存放 podspec 文件的仓库

1、创建私有 pod 仓库 [Specs](https://gitlab.com/ios/Specs.git)

2、在终端把远程的私有版本库添加到本地索引

```bash
pod repo add Specs https://gitlab.com/ios/Specs.git
```

<!-- more -->

然后会要求输入账号和密码

```bash
Last login: Fri Mar 15 14:39:34 on ttys000
caoqingzhongdeMacBook-Pro:~ caoqingzhong$ pod repo add Specs https://gitlab.com/ios/Specs.git
Cloning spec repo `Specs` from `https://gitlab.com/ios/Specs.git`
Username for 'https://gitlab.com': cqz
Password for 'https://cqz@gitlab.com':
caoqingzhongdeMacBook-Pro:~ caoqingzhong$
```

master 是 cocoaPods 上版本库的列表

```bash
open ~/.cocoapods/repos
```

### 二、创建代码仓库

1、Gitlab 上创建相关 demo 项目
2、先到要创建项目的目录然后执行。

```bash
pod lib create NetworkReachability
```

```bash
What platform do you want to use?? [ iOS / macOS ]

 > iOS

What language do you want to use?? [ Swift / ObjC ] （你用什么语言？）

 > ObjC

Would you like to include a demo application with your library? [ Yes / No ]（是否需要一个例子工程；）

 > Yes

Which testing frameworks will you use? [ Specta / Kiwi / None ]（选择一个测试框架；）

 > None

Would you like to do view based testing? [ Yes / No ]（是否基于View测试；）

 > No

What is your class prefix?（类的前缀；）

 >
```

3、然后本地文件夹路径会看到项目

4、替换 NetworkReachability 文件夹内的相关内容。

5、按照规范编写.podspec 文件。

6、在回到 Example 路径下，重新执行 pod install 操作

7、到相关目录验证.podspec 的正确性(如果通不过，也可以先提交至代码仓库，添加 tag，然后验证。)

```
1、pod lib lint 或者 pod spec lint 如果想要忽略警告：--allow-warnings eg: pod spec lint --allow-warnings
2、如果有错误，查看详细信息：pod spec lint --verbose
```

8、当你看到 NetworkReachability.podspec passed validation 时，说明验证通过了

9、因为 podspec 文件中获取 Git 版本控制的项目还需要 tag 号，所以我们要打上一个 tag

```bash
git status
git add .
git commit -m '编辑spec文件'
git remote add origin https://gitlab.com:ios/NetworkReachability.git #添加远端仓库
git push origin master  #提交到远端
git tag -m "第一次提交" "0.0.1" (要与NetworkReachability.podspec文件中的tag值保持一致)
git push --tags     #推送tag到远端仓库
```

10、向 Spec Repo 提交 podspec。执行 pod repo push 本地 repo 名 NAME.podspec --verbose --use-libraries --allow-warnings

```
pod repo push Specs NetworkReachability.podspec
当出现
Validating spec
 -> NetworkReachability (0.0.1)

Updating the `Specs' repo

From https://gitlab.com/ios/Specs
 * [new branch]      master     -> origin/master

Adding the spec to the `Specs' repo

 - [Add] NetworkReachability (0.0.1)

Pushing the `Specs' repo
```

11、GitLab 端查看项目

12、查看 本地 Repos

### 三、私有库的使用

1、用 Xcode 打开编辑 Podfile 文件

```podfile
project 'FT_iPhone.xcodeproj'

# Uncomment the next line to define a global platform for your project
 platform :ios, '9.0'
 inhibit_all_warnings!

source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/aliyun/aliyun-specs.git'
source 'https://gitlab.com/ios/Specs.git'

target 'FT_iPhone' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for FT_iPhone


#唱道私有库

#网络监测
pod 'NetworkReachability', '~> 0.0.2'
pod 'Font', '~> 0.0.3'


end

post_install do |installer|
    installer.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '9.0'

        # Pod文件在Debug模式下不进行编译优化,提升编译速度
        if config.name.include?("Debug")
                config.build_settings['GCC_OPTIMIZATION_LEVEL'] = '0'
            end
        end
    end
end
```

2、 pod search NetworkReachability

3、查看本地的 pod 库索引 pod repo

4、使用与其它第三方库一样，只是私有库版本更新维护是自己处理

### 四、创建私有库遇到的坑

1、pod search 搜索不到问题

```bash
 pod repo update ，如果还搜索不到: pod search EmptyView --simple 可搜到
```

2、提交代码以后，打上 tag。然后再去验证 spec 或者是 lib

3、打上标签

```bash
git tag -m "first release" "0.1.0" (要与DRCategories.podspec文件中的tag值保持一致)

git push --tags     #推送tag到远端仓库
```

4、推送到远端仓库

```
执行 pod repo push 本地repo名 NAME.podspec --verbose --use-libraries --allow-warnings
```

5、清除本地 spec 文件缓存

```
//查看所有spec文件的缓存，可以直接到路径下删除文件

pod cache list

//删除指定库的缓存文件

pod cache clean AFNetworking

//运行podfile文件但不更新本地spec文件

pod install --no-repo-update


最好使用 pod spec lint CZFTool.podspec --verbose (打印错误信息)

如果有引用到库framwork或C语言库的话必须使用
pod spec lint CZFTool.podspec --use-libraries // 验证
pod trunk push CZFTool.podspec  --use-libraries           // 上传
```

6、- ERROR | [iOS] unknown: Encountered an unknown error (Unable to find a specification for `XSLKeyChainCache (~> 0.1.0)` depended upon by `XSLOpenUDID`) during validation.

说明在你的本地 repo 里没有存在 XSLKeyChainCache 私有库
解决方案

- pod repo update 更新本地的私有库
- 指定私有库路径 pod sepc lint 文件名.podspec --sources='[https://gitlab.com/ios/LyricParser.git,https://gitlab.com/ios/Specs.git](https://gitlab.com/ios/LyricParser.git,https://gitlab.com/ios/Specs.git)'

#### 7、本地私有 pods

```
#自定义的第三方
#pod 'XLForm', :path => '../../'
pod 'FPS', :path    => './Programme/ThirdLibrary/FPS'
pod 'SCStackViewController', :path    => './Programme/ThirdLibrary/SCStackView'
```

#### 8 在每一个私有库的下面写一个更新的脚本，避免繁琐的操作
