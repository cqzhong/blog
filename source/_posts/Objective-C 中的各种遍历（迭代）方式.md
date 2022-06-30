---
title: Objective-C 中的各种遍历（迭代）方式
urlname: pwbl77
date: 2019-10-12 21:41:04 +0800
tags: [遍历,iOS]
categories: [移动端]
---

#### 使用 `for` 循环

要遍历字典、数组或者是集合，`for` 循环是最简单也用的比较多的方法，示例如下：

<!-- more -->

```javascript
// 普通的for循环遍历
-(void)iteratorWithFor {
    // 处理数组
    NSArray *arrayM = @[@"1",@"2",@"3",@"4"];
    NSInteger arrayMCount = [arrayM count];
    for (int i = 0; i<arrayMCount; i++) {
        NSString *obj = arrayM[i];
        NSLog(@"%@",obj);
    }

    // 处理字典
    NSDictionary *dictM = @{@"1":@"one",@"2":@"two",@"3":@"three"};
    NSArray *dictKeysArray = [dictM allKeys];
    for (int i = 0; i<dictKeysArray.count; i++) {
        NSString *key = dictKeysArray[i];
        NSString *obj = [dictM objectForKey:key];
        NSLog(@"%@:%@",key,obj);
    }

    // 处理集合
    NSSet * setM = [[NSSet alloc] initWithObjects:@"one",@"two",@"three",@"four", nil];
    NSArray *setObjArray = [setM allObjects];
    for (int i = 0; i<setObjArray.count; i++) {
        NSString *obj = setObjArray[i];
        NSLog(@"%@",obj);
    }

    // 反向遍历----降序遍历----以数组为例
    NSArray *arrayM2 = @[@"1",@"2",@"3",@"4"];
    NSInteger arrayMCount2 = [arrayM2 count] - 1;

    for (NSInteger i = arrayMCount2; i>0; i--) {
        NSString *obj = arrayM2[i];
        NSLog(@"%@",obj);
    }
}
```

- 优点：简单
- 缺点：由于字典和集合内部是无序的，导致我们在遍历字典和集合的时候需要借助一个新的『数组』作为中介来处理，多出了一部分开销。

#### 二、使用 `NSEnumerator` 遍历

`NSEnumerator` 的使用和基本的 for 循环类似，不过代码量要大一些。示例如下：

```javascript
// 使用NSEnumerator遍历
-(void)iteratorWithEnumerator {
    // 处理数组
    NSArray *arrayM = @[@"1",@"2",@"3",@"4"];
    NSEnumerator *arrayEnumerator = [arrayM objectEnumerator];
    NSString *obj;
    while ((obj = [arrayEnumerator nextObject]) != nil) {
        NSLog(@"%@",obj);
    }

    // 处理字典
    NSDictionary *dictM = @{@"1":@"one",@"2":@"two",@"3":@"three"};
    NSEnumerator *dictEnumerator = [dictM keyEnumerator];
    NSString *key;
    while ((key = [dictEnumerator nextObject]) != nil) {
        NSString *obj = dictM[key];
        NSLog(@"%@",obj);
    }


    // 处理集合
    NSSet * setM = [[NSSet alloc] initWithObjects:@"one",@"two",@"three",@"four", nil];
    NSEnumerator *setEnumerator = [setM objectEnumerator];
    NSString *setObj;
    while ((setObj = [setEnumerator nextObject]) != nil) {
        NSLog(@"%@",setObj);
    }


    // 反向遍历----降序遍历----以数组为例
    NSArray *arrayM2 = @[@"1",@"2",@"3",@"4"];
    NSEnumerator *arrayEnumerator2 = [arrayM2 reverseObjectEnumerator];
    NSString *obj2;
    while ((obj2 = [arrayEnumerator2 nextObject]) != nil) {
        NSLog(@"%@",obj2);
    }
}
```

- 优点：对于不同的数据类型，遍历的语法相似；内部可以简单的通过 reverseObjectEnumerator 设置进行反向遍历。
- 缺点：代码量稍大。

