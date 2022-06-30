---
title: 相册选中iCloud上的视频和照片
urlname: hu8gg7
date: 2019-03-29 11:46:52 +0800
tags: [iOS]
categories: [移动端]
---

### 1、判断图片来源的枚举

<!-- more -->

```objectivec
typedef NS_OPTIONS(NSUInteger, PHAssetSourceType) {

    PHAssetSourceTypeNone            = 0,

    PHAssetSourceTypeUserLibrary     = (1UL << 0),

    PHAssetSourceTypeCloudShared     = (1UL << 1),

    PHAssetSourceTypeiTunesSynced    = (1UL << 2),

} PHOTOS_AVAILABLE_IOS_TVOS(9_0, 10_0);
```

### 2、判断是否是 iCloud 视频

```objc
PHVideoRequestOptions* options = [[PHVideoRequestOptions alloc] init];

//是否允许蜂窝数据下缓存视频
options.networkAccessAllowed = true;

options.version = PHVideoRequestOptionsVersionCurrent;

options.deliveryMode = PHVideoRequestOptionsDeliveryModeAutomatic;

options.progressHandler = ^(double progress, NSError *error, BOOL *stop, NSDictionary *info) {

    //视频正在从iCloud缓存到本地相册，进度回调
 };

PHAsset *phAsset = (PHAsset *)asset.phAsset;


[[[QMUIAssetsManager sharedInstance] phCachingImageManager] requestAVAssetForVideo:phAsset options:options resultHandler:^(AVAsset * _Nullable asset, AVAudioMix * _Nullable audioMix, NSDictionary * _Nullable info) {

    //视频缓存完成之后的回调，如果视频是从iCloud上下载下来的，点击选中视频也会进入这个回调

}];
```
