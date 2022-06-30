---
title: React Native介绍
urlname: sglhgu
date: 2019-08-16 10:00:00 +0800
tags: [Android,iOS,react native]
categories: [移动端]
---

#### 一、背景

原生（Native）应用因其性能优秀、体验较好而获得了广大用户和开发者的欢迎。但是，原生应用开发周期长、支持设备有限等问题也困扰着开发者和商户，因而，跨平台移动应用开发成为技术开发者的重要追求。
由于 Apple 严格的审核标准，iOS 应用的发版很受影响，这对于大多数团队来说是不能接受的，所以热更新对于 iOS 应用来说就显得尤其重要。

<!-- more -->

#### 二、React Native 介绍

React Native (简称 RN)是 Facebook 于 2015 年 4 月开源的跨平台移动应用开发框架，是 Facebook 早先开源的 JS 框架 React 在原生移动应用平台的衍生产物，目前支持 iOS 和安卓两大平台。

使用语言：Javascript，类似于 HTML 的 JSX，以及变异 CSS 来开发移动应用。
React Native 是以 iOS 或者 Anroid 原生控件为后端，但以 React component 的方式 Expose 出来进行视图渲染的。所以能够达到界面流畅的效果。

编辑器：Visual Studio Code、 Xcode、Android Studio

#### 三、React Native 的应用场景

1、纯 RN 的 App，适用于一些界面比较少，功能比较简单的 App。
2、APP 只有部分页面是由 React Native 实现的，比如：我们常用的携程 App，它的首页下的很多模块都是由 React Native 实现的，这种开发模式被称为混合开发
3、采用 React Native 可以实现热更新。

#### 四、React Native 混合开发（iOS 篇）

- 配置好 React Native 依赖和项目结构。
- 配置项目目录结构: 首先创建一个空目录用于存放 React Native 项目，然后在其中创建一个/ios 子目录，把你现有的 iOS 项目拷贝到/ios 子目录中。
- 安装 JavaScript 依赖包，创建 package.json 的空文本文件。执行以下命令

```bash
yarn add react-native
```

所有 JavaScript 依赖模块都会被安装到项目根目录下的 node_modules/目录中（这个目录我们原则上不复制、不移动、不修改、不上传，随用随装）。

- iOS 原生应用内，使用 CocoaPods 倒入相关组件

```objc

pod 'React', :path => '../node_modules/react-native/'
pod 'React-Core', :path => '../node_modules/react-native/React'
pod 'React-DevSupport', :path => '../node_modules/react-native/React'
pod 'React-fishhook', :path => '../node_modules/react-native/Libraries/fishhook'
pod 'React-RCTActionSheet', :path => '../node_modules/react-native/Libraries/ActionSheetIOS'
pod 'React-RCTAnimation', :path => '../node_modules/react-native/Libraries/NativeAnimation'
pod 'React-RCTBlob', :path => '../node_modules/react-native/Libraries/Blob'
pod 'React-RCTImage', :path => '../node_modules/react-native/Libraries/Image'
pod 'React-RCTLinking', :path => '../node_modules/react-native/Libraries/LinkingIOS'
pod 'React-RCTNetwork', :path => '../node_modules/react-native/Libraries/Network'
pod 'React-RCTSettings', :path => '../node_modules/react-native/Libraries/Settings'
pod 'React-RCTText', :path => '../node_modules/react-native/Libraries/Text'
pod 'React-RCTVibration', :path => '../node_modules/react-native/Libraries/Vibration'
pod 'React-RCTWebSocket', :path => '../node_modules/react-native/Libraries/WebSocket'

pod 'React-cxxreact', :path => '../node_modules/react-native/ReactCommon/cxxreact'
pod 'React-jsi', :path => '../node_modules/react-native/ReactCommon/jsi'
pod 'React-jsiexecutor', :path => '../node_modules/react-native/ReactCommon/jsiexecutor'
pod 'React-jsinspector', :path => '../node_modules/react-native/ReactCommon/jsinspector'

pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'

pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'
```

```objc
pod install
```

