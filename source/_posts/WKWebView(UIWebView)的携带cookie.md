---
title: WKWebView(UIWebView)的携带cookie
urlname: vg39q0
date: 2018-09-04 11:55:05 +0800
tags: [WebView,iOS]
categories: [移动端]
---

### JS 携带 cookie 的形式

```javascript
  WKUserScript * cookieScript = [[WKUserScript alloc] initWithSource:self.documentCookie injectionTime:WKUserScriptInjectionTimeAtDocumentStart forMainFrameOnly:false];
  [self.webView.configuration.userContentController addUserScript:cookieScript];
  [webView evaluateJavaScript:self.documentCookie completionHandler:^(id result, NSError *error) {
  }];
- (NSString *)cookieHeaderField {
    return CDSQLITE_MANAGER.isLogin ? [NSString stringWithFormat:@"%@=%@;%@=%@;",CDRequestKeyDeviceID, [self uuid], CDRequestKeyToken, CDSQLITE_MANAGER.token] : @"";
}
- (NSString *)uuid {
    return [RMUUid getUUid];
}
- (NSString *)documentCookie {
    //max-age=60*60';domain=m-test.changguwen.com
    return [NSString stringWithFormat:@"document.cookie='%@path=/';",[self cookieHeaderField]];
}
```

<!-- more -->

### 2、PHP 携带 cookie 的形式

```objc
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:self.urlString] cachePolicy:NSURLRequestReturnCacheDataElseLoad timeoutInterval:60];

    //        NSDictionary *requestHeader = [NSHTTPCookie requestHeaderFieldsWithCookies:[NSHTTPCookieStorage sharedHTTPCookieStorage].cookies];
    //        [request setAllHTTPHeaderFields:requestHeader];

    [request addValue:[self cookieHeaderField] forHTTPHeaderField:@"Cookie"];
```

### 3、wk 插入 cookie

```objc
- (void)setupHTTPCookieStorage {

    NSString *host = [NSURL URLWithString:CDSQLITE_MANAGER.baseH5url].host;

    NSMutableDictionary *deviceCookieProperties = [NSMutableDictionary dictionary];
    [deviceCookieProperties setObject:CDRequestKeyDeviceID forKey:NSHTTPCookieName];
    [deviceCookieProperties setObject:[RMUUid getUUid] forKey:NSHTTPCookieValue];
    [deviceCookieProperties setObject:host forKey:NSHTTPCookieDomain];
    [deviceCookieProperties setObject:@"/" forKey:NSHTTPCookiePath];
    [deviceCookieProperties setObject:[[NSDate date] dateByAddingTimeInterval:2592000] forKey:NSHTTPCookieExpires];
//    [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:deviceCookieProperties]];


    NSMutableDictionary *tokenCookieProperties = [NSMutableDictionary dictionary];
    [tokenCookieProperties setObject:CDRequestKeyToken forKey:NSHTTPCookieName];
    [tokenCookieProperties setValue:CDSQLITE_MANAGER.token forKey:NSHTTPCookieValue];

    [tokenCookieProperties setObject:host forKey:NSHTTPCookieDomain];
    [tokenCookieProperties setObject:@"/" forKey:NSHTTPCookiePath];
    [tokenCookieProperties setObject:[[NSDate date] dateByAddingTimeInterval:2592000] forKey:NSHTTPCookieExpires];
//    [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:tokenCookieProperties]];

    if (@available(iOS 11.0, *)) {

        WKHTTPCookieStore *cookieStore = self.configuration.websiteDataStore.httpCookieStore;
        [cookieStore setCookie:[NSHTTPCookie cookieWithProperties:deviceCookieProperties] completionHandler:^{
        }];
        [cookieStore setCookie:[NSHTTPCookie cookieWithProperties:tokenCookieProperties] completionHandler:^{
        }];
    } else {

//        [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:deviceCookieProperties]];
//        [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:tokenCookieProperties]];
    }
}
```

### 设置 cookies 存储

