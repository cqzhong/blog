---
title: Swift Style Guide
urlname: kl8l3g
date: 2021-01-20 20:00:00 +0800
tags: [Swift,iOS]
categories: [移动端]
---

[本文转载自 zzzWill](https://www.yuque.com/zzzwill/rtyk7d/uawnap)
目前 Swift 编码规范网上主要有三版:

- [Google](https://google.github.io/swift/)
- [linkedin](https://github.com/linkedin/swift-style-guide)
- [raywenderlich](https://github.com/raywenderlich/swift-style-guide)
  > 本文属于 raywenderlich 版本的简化版.
  > 我在其中增加了一些批注和网络上的讨论，有助于知其所以然。

<!-- more -->

## 命名

命名的基本原则是： **清晰、一致**
详情建议参考[API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)（[中文版](https://swift.gg/2016/05/18/api-design-guidelines/)）

### 类前缀

Swift 会根据 Module 自定划分命名空间， 所以不需要再像 Objc 一样使用前缀了。
如果发生了冲突（不同 Module 中含有同名类），可以使用下面这种方式：

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

要注意的是，只有在有必要的时候才需要这么做。

### Delegate

当创建 Delegate 方法的时候，第一个匿名参数应该是事件源本身。(Apple 的系统库里很多这样的例子)
**建议：**

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**不建议：**

```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### 多使用类型推断

多加利用编译器的类型推断，能够让代码更短，更清晰。
**建议：**

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**不建议：**

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### 泛型

泛型的命名应使用大驼峰格式，并且要有描述性。
当泛型不具有明显的含义的时候，应该使用常用的泛型命名，比如： `T`, `U`, `V`
**建议：**

```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

**不建议：**

```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### 拼写

应该使用美式英语拼写（因为 Apple 的 API 就是这么干的）
**建议：**

```swift
let color = "red"
```

**不建议：**

```swift
let colour = "red"
```

## 代码组织方式

使用 extension 将代码按照功能逻辑分块。
每一个 extension 应该使用` // MARK:` - 来分隔，从而增加可读性。

### 实现协议

实现协议时 ，建议每个 extension 对应一个协议。

**建议：**

```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**不建议：**

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

### 未使用的代码

未使用的代码，包括 Xcode 自动生成的模板代码，应该及时清除掉。
**建议：**

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

**不建议：**

```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```

### 最小引入原则

只引入需要的模块。
当我们不需要使用 UIKit 的时候，只引入 Foundation 就可以了。
当我们需要引入 UIKit 的时候，也不需要再引入 Foundation 了（批注：因为 UIKit 会自行引入 Foundation）。
（批注：UIKit 与 Foundation 的引入， 就是有你没我的关系）
**建议：**

```swift
import UIKit
var view: UIView
var deviceModels: [String]
```

**建议：**

```swift
import Foundation
var deviceModels: [String]
```

**不建议：**

```swift
import UIKit
import Foundation
var view: UIView
var deviceModels: [String]
```

**不建议：**

```swift
import UIKit
var deviceModels: [String]
```

## 空格

- 缩进：建议使用 2 个空格来进行缩进，因为这样可以留下更多的空间写代码。（批注： ~~目前我还是更喜欢 4 个空格， 不过可以尝试一下~~。真香）

-

- 在方法以及控制流程（if/else/switch/while）声明的后面，同一行的位置跟左大括号

**建议：**

```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

**不建议：**

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

- 为了视觉以及代码组织上的清晰， 方法之间应该**有且仅有**一个空行。
- 冒号的左侧不应该有空格，右侧应该留一个空格。例外：三元运算符 `? :`，空字典`[:]`，方法选择器

**建议：**

```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**不建议：**

```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

- 一行代码的长度最好不要超过 70 个，否则应该换行。当然 70 并不是硬性规定。（批注： 总之一行别太长， 会牺牲掉可读性）
- 在一行结束后，避免多余的尾部空白。（批注：脸滚键盘了？）

**不建议：**

```swift
It is extra spaces (and tabs) at the end of line
                                                 ^^^^^ （尾部空白）
```

- 在文件末尾记得追加一个换行符。（批注：用以告知该文件已结束。具体讨论 参考[这里](https://stackoverflow.com/questions/5813311/no-newline-at-end-of-file)）

## 注释

注释应该保持时刻更新，不需要的注释要及时清除。
避免使用`/*...*/`来进行注释。（批注：讨论参考[这里](https://stackoverflow.com/questions/61022236/why-the-need-to-avoid-c-style-comments-in-swift)）

## 类和结构体

Class 是引用类型， 而 Struct 是值类型。

### Class 示例

```swift
class Circle: Shape { //冒号左侧无空格，右侧跟随空格
  var x: Int, y: Int  //相近含义的变量在同一行声明
  var radius: Double
  var diameter: Double {
    get {				//get前的缩进
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) { //不需要为函数增加关键字internal, 因为这是默认配置
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  override func area() -> Double {
    return Double.pi * radius * radius
  }
}

extension Circle: CustomStringConvertible {//使用extension实现协议
  var description: String {
    return "center = \(centerString) area = \(area())"
  }
  private var centerString: String { //使用private保持良好的封装性
    return "(\(x),\(y))"
  }
}
```

### self 的使用

能不用`self`就不用，因为 Swift 不需要使用`self`关键字来进行成员变量的访问以及方法的调用。
当编译器报错的时候（比如 逃逸闭包以及在初始化时与入参区分的时候）， 再补上。

### 计算属性

`get`closure 能省则省， 只有在与`set`共存的时候，再写。
**建议：**

```swift
var diameter: Double {
  return radius * 2
}
```

**不建议：**

```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

### final

一般情况下，不需要特意为我们的类使用 final 关键字（批注： 编译器会自动优化）。
但有时候使用`final`可以更好的解释我们的意图，比如：

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
  let value: T
  init(_ value: T) {
    self.value = value
  }
}
```

## 函数声明

函数声明应该尽量在一行内搞定：

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```

一行之内搞不定的时候，要多行对齐：

```swift
func reticulateSplines(
  spline: [Double],
  adjustmentFactor: Double,
  translateConstant: Int,
  comment: String
) -> Bool {
  // reticulate code goes here
}
```

不要使用`void`来表示没有入参， 使用`（）`就行了。
`void`可以用来表示无返回值。
**建议：**

```swift
func updateConstraints() -> Void {
  // magic happens here
}

typealias CompletionHandler = (result) -> Void
```

**不建议：**

```swift
func updateConstraints() -> () {
  // magic happens here
}

typealias CompletionHandler = (result) -> ()
```

## 函数调用

一行之内调用：

```swift
let success = reticulateSplines(splines)
```

多行调用要对齐：

```swift
let success = reticulateSplines(
  spline: splines,
  adjustmentFactor: 1.3,
  translateConstant: 2,
  comment: "normalize the display")
```

## Closure

多使用尾随闭包。
闭包的参数名最好不要省略掉。（批注： Xcode 的 AutoComplete 经常给省略掉）
**建议：**

```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```

**不建议：**

```swift
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

一行就能搞定的闭包， 使用隐式返回

```swift
attendeeList.sort { a, b in
  a > b
}
```

尾随闭包链式调用的时候语义要清晰， 是否要换行对齐之类的由你自行决定：

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```

## 类型

尽量使用 Swift 原生类型。
**建议：**

```swift
let width = 120.0                                    // Double
let widthString = "\(width)"                         // String
```

**不提倡：**

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**不建议：**

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

在使用 `Core Graphics` 相关 API 的时候， 还是乖乖的用`CGFloat`吧。

### 常量

使用`let`声明。
小技巧： 声明任何参数的时候都使用`let`， 直到编译器报错时再改用`var`
当需要声明全局常量的时候， 建议使用如下形式：

```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

**不建议：**

```swift
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // what is root2?
```

(批注:
两个问题：

1. 为什么要用 enum
1. 为什么要用 static let

当我们直接使用 `let e =...` 的时候，`e`一个全局变量；
当我们在` enum ``Math `中使用`static let e = ...`时， `e`是`Math`的一个[类型属性](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID264)。
`Math`的存在相当于创建了一个命名空间。
那用`Struct`可以么? 当然也可以，但`enum`不会存在被实例化的问题，所以更纯粹。讨论参考[这里](https://stackoverflow.com/questions/38585344/swift-constants-struct-or-enum)。
)

### 静态方法和类型属性

静态方法和类型属性跟全局函数和全局变量类似，除非有特殊的使用场景（批注：比如上面提到的定义全局常量），否则尽量少用。

### Optional

当对可选型进行一次性调用的时候，可以使用链式操作:

```swift
textContainer?.textLabel?.setNeedsDisplay()
//(批注：如果链中任何一个节点是 nil ，那么整个链就失败， 相当于objc中对nil发送消息， 并不会报错 )
```

当然，如果涉及的操作比较多，还是解包比较好：

```swift
if let textContainer = textContainer {
  // do many things with textContainer
}
```

解包时候的参数命名尽量与可选型同名比较方便：
**建议：**

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
  // do something with unwrapped subview and volume
}
```

**不建议：**

```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {//这种命名方式就显得有些多此一举
  if let realVolume = volume {
    // do something with unwrappedSubview and realVolume
  }
}
```

### 懒加载

与 Objc 中的懒加载类似，不再赘述。

### 类型推断

（ 批注：
让编译器多干活，我们少干点。像 Objc 这种类型声明，确实有些多此一举：

```swift
NSString *string = @"string";
//vs
let string = "string"
```

）
**建议：**

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5 //
```

**不建议：**

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names = [String]()
```

### 空数组/空字典的声明

**建议：**

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**不建议：**

```swift
var names = [String]()
var lookup = [String: Int]()
```

（批注： 两种声明方式并没有绝对的孰优孰劣， 只是为了**一致性，**建议使用第一种。讨论参考[这里](https://github.com/raywenderlich/swift-style-guide/issues/269)）

### 语法糖

能省则省。
**建议：**

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**不建议：**

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Free Func

不属于 Class 或者 type 的 Func 统称为 free func。
尽量少用 free func， 因为不好维护。
**建议：**

```swift
let sorted = items.mergeSorted()  // easily discoverable
rocket.launch()  // acts on the model
```

**不建议：**

```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

## 内存管理

内存管理主要目的是避免循环引用。

### 延长对象声明周期

（批注： 所谓延长生命周期， 顾名思义，就是对象本应被销毁了， 但因为某种原因（一般是 block 回调）需要被延长使用一段时间）
用`[weak self]` 而不是 `[unowned self]`。（区别参考[这里](https://medium.com/@kiran.jasvanee/difference-between-unowned-self-and-weak-self-in-swift-310c14961953)）
**建议：**

```swift
resource.request().onComplete { [weak self] response in
  guard let self = self else {//相当于objc中strongself
    return
  }
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**不建议：**

```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**不建议：**

```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## 访问控制

尽量多使用 private，fileprivate 只在必要的时候用。
访问权限的修饰符应该排在最前面，有` static，``@IBAction ` ，`@IBOutlet` ， `@discardableResult`的情况除外。
**建议：**

```swift
private let message = "Great Scott!"

class TimeMachine {
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**不建议：**

```swift
fileprivate let message = "Great Scott!"

class TimeMachine {
  lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

（批注：关于访问控制相关修饰符的权限区别
![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028212/1591546057118-5cd9d55b-ecf9-46a6-8793-d0e2a3b0c8f7.jpeg#crop=0&crop=0&crop=1&crop=1&height=1018&id=HhqXu&originHeight=1018&originWidth=1600&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=1600)）

## 控制语句

### 遍历

多用 for-in
**建议：**

```swift
for _ in 0..<3 {
  print("Hello three times")
}
```

**不建议：**

```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}
```

### 三元运算符

如果三元运算符能简化代码， 那就用。
常用的场景是在赋值的时候。
**建议：**

```swift
let value = 5
result = value != 0 ? x : y

let isHorizontal = true
result = isHorizontal ? x : y
```

**不建议：**

```swift
result = a > b ? x = c > d ? c : d : y
```

## 黄金大道

减少`{}`嵌套。
一般方法：多使用`return`或者`guard`
**建议：**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}
```

**不建议：**

```swift
guard
  let number1 = number1,
  let number2 = number2,
  let number3 = number3
  else {
    fatalError("impossible")
}
// do something with numbers
```

当涉及多个`optional`解包的时候，可以使用如下格式：
**建议：**

```swift
guard //guard 独自一行
  let number1 = number1, //注意对齐
  let number2 = number2,
  let number3 = number3
  else {
    fatalError("impossible")
}
// do something with num
```

**不建议：**

```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```

一般来说，当`guard`失败的时候， 应该使用`return`， `throw`，`continue`或者`fatalError()`等一句话返回，如果遇到多种退出节点都需要清理代码的时候， 可以考虑使用`defer`。

## 分号的使用

与 Objc 不同，Swift 不要求每个语句后面都需要加`；`。

## 括号的使用

if 语句也不需要使用`()`。
**建议：**

```swift
if name == "Hello" {
  print("World")
}
```

**不建议：**

```swift
if (name == "Hello") {
  print("World")
}
```

## 多行字符串

**建议：**

```swift
let message = """  //第一行不包含文本内容
  You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**不建议：**

```swift
let message = """You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**不建议：**

```swift
let message = "You cannot charge the flux " +
  "capacitor with a 9V battery.\n" +
  "You must use a super-charger " +
  "which costs 10 credits. You currently " +
  "have \(credits) credits available."
```

## 禁用 Emoji

不要使用 Emoji。

##
