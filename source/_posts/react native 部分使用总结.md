---
title: react native 部分使用总结
urlname: dhdcy0
date: 2018-08-14 12:58:06 +0800
tags: [iOS,react native]
categories: [移动端]
---

### 混合开发

- 配置好 React Native 依赖和项目结构。
- 1、事件绑定 传参

<!-- more -->

```jsx
正确 不被立即执行
 <Text
         <Text
          style={styles.text}
          onPress={this.press0}
        >{this.state.data0}</Text>
---------------------


        <Text
          style={styles.text}
          onPress={() => this.press1()}
        >{this.state.data1}</Text>

---------------------

        <Text
          style={styles.text}
          onPress={this.press2.bind(this)}
        >{this.state.data2}</Text>
---------------------

        <Text
          style={styles.text}
          onPress={this.press3.bind(this, 2222)}
        >{this.state.data3}</Text>
        <Text
          style={styles.text}
          onPress={()=>this.press4(2222)}
        >{this.state.data4}</Text>

---------------------
```

- 2、原生跳转至 RN

```objc
#import <React/RCTRootView.h>

- (IBAction)highScoreButtonPressed:(id)sender {
    NSLog(@"High Score Button Pressed");
    NSURL *jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.bundle?platform=ios"];

    RCTRootView *rootView =
      [[RCTRootView alloc] initWithBundleURL: jsCodeLocation
                                  moduleName: @"RNHighScores"
                           initialProperties:
                             @{
                               @"scores" : @[
                                 @{
                                   @"name" : @"Alex",
                                   @"value": @"42"
                                  },
                                 @{
                                   @"name" : @"Joel",
                                   @"value": @"10"
                                 }
                               ]
                             }
                               launchOptions: nil];
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view = rootView;
    [self presentViewController:vc animated:YES completion:nil];
}
```

// 3、指定平台

```jsx
  row: {
    flex: 1,
    flexDirection: 'row',
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: '#FFFFFF',
    width: 686,
    height: 264,
    marginLeft: 16,
    marginRight: 16,
    marginTop: 32,

    ...Platform.select({
      ios: {
        shadowColor: '#000000',
        shadowOffset: {h: 0, w: 0},
        shadowRadius: 5,
        shadowOpacity: 0.1,
      },

      android: {
      }
    })

  },
```

// 4、判断控件 显示和隐藏

```jsx
 // 渲染购买数据
  renderPurchased({ item }) {

    if (item.class_name) {

      return (
        <View style={styles.section}>
          <Text style={styles.section_text}>{item.class_name}</Text>
        </View>
      )
    }

    return (
      <View style={styles.row}>
        <View style={styles.corner}>
          <Image source={{ uri: item.album_small_cover}} style={styles.corner_img}/>
          <View style={styles.corner_right}>
            <Text style={styles.corner_right_title}>{item.album_title}</Text>
            <Text style={styles.corner_right_date}>{item.goods_time}</Text>
            {item.studied_count >= 0 ? hiddenProgress(item) : null}
          </View>
        </View>
      </View>
    );
  }



  // 判断是否显示进度条
var hiddenProgress =  (item) => {

  return (
    <Text style={styles.corner_right_progress} hidden="">{`学习进度 ${item.studied_count} / ${item.album_total_num}`}</Text>
  )
}
```

// 5、属性计算

```jsx
// 判断是否显示进度条
var hiddenProgress = (item) => {
  if (item.studied_count >= 0) {
    return (
      <View style={styles.corner_right_bottom_view}>
        <Text
          style={[styles.corner_right_date, styles.corner_right_progress]}
          hidden=""
        >{`学习进度 ${item.studied_count} / ${item.album_total_num}`}</Text>
        <View style={styles.corner_right_bottom_progressView}>
          <View
            style={[
              styles.corner_right_top_progress,
              {
                width: calculateProgress(
                  item.studied_count,
                  item.album_total_num
                ),
              },
            ]}
          ></View>
        </View>
      </View>
    );
  }

  return null;
};

// 计算进度条value
var calculateProgress = (count, total) => {
  let num = (count / total) * 100;
  return `${num.toString()}%`;
};
```

// 6、引入图片

```jsx
<Image source={require('./img/check.png')} />


// 引入asset内图片 不带后缀，只写图片名

<Image source={{uri: 'app_icon'}} style={{width: 40, height: 40}} />

// 引入 @2x @3x 内的图片
├── button.js
└── img
    ├── check.png
    ├── check@2x.png
    └── check@3x.png

<Image source={require('./img/check.png')} />
```

// 7、指定读取缓存

```jsx
<Image
  source={{
    uri: "https://facebook.github.io/react/logo-og.png",
    cache: "only-if-cached",
  }}
  style={{ width: 400, height: 400 }}
/>
```

// 8、设置图片变量

```jsx
var icon = this.props.active
  ? require("./my-icon-active.png")
  : require("./my-icon-inactive.png");
<Image source={icon} />;
```

// 9、使用 base64 的图片

```jsx
// 请记得指定宽高！
<Image
  style={{
    width: 51,
    height: 51,
    resizeMode: "contain",
  }}
  source={{
    uri:
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAzCAYAAAA6oTAqAAAAEXRFWHRTb2Z0d2FyZQBwbmdjcnVzaEB1SfMAAABQSURBVGje7dSxCQBACARB+2/ab8BEeQNhFi6WSYzYLYudDQYGBgYGBgYGBgYGBgYGBgZmcvDqYGBgmhivGQYGBgYGBgYGBgYGBgYGBgbmQw+P/eMrC5UTVAAAAABJRU5ErkJggg==",
  }}
/>
```

- 10 运行

```bash

// 查看模拟器列表
xcrun simctl list devices

// 选择模拟器运行

react-native run-ios --simulator "iPhone 4s"
```

- 11、原生传值 加载 rn

```objc
- (void)viewDidLoad
{
  [...]
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                   moduleName:appName
                                            initialProperties:props];
  rootView.frame = CGRectMake(0, 0, self.view.width, 200);
  [self.view addSubview:rootView];
}


- (instancetype)initWithFrame:(CGRect)frame
{
  [...]

  _rootView = [[RCTRootView alloc] initWithBridge:bridge
  moduleName:@"FlexibilityExampleApp"
  initialProperties:@{}];

  _rootView.delegate = self;
  _rootView.sizeFlexibility = RCTRootViewSizeFlexibilityHeight;
  _rootView.frame = CGRectMake(0, 0, self.frame.size.width, 0);
}

#pragma mark - RCTRootViewDelegate
- (void)rootViewDidChangeIntrinsicSize:(RCTRootView *)rootView
{
  CGRect newFrame = rootView.frame;
  newFrame.size = rootView.intrinsicContentSize;

  rootView.frame = newFrame;
}
```

- 12、TouchableOpacity 点击事件

```jsx
<TouchableOpacity
  onPress={() => {
    Alert.alert(`${item.album_title}`);
  }}
></TouchableOpacity>
```
