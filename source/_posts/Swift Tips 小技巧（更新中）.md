---
title: Swift Tips 小技巧（更新中）
urlname: uwrsdd
date: 2020-01-20 20:00:00 +0800
tags: [Swift,iOS]
categories: [移动端]
---

[本文转载自谢什么](https://www.yuque.com/xietian-dz3wk/ygtg29/zd2myq)

### 1.闭包（Closure）实现传值

```swift
typealias swiftBlock = (_ text: String) -> Void
var callBack: swiftBlock?

// 按钮设置点击事件
self.backButton.addTarget(self, action: #selector(self.backAction),
for: .touchUpInside)

// 按钮点击事件
func backAction() {
	if callBack != nil {
	   callBack!("")
	}
}

func callBackBlock(_ block: @escaping (_ text: String) -> Void) {
	callBack = block
}
```

<!-- more -->

### 2.代理（Delegate）实现传值

```swift
protocol ClickViewDelegate:NSObjectProtocol {
    func btnClickIndex(index:Int)
}
```

```swift
class SomeView: UIView {
    weak var delegate:ClickViewDelegate?

    /// 绑定代理实现
    func handleClick(btn: UIButton) {
        let index = btn.tag - 100
        delegate?.btnClickIndex(index: index)
    }
}
```

使用回调：

```swift
extension ViewController: ClickViewDelegate {
    func BtnClickIndex(index: Int) {
        print("isSelected:\(listArray[index])")
    }
}
```

### 3.自动计算导航栏高度

```swift
self.automaticallyAdjustsScrollViewInsets = false
```

### 4.Button 添加点击事件

```swift
button.addTarget(self, action: #selector(showSelectMenu(sender:)),
  for: .touchUpInside)

func showSelectMenu(sender: UIButton) {
	if self.tapSelectThemeClosure != nil {
		self.tapSelectThemeClosure()
	}
}
```

### 5.系统级 Alert

```swift
func showAlertView() {
    let alertController = UIAlertController(title: "XXX", message: "xxx",
    preferredStyle: .alert)
    let cancelAction = UIAlertAction(title: "取消", style: .cancel, handler: nil) let okAction = UIAlertAction(title: "确定", style: .default, handler: { action in
        print("点击确定")
    })
    alertController.addAction(cancelAction)
    alertController.addAction(okAction)
    self.present(alertController, animated: true, completion: nil)
}
```

### 6.手势处理

```swift
private var gestureRecognizer: UILongPressGestureRecognizer!
// 添加手势
private func addGestureRecognizer() {
     gestureRecognizer = UILongPressGestureRecognizer(target: self, action:
 #selector(longPrese(gestureRecognizer:)))
     gestureRecognizer.minimumPressDuration = 0.5
     self.addGestureRecognizer(gestureRecognizer)
     self.isEnableEdit(isEditor: false)
}

// - Parameter gestureRecognizer: <#gestureRecognizer description#>
func longPrese(gestureRecognizer: UILongPressGestureRecognizer) {
     let point = gestureRecognizer.location(in: self)
     switch gestureRecognizer.state {
     case .began:
         self.longPressBegin(point: point)
     case .changed:
         longPressChange(point: point)
     case .ended:
         longPressEnd()
     default:
         self.cancelInteractiveMovement()
     }
     return
}

// 开始
// - Parameter point: 点击开始的点
private func longPressBegin(point: CGPoint) {
}

// 移动
// - Parameter point: 移动时的点
private func longPressChange(point: CGPoint) {
}
```

### 7.TableView 注册可复用 Cell 视图

```swift
tableView.register(Demo1stTableViewCell.self, forCellReuseIdentifier: "reuseIdentifier”)
```

### 8.读取 Plist 文件数据

```swift
let plistPath = Bundle.main.path(forResource: "menuData", ofType: "plist")
let arrayAllMenu: Array<Any> = NSArray(contentsOfFile: plistPath!) as!  Array<Any>
for index in (0..<countItem) {
//    print("index",index,"\narrayAllMenu[index]" ,arrayAllMenu[index],"\ncountItem" ,countItem)
	arrMenu.append(arrayAllMenu[index])
}
```

### 9.使用 XIB View

```swift
darkMenuView = UINib(nibName: "MenuView", bundle: nil).instantiate(withOwner: nil, options: nil).first as! MenuView
```

### 10.Button 点击方法

```swift
backButton.addTarget(self, action: #selector(self.handleBackButtonPressed(_:)), for: .touchUpInside)

// 实现方法
@objc private func handleBackButtonPressed(_ button: UIButton) {
	barController.expand(on: false)
}
```

### 11.枚举变量

```swift
private enum Constants {
  static let normalStateHeight: CGFloat = 128
  static let compactStateHeight: CGFloat = 64
  static let expandedStateHeight: CGFloat = 284
}

let configuration = Configuration(
   compactStateHeight: Constants.compactStateHeight,
   normalStateHeight: Constants.normalStateHeight,
   expandedStateHeight: Constants.expandedStateHeight
)
```

### 12.View 动画

```swift
UIView.animate(withDuration: 0.20, delay: 0, options: .curveEaseInOut, animations: {
	self.setNeedsStatusBarAppearanceUpdate()
}, completion: nil)
```

### 13.RunTime 类方法-跳转

```swift
let vc:UIViewController = (NSClassFromString("SwiftAutoCellHeight."+type) as! UIViewController.Type).init()
self.navigationController?.pushViewController(vc, animated: true)
tableView.deselectRow(at: indexPath, animated: true)
let sectionModel = models[(indexPath as NSIndexPath).section]
var className = sectionModel.rowsTargetControlerNames[(indexPath as NSIndexPath).row]
className = "GTMRefreshDemo.\(className)"

if let cls = NSClassFromString(className) as? UIViewController.Type{
	let dvc = cls.init()
	self.navigationController?.pushViewController(dvc, animated: true)
}
```

### 14.token 打印

```swift
let nsdataStr = NSData.init(data: deviceToken)
let datastr = nsdataStr.description.replacingOccurrences(of: "<", with: "").replacingOccurrences(of: ">", with: "").replacingOccurrences(of: " ", with: "")
print("deviceToken:\(datastr)")
```

### 15.searchcontroller 使用

```swift
func setupSearchVc() {
   // 配置搜索控制器
   self.countrySearchController = ({
   let controller = UISearchController(searchResultsController: nil)
       controller.searchResultsUpdater = self  //两个样例使用不同的代理
       controller.hidesNavigationBarDuringPresentation = false
       controller.dimsBackgroundDuringPresentation = false
       controller.searchBar.searchBarStyle = .minimal
       controller.searchBar.sizeToFit()
       return controller
   })()
}

extension ViewController: UISearchResultsUpdating{
   // 实时进行搜索
   func updateSearchResults(for searchController: UISearchController) {
       print("search_key:\(searchController.searchBar.text!)")
   }
}
```

### 16.navi 大标题

```swift
if #available(iOS 11.0, *) {
  navigationController?.navigationBar.prefersLargeTitles = true
  navigationItem.searchController = countrySearchController
  navigationController?.navigationItem.largeTitleDisplayMode = .automatic
} else {
  // Fallback on earlier versions
}
```

### 17.TableView 偏移问题

```swift
if #available(iOS 11.0, *) {
   self.tableView.estimatedRowHeight = 0;
   self.tableView.estimatedSectionHeaderHeight = 0;
   self.tableView.estimatedSectionFooterHeight = 0;
//  tableView.contentInsetAdjustmentBehavior = .never
   print("adjustedContentInset.top:\(tableView.adjustedContentInset.top)")
} else {
   automaticallyAdjustsScrollViewInsets = false;
};

tableView.separatorStyle = UITableViewCellSeparatorStyle.none
tableView.register(AutoMHTableViewCell.self , forCellReuseIdentifier: "cell")
// 自动高度控制
tableView.estimatedRowHeight = 44
tableView.rowHeight = UITableViewAutomaticDimension
```

### 18.TableView 滚动中哪个 cell 滚动到顶部

```swift
override func scrollViewDidScroll(_ scrollView: UIScrollView) {
    var path: IndexPath? = tableView.indexPathForRow(at: CGPoint(x: scrollView.contentOffset.x, y: scrollView.contentOffset.y))
    print("这是第\(path?.row ?? 0)行")
}
```

### 19.weak self 避免循环引用方法

```swift
request(url).responseModel { [weak self] response in
   guard let strongSelf = self else { return }
   guard let model = response.result.value else {
       return
   }
   guard let strongSelf = self else {
       return
   }
   strongSelf.int = model.int
   strongSelf.label.text = strongSelf.string + model.string
   ...
}

{[weak self] in
    if let weSelf = self{
        // 执行代码
    }
}
```

### 20.SwiftyJSON 使用

```swift
let jsons = JSON(data: jsonData!)
log.debug("A debug message-\(jsons)")
let date = jsons["time"].stringValue
log.debug("A debug message-\(date)")

let list = jsons["data"]["cardList"]
log.debug("A debug message-\(list)")
arrayHeaderList = [jsons["data"]["cardList"]]
log.debug("A debug message-\(arrayHeaderList)")
```

### 21.用 xib 写 viewController 时创建对象用这个方法

```swift
// XIB 加载
let rightSelectView = RightSelectViewController(nibName: "RightSelectViewController", bundle: nil)

// storyboard 加载
let _drawVC = storyboard?.instantiateViewController(withIdentifier: "ViewController") as? ViewController
```

### 22.data 相互转化 String

```swift
// 字符串转data型
let data = str.data!(using: String.Encoding.utf8.rawValue)!

// data转string型
let str =  NSString(data:data! ,encoding: String.Encoding.utf8.rawValue)
```

### 23.runtime 方式-延展添加属性

```swift
let key = UnsafeRawPointer(bitPattern: "key".hashValue)
extension UIScrollView {
   var refreshView : WaveView? {
       set{
//            let key: UnsafeRawPointer! = UnsafeRawPointer(bitPattern: key.hashValue)
           objc_setAssociatedObject(self, key!, newValue, .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
       }
       get{
//            let key: UnsafeRawPointer! = UnsafeRawPointer(bitPattern: key.hashValue)
           return objc_getAssociatedObject(self, key!) as? WaveView
       }
   }
}
```

### 24.点击空白键盘回收

分开控制

```swift
//添加手势
view.addGestureRecognizer(UITapGestureRecognizer(target:self, action:#selector(self.handleTap(sender:))))

//收回键盘方法
func handleTap(sender: UITapGestureRecognizer) {
   if sender.state == .ended {
       inputPhone.resignFirstResponder()
       inputPW.resignFirstResponder()
   }
   sender.cancelsTouchesInView = false
}
```

集中控制

```swift
extension LoginViewController {
   override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
       view.endEditing(true)
   }
}
```

### 25.模态回退几种方法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/302712/1589167033817-d382b4d1-d74c-4fea-a016-da8e087be621.png#align=left&display=inline&height=83&margin=%5Bobject%20Object%5D&name=image.png&originHeight=330&originWidth=1058&size=51724&status=done&style=none&width=265)

```swift
// 手动计算层数
self.presentingViewController?.presentingViewController?.dismiss(animated: true, completion: nil)

// 自动回退到底层 方法一：
self.view.window?.rootViewController?.dismiss(animated: true, completion: nil)

// 自动回退到底层 方法二：
// 获取根VC
var rootVC = self.presentingViewController
while let parent = rootVC?.presentingViewController {
    rootVC = parent
}
// 释放所有下级视图
rootVC?.dismiss(animated: true, completion: nil)
```

### 26.手势操作

```swift
// 注册手势
let swipe = UIScreenEdgePanGestureRecognizer(target:self, action:#selector(swipe(_:)))
swipe.edges = UIRectEdge.left // 从左边缘开始滑动
self.view.addGestureRecognizer(swipe)

// 滑动事件
func swipe(_ recognizer:UIScreenEdgePanGestureRecognizer){
	print("left edgeswipe ok")
	let point=recognizer.location(in: self.view)
	// 这个点是滑动的起点
	print(point.x)
	print(point.y)
}
```

### 27.按钮懒加载

```swift
lazy var button: UIButton = self.makeButton()
// 返回实现
func makeButton() -> UIButton {
	let button = UIButton()
	button.setTitle("Show ImagePicker", for: .normal)
	button.setTitleColor(UIColor.black, for: .normal)
	button.addTarget(self, action: #selector(buttonTouched(button:)), for: .touchUpInside)
	return button
}
```

### 28.图片点检代码段

```swift
class func bundledImage(named: String) -> UIImage? {
	let image = UIImage(named: named)
	if image == nil {
		return UIImage(named: named, in: Bundle(for: SwiftWebVC.classForCoder()), compatibleWith: nil)
	} // Replace MyBasePodClass with yours
	return image
}
```

### 29.从 Storyboard 中加载视图 三种方法

```swift
let sb = UIStoryboard(name: "Main", bundle: nil)
let vc = sb.instantiateViewController(withIdentifier: "ScanIDCardViewController") as! ScanIDCardViewController

// <1>模态
self.present(vc, animated: true, completion: nil)

// <2>导航
self.navigationController!.pushViewController(vc, animated: true)

// <3>渲染
let logoView  = UIStoryboard(name: "Main", bundle: nil).instantiateViewController(withIdentifier: "ScanIDCardViewController")
// 添加获取到的视图控制器的视图
self.view.addSubview(logoView.view)
// 添加子视图控制器
addChildViewController(logoView)
logoView.didMove(toParentViewController: self)
```

### 30.泛型加载 XIB 内容

```swift
/// 加载 UIView 类型的 xib
func loadNib<T>(_ as: T.Type) -> T where T: UIView {
	return Bundle.main.loadNibNamed("\(T.self)", owner: nil, options: nil)?.first as! T
}

/// 加载 UIView 类型的 xib（并设置 frame 参数）
func loadNib<T>(_ as: T.Type, frame: CGRect) -> T where T: UIView {
	let TView = loadNib(`as`)
	TView.frame = frame
	return TView
}
```

### 31.使用泛型，自动转换类型

```swift
/// 下标的返回类型支持泛型
struct GenericDictionary<Key: Hashable, Value> {
   private var data: [Key: Value]

   init(data: [Key: Value]) {
       self.data = data
   }

   subscript<T>(key: Key) -> T? {
       return data[key] as? T
   }
}

/// Swift4 下标类型同样支持泛型
extension GenericDictionary {
   subscript<Keys: Sequence>(keys: Keys) -> [Value] where Keys.Iterator.Element == Key {
       var values: [Value] = []
       for key in keys {
           if let value = data[key] {
               values.append(value)
           }
       }
       return values
   }
}
```

使用实例：

```swift
//字典类型: [String: Any]
let earthData = GenericDictionary(data: ["name": "Earth", "population": 7500000000, "moons": 1])

//自动转换类型，不需要在写 "as? String"
let name: String? = earthData["name"]
print(name)

//自动转换类型，不需要在写 "as? Int"
let population: Int? = earthData["population"]
print(population)

// Array下标
let nameAndMoons = earthData[["moons", "name"]]        // [1, "Earth"]
print(nameAndMoons)

// Set下标
let nameAndMoons2 = earthData[Set(["moons", "name"])]  // [1, "Earth"]
print(nameAndMoons2)
```

### 32.对象持久化

如果要将一个对象持久化，需要把这个对象序列化。过去的做法是实现 NSCoding 协议，但实现 NSCoding 协议的代码写起来很繁琐，尤其是当属性非常多的时候。Swift4 中引入了 Codable 协议，可以大大减轻了我们的工作量。

```swift
/// Codable 协议
struct Language: Codable {
   var name: String
   var version: Int
}
```

使用 Codable 协议序列化、反序列化

```swift
let swift = Language(name: "Swift", version: 4)
//encoded对象
let encodedData = try? JSONEncoder().encode(swift)
//从encoded对象获取String
let jsonString = String(data: encodedData!, encoding: .utf8)
print(jsonString)
let decodedData = try! JSONDecoder().decode(Language.self, from: encodedData!)
print(decodedData.name, decodedData.version)
```

### 33.截取出子字符串

针对 Swift4 String 的特性，这里对 String 进行扩展，新增一个 subString 方法。直接可以根据起始位置（Int 类型）和需要的长度（Int 类型），来截取出子字符串。

```swift
extension String {
   //根据开始位置和长度截取字符串
   func subString(start:Int, length:Int = -1) -> String {
       var len = length
       if len == -1 {
           len = self.count - start
       }
       let st = self.index(startIndex, offsetBy:start)
       let en = self.index(st, offsetBy:len)
       return String(self[st ..< en])
   }
}
```

使用实例：

```swift
let str1 = "欢迎访问hangge.com"
let str2 = str1.subString(start: 4, length: 6)
print("原字符串：\(str1)")
print("截取出的字符串：\(str2)")
```

### 34.数组交换方法（新旧方法）

```swift
// 废弃交换方法
//        var a = 1
//        var b = 2
//        swap(&a, &b)
//        print(a, b)
// 后面 swap() 方法将会被废弃，建议使用 tuple（元组）特性来实现值交换，也只需要一句话就能实现：
//        var a = 1
//        var b = 2
//        (b, a) = (a, b)
//        print(a, b)

// 使用 tuple 方式的好处是，多个变量值也可以一起进行交换：
var a = 1
var b = 2
var c = 3
(a, b, c) = (b, c, a)
print(a, b, c)

// 补充一下：现在数组增加了个 swapAt 方法可以实现两个元素的位置交换。
var fruits = ["apple", "pear", "grape", "banana"]
// 交换元素位置（第2个和第3个元素位置进行交换）
fruits.swapAt(1, 2)
print(fruits)
```

### 35.iPhone X 适配 全局常量

```swift
// 适配 iPhoneX
let is_iPhoneX = (kScreenW == 375.0 && kScreenH == 812.0 ?true:false)
let kNavibarH: CGFloat = is_iPhoneX ? 88.0 : 64.0
let kTabbarH: CGFloat = is_iPhoneX ? 49.0+34.0 : 49.0
let kStatusbarH: CGFloat = is_iPhoneX ? 44.0 : 20.0
let iPhoneXBottomH: CGFloat = 34.0
let iPhoneXTopH: CGFloat = 24.0
```

### 36.TableView 泛型版 防止重用简化注册 Cell

```swift
extension UITableView {
   /*
    弹出一个静态的cell，无须注册重用，例如:
    let cell: UITableViewCell = tableView.generic_dequeueStaticCell(indexPath: indexPath as NSIndexPath)
    即可返回一个类型为GrayLineTableViewCell的对象

    - parameter indexPath: cell对应的indexPath
    - returns: 该indexPath对应的cell
    */

   func generic_dequeueStaticCell<T: UITableViewCell>(indexPath: NSIndexPath) -> T {
       let reuseIdentifier = "staticCellReuseIdentifier - \(indexPath.description)"
       if let cell = self.dequeueReusableCell(withIdentifier: reuseIdentifier) as? T {
           return cell
       } else {
           let cell = T(style: .default, reuseIdentifier: reuseIdentifier)
           return cell
       }
   }
}
```

### 37.2018 更新 UI 数据参数

![image.png](https://cdn.nlark.com/yuque/0/2020/png/302712/1589167742580-3df7daf3-f0ab-4153-9c51-bdba721d9707.png#align=left&display=inline&height=214&margin=%5Bobject%20Object%5D&name=image.png&originHeight=428&originWidth=840&size=70153&status=done&style=none&width=420)

### 38.TableView 系统 Cell

注册 Cell

```swift
tableView.register(UITableViewCell.self, forCellReuseIdentifier: "musicCell")
```

返回样式

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)
 -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "musicCell")!
    let music = musicListViewModel.data[indexPath.row]
    cell.textLabel?.text = music.name
    cell.detailTextLabel?.text = music.singer
    return cell
}
```

### 39.闭包创建（内联函数）

```swift
var newsFilerHeaderView : NewsFilerHeaderView = {
    let newsFilerHeaderView = NewsFilerHeaderView(frame: CGRect(x:0, y:0, width:BT_screenWidth, height:180))
    newsFilerHeaderView.color = UIColor.qmui_color(withHexString: "#666666")
    return newsFilerHeaderView
}()
```

### 40.获取在父类中坐标

- 父类：view
- 子类：cell

```swift
print(self?.view.convert(cell.frame, from: cell.superview))
```

### 41.TableView 列表自适应约束

```swift
///懒加载创建
private lazy var tableView: UITableView = {
   let tableView = UITableView(frame: CGRect.zero, style: UITableViewStyle.plain)
   tableView.separatorStyle = UITableViewCellSeparatorStyle.none
   tableView.delegate = self
   tableView.dataSource = self

   tableView.register(AutoMHTableViewCell.self , forCellReuseIdentifier: customcellIdentifier)

   tableView.estimatedRowHeight = 44
   tableView.rowHeight = UITableViewAutomaticDimension
   return tableView
}()

