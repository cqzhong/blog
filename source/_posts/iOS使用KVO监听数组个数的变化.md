---
title: iOS使用KVO监听数组个数的变化
urlname: um2byr
date: 2019-11-12 11:00:00 +0800
tags: [iOS,KVO,数组]
categories: [移动端]
---

iOS 默认不支持对数组的 KVO,因为普通方式监听的对象的地址的变化，而数组地址不变，而是里面的值发生了改变

<!-- more -->

#### 要实现监听 KVO 监听数组个数变化，整个过程需要三个步骤(与普通监听一致)

- 1、建立观察者及观察的对象
- 2、处理 key 的变化(根据 key 的变化刷新 UI)
- 3、移除观察者

```objc

@interface CDDeleteModel : NSObject

@property (strong,nonatomic) NSMutableSet *modelDelMutaSet;

@end


@implementation CDDeleteModel

- (NSMutableSet *)modelDelMutaSet {
    if (!_modelDelMutaSet) _modelDelMutaSet = [NSMutableSet set];
    return _modelDelMutaSet;
}

@end
```

使用方法

```objc

#import "CDDeleteModel.h"

@property (nonatomic, strong) CDDeleteModel *delModel;


#define DEL_OPERATING_MODEL [self.delModel mutableSetValueForKeyPath:@"modelDelMutaSet"]


    [DEL_OPERATING_MODEL removeAllObjects];

    [DEL_OPERATING_MODEL addObjectsFromArray:self.downloadMutaArray];

    [DEL_OPERATING_MODEL addObject:_downloadMutaArray[indexPath.row]];

    [DEL_OPERATING_MODEL removeObject:_downloadMutaArray[indexPath.row]];

    if (![DEL_OPERATING_MODEL containsObject:self.downloadMutaArray[idxPath.row]]) {

        [self.tableView reloadRowAtIndexPath:idxPath withRowAnimation:UITableViewRowAnimationNone];
    }


//使用KVOController
#import <KVOController/KVOController.h>

#pragma mark - Private Method
- (void)setFBKVOControllerAction {

    @weakify(self);
    FBKVOController *KVOController = [FBKVOController controllerWithObserver:self];
    self.KVOController = KVOController;

    [self.KVOController  observe:self.delModel keyPath:@"modelDelMutaSet" options:NSKeyValueObservingOptionOld | NSKeyValueObservingOptionNew block:^(id  _Nullable observer, id  _Nonnull object, NSDictionary<NSString *,id> * _Nonnull change) {

        @strongify(self);

        //change[NSKeyValueChangeNewKey]

        //在这里监听到 [self.downloadMutaArray count]个数的变化
    }];
}
```
