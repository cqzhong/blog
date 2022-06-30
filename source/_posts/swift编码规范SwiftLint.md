---
title: swift编码规范SwiftLint
urlname: gpzvcg
date: 2021-01-20 20:00:00 +0800
tags: [Swift,iOS]
categories: [移动端]
---

[本文转载自谢什么](https://www.yuque.com/xietian-dz3wk/ygtg29/rxbukg)

### 什么是 SwiftLint？

实施 Swift 样式和约定的工具。它是一个基于 GitHub 的 Swift Style Guide 松散地实施 Swift 样式和约定的工具，它与 Clang 和 SourceKit 挂钩，可以使用源文件的 AST 表示获得更准确的结果。（访问：[https://github.com/realm/SwiftLint](https://github.com/realm/SwiftLint)）

<!-- more -->

SwiftLint 使用简单，与 Xcode 紧密结合，更适用于包含 ObjC 和 Swift 代码的大型混合项目中。

![791315-30d711a690490fe9.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572918721104-e3141d8c-a3c3-4be9-b771-2bd64137c27f.webp#align=left&display=inline&height=349&margin=%5Bobject%20Object%5D&name=791315-30d711a690490fe9.webp&originHeight=675&originWidth=1200&size=22622&status=done&style=none&width=620)

# 使用场景

熟悉 Python 的同学一定对 Pylint 不会陌生，Pylint 是一个 Python 代码分析工具，它分析 Python 代码中的错误，查找不符合代码风格标准（Pylint 默认使用的代码风格是 PEP 8，具体信息，请参阅参考资料）和有潜在问题的代码。

Python 是一门很强调格式的语言，毕竟人家连大括号都没有，反观 Swift，似乎不按照严格的规范编码也没有太大的问题？错！写出良好代码风格的代码，可能比能否写出高效的代码更为重要。于是，SwiftLint 就诞生了。

SwiftLint 是一个强制使用者按照 Github 的 Swift 编码规范指南来开发的一种工具，它会将所有不符合 Swift 规范的代码全部用 warning 标注出来，一些严重的违背规则的代码甚至让它无法通过编译（江山一片红），想想是不是就很刺激呢？

![screenshot.png](https://cdn.nlark.com/yuque/0/2019/png/302712/1572919742747-f3cb431a-d025-452a-b8f3-c944682c4762.png#align=left&display=inline&height=196&margin=%5Bobject%20Object%5D&name=screenshot.png&originHeight=196&originWidth=1312&size=72804&status=done&style=none&width=1312)

### CodeReview 一般做这样几件事情：

1. 代码风格规范统一
1. 防止低效代码、冗余代码
1. 防止出现可见的明显 BUG

第一点，实在属于机械操作，就好像第一次工业革命的人工织布，是处在必然被淘汰的行列。
第二、三点富含了大量人脑的思考，属于暂时还无法替代的操作（AI 时代应该可以）。

# 常用安装

### 版本要求

SwiftLint 工作于 SourceKit 这一层，所以 Swift 版本发生变化时它也能继续工作！
这也是 SwiftLint 轻量化的原因，因为它不需要一个完整的 Swift 编译器，它只是与已经安装在你的电脑上的官方编译器进行通信。

| Swift 版本      | 最后一个 SwiftLint 支持版本 |
| --------------- | --------------------------- |
| Swift 1.x       | SwiftLint 0.1.2             |
| Swift 2.x       | SwiftLint 0.18.1            |
| Swift 3.x       | SwiftLint 0.25.1            |
| Swift 4.0-4.1.x | SwiftLint 0.28.2            |
| Swift 4.2.x     | SwiftLint 0.35.0            |
| Swift 5.x       | 最新的                      |

#### (1) 全局安装

全局安装就不得不提到我们的老朋友 HomeBrew 了，如果你没有安装，那么请看[这里](https://link.jianshu.com/?t=https://brew.sh/index_zh-cn.html)。
全局安装非常简单,首先我们需要通过 `brew` 命令安装 `SwiftLint`:

```
brew install swiftlint
```

然后添加编译脚本：

```
# Swiftlint
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

最后在脚本框中添加后:

![截屏2019-11-05上午10.10.40.png](https://cdn.nlark.com/yuque/0/2019/png/302712/1572919859754-b78f90c5-829b-4f72-ad41-26e0d53db5c2.png#align=left&display=inline&height=258&margin=%5Bobject%20Object%5D&name=%E6%88%AA%E5%B1%8F2019-11-05%E4%B8%8A%E5%8D%8810.10.40.png&originHeight=628&originWidth=1374&size=128496&status=done&style=none&width=565)

OK，大功告成，就可以编译啦

#### (2) 局部安装

除了全局安装，我们也可以通过 CocoaPods 进行安装。在 Podfile 上添加相关的依赖:

```
pod 'SwiftLint'
```

然后跟全局方法一样，在 `Run Script` 中添加命令，但是内容有些许的不同:

```
# Swiftlint Pods
"${PODS_ROOT}/SwiftLint/swiftlint"
```

![截屏2019-11-05上午10.13.18.png](https://cdn.nlark.com/yuque/0/2019/png/302712/1572920007136-9962cfef-a7fb-40c3-97c5-2fb27579cf36.png#align=left&display=inline&height=480&margin=%5Bobject%20Object%5D&name=%E6%88%AA%E5%B1%8F2019-11-05%E4%B8%8A%E5%8D%8810.13.18.png&originHeight=480&originWidth=1368&size=84763&status=done&style=none&width=1368)

OK,接下来就让我们来试试看 SwiftLint 吧！

#### (3) fastlane 安装

你可以用[fastlane 官方的 SwiftLint 功能](https://docs.fastlane.tools/actions/swiftlint)来运行 SwiftLint 作为你的 Fastlane 程序的一部分

```
swiftlint(
    mode: :lint,                            # SwiftLint模式: :lint (默认) 或者 :autocorrect
    executable: "Pods/SwiftLint/swiftlint", # SwiftLint的程序路径 (可选的). 对于用CocoaPods集成SwiftLint时很重要
    path: "/path/to/lint",                  # 特殊的检查路径 (可选的)
    output_file: "swiftlint.result.json",   # 检查结果输出路径 (可选的)
    reporter: "json",                       # 输出格式 (可选的)
    config_file: ".swiftlint-ci.yml",       # 配置文件的路径 (可选的)
    files: [                                # 指定检查文件列表 (可选的)
        "AppDelegate.swift",
        "path/to/project/Model.swift"
    ],
    ignore_exit_status: true,               # 允许fastlane可以继续执行甚至是Swiftlint返回一个非0的退出状态(默认值: false)
    quiet: true,                            # 不输出像‘Linting’和‘Done Linting’的状态日志 (默认值: false)
    strict: true                            # 发现警告时报错? (默认值: false)
)
```

> 官方还介绍 Mint 等安装方法，用得到自己去看吧~

# 使用方法

### 命令行

```
$ swiftlint help
Available commands:

   autocorrect  Automatically correct warnings and errors
   help         Display general or command-specific help
   lint         Print lint warnings and errors for the Swift files in the current directory (default command)
   rules        Display the list of rules and their identifiers
   version      Display the current version of SwiftLint
```

### 项目集成

在给项目初次接入 SwiftLint 的时候，你可能会被下面这样的情景给吓到：

![791315-482c9921b7c884e8.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572920357837-4291e556-9610-404a-8f18-c6234bab1d8d.webp#align=left&display=inline&height=40&margin=%5Bobject%20Object%5D&name=791315-482c9921b7c884e8.webp&originHeight=72&originWidth=830&size=4510&status=done&style=none&width=457)

但是呢，不用慌，我们可以看一下这是什么情况。仔细观察一番之后，我们会发现，绝大多数的 Warning 都是这个原因：

![791315-af68b2e2062ac1e3.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572920388360-6755c3d0-0495-45b8-8657-c55387c6b0bd.webp#align=left&display=inline&height=120&margin=%5Bobject%20Object%5D&name=791315-af68b2e2062ac1e3.webp&originHeight=120&originWidth=1200&size=20188&status=done&style=none&width=1200)

WTF？我这一行哪里有空格符？简直就是冤枉啊！但其实是这样的，虽然看起来好像没有空格符，但是在换行符之间不应该掺杂制表符，也就是说你实际的代码是这样的:

```
\n \tab \n
```

所以呢，人家的 Warning 也不是没有道理的嘛。那么我们该怎么来消除这些 Warning 呢？
首先，我们要确保，以后我们所码的代码不再出现这样的格式，所以我们需要把 `Text Editing` 里面的 `Including whitespace-only lines` 勾选上:

![WeWork Helper20191105102141.png](https://cdn.nlark.com/yuque/0/2019/png/302712/1572920517418-157dd3f8-76a3-4fed-b28c-485b541dc6e2.png#align=left&display=inline&height=370&margin=%5Bobject%20Object%5D&name=WeWork%20Helper20191105102141.png&originHeight=1116&originWidth=1600&size=267071&status=done&style=none&width=531)

这样可以保证我们今后的换行不再出现制表符的警告，然后，我们就需要处理现有的代码。现有的代码，少说也有几万行，手动改不是要累死人？这个时候就轮到 SwiftLint 的命令行出场了。

我们将目录切换到工程的根目录之下，然后敲击如下命令:

```
swiftlint autocorrect
```

然后我们就会发现，所有的空格符 Warning 都消失了。这都得益于我们刚刚所进行的命令行操作，它会将已知的能够自动修复的 Error 和 Warning 都自动修复，大大的减轻了我们的工作量。

### 配置 .swiftlint.yml 文件

但是又出现了一个问题，在项目的根目录之下执行自动纠正，SwiftLint 会将 Pods 文件夹中的 Swift 文件也一起纠正了，第三方的框架原则上能不动就不动，那么我们该怎么办呢？

先给规则文档，用到自取：[https://github.com/realm/SwiftLint/blob/master/Rules.md](https://github.com/realm/SwiftLint/blob/master/Rules.md)

这个时候就需要 .swiftlint.yml 文件了。那么这是一个什么文件呢？在使用 SwiftLint 的时候，很多时候我们会碰到一些自定义的规则需求，这个时候就需要 .swiftlint.yml 来解决问题了。

1. 打开终端, cd 到项目根目录下
1. 输入: touch .swiftlint.yml
1. 执行完该命令后, 在文件夹中你可能找不到该 yml 格式文件,那是因为文件被隐藏了
1. 关于隐藏/显示隐藏文件(命令一样): command + shift + .

下面我们来认识一下主要的几个配置选项，其实看注释就基本明白了。

```
disabled_rules: # 执行时排除掉的规则
  - colon
  - comma
  - control_statement
opt_in_rules: # 一些规则仅仅是可选的
  - empty_count
  - missing_docs
  # 可以通过执行如下指令来查找所有可用的规则:
  # swiftlint rules
included: # 执行 linting 时包含的路径。如果出现这个 `--path` 会被忽略。
  - DemoPro
excluded: # 执行 linting 时忽略的路径。 优先级比 `included` 更高。
  - Carthage
  - Pods
  - Source/ExcludedFolder
  - Source/ExcludedFile.swift

# 可配置的规则可以通过这个配置文件来自定义
# 二进制规则可以设置他们的严格程度
force_cast: warning # 隐式
force_try:
  severity: warning # 显式
# 同时有警告和错误等级的规则，可以只设置它的警告等级
# 隐式
line_length: 110
# 可以通过一个数组同时进行隐式设置
type_body_length:
  - 300 # warning
  - 400 # error
# 或者也可以同时进行显式设置
file_length:
  warning: 500
  error: 1200
# 命名规则可以设置最小长度和最大程度的警告/错误
# 此外它们也可以设置排除在外的名字
type_name:
  min_length: 4 # 只是警告
  max_length: # 警告和错误
    warning: 40
    error: 50
  excluded: iPhone # 排除某个名字
identifier_name:
  min_length: # 只有最小长度
    error: 4 # 只有错误
  excluded: # 排除某些名字
    - id
    - URL
    - GlobalAPIKey
reporter: "xcode" # 报告类型 (xcode, json, csv, checkstyle)
```

参考模板

```
disabled_rules: # rule identifiers to exclude from running
  - colon
  - comma
  - control_statement
  - identifier_name #rule for checking variable conditions (Upper case , lower case , underscore )
  - force_cast
  - shorthand_operator

cyclomatic_complexity:
  warning: 25 # two nested ifs are acceptable
  error: 50   # six nested ifs shows warning, 6 causes compile error


opt_in_rules: # some rules are only opt-in
  # - empty_count
  # Find all the available rules by running:
  # swiftlint rules

#included: # paths to include during linting. `--path` is ignored if present.
#  - Source

excluded: # paths to ignore during linting. Takes precedence over `included`.
  - Carthage
  - Pods
  - AppFolder\ App/Class/*
 # - AppFolder\ App/ViewController/* //Enabled for this

analyzer_rules: # Rules run by `swiftlint analyze` (experimental)
  - explicit_self

# configurable rules can be customised from this configuration file
# binary rules can set their severity level
# force_cast: warning # implicitly
force_try:
  severity: warning # explicitly

# rules that have both warning and error levels, can set just the warning level
# implicitly

line_length: 200
# they can set both implicitly with an array

type_body_length:
  - 300 # warning
  - 600 # error
# or they can set both explicitly

file_length:
  warning: 500
  error: 2500

function_body_length:
  - 200 #warning
  - 300 #error

# naming rules can set warnings/errors for min_length and max_length
# additionally they can set excluded names

type_name:
  min_length: 4 # only warning
  max_length: # warning and error
    warning: 40
    error: 50
  excluded: iPhone # excluded via string
  allowed_symbols: ["_"] # these are allowed in type names
identifier_name:
  min_length: # only min_length
    error: 4 # only error
  excluded: # excluded via string array
    - id
    - URL
    - GlobalAPIKey

identifier_name:
#  allowed_symbols: "_"
  max_length:
    warning: 45
    error: 60
  min_length:
    warning: 1


reporter: "xcode" # reporter type (xcode, json, csv, checkstyle, junit, html, emoji, sonarqube, markdown)
```

### 源码注释

可以通过在一个源文件中定义一个如下格式的注释来关闭某个规则：

```
// swiftlint:disable <rule>
```

在该文件结束之前或者在定义如下格式的匹配注释之前，这条规则都会被禁用：

```
// swiftlint:enable <rule>
```

例如：

```
// swiftlint:disable opening_brace
func initTakeScreenshot(launchOptions: [AnyHashable: Any]?){
    // swiftlint:enable opening_brace
    if let options = launchOptions {
        let userInfo = options[UIApplicationLaunchOptionsKey.remoteNotification]
        NotificationCenter.default.post(name: Notification.Name.UIApplicationUserDidTakeScreenshot, object: userInfo)
    }
}
```

规则关闭之前

![](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572920873314-86096a28-6d9d-47cd-855a-26992d79eb2b.webp#align=left&display=inline&height=64&margin=%5Bobject%20Object%5D&originHeight=64&originWidth=1200&size=0&status=done&style=none&width=1200)

规则关闭之后

![](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572920873272-f7461037-4078-4279-9014-9ab80d434004.webp#align=left&display=inline&height=88&margin=%5Bobject%20Object%5D&originHeight=88&originWidth=1200&size=0&status=done&style=none&width=1200)

也可以通过添加 :previous, :this 或者 :next 来使关闭或者打开某条规则的命令分别应用于前一行，当前或者后一行代码。

例如:

```
// swiftlint:disable:next force_cast
let noWarning = NSNumber() as! Int
let hasWarning = NSNumber() as! Int
let noWarning2 = NSNumber() as! Int // swiftlint:disable:this force_cast
let noWarning3 = NSNumber() as! Int
// swiftlint:disable:previous force_cast
```

> 建议使用代码块来简化操作吧~

### 嵌套配置

SwiftLint 支持通过嵌套配置文件的方式来对代码分析过程进行更加细致的控制。

- 在你的根 .swiftlint.yml 文件里设置 use_nested_configs: true 值。
- 在目录结构必要的地方引入额外的 .swiftlint.yml 文件。
- 每个文件被检查时会使用在文件所在目录下的或者父目录的更深层目录下的配置文件。否则根配置文件将会生效。
- excluded，included，和 use_nested_configs 在嵌套结构中会被忽略。

### 自动更正

- `SwiftLint` 可以自动修正某些错误，磁盘上的文件会被一个修正后的版本覆盖。
- 请确保在对文件执行 `swiftlint autocorrect` 之前有对它们做过备份，否则的话有可能导致重要数据的丢失。
- 因为在执行自动更正修改某个文件后很有可能导致之前生成的代码检查信息无效或者不正确，所以当在执行代码更正时标准的检查是无法使用的。

### Maker

在实际的使用中，.swiftlint.yml 的创建也是非常频繁的，尤其是如果你是通过模块化的开发，那么每当新建一个模块或者给一个模块添加 Lint 的时候，你就需要创建至少一个 .yml 文件，所以个人建议可以给 SwiftLint 添加一个辅助的命令行。

我使用 Swift 简单实现了一个 Maker，将其编译之后的二进制可执行文件拷贝到 /usr/local/bin 之后就可以使用了。

![791315-5a4338f885454b1b.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572921131024-c489a5c7-29b8-4b46-b622-9d3428f969c1.webp#align=left&display=inline&height=191&margin=%5Bobject%20Object%5D&name=791315-5a4338f885454b1b.webp&originHeight=340&originWidth=960&size=40740&status=done&style=none&width=540)

之后我们通过下面的命令就可以快速的创建一个 YML 文件，我们只需要在模板文件的基础上进行修改就可以了:

```
maker -y
```

![791315-efdb8ba6877d1c98.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572921165233-afd04806-ded6-4845-b9a5-6a2b070db99a.webp#align=left&display=inline&height=59&margin=%5Bobject%20Object%5D&name=791315-efdb8ba6877d1c98.webp&originHeight=84&originWidth=596&size=7152&status=done&style=none&width=416)

#### 1)Xcode 编译方案

首先我们打开 Maker 工程，然后点击 `Archive`（签名的事情自己搞定啊）：

![截屏2019-11-05上午10.34.13.png](https://cdn.nlark.com/yuque/0/2019/png/302712/1572921268762-ba874e20-3ad8-4257-b0fe-21ea80dedceb.png#align=left&display=inline&height=822&margin=%5Bobject%20Object%5D&name=%E6%88%AA%E5%B1%8F2019-11-05%E4%B8%8A%E5%8D%8810.34.13.png&originHeight=822&originWidth=1810&size=1332236&status=done&style=none&width=1810)

稍等片刻，我们的程序就编译完成了，导出到指定文件路径，只需要两句命令：
![791315-29492fed69720c19.webp](https://cdn.nlark.com/yuque/0/2019/webp/302712/1572921357569-64ef846c-9b91-49e5-b061-8912142f5fa6.webp#align=left&display=inline&height=134&margin=%5Bobject%20Object%5D&name=791315-29492fed69720c19.webp&originHeight=310&originWidth=1200&size=19952&status=done&style=none&width=520)

#### 2)命令行编译方案

第二种方法直接通过纯命令的方式,更加简单：

```
cd path # 这里的path是指main.swift所在的文件目录之下
swiftc main.swift Maker.swift -o Maker # 编译生成二进制文件
cp Maker /usr/local/bin # 复制到目标目录之下
```

OK！ 大功告成！ 快试试 Maker 吧！

# 整理参考

- [https://github.com/realm/SwiftLint](https://github.com/realm/SwiftLint/blob/master/README_CN.md)
- [https://www.jianshu.com/p/40aa8695503f](https://www.jianshu.com/p/40aa8695503f)
- [https://www.jianshu.com/p/d8fef88b26de](https://www.jianshu.com/p/d8fef88b26de)
- [https://google.github.io/swift/](https://google.github.io/swift/)
