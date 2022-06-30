---
title: iOS 修改UserAgent
urlname: tb6mci
date: 2018-09-04 11:55:05 +0800
tags: [iOS,WebView]
categories: [移动端]
---

首部字段 User-Agent 会将创建请求的浏览器信息和用户代理等信息传达给服务器。
由网络爬虫发起请求时，有可能会在字段哪添加爬虫作者等电子邮箱地址。此外，如果请求经过代理，那么中间也很可能被添加上代理服务器名称

<!-- more -->

#### WKWebView 设置 navigator.userAgent

```objc
- (void)setupWebViewUserAgent {

    @weakify(self);
    [_webView evaluateJavaScript:@"navigator.userAgent" completionHandler:^(id result, NSError *error) {

        @strongify(self);
        NSString * userAgent= [NSString stringWithFormat:@"%@", result];
        if (![userAgent qmui_includesString:kWebViewUserAgent]) {

            NSString *newAgent = [NSString stringWithFormat:@"%@%@",userAgent, kWebViewUserAgent];
            NSDictionary *dictionary = @{@"UserAgent": newAgent};
            [[NSUserDefaults standardUserDefaults] registerDefaults:dictionary];
            [[NSUserDefaults standardUserDefaults] synchronize];

            [self.webView setCustomUserAgent:newAgent];
        }
    }];
}
```

#### UIWebView 设置 navigator.userAgent

```objc
//MARK: 配置浏览器信息
- (void)configNavigatorUserAgent {

    NSString *userAgent = [[UIWebView new] stringByEvaluatingJavaScriptFromString:@"navigator.userAgent"];
    if (![userAgent qmui_includesString:kWebViewUserAgent]) {

        userAgent = [NSString stringWithFormat:@"%@%@",userAgent, kWebViewUserAgent];
        NSDictionary *dictionary = @{@"UserAgent": userAgent};
        [[NSUserDefaults standardUserDefaults] registerDefaults:dictionary];
        [[NSUserDefaults standardUserDefaults] synchronize];
    }
}
```
