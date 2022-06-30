---
title: iOS内购总结
urlname: ikyvut
date: 2018-07-21 12:58:06 +0800
tags: [iOS,苹果内购]
categories: [移动端]
---

## 内购逻辑图

<!-- more -->

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602930701163-4e439d69-c257-4998-9ce1-0eb1e7bddb14.png#align=left&display=inline&height=800&margin=%5Bobject%20Object%5D&originHeight=800&originWidth=1315&size=0&status=done&style=none&width=1315)

#### 1、内购的四种类型

- 消耗性项目：在支付成功以后，苹果后台就不会再返回该物品的票据信息。
- 非消耗性项目：支付成功以后，会永久保留该票据信息，每次校验都会返回。
- 自动续期订阅：支付成功以后，每次都会自动续期，每次订单都是同一个 ID，但标识不同，每次续期成功后返回该 ID
- 非续期订阅：支付成功以后，在有效期内，每次校验订单信息，都会返回处于有效期内订单信息，有效期过期之后不再返回。

#### 2、发起支付

- 此处是拿 iTunes 注册的商品 ID 去像苹果服务器支付

#### 3、 苹果服务器返回票据信息给 App

#### 4、App 将返回的票据信息发送给自家服务器

#### 5、App 服务器携带票据信息，向苹果服务器验证。（应放到服务端去进行验证，此处给一个例子 app 端向服务端验证）

```objectivec

- (void)verifyReceipt:(NSData *)receipt purchaseWithPaymentTransaction:(SKPaymentTransaction *)transaction isTestServer:(BOOL)flag {

        //交易验证
        NSError *error;
        NSDictionary *requestContents = @{

                                          @"receipt-data": [receipt base64EncodedStringWithOptions:0]

                                          };

        NSData *requestData = [NSJSONSerialization dataWithJSONObject:requestContents

                                                              options:0

                                                                error:&error];

        //In the test environment, use https://sandbox.itunes.apple.com/verifyReceipt

        //In the real environment, use https://buy.itunes.apple.com/verifyReceipt



        NSString *serverString = @"https://buy.itunes.apple.com/verifyReceipt";

        if (flag) {

                serverString = @"https://sandbox.itunes.apple.com/verifyReceipt";

        }

        NSURL *storeURL = [NSURL URLWithString:serverString];

        NSMutableURLRequest *storeRequest = [NSMutableURLRequest requestWithURL:storeURL];

        [storeRequest setHTTPMethod:@"POST"];

        [storeRequest setHTTPBody:requestData];



        NSOperationQueue *queue = [[NSOperationQueue alloc] init];

        [NSURLConnection sendAsynchronousRequest:storeRequest queue:queue

                               completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {



                                       if (connectionError) {

                                               // 无法连接服务器,购买校验失败

                                               [self finished:transaction content:nil withError:connectionError];

                                       } else {

                                               NSError *error;

                                               NSDictionary *jsonResponse = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];

                                               if (!jsonResponse) {

                                                       // 苹果服务器校验数据返回为空校验失败

                                                       NSError *recepitErr = [NSError errorWithDomain:PayiTunesErrorDomain code:-1 userInfo:@{NSLocalizedDescriptionKey : ErrorDesForCode(PayiTunesErrorCodeCloudServiceNetworkConnectionFailed)}];

                                                       [self finished:transaction content:nil withError:recepitErr];

                                               }

                                               NSLogDebug(@"----验证结果 %@",jsonResponse);



                                               // 先验证正式服务器,如果正式服务器返回21007再去苹果测试服务器验证,沙盒测试环境苹果用的是测试服务器

                                               NSString *status = [NSString stringWithFormat:@"%@",jsonResponse[@"status"]];

                                               if (status && [status isEqualToString:@"21007"]) {

                                                       [self verifyReceipt:receipt purchaseWithPaymentTransaction:transaction isTestServer:YES];

                                               } else if(status && [status isEqualToString:@"0"]){

                                                       [self finished:transaction content:nil withError:nil];

                                               }

                                       }

                               }];

}
```

#### 6、返回校验结果，服务器根据不同类型商品去选择对应处理方式

- 如果应用内都是消耗性物品购买。只需去 inapp 字段的第一个值就可以
- 如果应用内有第二种和第四种，就需要遍历循环 inapp 的字段处理，判断里面的 original_transaction_id,是否已经被使用过，如果使用过，忽略。如果未使用，标记对应状态，入库存储。
- 如果有第三种状态，同样遍历循环 inapp 字段，判断 original_transaction_id 和 transaction_id，是否需要增加续订日子(未测试)

