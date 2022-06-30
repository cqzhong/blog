---
title: mPaaS 查看页面内网络请求
urlname: zytb9t
date: 2020-12-15 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

#### 查看网络请求

1、新建`PSDWebViewURLProtocol`类别

2、`PSDWebViewURLProtocol+GGZJ.h`文件代码如下

<!-- more -->

```objc
NS_ASSUME_NONNULL_BEGIN
@interface PSDWebViewURLProtocol:NSObject
@end

@interface PSDWebViewURLProtocol (GGZJ)

@end

NS_ASSUME_NONNULL_END
```

3、`PSDWebViewURLProtocol+GGZJ.m`文件代码如下

```objc
+ (void)load {
    Method originalMethod = class_getClassMethod([NSClassFromString(@"PSDWebViewURLProtocol") class], @selector(canInitWithRequest:));

    Method swizzledMethod = class_getClassMethod([self class], @selector(canInitWithRequestS:));
    method_exchangeImplementations(originalMethod, swizzledMethod);
}
+ (BOOL)canInitWithRequestS:(NSMutableURLRequest *)request {

    //        GGZJDLog(@"------: %@", request.allHTTPHeaderFields);
    //        GGZJDLog(@"host99999: %@: %@",[request.URL host], request.HTTPMethod);
    //        GGZJDLog(@"HTTPBody00000: %@",[[NSString alloc] initWithData:request.HTTPBody encoding:NSUTF8StringEncoding]);
    return [self canInitWithRequestS:request];
}
```

#### 设置 https 证书信任 `PSDWebViewURLProtocol+GGZJ.m`文件内添加以下代码

```objc
-(void)connection:(NSURLConnection *)connection willSendRequestForAuthenticationChallenge:(NSURLAuthenticationChallenge *)challenge {

    // 判断是否是信任服务器证书
    if(challenge.protectionSpace.authenticationMethod == NSURLAuthenticationMethodServerTrust) {
        // 告诉服务器，客户端信任证书
        // 创建凭据对象
        NSURLCredential *credntial = [NSURLCredential credentialForTrust:challenge.protectionSpace.serverTrust];
        // 告诉服务器信任证书
        [challenge.sender useCredential:credntial forAuthenticationChallenge:challenge];
    }
}
```
