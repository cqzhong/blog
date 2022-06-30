---
title: mPaaS接入微信登录、分享、支付
urlname: gqohkp
date: 2021-01-03 20:20:00 +0800
tags: [微信支付,iOS]
categories: [移动端]
---

#### 1、接入 mPaaS 分享模块

- [接入社交分享组件 MPShareKit](https://help.aliyun.com/document_detail/49777.html?spm=a2c4g.11186623.6.1580.5474b3e9FaenrD)
- URL Scheme、[Universal Link](https://www.yuque.com/docs/share/06d04c68-8124-424b-b845-a19e01fa3e07)

<!-- more -->

![1.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1609672700560-079786e4-af1b-45f0-9e34-9131364ce482.png#align=left&display=inline&height=265&margin=%5Bobject%20Object%5D&name=1.png&originHeight=1060&originWidth=1748&size=466066&status=done&style=none&width=437#crop=0&crop=0&crop=1&crop=1&height=1060&id=F1Pyr&originHeight=1060&originWidth=1748&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=&width=1748)

- 在 DTFrameworkInterface 的分类中实现下列方法，并在此方法中以词典的形式返回 key、secret 的值。

```objectivec
- (void)application:(UIApplication *)application beforeDidFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    [self configShareClient];
}
/// 配置分享模块key
- (void)configShareClient {
    NSDictionary *configDic = @{@"weixin" : @{@"key":GGZJ_WX_APP_KEY, @"secret":GGZJ_WX_APP_SECRET, @"universalLink":GGZJ_WX_APP_UL}};
    [APSKClient registerAPPConfig:configDic];
    [WXApi registerApp:GGZJ_WX_APP_KEY universalLink:GGZJ_WX_APP_UL];
}
```

#### 2、[自定义 JSAPI](https://help.aliyun.com/document_detail/55577.html?spm=a2c4g.11186623.6.1337.31037192yyluBy)

实现微信登录、分享、支付 的 JSAPI 文件如下
注意事项:

- 微信支付申请的 wxKey 要绑定到商户号下
- 微信支付为客户端生成预支付订单号、需两次加签
- 生成签名过程中使用的 key="app 密钥" app 密钥在商户号 id 下

代码示例:
​

- ​[GZJsApiHandler4WXPay.h](https://www.yuque.com/attachments/yuque/0/2021/h/1028501/1609673315818-3ed6d0d1-fd71-47a2-8cc7-71a7b51d84b0.h?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673315818-3ed6d0d1-fd71-47a2-8cc7-71a7b51d84b0.h%22%2C%22name%22%3A%22GZJsApiHandler4WXPay.h%22%2C%22size%22%3A147%2C%22type%22%3A%22%22%2C%22ext%22%3A%22h%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673315730-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673315818-3ed6d0d1-fd71-47a2-8cc7-71a7b51d84b0.h%22%2C%22id%22%3A%22NtS6v%22%2C%22card%22%3A%22file%22%7D)

- ​[GZJsApiHandler4WXPay.m](https://www.yuque.com/attachments/yuque/0/2021/m/1028501/1609673316001-36d84e8b-4dd6-4714-91cf-86ee765b6acf.m?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673316001-36d84e8b-4dd6-4714-91cf-86ee765b6acf.m%22%2C%22name%22%3A%22GZJsApiHandler4WXPay.m%22%2C%22size%22%3A8470%2C%22type%22%3A%22%22%2C%22ext%22%3A%22m%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673315730-1%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673316001-36d84e8b-4dd6-4714-91cf-86ee765b6acf.m%22%2C%22id%22%3A%22VAp0T%22%2C%22card%22%3A%22file%22%7D)

- ​[GZJsApiHandler4WXLogin.h](https://www.yuque.com/attachments/yuque/0/2021/h/1028501/1609673325934-f5a3d5f6-ecf1-4daf-8575-f752741c0f30.h?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673325934-f5a3d5f6-ecf1-4daf-8575-f752741c0f30.h%22%2C%22name%22%3A%22GZJsApiHandler4WXLogin.h%22%2C%22size%22%3A149%2C%22type%22%3A%22%22%2C%22ext%22%3A%22h%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673325846-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673325934-f5a3d5f6-ecf1-4daf-8575-f752741c0f30.h%22%2C%22id%22%3A%22UCxMS%22%2C%22card%22%3A%22file%22%7D)

- ​[GZJsApiHandler4WXLogin.m](https://www.yuque.com/attachments/yuque/0/2021/m/1028501/1609673326161-ea0e7e4c-fb37-49b5-9203-01bc9a9d932d.m?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673326161-ea0e7e4c-fb37-49b5-9203-01bc9a9d932d.m%22%2C%22name%22%3A%22GZJsApiHandler4WXLogin.m%22%2C%22size%22%3A2353%2C%22type%22%3A%22%22%2C%22ext%22%3A%22m%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673325846-1%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673326161-ea0e7e4c-fb37-49b5-9203-01bc9a9d932d.m%22%2C%22id%22%3A%22aImmZ%22%2C%22card%22%3A%22file%22%7D)

- ​[GZJsApiHandler4OpenShare.h](https://www.yuque.com/attachments/yuque/0/2021/h/1028501/1609673332213-b2d228aa-ad01-4336-9e12-ee36a9d2ed7a.h?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673332213-b2d228aa-ad01-4336-9e12-ee36a9d2ed7a.h%22%2C%22name%22%3A%22GZJsApiHandler4OpenShare.h%22%2C%22size%22%3A151%2C%22type%22%3A%22%22%2C%22ext%22%3A%22h%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673332117-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fh%2F1028501%2F1609673332213-b2d228aa-ad01-4336-9e12-ee36a9d2ed7a.h%22%2C%22id%22%3A%22KTOrY%22%2C%22card%22%3A%22file%22%7D)

- ​[GZJsApiHandler4OpenShare.m](https://www.yuque.com/attachments/yuque/0/2021/m/1028501/1609673332402-f1e068ee-2e54-4634-9f46-350f98c51bd3.m?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673332402-f1e068ee-2e54-4634-9f46-350f98c51bd3.m%22%2C%22name%22%3A%22GZJsApiHandler4OpenShare.m%22%2C%22size%22%3A2480%2C%22type%22%3A%22%22%2C%22ext%22%3A%22m%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221609673332117-1%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fm%2F1028501%2F1609673332402-f1e068ee-2e54-4634-9f46-350f98c51bd3.m%22%2C%22id%22%3A%22giKnA%22%2C%22card%22%3A%22file%22%7D)
