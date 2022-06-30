---
title: AlicloudHTTPDNS接入
urlname: tslmmn
date: 2018-11-08 14:04:11 +0800
tags: [iOS]
categories: [移动端]
toc: true
---

### 一、pods 导入

```bash
pod 'AlicloudHTTPDNS', '~> 1.6.17'
```

<!-- more -->

### 二、启动配置

```objectivec
@interface CDConfigMoudle () <HttpDNSDegradationDelegate>

@end

- (void)modSetUp:(BHContext *)context {

[self configurationHttpDNSService:(context.env == BHEnvironmentDev || context.env == BHEnvironmentTest)];

[self configurationNetwork:context];

}

//MARK: - DNS配置
- (void)configurationHttpDNSService:(BOOL)debug {

HttpDnsService *httpdns = [[HttpDnsService alloc] autoInit];
// 为HTTPDNS服务设置降级机制
[httpdns setDelegateForDegradationFilter:self];
// 允许返回过期的IP
[httpdns setExpiredIPEnabled:true];

[httpdns setLogEnabled:debug];

[httpdns setHTTPSRequestEnabled:true];

NSArray* preResolveHosts = @[@"sapi.changguwen.com", @"m.changguwen.com"];
// 设置预解析域名列表
[httpdns setPreResolveHosts:preResolveHosts];

NSDictionary *IPRankingDatasource = @{
@"sapi.changguwen.com" : @80,
@"m.changguwen.com" : @80
};
//     IP 优选功能，设置后会自动对IP进行测速排序，可以在调用 `-getIpByHost` 等接口时返回最优IP。
[httpdns setIPRankingDatasource:IPRankingDatasource];

}
///MARK: HttpDNSDegradationDelegate
- (BOOL)shouldDegradeHTTPDNS:(NSString *)hostName {

//存在网络代理
if ([[CDNetworkReachability manager] configureProxies]) {
return true;
}
return false;
}


//MARK: - 配置请求
- (void)configurationNetwork:(BHContext *)context {

YTKNetworkConfig *config = [YTKNetworkConfig sharedConfig];
config.securityPolicy.allowInvalidCertificates = true;
[config.securityPolicy setValidatesDomainName:false];

switch (context.env) {
case BHEnvironmentDev: {

config.baseUrl = kRequestURLBaseDebug;
break;
}
case BHEnvironmentTest: {

config.baseUrl = kRequestURLBaseTest;
break;
}
case BHEnvironmentStage: {

config.baseUrl = kRequestURLBaseAdHoc;
break;
}
case BHEnvironmentProd: {

config.baseUrl = kRequestURLBaseRelease;
break;
}
default:
break;
}

[CDSQLiteManager manager].debugURLBase = config.baseUrl;

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

//        if (context.env == BHEnvironmentStage ||
//            context.env == BHEnvironmentProd) {

NSURL *baseUrl = [NSURL URLWithString:config.baseUrl];
NSString *ip = [[HttpDnsService sharedInstance] getIpByHostAsyncInURLFormat:baseUrl.host];
if (ip.length ==0) return ;

NSRange hostFirstRange = [baseUrl.absoluteString rangeOfString:baseUrl.host];
if (NSNotFound != hostFirstRange.location) {
NSString *newUrl = [baseUrl.absoluteString stringByReplacingCharactersInRange:hostFirstRange withString:ip];
config.baseUrl = newUrl;
}

//        }
});

//    config.cdnUrl = @"";
[config addUrlFilter:[CDRequestURLFilter filter]];

YTKNetworkAgent *agent = [YTKNetworkAgent sharedAgent];
[agent setValue: [NSSet setWithObjects:@"application/json", @"text/html",@"text/json",@"text/javascript", @"text/plain",@"text/xml",@"image/*", nil]
forKeyPath:@"jsonResponseSerializer.acceptableContentTypes"];
}
```

### 三、网络变化

```objectivec
- (void)callBackForCurrentStatus
{

#if defined(DEBUG) || defined(CDTEST)

#else

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

NSURL *baseUrl = [NSURL URLWithString:CDSQLITE_MANAGER.debugURLBase];
NSString *ip = [[HttpDnsService sharedInstance] getIpByHostAsyncInURLFormat:baseUrl.host];
if (ip.length ==0) return ;

NSRange hostFirstRange = [baseUrl.absoluteString rangeOfString:baseUrl.host];
if (NSNotFound != hostFirstRange.location) {
NSString *newUrl = [baseUrl.absoluteString stringByReplacingCharactersInRange:hostFirstRange withString:ip];
[YTKNetworkConfig sharedConfig].baseUrl = newUrl;
}
});

#endif

}
```

### 四、返回参数

```bash
[文件名:CDBaseRequest.m] [函数名:-[CDBaseRequest requestCompleteFilter]] [第212行: 打印网络请求：{<CDCodeRequest: 0x2807ce800>
== 网络内容 ==
接口===https://112.124.157.220/v1/information/shipmes?phone=13162079587
请求头==={"Client-Method":"GET","Accept-Language":"zh-Hans-CN;q=1","Client-Platform":"ios","Client-Platform-Version":"12.0","Client-Token":"","Client-Deviceid":"1efee266151c70b689355ed83559bba6","Host":"sapi.changguwen.com","Client-Source":"app","Client-Network":"Wi-Fi","User-Agent":"CDProgramme\/1.0.1 (iPad; iOS 12.0; Scale\/2.00)","Client-Version":"1.0.1"}
参数==={"phone":"13162079587"}
内容==={
"msg" : "请求成功",
"data" : 0,
"code" : 0
}
}]
```
