---
title: iOS后台下载注意问题
urlname: qdu1bx
date: 2019-03-07 11:46:52 +0800
tags: [iOS]
categories: [移动端]
---

### 设置 session

<!-- more -->

```objectivec

AppDelegate中
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

 1、启动App时候设置下载的NSURLSession，这时候如果上次下载内容未保存，会进入NSURLSessionDownloadDelegate的代理方法 URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task didCompleteWithError:(NSError *)error
 2、存在error，从error中取出 NSData *resumeData = error ? [error.userInfo objectForKey:NSURLSessionDownloadTaskResumeData]:nil; 然后怼resumeData进行存储，便于恢复下载时候找到NSURLSessionDownloadTask
    [[CDDownloadTool sharedDownloadTool] configureBackroundSession];
}
```

- 设置下载标示 - (void)application:(UIApplication \*)application handleEventsForBackgroundURLSession:(NSString \_)identifier
  completionHandler:(void (^)(void))completionHandler 内设置后台下载完成标识符。
- 在清除缓存文件时候 注意 Library - cache - com.apple.nsurlsessiond 的文件夹，清除后一定要重建，不然会造成下载失败。