```objc

+ (void)resetHTTPCookieStorage {

    @synchronized (self) {

        //清理NSHTTPCookieStorage存储的cookies
        NSHTTPCookieStorage *cookieJar = [NSHTTPCookieStorage sharedHTTPCookieStorage];
        NSArray *cookieArray = [NSArray arrayWithArray:[cookieJar cookies]];
        for (id obj in cookieArray) {
            [cookieJar deleteCookie:obj];
        }

        if (!CDSQLITE_MANAGER.isLogin) return;


        CDDLog(@"[RMUUid getUUid]:  %@", [RMUUid getUUid]);

        NSString *host = [NSURL URLWithString:CDSQLITE_MANAGER.baseH5url].host;

        NSMutableDictionary *deviceCookieProperties = [NSMutableDictionary dictionary];
        [deviceCookieProperties setObject:CDRequestKeyDeviceID forKey:NSHTTPCookieName];
        [deviceCookieProperties setObject:[RMUUid getUUid] forKey:NSHTTPCookieValue];
        [deviceCookieProperties setObject:host forKey:NSHTTPCookieDomain];
        [deviceCookieProperties setObject:@"/" forKey:NSHTTPCookiePath];
        [deviceCookieProperties setObject:[[NSDate date] dateByAddingTimeInterval:2592000] forKey:NSHTTPCookieExpires];
        [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:deviceCookieProperties]];


        NSMutableDictionary *tokenCookieProperties = [NSMutableDictionary dictionary];
        [tokenCookieProperties setObject:CDRequestKeyToken forKey:NSHTTPCookieName];
        [tokenCookieProperties setValue:CDSQLITE_MANAGER.token forKey:NSHTTPCookieValue];

        [tokenCookieProperties setObject:host forKey:NSHTTPCookieDomain];
        [tokenCookieProperties setObject:@"/" forKey:NSHTTPCookiePath];
        [tokenCookieProperties setObject:[[NSDate date] dateByAddingTimeInterval:2592000] forKey:NSHTTPCookieExpires];
        [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:[NSHTTPCookie cookieWithProperties:tokenCookieProperties]];

    }
}
```

### 查询响应后的 cookie

```objc

#pragma mark - WKNavigationDelegate
// 在收到响应后，决定是否跳转
- (void)webView:(WKWebView *)webView decidePolicyForNavigationResponse:(WKNavigationResponse *)navigationResponse decisionHandler:(void (^)(WKNavigationResponsePolicy))decisionHandler{

    if (@available(iOS 12.0, *)) {//iOS11也有这种获取方式，但是我使用的时候iOS11系统可以在response里面直接获取到，只有iOS12获取不到
        @weakify(self);
        WKHTTPCookieStore *cookieStore = webView.configuration.websiteDataStore.httpCookieStore;
        [cookieStore getAllCookies:^(NSArray* cookies) {

            @strongify(self);
            [self queryCookies:cookies];
        }];
    }else {
        NSHTTPURLResponse *response = (NSHTTPURLResponse *)navigationResponse.response;
        NSArray *cookies =[NSHTTPCookie cookiesWithResponseHeaderFields:[response allHeaderFields] forURL:response.URL];
        [self queryCookies:cookies];
    }

    //允许跳转,这句是必须加上的，不然会异常
    decisionHandler(WKNavigationResponsePolicyAllow);
    //不允许跳转
    //decisionHandler(WKNavigationResponsePolicyCancel);
}


-(void)queryCookies:(NSArray *)cookies {


//    if (cookies.count == 0) {
//
//        [WKWebView resetHTTPCookieStorage];
//        return;
//    }
//
    BOOL isHasDevice = false;
    BOOL isHasToken = false;

    for (NSHTTPCookie *cookie in cookies) {

        CDDLog(@"name: %@ ; value: %@; count: %ld", cookie.name, cookie.value, cookies.count);

        if ([cookie.name isEqualToString:CDRequestKeyDeviceID] &&
            [cookie.value isEqualToString:[self uuid]]) {

            isHasDevice = true;
        }

        if ([cookie.name isEqualToString:CDRequestKeyToken]) {

            isHasToken = true;
        }
    }
//
//    if (!isHasDevice || !isHasToken) [WKWebView resetHTTPCookieStorage];
}
```

## UIWebView 的 cookies