- 添加入口内文件：index.ios.js (注意 index.js、 index.ios.js、 index.android.js、index.native.js 含义不同 参见 [特定平台代码](https://reactnative.cn/docs/platform-specific-code/) )
  在入口类文件中 import 相关 js 页面，打包生成 jsbundle 文件时候，是从入口类文件里面找它所有 import 的文件然后合并导出到 main.jsbundle
  将 main.jsbundle 导入到 Xcode 工程内。
- iOS 加载 React Native 页面 （在开发、测试环境调试时候，需要本地开启一个 nodejs 的服务， 开发完成后将 js 打包为 jsbundle 文件集成到项目中）

```objc
#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.

    NSURL *jsCodeLocation = [self sourceURL];
    RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation moduleName:@"purchased_ios" initialProperties:nil launchOptions:nil];
    [self.view addSubview:rootView];
    [rootView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.left.right.equalTo(self.view);
        make.top.equalTo(self.view).offset(SafeAreaTopHeight);
        make.bottom.equalTo(self.view).offset(-SafeAreaBottomHeight);
    }];

- (NSURL *)sourceURL {

#if defined(DEBUG) || defined(CDTEST)

    return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"src/purchased.ios" fallbackResource:nil];
#else
    return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif

}
```

执行命令生成 jsbundle 文件：

```bash

// 需要在 node_modules同级目录中新建：release_ios文件夹
react-native bundle --entry-file index.ios.js --platform ios --dev false --bundle-output release_ios/main.jsbundle --assets-dest release_ios/

/*
* 选择生成的asset文件夹与 main.jsbundle文件，拖拽至 Xcode 的项目导航面板中
* 运行打包，摆脱对本地nodejs服务器的依赖
*/
```

#### 五、React Native 混合开发（Android 篇）

1、提前安装所需配置

- Java SDK
- AndroidStudio
- Node

2、android 的集成 React Native

- 新建 android 项目或者在现有项目的 terminal 中执行命令

```bash
npm init

package name: (rnappdemo) 输入项目名称，全部小写：rnappdemo
version: (1.0.0) 输入版本号，可以直接回车，也可以输入自己想要的初始版本号
description 输入项目描述，随便输入：first reactnativeapp
entry point: (index.js)输入reactnative的入口文件：index.android.js
test command: 输入：no
git repository: 输入：no
keywords: 输入：no
author: 输入作者信息
license: (ISC) 输入许可 默认ISC
```

- 在终端窗口执行:npm install --save react react-native 安装 React 和 React Native
  执行完成会在项目根目录多出 node_modules 目录，说明安装成功
- 在项目根目录下新建一个名为.flowconfig 的文件，将 `[https://raw.githubusercontent.com/facebook/react-native/master/.flowconfig](https://raw.githubusercontent.com/facebook/react-native/master/.flowconfig)` 网页的内容复制到.flowconfig 文件中
- 在 package.json 文件中的 script 标签里添加"start": "nodenode_modules/react-native/local-cli/cli.js start"
- 在 app 模块 build.gradle 文件中添加在 android->defaultConfig 最后添加 NDK 支持：
  ndk {
  abiFilters "armeabi-v7a", "x86"
  }

在 android 里面添加如下两个配置：
packagingOptions {
exclude "lib/arm64-v8a/librealm-jni.so"
}
在 dependencies 里面添加 reactnative 依赖：（+最好使用 0.60 以下版本）
compile "com.facebook.react:react-native:+"

- 项目的 build.gradle 配置
  在 allprojects->respositories 中添加以下内容，加载最新的 react-native 库，然后点击右上角的 Sync now 同步
  maven {
  // All of React Native (JS, Android binaries) is installed from npm
  url "\$rootDir/node_modules/react-native/android"
  }
- 配置 app 模块 AndroidManifest.xml 文件
  添加网络权限：
  `<uses-permissionandroid:name="android.permission.INTERNET" />`
  添加弹窗权限：（这个可以再上线后取消掉）
  `<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>`
  在 application 里添加 reactnative 弹窗内置页面 activity，否则摇晃手机并打开开发者菜单后，点击 Dev Setting，会直接 Crash：
  `<activityandroid:name="com.facebook.react.devsupport.DevSettingsActivity"/>`
- 在项目根目录下添加 index.android.js 文件

```javascript
import React from "react";
import { AppRegistry, StyleSheet, Text, View } from "react-native";
class HelloWorld extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.hello}>第一个混合RNAPP</Text>
      </View>
    );
  }
}
var styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
  },
  hello: {
    fontSize: 20,
    textAlign: "center",
    margin: 10,
  },
});
AppRegistry.registerComponent("HelloWorld", () => HelloWorld);
```

- 新建 MyReactActivity

```javascript

public class MainApplication extends Application implements ReactApplication {
    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
            return BuildConfig.DEBUG;
        }
        @Override
        protected List<ReactPackage> getPackages() {
            return Arrays.<ReactPackage>asList(
                    new MainReactPackage()
            );
        }
    };

    @Override
    public ReactNativeHost getReactNativeHost() {
        return mReactNativeHost;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        SoLoader.init(this,false);
    }
}
```

3、打包运行

- 手动生成 bundle 文件
  在 app/src/main 下新建 assets 目录，在终端窗口 Terminal 中执行：
  react-native bundle --platform android --dev false--entry-file index.js --bundle-output app/src/main/assets/index.android.bundle--assets-dest app/src/main/res/
  说明：
  --platform：平台
  --dev：开发模式
  --entry-file：条目文件
  --bundle-output：bundle 文件生成的目录
  --assets-dest：资源文件生成的目录
  执行完成后，会在 assets 目录下生成两个文件：index.android.bundle
- 运行 app 即可看到效果

#### 热更新

一、CodePush 热更新
CodePush server：`[http://www.code-push.com/](http://www.code-push.com/)`
参考：`[https://www.jianshu.com/p/6a5e00d22723](https://www.jianshu.com/p/6a5e00d22723)`

二、pushy 进行热更新
参考：`[https://blog.csdn.net/xiangzhihong8/article/details/73201421](https://blog.csdn.net/xiangzhihong8/article/details/73201421)`
参考文档：`[https://github.com/reactnativecn/react-native-pushy/blob/master/docs/guide.md](https://github.com/reactnativecn/react-native-pushy/blob/master/docs/guide.md)`

都是将 jsbundle 文件提交至对应的服务器上，都支持强制更新，动态更新，更新失败回滚，在各自项目中编写更新代码（不建议用弹窗提醒用户更新的方式，这样 iOS 审核会被拒）
