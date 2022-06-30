---
title: 把vConsole注入到WKWebView使所有的页面都显示
urlname: gbtpb6
date: 2020-12-08 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

#### 1、新建 pass.js 文件

- 将[https://cdn.bootcss.com/vConsole/3.2.2/vconsole.min.js](https://cdn.bootcss.com/vConsole/3.2.2/vconsole.min.js) 这个链接打开，将内容全部复制到 pass.js 里面

<!-- more -->

- pass.js 下面再加上这句话 let vConsole = new VConsole();console.log("test");

[pass.js](https://www.yuque.com/attachments/yuque/0/2020/js/1028501/1607398887346-12e01ad2-e7d6-41f9-b99f-4b75d7f3211e.js?_lake_card=%7B%22uid%22%3A%221607398887181-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fjs%2F1028501%2F1607398887346-12e01ad2-e7d6-41f9-b99f-4b75d7f3211e.js%22%2C%22name%22%3A%22pass.js%22%2C%22size%22%3A92542%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%220e24K%22%2C%22card%22%3A%22file%22%7D)  pass 文件下载地址

#### 2、创建`GGZJH5WKWebView`继承`H5WKWebView`

```objectivec
#import <UIKit/UIKit.h>
#import <NebulaBiz/H5WKWebView.h>

NS_ASSUME_NONNULL_BEGIN

@interface GGZJH5WKWebView : H5WKWebView
@end

NS_ASSUME_NONNULL_END

#import "GGZJH5WKWebView.h"

@implementation GGZJH5WKWebView
- (instancetype)initWithFrame:(CGRect)frame configuration:(WKWebViewConfiguration *)configuration {

//    [configuration.preferences setValue:@YES forKey:@"allowFileAccessFromFileURLs"];
    NSString *path = [[NSBundle mainBundle] pathForResource:@"pass" ofType:@"js"];
    NSString *filename = [[NSString alloc]initWithContentsOfFile:path encoding:NSUTF8StringEncoding error:nil];
    WKUserScript *script = [[WKUserScript alloc] initWithSource:filename injectionTime:WKUserScriptInjectionTimeAtDocumentEnd forMainFrameOnly:true];
    [configuration.userContentController addUserScript:script];
    return [super initWithFrame:frame configuration:configuration];
}
@end
```

#### 3、基于 Nebula 容器创建的 H5 页面嵌入的 webview 基类

```objectivec
- (void)application:(UIApplication *)application afterDidFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [MPNebulaAdapterInterface shareInstance].nebulaWebViewClass = [GGZJH5WKWebView class];
}
```

#### 4、运行项目使用 h5 容器打开 h5 页面

```objectivec
[[MPNebulaAdapterInterface shareInstance] startH5ViewControllerWithParams:@{@"url": @"https://www.baidu.com"}];
```
