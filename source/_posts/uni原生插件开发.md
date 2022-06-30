---
title: uni原生插件开发
urlname: gr8un5
date: 2020-05-31 20:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

本文主要介绍如何在 iOS 平台开发 uni-app 原生插件，在您阅读此文档时，您需要具备 iOS 应用开发经验，对 HTML、JavaScript、CSS 等前端开发有一定的了解，并且熟悉在 JavaScript 和 Objective-C 环境下的 JSON 格式数据操作等。

<!-- more -->

### 一、[官方开发教程](https://nativesupport.dcloud.net.cn/NativePlugin/course/ios)

### [GitLab 示例项目](http://git.51gonggui.com/cqzhong/hbuilder-uniplugin.git)

### iOS 原生插件踩坑记录

##### 1、原生插件是基于 WeexSDK 规范来实现，扩展原生功能有两种方式:

- module：不需要参与页面布局，只需要通过 API 调用原生功能，比如：获取当前定位信息、数据请求等功能，通过扩展 module 的方式来实现；
- component：需要参与页面布局，比如：map、image 等需要显示 UI 的功能，通过扩展 component 即组件的方法来实现；

##### 2、component 只能在 nvue 文件中使用，不需要引入即可直接使用

##### 3、module 支持在 vue 和 nvue 中调用

##### 4、package.json 中的 id 要和 nativeplugins 下的文件夹名字一致，不然打包会报错：“插件不合法，该插件在 nativeplugins 目录下不存在

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602938716500-9177f9ee-7224-4885-bb8c-b58a8367ab4e.png#align=left&display=inline&height=2072&margin=%5Bobject%20Object%5D&originHeight=2072&originWidth=2280&size=0&status=done&style=none&width=2280)
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602938715105-e7e0a3c5-b7ea-4a76-a969-1816bb21b36e.png#align=left&display=inline&height=2052&margin=%5Bobject%20Object%5D&originHeight=2052&originWidth=2628&size=0&status=done&style=none&width=2628)

##### 5、传值方案

- 前端给原生传值

1. 注册参数传值
1. 获取组件实例传值

- 原生给前端传值

```objc
    [self fireEvent:@"loginButtonEvent" params:@{@"tag":@(btn.tag).stringValue} domChanges:nil];
```

##### 6、nuve 文件代码

```javascript
<template>
  <view>
    <template v-if="isiOS">
     <dc-xrlogin class="login" ref="xrlogin" style="width:750upx;height:600upx" @loginButtonEvent="onLoginButtonEvent"></dc-xrlogin>
    </template>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        isiOS: true
      };
    },
    onLoad(options) {
      uni.getSystemInfo({
        success(res) {
          this.isiOS = (res.platform === 'ios') ? true : false
        }
      })
    },
    mounted() {

      if (this.$refs.xrlogin) {
        this.$refs.xrlogin.reloadData({
          'key': 'value'
        });
      }
    },
    methods: {
      onLoginButtonEvent: function(e) {
        uni.showToast({
          title: `按钮的tag值：${JSON.stringify(e)}`,
          icon: 'none',
          duration: 5000
        });
      }

    }
  }
</script>

<style lang="less">
.login {
  background-color: gray;
}
</style>
```

##### 7、原生插件代码

```objc

#import "WXComponent.h"

NS_ASSUME_NONNULL_BEGIN

@interface XRLoginComponent : WXComponent

@end

NS_ASSUME_NONNULL_END
```

```objc
#import "XRLoginComponent.h"
#import "WeexSDK.h"
#import "WXConvert.h"
//布局
#import "Masonry.h"

@interface XRLoginComponent()

@property (nonatomic, strong) UIView *loginView;
@property (nonatomic, strong) UIButton *wxButton;
@end

@implementation XRLoginComponent

- (instancetype)initWithRef:(NSString *)ref type:(NSString *)type styles:(NSDictionary *)styles attributes:(NSDictionary *)attributes events:(NSArray *)events weexInstance:(WXSDKInstance *)weexInstance {
    if(self = [super initWithRef:ref type:type styles:styles attributes:attributes events:events weexInstance:weexInstance]) {

    }
    return self;
}
#pragma mark - Life Cycle Methods

- (UIView *)loadView {

    return self.loginView;
}

- (void)viewDidLoad {

    [self.loginView addSubview:self.wxButton];
//    self.wxButton.frame = CGRectMake(0, 100, 300, 50);
    [self.wxButton mas_makeConstraints:^(MASConstraintMaker *make) {

        make.left.equalTo(self.loginView.mas_left).offset(24);
        make.right.equalTo(self.loginView.mas_right).offset(-24);
        make.centerY.equalTo(self.loginView);
    }];
}

- (void)loginButtonEvent:(UIButton *)btn {
//    [self fireEvent:@"loginButtonEvent" params:@{@"customk":@"customValue"}];

    [self fireEvent:@"loginButtonEvent" params:@{@"tag":@(btn.tag).stringValue} domChanges:nil];

}

#pragma mark - Setter Getter Methods
- (UIView *)loginView {
    if (!_loginView) {
        _loginView = [UIView new];
    }
    return _loginView;
}
- (UIButton *)wxButton {
    if (!_wxButton) {
        _wxButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _wxButton.tag = 1001;
        [_wxButton addTarget:self action:@selector(loginButtonEvent:) forControlEvents:UIControlEventTouchUpInside];
        [_wxButton setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
        _wxButton.backgroundColor = [UIColor blueColor];
        [_wxButton setTitle:@"微信登录" forState:UIControlStateNormal];
    }
    return _wxButton;
}

// 通过 WX_EXPORT_METHOD 将方法暴露给前端
WX_EXPORT_METHOD(@selector(reloadData:))

- (void)reloadData:(NSDictionary *)options {
    // options 为前端传递的参数
    NSLog(@"%@",options);
    [self fireEvent:@"loginButtonEvent" params:options domChanges:nil];
}

@end
```

##### 8、原生 iOS 和 uniapp 交互的另一种方式

```objc
// iOS端代码
[[NSUserDefaults standardUserDefaults]setValue:@"我是传递的内容" forKey:@"name"];

// uniapp js代码
var userDefaul =  plus.ios.importClass("NSUserDefaults");
var name = userDefaul.standardUserDefaults().stringForKey("name");//获取内容
```