#### 7、 如果是网络问题或者是服务器数据问题，导致客户端接收到错误信息，则该笔交易未完成，但是苹果后台的标示是已付款。则 app 再次向苹果服务器发起支付的时候，不会扣费，之后返回对应票据信息。

#### 8、举个例子：放到本地去向苹果服务器做验证

```objc

2018-07-21 16:29:24.180774+0800 NiuWaJiaoYu[2583:584940] ------------- 请求对应的产品信息--(
    "com.NiuWa.niuwaketang_yue"

) --------------

2018-07-21 16:29:25.295599+0800 NiuWaJiaoYu[2583:584940] --------------收到产品反馈消息---------------------

2018-07-21 16:29:25.295966+0800 NiuWaJiaoYu[2583:584940] =====================产品信息==================

2018-07-21 16:29:25.296181+0800 NiuWaJiaoYu[2583:584940] productID:(

)

2018-07-21 16:29:25.296263+0800 NiuWaJiaoYu[2583:584940] 产品付费数量:1

2018-07-21 16:29:25.296341+0800 NiuWaJiaoYu[2583:584940] [pro description] <SKProduct: 0x170200040>

2018-07-21 16:29:25.296438+0800 NiuWaJiaoYu[2583:584940] [pro localizedTitle]  牛娃课堂月会员

2018-07-21 16:29:25.296530+0800 NiuWaJiaoYu[2583:584940] [pro localizedDescription]  购买此会员后，可以在接下来一个月的时间内免费使用牛娃课堂所有学习资源。

2018-07-21 16:29:25.296655+0800 NiuWaJiaoYu[2583:584940] [pro price]  1598

2018-07-21 16:29:25.296705+0800 NiuWaJiaoYu[2583:584940] [pro productIdentifier]  com.NiuWa.niuwaketang_yue

2018-07-21 16:29:25.296818+0800 NiuWaJiaoYu[2583:584940] 商品添加进列表 com.NiuWa.niuwaketang_yue

2018-07-21 16:29:25.299607+0800 NiuWaJiaoYu[2583:584940] =====================产品信息==================

2018-07-21 16:29:25.299747+0800 NiuWaJiaoYu[2583:584940] ------------- 请求对应的产品信息完毕--<SKProductsRequest: 0x174432480> --------------

2018-07-21 16:29:29.770250+0800 NiuWaJiaoYu[2583:584940] [Harpy]: Please make sure that you have set _presentationViewController before calling checkVersion, checkVersionDaily, or checkVersionWeekly.

2018-07-21 16:29:32.144827+0800 NiuWaJiaoYu[2583:584940] [Harpy]: Please make sure that you have set _presentationViewController before calling checkVersion, checkVersionDaily, or checkVersionWeekly.

2018-07-21 16:29:32.870369+0800 NiuWaJiaoYu[2583:584940] 交易完成  com.NiuWa.niuwaketang_yue

2018-07-21 16:29:32.870486+0800 NiuWaJiaoYu[2583:584940] 交易结束,验证票据信息

2018-07-21 16:29:35.997449+0800 NiuWaJiaoYu[2583:584940] 拿到票据信息向苹果服务器验证：

 请求链接：https://sandbox.itunes.apple.com/verifyReceipt,

 请求头信息：{

},

 请求Method：POST,

 请求Body: {"receipt-data":"MIIomAYJKoZIhvcNAQcCoIIoiTCCKIUCAQExCzAJBgUrDgMCGgUAMIIYOQYJKoZIhvcNAQcBoIIYKgSCGCYxghgiMAoCAQgCAQEEAhYAMAoCARQCAQEEAgwAMAsCAQECAQEEAwIBADALAgELAgEBBAMCAQAwCwIBDgIBAQQDAgFrMAsCAQ8CAQEEAwIBADALAgEQAgEBBAMCAQAwCwIBGQIBAQQDAgEDMAwCAQoCAQEEBBYCNCswDQIBDQIBAQQFAgMBh88wDQIBEwIBAQQFDAMxLjAwDgIBCQIBAQQGAgRQMjUwMBgCAQICAQEEEAwOY29tLm5pdXdhLmlwYWQwGAIBAwIBAQQQDA4yMDE4MDMxNTEwMjE1NTAYAgEEAgECBBB7PLpH6Z\/pOfQeVikMoYe3MBsCAQACAQEEEwwRUHJvZHVjdGlvblNhbmRib3gwHAIBBQIBAQQU4I0JGhOO5ZGf5WnQHsj0Tu5hle8wHgIBDAIBAQQWFhQyMDE4LTA3LTIxVDA4OjI5OjMyWjAeAgESAgEBBBYWFDIwMTMtMDgtMDFUMDc6MDA6MDBaMEgCAQYCAQEEQBnfPN4Gyd3ojT7k+xTs7nQyCmfs\/kXep9X+eOFlzcr66F\/iZTc5dh31ejK0YTxvksuvLCFyJ8lrH\/kceHxnnxcwSgIBBwIBAQRCYr6RiZfopSC4\/wQoo0nUHRSYXCxiQgCdUkHp9iITsaOCKx\/7lVuPPsRRw6+Q3shOBnL9rQW4xHsStSmJOGfj0eLMMIIBXQIBEQIBAQSCAVMxggFPMAsCAgasAgEBBAIWADALAgIGrQIBAQQ

2018-07-21 16:29:38.168848+0800 NiuWaJiaoYu[2583:585724] ----验证结果 {

    environment = Sandbox;

    receipt =     {

        "adam_id" = 0;

        "app_item_id" = 0;

        "application_version" = 20180315102155;

        "bundle_id" = "com.niuwa.ipad";

        "download_id" = 0;

        "in_app" =         (

                        {

                "is_trial_period" = false;

                "original_purchase_date" = "2018-03-22 04:20:37 Etc/GMT";

                "original_purchase_date_ms" = 1521692437000;

                "original_purchase_date_pst" = "2018-03-21 21:20:37 America/Los_Angeles";

                "original_transaction_id" = 1000000384329524;

                "product_id" = "com.NiuWa.niuwaketang_ji";

                "purchase_date" = "2018-03-22 04:20:37 Etc/GMT";

                "purchase_date_ms" = 1521692437000;

                "purchase_date_pst" = "2018-03-21 21:20:37 America/Los_Angeles";

                quantity = 1;

                "transaction_id" = 1000000384329524;

            },

                        {

                "is_trial_
```

