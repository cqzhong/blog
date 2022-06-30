---
title: iOS-OC开发编码规范
urlname: tp50ia
date: 2020-09-18 15:04:11 +0800
tags: [iOS,代码规范]
categories: [移动端]
---

关于 Objective-C 的编码规范，苹果和谷歌都已经有很好的总结：

- [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
- [Google Objective-C Style Guide](https://google.github.io/styleguide/objcguide.html)

<!-- more -->

## 代码规范

### 头文件

```objc
@import AVFoundation; # 标准组件库库头文件，一般为苹果sdk自带的 framework
#import DKViewController.h # 非标准库头文件

全局宏

常量定义

全局变量

类定义
```

切记不要 `#import <AVFoundation/AVFoundation.h>`，在 iOS7 以后推荐用 [@import ](/import) module;来引用苹果的组件。

### 例子

```
@import AVFoundation;
#import "Test.h"

#define KTest 0

static CGFloat const kALAnimationDelay = 0.1;
static NSString *ID = @"cell";

@interface MyClass ()

@property (nonatomic) NSObject *test;

@end
```

### 类结构

使用 #pragma mark – xxx 来分类方法

```
#pragma mark – Getters and Setters # 懒加载

#pragma mark - Life Cycle # 实例生命周期

#pragma mark - Events # 事件

#pragma mark - Public Methods # .h 公开方法

#pragma mark - UITableViewDataSource # 系统代理方法
#pragma mark - UITableViewDelegate
#pragma mark - UITextFieldDelegate

#pragma mark - Custom Delegates # 自定义代理

(#pragma mark – Private Methods # 私有方法)
```

### Private Methods

正常情况下 ViewController 里面不应该写，能抽掉尽量抽掉

- 不是 delegate 方法的，也不是 event response 方法的，也不是 life cycle 方法的，就是 private method。
- 正常情况下 ViewController 里面是不会存在 private methods 的，这个 private methods 一般是用于日期换算、图片裁剪等的小功能。这种小功能要么把它写成一个 category，要么把它做成一个模块，哪怕这个模块只有一个函数也行。
- ViewController 基本上是大部分业务的载体，本身代码已经相当复杂，所以跟业务关联不大的东西，能不放在 ViewController 里面就不要放。另外一点，这个 private method 的功能这时候只是你用得到，但是将来说不定别的地方也会用到，一开始就独立出来，有利于将来的代码复用。

### 懒加载

很多开发工程师，初始化属性的位置比较随意，有单独添加一个初始化方法类似 setupView 的，有在 init 初始化的，各种情况都有，导致团队协作的时候代码显得非常乱。
初始化方式不一致，这样做非常有可能破坏了每个方法功能的单一性（每个方法只做一件事）。

```
@interface CustomObject ()

@property (nonatomic, strong) UILabel *label;

@end

@implementation

#pragma mark - getters and setters

- (UILabel *)label
{
    if (_label == nil) {
        _label = [[UILabel alloc] init];
        _label.text = @"1234";
        _label.font = [UIFont systemFontOfSize:12];
        ... ...
    }
    return _label;
}
@end

#pragma mark - life cycle

- (void)viewDidLoad
{
    [super viewDidLoad];
    [self.view addSubview:self.label];
}

- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];
    self.label.frame = CGRectMake(1, 2, 3, 4);
}
```

### 空格

使用空格而不是制表符 Tab
不要在工程里使用 Tab 键，使用空格来进行缩进。在 Xcode > Preferences > Text Editing 将 Tab 和自动缩进都设置为 4 个空格。（Google 的标准是使用两个空格来缩进，但这里还是推荐使用 Xcode 默认的设置。）
每个方法或者功能块之间为了结构清晰，应当只空一行。
该空格的地方要加空格，比如每个方法的开头，在 - 和 (return_type) 之间应该有且只有一个空格；写 property 时，严格按照下面的格式。

```
@interface SomeClass : NSObject

@property (noatomic, weak) UIView *aView

- (void)someMethod;

@end

@implementation SomeClass

- (void)setAView:(NSInteger )aview
{

}

- (void)someMethod
{

}
@end
```

### 方法书写

先不讨论一个方法带很多参数好不好，假设真的有很多参数导致方法名很长，应该用 : 对齐，每个参数一行，以提高可阅读性。

```
- (id)initWithModel:(IPCModle)model
        connectType:(IPCConnectType)connectType
         resolution:(IPCResolution)resolution
           authName:(NSString *)authName
           password:(NSString *)password
                mac:(NSString *)mac
               azIp:(NSString *)az_ip
              azDns:(NSString *)az_dns
              token:(NSString *)token
              email:(NSString *)email
           delegate:(id)delegate;
```

反之如果只有两三个参数，就不要这样分行写了，一行解决即可。

```
- (id)initWithAuthName:(NSString *)authName password:(NSString *)password;
```

### 方法调用

函数调用不要分行，统一单行写完，让编译器自动换行即可。特别是有 block 嵌套的时候，分行会非常难看。一般写完代码的时候可以用一个小技巧来格式化代码：选中代码，然后先按 Command + Alt + [，再按 Command + Alt + ]。就是把这一段代码往上移一行，再往下移一行，回到原来的样子，但是会经过 xCode 的排版，也就达到格式化代码的效果。

例如用:

```
// blocks are easily readable
 [UIView animateWithDuration:1.0 animations:^{
     // something
 } completion:^(BOOL finished) {
     // something
 }];
```

而不是

```
// colon-aligning makes the block indentation hard to read
 [UIView animateWithDuration:1.0
                  animations:^{
                      // something
                  }
                  completion:^(BOOL finished) {
                      // something
                  }];
```

### if 语句

表达式大括号和其他大括号(if/else/switch/while 等)总是在同一行语句打开但在新行中关闭。如果没有 else 并且括号内只有一行语句，可以和 if 语句同行，并且不需要括号。

```
if (user.isHappy) {
    //Do something
} else {
    //Do something else
}

if (somethingIsBad) return something;
```

### switch 语句

大括号在 case 语句中并不是必须的，除非编译器强制要求。当一个 case 语句包含多行代码时，大括号应该加上。

```
switch (condition) {
        case 1:
            // ...
            break;
        case 2: {
            // ...
            // Multi-line example using braces
            break;
        }
        case 3:
            // ...
            break;
        default:
            // ...
            break;
    }
```

当在 switch 使用枚举类型时，default 是不需要的。例如：

```

RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;
    switch (menuType) {
        case RWTLeftMenuTopItemMain:
            // ...
            break;
        case RWTLeftMenuTopItemShows:
            // ...
            break;
        case RWTLeftMenuTopItemSchedule:
            // ...
            break;
    }
```

### 注释

代码中尽量少注释，让代码能自我描述。不过当需要注释的时候，能需要清楚地解释某个代码块的含义和作用。注释应当保持最新，如果不必要请删除。

### 点语法

- 点语法是一种很方便封装访问方法调用的方式。当你使用点语法时，通过使用 getter 或 setter 方法，属性仍然被访问或修改。
- 点语法应该总是被用来访问和修改属性，因为它使代码更加简洁。[ ] 符号更偏向于用在其它例子。

例如用

```
view.backgroundColor = [UIColor orangeColor];
```

而不是

```
[view setBackgroundColor:[UIColor orangeColor]];
```

当然，也有特例，有些控件直接点语法是无效的，比如

```
btn.titleLabel.text = @"xxx";
```

是最容易被坑的，编译器不会告诉你错了，但按钮事件是分状态的，写 [ ] 就可以清楚地看到。正确地赋值语句应该是：

```
[btn setTitle:@"xxx" forState:UIControlEventTouchUpInside];
```

### Init 和 类构造方法

```
- (instancetype)init
{
  if (self = [super init]) {
    // ...
  }
  return self;
}
```

当类构造方法被使用时，它应该返回类型是 instancetype 而不是 id，这样确保编译器正确地推断结果类型。

```
@interface Airplane
+ (instancetype)airplaneWithTestType:(DKTestType)type;
@end
```

### RAC 规范

在使用 RAC 的方法的前一行要写@weakify(self); 在 RAC 方法回调中的第一行写 @strongify(self); weakify 和 strongify 一定是成双存在，weakify 在一个方法的作用域内只能写一次；而 strongify 在每个 RAC 的 block 回调中都要写一次，嵌套时只需要在最外层写一次。

```
@weakify(self);
[[self.loadMoreBtn rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(id x) {
    @strongify(self);
    [DKProgressHUD showLoadingToView:self.view];
    [[self.vm.loadWeekNewsCommand execute:@(++page)] subscribeNext:^(id x) {
        // ... 此处的 block 不需要写 strongify
    } error:^(NSError *error) {
        @strongify(self);
        // ...
    }];
}];
```

注意，跟 RAC 相关的对象，都要用 self.xxx 的方式去调用，不要写成变量，也不要用下划线去调用\_xxx，否则会导致循环引用引起控制器被强引用，最明显的结果就是不走 dealloc 方法，会导致很多问题，比如 RAC 重复绑定数据引起程序奔溃等。

在 viewModel 中定义 RACCommand 时，一定要写 [subscriber sendCompleted]; 否则会阻塞 UI 主线程，导致 command 只能 execute 一次。

```
@weakify(self);
_loadOldDataCommand = [[DKCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
    return [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        @strongify(self);
        self.items = DKGetCache(KITEM);
        [subscriber sendNext:nil];
        [subscriber sendCompleted];
        return nil;
    }];
}];
```

## 命名规范

### 命名原则

- 一般性原则：

  - 可读性高(简洁且清晰)和防止命名冲突(通过加前缀后缀来保证)。
  - Objective-C 的命名通常都比较长, 名称遵循驼峰式命名法. 一个好的命名标准很简单, 就是做到在开发者一看到名字时, 就能够懂得它的含义和使用方法. 另外, 每个模块都要加上自己的前缀, 前缀在编程接口中非常重要, 可以区分软件的功能范畴并防止不同文件或者类之间命名发生冲突, 比如 DKTableView、DKDataBase
    | 代码 | 是否正确规范 |
    | --- | --- |
    | insertObject:atIndex: | 正确 |
    | insert:at: | 不清晰，没有说明插入什么，插到哪里 |
    | removeObjectAtIndex: | 正确 |
    | removeObject: | 正确，因为⽅法是⽤来移除作为参数的对象 |
    | remove: | 不清晰，没有说明要移除什么 |

- 一致性
  尽可能与 Cocoa 编程接⼝命名保持一致。如果不太确定某个命名的⼀致性，请浏览头文件或参考文档中的范例，在使⽤多态方法的类中，命名的⼀致性⾮常重要。在不同类中实现相同功能的⽅法应该具有同的名称。
  | 代码 | 点评 |
  | --- | --- |
  | – (NSInteger)tag | 在 NSView, NSCell, NSControl 中有定义 |
  | – (void)setStringValue:(NSString \*) | 在许多 Cocoa classes 中都有定义 |

## 文件命名

文件的扩展名应该如下:

| 文件名规范 | 文件类型                     |
| ---------- | ---------------------------- |
| .h         | C / C++ / Objective-C 头文件 |
| .m         | Objective-C 实现文件         |
| .mm        | Objective-C / C++ 实现文件   |
| .cc        | 纯 C++ 实现文件              |
| .c         | 纯 C 实现文件                |

分类的文件名应该包含被扩展的类名，如：NSString+DKUtils.h 或 NSTextView+DKAutocomplete.h。

## 编码命名

### 类的命名

- 类名（以及类别、协议名）应首字母大写，并以驼峰格式分割单词。
- 所有类名、枚举、结构、protocol 定义时最好加一个统一的标示符，可以是项目缩写，或者个人项目的名称缩写，目前规定都加上大写的 DK 作为根前缀。
- 根据功能模块可以给功能模块的类添加功能模块的名称前缀，如用户中心的控制器，属于 Profile 模块，可以命名为 DKProfileViewController。

### 声明定义

- 所有 protocol 定义时，都加上后缀 Delegate。
  比如 DKRefreshViewDelegate，表示 RefreshView 的协议
- 所有的通知名都加上 Notification，通知名格式为：
  [ 触发通知的类名 ] + [Did/Will] + [ 动作 ] + Notification

例如：

```
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```

### 方法命名

- 小驼峰原则
  方法名应遵守小驼峰原则首字母小写，其他单词首字母大写,每个空格分割的名称以动词开头。执行性的方法应该以动词开头，小写字母开头，返回性的方法应该以返回的内容开头，但之前不要加 get。

```
- (void)insertModel:(id)model atIndex:(NSUInteger)atIndex;

- (instancetype)arrayWithArray:(NSArray *)array;
```

- 代理方法
  以发送代理的对象类名作为代理方法名的开始（去掉类名的前缀，并且小写开头）

```
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;

- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
```

### 枚举命名

```
typedef NS_ENUM(NSInteger, DKGlobalConstants) {
   DKPinSizeMin = 1,
   DKPinSizeMax = 5,
   DKPinCountMin = 100,
   DKPinCountMax = 500,
};
```

不使用下面这种方式

```
enum GlobalConstants {
   kMaxPinSize = 5,
   kMaxPinCount = 500,
};
```

### 宏命名

以大写 K 开头，后面单词首字母大写，其余小写。如：#define KMaxHeigt 100.0

### 属性变量命名

变量名使用小驼峰法, 使变量名尽量可以推测其用途属性具有描述性。别一心想着少打几个字母，让你的代码可以迅速被理解更加重要。

每个属性命名都加上类型后缀，如按钮就加上 btn 后缀，imageView 就加上 imgView。

如果是布尔值的成员变量，要定义 getter 方法 为 isXXX

```
@property (nonatomic, assign, getter=isEditable) BOOL editable;
```

| 标题         | 说明                               |
| ------------ | ---------------------------------- |
| 类成员变量名 | 成员变量用小驼峰法命名并前缀下划线 |
| 局部变量名   | 遵守小驼峰命名规则                 |
| const 常量   | 以小写 k 开头，后面单词首字母大写  |

## 项目编码规范

### Controller 编码规范

- 传统的 Controller 层代码都是非常臃肿的，在快速迭代开发中就显的非常恶心，维护代码非常不便，所以编码中要尽量使控制器瘦身。
- 在 Controller 中，瘦身的方法有很多种，例如架构层用 MVVM 可以从 Controller 中抽离部分代码到 ViewModel 中，但是力度还是不够。目前比较好的做法是借鉴组件化开发的思想，把一个界面分为几大块，将 view 的具体操作和布局分离出控制器，变成一个个 view 组件，控制器要做的就是拼装并利用 Masonry 布局好这些 view 组件。
- 控制器要不要调用调用接口看具体需求，能放到 view 中做就尽量在 view 中做，瘦身彻底一些。

```
#pragma mark - Life Cycle 生命周期

- (void)viewDidLoad
{
    [super viewDidLoad];

    [self setUpView];
}

- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];

    [self events];
}

#pragma mark - Events 事件
// 设置 view
- (void)setUpView
{
    self.view.backgroundColor = [UIColor whiteColor];

    @weakify(self);
    // 第一个 view
    UIView *firstView = [[UIView alloc] init];
    [self.view addSubview:firstView];
    [firstView mas_makeConstraints:^(MASConstraintMaker *make) {
        @strongify(self);
        make.top.left.right.equalTo(self.view);
        make.height.mas_equalTo(100);
    }];

    //  第二个 view
    UIView *secondView = [[UIView alloc] init];
    [self.view addSubview:secondView];
    [secondView mas_makeConstraints:^(MASConstraintMaker *make) {
        @strongify(self);
        make.top.equalTo(firstView.mas_bottom);
        make.left.right.equalTo(self.view);
        make.height.mas_equalTo(100);
    }];

    //  第三个 view
    UIView *thridView = [[UIView alloc] init];
    [self.view addSubview:thridView];
    [thridView mas_makeConstraints:^(MASConstraintMaker *make) {
        @strongify(self);
        make.top.equalTo(firstView.mas_bottom);
        make.left.right.equalTo(self.view);
        make.height.mas_equalTo(100);
    }];
}

// 事件
- (void)events
{
    // 调用接口
    [[DKService fetchSomething] subscribeNext:^(id x) {
        // 得到接口数据
    }];
}
```

### View 编码规范

- view 中做的事情会比传统的只做布局会复杂一些，像一些跳转和调用 api 接口都需要由它完成。
- view 在做这些操作的时候会遇到一些问题，例如 view 中跳转问题，数据传传递问题。这些都很容易解决，跳转问题需要拿到当前控制器的根控制器去跳转，有两种情况：

1. 没有 tabbarController 拿到根控制器：`(UINavigationController *)[UIApplication sharedApplication].keyWindow.rootViewController`
1. 有 tabbarController 拿到根控制器：

```
(UINavigationController *)((UITabBarController *)[UIApplication sharedApplication].keyWindow.rootViewController).selectedViewController
```

- 数据传递一般是使用 Notification，使用前一定要注意命名规范：

[ 触发通知的类名 ] + [Did/Will] + [ 动作 ] + Notification