```objc
#import "CDFoundViewController.h"

@interface CDFoundViewController () <UIWebViewDelegate>

@property (nonatomic, strong) UIWebView *webView;

@end

@implementation CDFoundViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    [self setCookie];

    [self.view addSubview:self.webView];

//    NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:[NSURL URLWithString:@"https://m-test.changguwen.com/msgt-ceping"]];

    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://m-test.changguwen.com/active?t=4l7MXdyJjgNn2V7XdZb5Pq3KwLYWmQ1E"] cachePolicy:NSURLRequestReturnCacheDataElseLoad timeoutInterval:60];

//    NSArray *cookies = [NSHTTPCookieStorage sharedHTTPCookieStorage].cookies;
//
//    NSDictionary *requestHeader = [NSHTTPCookie requestHeaderFieldsWithCookies:cookies];

//    [request setAllHTTPHeaderFields:requestHeader];
    [request addValue:@"deviceid=db7ed8b2a0e77f1c14dad174315958e8; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOiJYS3h5WU44TEQ5cmRCajVtWVJaNW0xYlI0M1pHN1dQcCIsImV4cCI6MTU2MzQxNTc5NCwiaXNzIjoiZGI3ZWQ4YjJhMGU3N2YxYzE0ZGFkMTc0MzE1OTU4ZTgifQ.0FEOpxg9mVl99L_jesQ684qo7F_nk5BNJ2ksWdaySqY" forHTTPHeaderField:@"Cookie"];


    [_webView loadRequest:request];

}
- (void)setCookie{

    NSMutableArray *arr = [NSMutableArray array];


    NSMutableDictionary *cookieProperties1 = [NSMutableDictionary dictionary];
    [cookieProperties1 setObject:@"deviceid" forKey:NSHTTPCookieName];
    [cookieProperties1 setObject:@"db7ed8b2a0e77f1c14dad174315958e8" forKey:NSHTTPCookieValue];

    [cookieProperties1 setObject:@"m-test.changguwen.com" forKey:NSHTTPCookieDomain];
    [cookieProperties1 setObject:@"/" forKey:NSHTTPCookiePath];
//    [cookieProperties setObject:@"0" forKey:NSHTTPCookieVersion];
//    [cookieProperties1 setObject:[[NSDate date] dateByAddingTimeInterval:2629743] forKey:NSHTTPCookieExpires];

    NSHTTPCookie *cookieuser1 = [NSHTTPCookie cookieWithProperties:cookieProperties1];
    [arr addObject:cookieuser1];


    NSMutableDictionary *cookieProperties2 = [NSMutableDictionary dictionary];

    [cookieProperties2 setObject:@"token" forKey:NSHTTPCookieName];
    [cookieProperties2 setObject:@"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOiJYS3h5WU44TEQ5cmRCajVtWVJaNW0xYlI0M1pHN1dQcCIsImV4cCI6MTU2MzQxNTc5NCwiaXNzIjoiZGI3ZWQ4YjJhMGU3N2YxYzE0ZGFkMTc0MzE1OTU4ZTgifQ.0FEOpxg9mVl99L_jesQ684qo7F_nk5BNJ2ksWdaySqY" forKey:NSHTTPCookieValue];

    [cookieProperties2 setObject:@"m-test.changguwen.com" forKey:NSHTTPCookieDomain];
    [cookieProperties2 setObject:@"/" forKey:NSHTTPCookiePath];
    //    [cookieProperties setObject:@"0" forKey:NSHTTPCookieVersion];
//    [cookieProperties2 setObject:[[NSDate date] dateByAddingTimeInterval:2629743] forKey:NSHTTPCookieExpires];


    NSHTTPCookie *cookieuser2 = [NSHTTPCookie cookieWithProperties:cookieProperties2];
    [arr addObject:cookieuser2];


    [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookies:arr forURL:[NSURL URLWithString:@"https://m-test.changguwen.com/active?t=4l7MXdyJjgNn2V7XdZb5Pq3KwLYWmQ1E"] mainDocumentURL:nil];
}
//MARK: UIWebViewDelegate
- (void)webViewDidStartLoad:(UIWebView *)webView {
}
- (void)webViewDidFinishLoad:(UIWebView *)webView {


}
- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error {
}

- (UIWebView *)webView {
    if (!_webView) {
        _webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 0, SCREEN_WIDTH, SCREEN_HEIGHT)];
        _webView.backgroundColor = [UIColor whiteColor];
        _webView.delegate = self;
        _webView.allowsLinkPreview = true;
    }
    return _webView;
}
/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
```