///约束
override func updateViewConstraints() {
   if #available(iOS 11.0, *) {
       tableView.contentInsetAdjustmentBehavior = .never
   } else {
       automaticallyAdjustsScrollViewInsets = false
   }
   self.tableView.snp.makeConstraints { (make) in
       if #available(iOS 11.0, *) {
           make.top.equalTo(self.view.safeAreaLayoutGuide)
           make.left.right.equalTo(self.view)
           make.bottom.equalTo(self.view.safeAreaLayoutGuide)
       } else {
           make.size.equalTo(self.view)
           make.center.equalTo(self.view)
       }}

   super.updateViewConstraints()
}
```

### 42.显示原彩效果

```swift
let originalImage = UIImage(named: "chain")?.withRenderingMode(.alwaysOriginal)
```

### 43.图片压缩

```swift
static func compressImageQuality(_ image: UIImage, toByte maxLength: Int) -> UIImage {
   var compression: CGFloat = 1
   guard var data = UIImageJPEGRepresentation(image, compression),
       data.count > maxLength else { return image }
   print("Before compressing quality, image size =", data.count / 1024, "KB")

   var max: CGFloat = 1
   var min: CGFloat = 0
   for _ in 0..<6 {
       compression = (max + min) / 2
       data = UIImageJPEGRepresentation(image, compression)!
       print("Compression =", compression)
       print("In compressing quality loop, image size =", data.count / 1024, "KB")
       if CGFloat(data.count) < CGFloat(maxLength) * 0.9 {
           min = compression
       } else if data.count > maxLength {
           max = compression
       } else {
           break
       }
   }
   print("After compressing quality, image size =", data.count / 1024, "KB")
   return UIImage(data: data)!
}