#### 9、拿到票据信息向苹果服务器做验证的状态码

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602930699959-7be52f08-a367-4fed-be19-7be9470ba251.png#align=left&display=inline&height=212&margin=%5Bobject%20Object%5D&originHeight=212&originWidth=714&size=0&status=done&style=none&width=714)

## iOS IAP 掉单处理

有哪些请况会发生掉单：
1、在 ApplePay 付款成功后由于网络或各种原因没有返回 Transaction（SKPaymentTransaction),从而不能得到凭证去 Apple 服务器验证订单的正确性。
2、苹果服务器成功返回了 Transaction,但是在 APP 在上传凭证给服务器时发生了网络或各种原因，造成了凭证的丢失，产生了掉单（用户付了款却没有得到相应的商品）

解决方案
[SKPaymentQueue defaultQueue]这个队列里面存着所有的已支付，未支付的订单，而且需要手动移除，而 APP 每次启动的时候都会去判断这个队列里面是否为空，如果不为空的话会调用代理的
// 此函数回调队列中所有订单信息

- (void)paymentQueue:(SKPaymentQueue \_)queue updatedTransactions:(NSArray \*)transactions
  所以我们可以把 AppDelegate 设置成这个协议的代理并实现这个方法，当然我一般是会写一个遵循的工具类单例，毕竟协议是一对一的，不管是哪里的支付回调，都只走这个类，统一处理。
  移除代码： [[SKPaymentQueue defaultQueue]finishTransaction:transaction]

当我们创建苹果订单初始化 SKPayment 时我们应该使用 SKMutablePayment,这个类里面有一个参数叫 applicationUsername 的成员变量，我们可以把后台服务器的订单号写到这里，在付款成功后返回的 SKPaymentTransaction 里面能拿到这个参数，然后就带着它去请求本地服务器

```
        CDDLog(@"=====订单号%@",tran.payment.applicationUsername);
```

附：[苹果官方内购验证文档](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html#//apple_ref/doc/uid/TP40010573-CH104-SW1)