#### 三、使用 `for...in` 遍历

在 `Objective-C 2.0` 中增加了 `for ...in` 形式的快速遍历。此种遍历方式语法简洁，速度飞快。示例如下：

```javascript
// 使用for...In进行快速遍历
-(void)iteratorWithForIn {
    // 处理数组
    NSArray *arrayM = @[@"1",@"2",@"3",@"4"];
    for (id obj in arrayM) {
        NSLog(@"%@",obj);
    }

    // 处理字典
    NSDictionary *dictM = @{@"1":@"one",@"2":@"two",@"3":@"three"};
    for (id obj in dictM) {
        NSLog(@"%@",dictM[obj]);
    }

    // 处理集合
    NSSet * setM = [[NSSet alloc] initWithObjects:@"one",@"two",@"three",@"four", nil];
    for (id obj in setM) {
        NSLog(@"%@",obj);
    }

    // 反向遍历----降序遍历----以数组为例
    NSArray *arrayM2 = @[@"1",@"2",@"3",@"4"];
    for (id obj in [arrayM2 reverseObjectEnumerator]) {
        NSLog(@"%@",obj);
    }
}
```

- 优点：
  1.  语法简洁；
  1.  效率最高；
- 缺点：无法获得当前遍历操作所针对的下标。

#### 四、基于 Block 的遍历方式

基于 Block 的方式来进行遍历是最新引入的方法。它提供了遍历数组|字典等类型数据的最佳实践。示例如下：

```javascript
// 基于块（block）的遍历方式
-(void)iteratorWithBlock {
    // 处理数组
    NSArray *arrayM = @[@"1",@"2",@"3",@"4"];
    [arrayM enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"%zd--%@",idx,obj);
    }];

    // 处理字典
    NSDictionary *dictM = @{@"1":@"one",@"2":@"two",@"3":@"three"};
    [dictM enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        NSLog(@"%@:%@",key,obj);
    }];

    // 处理集合
    NSSet * setM = [[NSSet alloc] initWithObjects:@"one",@"two",@"three",@"four", nil];
    [setM enumerateObjectsUsingBlock:^(id  _Nonnull obj, BOOL * _Nonnull stop) {
        NSLog(@"%@",obj);
    }];

    // 反向遍历----降序遍历----以数组为例
    NSArray *arrayM2 = @[@"1",@"2",@"3",@"4"];
    [arrayM2 enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"%zd--%@",idx,obj);
    }];
}
```

- 优点：
  1.  遍历时可以直接从 block 中获得需要的所有信息，包括下标、值等。特别相对于字典而言，不需要做多余的编码即可同时获得 key 和 value 的值。
  1.  能够直接修改 block 中 key 或者 obj 的类型为真实类型，可以省去类型转换的工作。
  1.  可以通过`NSEnumerationConcurrent`枚举值开启并发迭代功能。
- 说明
  基于 Block 的遍历方式在实现反向遍历的时候也非常简单，使用`enumerateObjectsWithOptions`方法，传递`NSEnumerationReverse`作为参数即可，在处理遍历操作的时候推荐基于 Block 的遍历方式。

#### 五、使 GCD 中的`dispatch_apply`函数

使用`GCD`中的`dispatch_apply`函数也能实现字典、数组等的遍历，该函数比较适合处理耗时较长、迭代次数较多的情况。示例如下：

```javascript
// 使用GCD中的dispatch_apply函数
-(void)iteratorWithApply {
    // 处理数组
    NSArray *arrayM = @[@"1",@"2",@"3",@"4"];

    // 获得全局并发队列
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);

    dispatch_apply(arrayM.count, queue, ^(size_t index) {
        NSLog(@"%@--%@",arrayM[index],[NSThread currentThread]);
    });
}
```

- 优点：开启多条线程并发处理遍历任务，执行效率高。
- 缺点：
  1.  对于字典和集合的处理需借助数组；
  1.  无法实现反向遍历。