static func compressImageSize(_ image: UIImage, toByte maxLength: Int) -> UIImage {
   guard var data = UIImageJPEGRepresentation(image, 1) else { return image }
   print("Before compressing size, image size =", data.count / 1024, "KB")

   var resultImage: UIImage = image
   var lastDataLength: Int = 0
   while data.count > maxLength, data.count != lastDataLength {
       lastDataLength = data.count
       let ratio: CGFloat = CGFloat(maxLength) / CGFloat(data.count)
       print("Ratio =", ratio)
       let size: CGSize = CGSize(width: Int(resultImage.size.width * sqrt(ratio)),
                                 height: Int(resultImage.size.height * sqrt(ratio)))
       UIGraphicsBeginImageContext(size)
       resultImage.draw(in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
       resultImage = UIGraphicsGetImageFromCurrentImageContext()!
       UIGraphicsEndImageContext()
       data = UIImageJPEGRepresentation(resultImage, 1)!
       print("In compressing size loop, image size =", data.count / 1024, "KB")
   }
   print("After compressing size loop, image size =", data.count / 1024, "KB")
   return resultImage
}

static func compressImage(_ image: UIImage, toByte maxLength: Int) -> UIImage {
   var compression: CGFloat = 1
   guard var data = UIImageJPEGRepresentation(image, compression),
       data.count > maxLength else { return image }
   print("Before compressing quality, image size =", data.count / 1024, "KB")

   // Compress by size
   var max: CGFloat = 1
   var min: CGFloat = 0
   for _ in 0..<6 {
       compression = (max + min) / 2
       data = UIImageJPEGRepresentation(image, compression)!
       print("Compression =", compression)
       print("In compressing quality loop, image size =", data.count / 1024, "KB")
       if CGFloat(data.count) < CGFloat(maxLength) * 0.9 {
           min = compression
       } else if data.count > maxLength {
           max = compression
       } else {
           break
       }
   }
   print("After compressing quality, image size =", data.count / 1024, "KB")
   var resultImage: UIImage = UIImage(data: data)!
   if data.count < maxLength { return resultImage }

   // Compress by size
   var lastDataLength: Int = 0
   while data.count > maxLength, data.count != lastDataLength {
       lastDataLength = data.count
       let ratio: CGFloat = CGFloat(maxLength) / CGFloat(data.count)
       print("Ratio =", ratio)
       let size: CGSize = CGSize(width: Int(resultImage.size.width * sqrt(ratio)),
                                 height: Int(resultImage.size.height * sqrt(ratio)))
       UIGraphicsBeginImageContext(size)
       resultImage.draw(in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
       resultImage = UIGraphicsGetImageFromCurrentImageContext()!
       UIGraphicsEndImageContext()
       data = UIImageJPEGRepresentation(resultImage, compression)!
       print("In compressing size loop, image size =", data.count / 1024, "KB")
   }
   print("After compressing size loop, image size =", data.count / 1024, "KB")
   return resultImage
}
```

### 44.下拉刷新 提示条目数

```swift
extension UITableView {
   /// 下拉刷新 提示条目数
   ///
   /// - Parameters:
   ///   - text: 提示文字
   ///   - delayTime: 停留展示时间
   ///   - height: 条幅高度
   func addHeadTip(text: String?, delayTime: Double, height: CGFloat) {
       let headLabel = UILabel(frame: CGRect(x: 0, y: -height - height - height / 3 * 2, width: UIScreen.main.bounds.size.width, height: height))
       UIView.animate(withDuration: 0.63, animations: {
           headLabel.frame = CGRect(x: 0, y: height, width: UIScreen.main.bounds.size.width, height: height)
           self.tableHeaderView = headLabel
       })

       headLabel.textColor = UIColor.qmui_color(withHexString: "#2D8EFF")
       headLabel.backgroundColor = UIColor.qmui_color(withHexString: "#CFE6F4")
       headLabel.font = UIFont.systemFont(ofSize: 12)
       headLabel.textAlignment = .center
       headLabel.text = text

       DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + delayTime) {
           UIView.animate(withDuration: 0.5, animations: {
               headLabel.frame = CGRect(x: 0, y: 0, width: UIScreen.main.bounds.size.width, height: 0)
               self.tableHeaderView = headLabel
           }) { finished in
               self.tableHeaderView = nil
           }
       }
   }
}
```

### 45.NSAttributedString 富文本 HTML String 相互转化

NSAttributedString 富文本转 HTML String (延展方法)

```swift
extension NSAttributedString {
    /// 转超文本格式(Html)
    ///
    /// - Returns: Html String
    func html() -> String {
        let exportParams = [.documentType: NSAttributedString.DocumentType.html,
        .characterEncoding: String.Encoding.utf8.rawValue] as [NSAttributedString.DocumentAttributeKey : Any]

        do {
            let htmlData = try self.data(from: NSMakeRange(0, self.length), documentAttributes: exportParams)
            let htmlString = NSString.init(data: htmlData, encoding: String.Encoding.utf8.rawValue)
            return htmlString! as String
        } catch let error as NSError {
            print(error.localizedDescription)
            return ""
        }
    }
}
```

HTML String 转 NSAttributedString 富文本(延展方法)

```swift
extension String {
    /// html转富文本格式
    ///
    /// - Returns: 富文本格式
    func attributedString() -> NSAttributedString? {
        do {
            let attrStr = try NSAttributedString(data: self.data(using: String.Encoding.unicode, allowLossyConversion: true)!, options: [.documentType: NSAttributedString.DocumentType.html,.characterEncoding: String.Encoding.utf8.rawValue], documentAttributes: nil)
            return attrStr

        } catch let error as NSError {
            print(error.localizedDescription)
            return nil
        }
    }
}
```

### 46.获取文字高度、宽度

```swift
/// 获取文字高度
///
/// - Parameters:
///   - text: 文字
///   - font: 字号
///   - width: 宽度
/// - Returns: 返回高度
public func getTextHeigh(text: String, font: UIFont, width: CGFloat) -> CGFloat {
    let size = CGSize(width: width, height: 1000)
    let dic = NSDictionary(object: font, forKey: kCTFontAttributeName as! NSCopying)
    let stringSize = text.boundingRect(with: size, options: .usesLineFragmentOrigin, attributes: dic as? [NSAttributedStringKey : AnyObject], context:nil).size
    return stringSize.height
}

/// 获取文字宽度
///
/// - Parameters:
///   - text: 文字内容
///   - font: 字号
///   - height: 宽度
/// - Returns: 返回高度
public func getTextWidth(text: String,font: UIFont, height: CGFloat) -> CGFloat {
    let size = CGSize(width: 1000, height: height)
    let dic = NSDictionary(object: font, forKey: kCTFontAttributeName as! NSCopying)
    let stringSize = text.boundingRect(with: size, options: .usesLineFragmentOrigin, attributes: dic as? [NSAttributedStringKey : AnyObject], context:nil).size
    return stringSize.width
}
```

### 47.根据 class 名称，获取 class

```swift
/// 根据class名称，获取class
///
/// - Parameter className: class名称
/// - Returns: class
public func getClass(className: String) -> AnyClass? {
    if  let appName: String = Bundle.main.object(forInfoDictionaryKey: "CFBundleName") as! String? {
        let classStringName = "\(appName).\(className)"
        return NSClassFromString(classStringName)
    }
    return nil
}
```

### 48.多线程记事

1）场景：多接口拼接数据进行加载
解决：并发任务 + 同步执行

```swift
func groupQueue(){
   // 如果想在dispatch_queue中所有的任务执行完成后再做某种操作可以使用DispatchGroup
   // 将队列放入DispatchGroup
   let group = DispatchGroup()

   let queueBook = DispatchQueue(label: "book")
   queueBook.async(group: group, execute: {
       // download book
       print("download book")
   })

   let queueVideo = DispatchQueue(label: "video")
   queueVideo.async(group: group, execute: {
       // download video
       print("download video")
   })

   group.wait() // 如果有多个并发队列在一个组里，我们想在这些操作执行完了再继续，调用wait

   group.notify(queue: DispatchQueue.main, execute: {
       // download successed
        print("download successed")
   })
}
```

2）场景：并发队列读写等待
解决：等待写入完成后才能读取

```swift
func barrierDemo(){
   var value = 10
   let wirte = DispatchWorkItem(qos: .default, flags: .barrier, block:{
       value += 100
       print("Please waiting for writing data")
   })
   let dataQueue = DispatchQueue(label: "data", qos: .default, attributes: .concurrent)
   dataQueue.async(execute: wirte)

   dataQueue.async {
        print("I am waiting for value = ", value)
   }
}
```

3）场景：异步加载网络图片
解决：使用主线程同步给图片赋值

```swift
func fetchImage(){
   // 方法一：
   let imageUrl = URL(string: "http://www.appcoda.com/wp-content/uploads/2015/12/blog-logo-dark-400.png")!
   let session = URLSession(configuration: .default)
   let task = session.dataTask(with: imageUrl, completionHandler:{ (imageData, response, error) in
       if let data = imageData{
           print("Did download image data")
           DispatchQueue.main.async {
               self.imageView.image = UIImage(data: data)
           }
       }
   })
   task.resume()

   // 方法二：
   let imageURL: URL = URL(string: "http://www.appcoda.com/wp-content/uploads/2015/12/blog-logo-dark-400.png")!

   (URLSession(configuration: URLSessionConfiguration.default)).dataTask(with: imageURL, completionHandler: { (imageData, response, error) in
       if let data = imageData {
           print("Did download image data")

           DispatchQueue.main.async {
                 self.imageView.image = UIImage(data: data)
           }

       }
   }).resume()
}
```

### 49.单例写法(大喵推荐)

```swift
class MyManager {
   private static let sharedInstance = MyManager()

   class var sharedManager: MyManager {
       return sharedInstance
   }
}
```

### 50.防止按钮重复点击

```swift
extension UIButton {

   /// 按钮对象 Key
   private struct UIButtonObjectKeys {
       static var accpetEventInterval = "UIButtonObjectKeys.accpetEventInterval"
       static var acceptEventTime = "UIButtonObjectKeys.acceptEventTime"
   }

   /// 设置的点击间隔
   var acceptEventInterval: TimeInterval {
       set {
           objc_setAssociatedObject(self,
                                    &UIButtonObjectKeys.accpetEventInterval,
                                    newValue,
                                    .OBJC_ASSOCIATION_RETAIN)
       }
       get {
           return (objc_getAssociatedObject(self,&UIButtonObjectKeys.accpetEventInterval) as? TimeInterval) ?? 0.75
       }
   }

   /// 储存的上一次点击时间
   private var acceptEventTime: TimeInterval {
       set {
           objc_setAssociatedObject(self,
                                    &UIButtonObjectKeys.acceptEventTime,
                                    newValue, .OBJC_ASSOCIATION_RETAIN)
       }
       get {
           return (objc_getAssociatedObject(self, &UIButtonObjectKeys.acceptEventTime) as? TimeInterval) ?? 0.5
       }
   }

   /// 点击方法
   ///
   /// - Parameters:
   ///   - action: 响应体
   ///   - target: 响应时间
   ///   - event: 点击类型
   open override func sendAction(_ action: Selector, to target: Any?, for event: UIEvent?) {
       // 如果这次点击时间减去上次点击事件小于设置的点击间隔，则不执行
       if Date().timeIntervalSince1970 - self.acceptEventTime < self.acceptEventInterval { return }
       // 存储最后一次点击执行的时间
       if self.acceptEventInterval > 0 {
           self.acceptEventTime = Date().timeIntervalSince1970
       }
       // 执行action
       super.sendAction(action, to: target, for: event)
   }
}
```

### 51.计算 Label 行数(包括根据字符串计算)

```swift
extension UILabel {
   func lineCounts() -> Int {
       let labelHeight: CGFloat = sizeThatFits(CGSize(width: frame.size.width, height: CGFloat(MAXFLOAT))).height
       let count = Int((labelHeight) / font.lineHeight)
       return count
   }
}
```

字符串计算

```swift
func getLinesFromString(text:String, font:UIFont, width:CGFloat) -> Int{
	let label:UILabel = UILabel(frame: CGRect(x: 0, y: 0, width: width, height: CGFloat.greatestFiniteMagnitude))
	label.numberOfLines = 0
	label.lineBreakMode = NSLineBreakMode.byWordWrapping
	label.font = font
	label.text = text
	label.sizeToFit()
	return label.lineCounts()
}
```

### 52.GCD 异步处理

1）单例

```swift
// 单例
private var once2:String = {
   print("once")
   return "once"
}()
```

2）异步

```swift
// 异步
func testGCDThread01() {
   //1
   DispatchQueue.global(qos: .default).async {
       //处理耗时操作的代码块...
       print("do work")
       sleep(2)
       //操作完成，调用主线程来刷新界面
       DispatchQueue.main.async {
           print("main refresh")
       }
   }

   //2
   DispatchQueue.global(qos: .default).async {
       //处理耗时操作的代码块...
       print("do work2")
       sleep(2)
       //操作完成，调用主线程来刷新界面
       DispatchQueue.main.async {
           print("main refresh2")
       }
   }
}
```

3）同步

```swift
// 同步
func testGCDThread02() {
   //不会造成死锁，但会一直等待代码块执行完毕
   DispatchQueue.global(qos: .default).sync {
       print("--sync1")
       sleep(2)
   }
   print("end1")

   DispatchQueue.global(qos: .default).sync {
       print("--sync2")
       sleep(2)
   }
   print("end2")

// 死锁
//        DispatchQueue.main.sync {
//            print("sync3")
//            sleep(2)
//        }
//        print("end3")
}
```

4）延时

```swift
func after() {
   DispatchQueue.global(qos: .default).asyncAfter(deadline: DispatchTime.now() + 2.0) {
       print("after!")
   }
}
```

5）任务延迟和取消

```swift
func afterAndCancel() {
   //将要执行的操作封装到DispatchWorkItem中
   let task = DispatchWorkItem { print("after!") }
   //延时2秒执行
   DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 2, execute: task)
   //取消任务
   task.cancel()
}
```

6）批量处理

```swift
func allHandle() {
   //获取系统存在的全局队列
   let queue = DispatchQueue.global(qos: .default)
   //定义一个group
   let group = DispatchGroup()
   //并发任务，顺序执行
   queue.async(group: group) {
       sleep(2)
       print("block1")
   }
   queue.async(group: group) {
       print("block2")
   }
   queue.async(group: group) {
       print("block3")
   }

   //1,所有任务执行结束汇总，不阻塞当前线程
   group.notify(queue: .global(), execute: {
       print("group done")
   })
   //2,永久等待，直到所有任务执行结束，中途不能取消，阻塞当前线程
   group.wait()
   print("任务全部执行完成")
}
```

7）信号量处理

```swift
func threadFor() {
   //获取系统存在的全局队列
   let queue = DispatchQueue.global(qos: .default)

   //使用信号量保证正确性
   //创建一个初始计数值为1的信号
   let semaphore = DispatchSemaphore(value: 1)

   //定义一个异步步代码块
   queue.async {
       //通过concurrentPerform，循环变量数组
       DispatchQueue.concurrentPerform(iterations: 6) {(index) -> Void in
           print(index)
       }

       //执行完毕，主线程更新
       DispatchQueue.main.async {
           //永久等待，直到Dispatch Semaphore的计数值 >= 1
           semaphore.wait()
           print("done")
           //发信号，使原来的信号计数值+1
           semaphore.signal()
       }
   }
}
```

### 53.读取本地 JSON 文件

```swift
func jsonTestData() {
   let path = Bundle.main.path(forResource: "TaskCenterJsonData.json", ofType: nil)
   if let jsonPath = path {
       let jsonData = NSData(contentsOfFile: jsonPath)

       do {
           let dictArr = try JSONSerialization.jsonObject(with: jsonData! as Data, options: JSONSerialization.ReadingOptions.mutableContainers)

               print(dictArr)

       }catch {
           print(error)
       }
   }
}
```

### 54.通过 view 找到 viewController

```swift
extension UIView {
   //返回该view所在VC
   func firstViewController() -> UIViewController? {
       for view in sequence(first: self.superview, next: { $0?.superview }) {
           if let responder = view?.next {
               if responder.isKind(of: UIViewController.self){
                   return responder as? UIViewController
               }
           }
       }
       return nil
   }
}
```

### 55.runtime textField String 相关延展方法

```swift
var maxTextNumberDefault = 15
extension UITextField{
   /// 使用runtime给textField添加最大输入数属性,默认15
   var maxTextNumber: Int {
       set {
           objc_setAssociatedObject(self, &maxTextNumberDefault, newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_COPY_NONATOMIC)
       }
       get {
           if let rs = objc_getAssociatedObject(self, &maxTextNumberDefault) as? Int {
               return rs
           }
           return 15
       }
   }
   /// 添加判断数量方法
   func addChangeTextTarget(){
       self.addTarget(self, action: #selector(changeText), for: .editingChanged)
   }
   @objc func changeText(){
       //判断是不是在拼音状态,拼音状态不截取文本
       if let positionRange = self.markedTextRange{
           guard self.position(from: positionRange.start, offset: 0) != nil else {
               checkTextFieldText()
               return
           }
       }else {
           checkTextFieldText()
       }
   }
   /// 判断已输入字数是否超过设置的最大数.如果是则截取
   func checkTextFieldText(){
       guard (self.text?.length)! <= maxTextNumber  else {
           self.text = (self.text?.stringCut(end: maxTextNumber))!
           return
       }
   }
}
extension String {
   var length: Int {
       ///更改成其他的影响含有emoji协议的签名
       return self.utf16.count
   }
   /// 截取第一个到第任意位置
   ///
   /// - Parameter end: 结束的位值
   /// - Returns: 截取后的字符串
   func stringCut(end: Int) -> String{
       if !(end <= count) { return self }
       let sInde = index(startIndex, offsetBy: end)
       return String(self[..<sInde])
   }
}
```

### 56.导航栏 BarButtonItem 简单设置

```swift
navigationItem.rightBarButtonItem = UIBarButtonItem(
     barButtonSystemItem: .add,
     target: self,
     action: #selector(addButtonTapped))
let reloadDataButton = UIBarButtonItem(
     barButtonSystemItem: .refresh,
     target: self,
     action: #selector(reloadButtonTapped))
navigationItem.leftBarButtonItem = reloadDataButton
```

### 57.UITextView 高度变化

在调整了 UITextView 的高度之后，继续输入会导致内容(UITextContainerView 里的文字)抖动

```swift
textView.scrollRangeToVisible(textView.selectedRange)
```

获取 UITextView 内的文字高度以及行数的方法

```swift
let height = textView.sizeThatFits(CGSize.init(width: textView.frame.size.width, height: CGFloat.greatestFiniteMagnitude)).height
let line = Int(height/(textView.font?.lineHeight)!)
```

### 58.KVO 相关

系统级

```swift
// 设置监听
scrollview.addObserver(self, forKeyPath: "contentOffset", options: NSKeyValueObservingOptions.new, context: nil);
// 监听属性值发生改变时回调
override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
}
```

> 推荐 FaceBook：KVOController

### 59.纯代码自定义 View

创建自定义控件

```swift
import UIKit
class CustomView: UIView {
    var lab:UILabel!
    var btn:UIButton!
    // MARK: - 将需要添加的子控件在这里进行初始化
    override init(frame: CGRect) {
        super.init(frame: frame)
        //初始化
        lab = UILabel()
        lab.textAlignment = .center
        lab.font = UIFont.systemFont(ofSize: 12)
        self.addSubview(lab)
        btn = UIButton()
        self.addSubview(btn)
    }
	// MARK: - 设置子控件的位置
    override func layoutSubviews() {
        super.layoutSubviews()
        // 设置 子控件 frame, 也可以在这里使用自动布局
        lab.frame = CGRect(x:10, y:10, width:100, height:40)
        btn.frame = CGRect(x:lab.frame.origin.x, y:lab.frame.maxY + 10, width:100, height:40)
    }
	// MARK: - 传入model对子控件进行配置，这里暂用NSObject
    func setUp(model:NSObject) {
        lab.text = "你好"//model.xx
        btn.setTitle("确定", for: .normal) //title:model.xx
    }
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```

使用自定义控件

```swift
// 纯代码 view
let view = CustomView()
view.frame = CGRect(x:10, y:100, width:200, height:100)
view.backgroundColor = UIColor.cyan
view.setUp(model: "" as NSObject)
self.view.addSubview(view)
```

### 60.获取文件夹大小方法

```swift
/// 文件夹是否为空
func isEmptyFile(filePath: String) -> Swift.Bool{
    return getFileSize(filePath: filePath) == 0
}

/// 获取文件夹大小
func getFileSize(filePath: String) -> Int {
    guard isExist(filePath: filePath) else {
        print("当前路径为空: \(filePath)")
        return 0
    }
    // 取出文件夹下所有文件数组
    let fileArr = FileManager.default.subpaths(atPath: filePath)
    // 快速枚举出所有文件名 计算文件大小
    var size = 0
    for file in fileArr! {
        // 把文件名拼接到路径中
        let path = filePath.appendingFormat("/\(file)")
        // 取出文件属性
        guard let floder = try? FileManager.default.attributesOfItem(atPath: path) else {
            return 0
        }
        // 用元组取出文件大小属性
        for (abc, bcd) in floder where abc == FileAttributeKey.size {
            size += (bcd as AnyObject).integerValue
        }
    }
    return size
}
```
