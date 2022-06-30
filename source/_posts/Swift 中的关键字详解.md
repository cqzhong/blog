---
title: Swift 中的关键字详解
urlname: zuc9ws
date: 2021-01-26 20:00:00 +0800
tags: [Swift,iOS]
categories: [移动端]
---

要学习 Swift 这门语言，就必须先了解 Swift 的关键字及对应的解释。这里就列一下在 Swift 中常用到的关键字。
关键字是类似于标识符的保留字符序列，除非用重音符号（`）将其括起来，否则不能用作标识符。关键字是对编译器具有特殊意义的预定义保留标识符。常见的关键字有以下 4 种。
  与声明有关的关键字：class、deinit、enum、extension、func、import、init、let、protocol、static、struct、subscript、typealias 和 var。
与语句有关的关键字：break、case、continue、default、do、else、fallthrough、if、in、for、return、switch、where 和 while。
表达式和类型关键字：as、dynamicType、is、new、super、self、Self、Type、**COLUMN**、**FILE**、**FUNCTION**和**LINE**。
在特定上下文中使用的关键字：associativity、didSet、get、infix、inout、left、mutating、none、nonmutating、operator、override、postfix、precedence、prefix、rightset、unowned、unowned(safe)、unowned(unsafe)、weak 和 willSet。
  常见关键字的介绍：

<!-- more -->

## 访问权限关键字

#### 1、open

在 Swift 中，open 修饰的对象表示可以被任何人使用，包括 override 和继承。例如：

```swift
import UIKit
// 在本类外的范围外可以被继承
open class ParentClass1: NSObject {
    // 这个方法在任何地方都可以被override
    open func abc() {
        print("abc")
    }
}
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        //这里可以调用module1的方法
        let child = ChildrenClass1.init()
        child.abc()
        /**
         打印结果：abc
         */
    }

    class ChildrenClass1: ParentClass1 {

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```



#### 2、public

在 Swift 中，public 表示公有访问权限，类或者类的公有属性或者公有方法可以从文件或者模块的任何地方进行访问。但在其他模块(一个 App 就是一个模块，一个第三方 API, 第三等方框架等都是一个完整的模块)不可以被 override 和继承，而在本模块内可以被 override 和继承。



#### 3、internal（默认访问级别，internal 修饰符可写可不写）

- internal 访问级别所修饰的属性或方法在源代码所在的整个模块都可以访问。
- 如果是框架或者库代码，则在整个框架内部都可以访问，框架由外部代码所引用时，则不可以访问。
- 如果是 App 代码，也是在整个 App 代码，也是在整个 App 内部可以访问。

#### 4、private

在 Swift 中，private 私有访问权限。private 访问级别所修饰的属性或者方法只能在当前类里访问。
（注意：Swift4 中，extension 里也可以访问 private 的属性。）
![private.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1611657618972-f7dbd912-1419-42c4-89ef-86793dadb7f9.png#align=left&display=inline&height=198&margin=%5Bobject%20Object%5D&name=private.png&originHeight=198&originWidth=512&size=30334&status=done&style=none&width=512)

#### 5、fileprivate

fileprivate 访问级别所修饰的属性或者方法在当前的 Swift 源文件里可以访问。（比如上面样例把 private 改成 fileprivate 就不会报错了）

**说明：5 种修饰符访问权限排序 open> public > interal > fileprivate > private。**
\*\*

## 其它

#### 1、class

在 Swift 当中, 我们使用 Class 关键字去声明一个类和声明类方法, 比如:

```swift
class Person {

        // 给方法添加class关键字表示类方法
        class func work() {
            print("Type Method: Person: 学生.")
        }

    }
```

这样我们就声明了一个 Student 类。

#### 2、let

Swift 里用 let 修饰的变量会是一个不可变的常量，即我们不可以对它进行修改。（注意：我们用 let 修饰的常量是一个类, 我们可以对其所在的属性进行修改）比如：

```swift
class Student {
        let name = "xiaoMing"
        var age = 16
        var height = 160
    }

let studen = Student();
        studen.age +=  2
        print("student age is \(studen.age)")
        //输出结果为student age is 18
```

在 Student 类里 name 是不可变的，如果要对它进行修改就要将 let 改为 var。注意：如果这个时候, 再声明一个 Student 常量去引用 student, 那么 studen1 所指向的内存块是和 studen 相同的.

#### 3、var

Swift 中用 var 修饰的变量是一个可变的变量，即可以对它进行修改。注意：我们不会用 var 去引用一个类, 也没有必要。比如：

```swift
class Student {
        let name = "xiaoMing"
        var age = 16
        var height = 160
    }

 let studen = Student();
 studen.age +=  2

 var student2 = studen;
 student2.age -=  3
 print("student age is \(studen.age)")
 print("student2 age is \(student2.age)")

 /**
 输出结果为
 student age is 15
 student2 age is 15
*/
```

由输出结果可以看出他们所指向的内存块都是同一个。

#### 4、struct

在 Swift 中, 我们使用 struct 关键字去声明结构体，Swift 中的结构体并不复杂，与 C 语言的结构体相比，除了成员变量，还多了成员方法。使得它更加接近于一个类。个人认为可以理解为是类的一个轻量化实现。比如：

```swift
struct Person {
        var name:String
        var age:Int

        func introduce(){
            print("我叫：\(name),今年\(age)岁")
        }
    }

var person = Person(name: "xiaoMing",age: 20)
person.name = "xiaoMing"
print("person.name = \(person.name)")
person.introduce()

 /**
 输出结果为：
 person.name = xiaoMing
 我叫：xiaoMing,今年20岁
 */
```

语法与 C 语言或者 OC 类似，Swift 中的结构体，在定义成员变量时一定要注明类型。另外要注意一下的是，结构体属于值类型，而 Swift 中的类属于引用类型。他们在内存管理方面会有不同。



#### 5.enum

在 Swift 中, 我们使用 enum 关键字去声明枚举。枚举是一种常见的数据类型，他的主要功能就是将某一种有固定数量可能性的变量的值，以一组命名过的常数来指代。比如正常情况下方向有四种可能，东，南，西，北。我们就可以声明一组常量来指代方向的四种可能。使用枚举可以防止用户使用无效值，同时该变量可以使代码更加清晰。比如：

```swift
enum Orientation:Int{
        case East
        case South
        case West
        case North
    }
    /**
     或者
    enum Orientation:Int{
     case East,South,West,North
     }
     */

print(Orientation.East.rawValue)
        /**
         输出结果 0

         */
```

注意：我们在定义枚举时，一定要指定类型，否则在使用时就会报错。枚举类型的值如果没有赋值，他就按照默认的走，可以赋予我们自己想要的值。

#### 6、final

Swift 中，final 关键字可以在 class、func 和 var 前修饰。表示 不可重写 可以将类或者类中的部分实现保护起来,从而避免子类破坏。详细了解可以去看看（[Swift - final 关键字的介绍，以及使用场景](http://www.cnblogs.com/Free-Thinker/p/4843839.html)）。比如：

```swift
class Fruit {
        //修饰词 final 表示 不可重写 可以将类或者类中的部分实现保护起来,从而避免子类破坏
        final  func price(){
            print("price")
        }
    }

    class Apple : Fruit {//类继承
//        //重写父类方法
//        override func price() {
//            print("重写父类的price 方法")
//        }
}
```

这里重写  price()这个函数就会报错。

#### 7、override

在 Swift 中, 如果我们要重写某个方法, 或者某个属性的话, 我们需要在重写的变量前增加一个 override 关键字, 比如:

```swift
class Fruit {
        var  sellPrice : Double = 0

        func info() -> () {
            print("fruit")
        }
        //修饰词 final 表示 不可重写 可以将类或者类中的部分实现保护起来,从而避免子类破坏
        final  func price(){
            print("price")
        }
    }

    class Apple : Fruit {//类继承
        func eat () -> () {
            print("apple22222")
        }
        //重写父类方法
        override func info() {
            print("重写父类的info 方法00000")
        }
        //重写父类的属性  重写父类的属性或者方法要使用关键字 override 进行修饰
        override var sellPrice: Double {
            get {
                print("kkkkkkk\(super.sellPrice)")
                return super.sellPrice + 3
            }
            set {
                print("qqqqq")
                super.sellPrice = newValue * newValue
            }
        }
    }


        let app = Apple()
        app.info()
        app.sellPrice = 20.0
        let  adb = app.sellPrice

        print("adb == \(adb)")

        /**
         输出结果为：
         重写父类的info 方法00000
         qqqqq
         kkkkkkk400.0
         adb == 403.0
         */
```

#### 8、subscript

在 Swft 中，subscript 关键字表示下标，可以让 class、struct、以及 enum 使用下标访问内部的值。其实就可以快捷方式的设置或者获取对应的属性, 而不需要调用对应的方法去获取或者存储, 比如官网的一个实例:

```swift
struct Matrix {
        let rows: Int, columns: Int
        var grid: [Double]
        init(rows: Int, columns: Int) {
            self.rows = rows
            self.columns = columns
            grid = Array(repeating: 0.0, count: rows * columns)
        }


        func indexIsValid(row: Int, column: Int) -> Bool {
            return row >= 0 && row < rows && column >= 0 && column < columns
        }
        subscript(row: Int, column: Int) -> Double {
            get {
                assert(indexIsValid(row: row, column: column), "Index out of range")
                return grid[(row * columns) + column]
            }
            set {
                assert(indexIsValid(row: row, column: column), "Index out of range")
                grid[(row * columns) + column] = newValue
            }
        }
    }

var matrix = Matrix(rows: 2, columns: 2)
        matrix[0, 1] = 1.5
        matrix[1, 0] = 3.2
        print("matrix == \(matrix)")
        /**
         打印结果：
         matrix == Matrix(rows: 2, columns: 2, grid: [0.0, 1.5, 3.2000000000000002, 0.0])

         */
```

#### 9、static

Swift 中，用 static 关键字声明静态变量或者函数，它保证在对应的作用域当中只有一份, 同时也不需要依赖实例化。注意：（`用static关键字指定的方法是类方法，他是不能被子类重写的`）比如：

```swift
class Person {

        // 给方法添加class关键字表示创建类方法
        class func work() {
            print("Type Method: Person: 学生.")
        }

        // 使用static关键字创建类方法
        static func ageOfPerson(name: String) {
            print("Type Method: Person name: \(name)")
        }

        // 可以和类方法重名, 以及一样的参数.
        func nameOfPerson(name: String) {
            print("Instance Method: person name \(name)")
        }

    }

    class Student: Person {

        //注意 子类Student的实例方法work(), 和父类的类方法work()没有关系, 二者不在同一个级别上, 不存在同名问题.
        func work() {
            print("Instance Method: Student: University Student")
        }
    }
```

#### 10、mutating

Swift 中，mutating 关键字指的是可变即可修改。用在 structure 和 enumeration 中,虽然结构体和枚举可以定义自己的方法，但是默认情况下，实例方法中是不可以修改值类型的属性。为了能够在实例方法中修改属性值，可以在方法定义前添加关键字 mutating。比如：

```swift
struct rect {
        var width = 0,height = 0
        mutating func changeRect(x:Int, y:Int) {
            self.width += x
            self.height += y
        }

    }


    enum Direction {
        case Top, Left, Right, Bottom
        mutating func lookDirection() {
            switch self {
            case .Top:
                self = .Top
            case .Left:
                self = .Left
            case .Right:
                self = .Right
            case .Bottom:
                self = .Bottom
            }
            print("self === \(self)")
        }
    }


    var re = rect(width: 5, height: 5)
        re.changeRect(x: 32, y: 15)
        print("re = \(re)")

        /**
         打印结果为：re = rect(width: 37, height: 20)
         */

        var dir = Direction.Left
        dir.lookDirection()
        /**
         打印结果为：self === Left
         */
```

#### 11、typealias

在 Swift 中，使用关键字 typealias 定义类型别名(typealias 就相当于 objective-c 中的 typedef)，就是将类型重命名，看起来更加语义化。比如：

```swift
typealias Width = CGFloat
        typealias Height = CGFloat
        func rectangularArea(width:Width, height:Height) -> Double {
            return Double(width*height)
        }
        let area: Double = rectangularArea(width: 10, height: 20)
        print(area)

        /**
         结果为：200.0
         */
```

#### 12、lazy

在 Swift 中，lazy 关键修饰的变量, 只有在第一次被调用的时候才会去初始化值（即懒加载）。比如：

```swift
lazy var first = NSArray(objects: "1","2")
```

注意：用 lazy 修饰的变量必须是用 var 声明的, 因为属性的初始值可能在实例构造完成之后才会得到。而常量属性在构造过程完成之前必须要有初始值，因此无法声明成延迟属性。如果被 lazy 修饰的变量没有在初始化时就被多个线程调用, 那就没有办法保证它只被初始化一次了。



#### 13、init

在 Swift 中，init 关键字也表示构造器。比如：

```swift
class PerSon {
        var name:String
        init?(name : String) {
            if name.isEmpty { return nil }
            self.name = name
        }

    }

let person = PerSon.init(name: "xiaoQiang")
        print("person is name \(String(describing: person?.name))")

/**
打印结果为：person is name Optional("xiaoQiang")
*/
```

在例子里在 init 后面加个”?”号, 表明该构造器可以允许失败。

#### 14、required

在 Swift 里，required 是用来修饰 init 方法的，说明该构造方法是必须实现的。比如：

```swift
class PerSon {
        var name:String
        required init(name : String) {
            self.name = name
        }
    }

    class Student: PerSon {
        required init(name:String) {
            super.init(name: name)
        }
    }
```

从上面的代码示例中不难看出，如果子类需要添加异于父类的初始化方法时，必须先要实现父类中使用`required`修饰符修饰过的初始化方法，并且也要使用`required`修饰符而不是`override`。
注意：　（1）`required`修饰符只能用于修饰类初始化方法。
　　　　（2）当子类含有异于父类的初始化方法时（初始化方法参数类型和数量异于父类），子类必须要实现父类的`required`初始化方法，并且也要使用`required`修饰符而不是`override`。
　　　　（3）当子类没有初始化方法时，可以不用实现父类的`required`初始化方法。



#### 15、extension

在 swift 中，extension 与 Objective-C 的 category 有点类似，但是 extension 比起 category 来说更加强大和灵活，它不仅可以扩展某种类型或结构体的方法，同时它还可以与 protocol 等结合使用，编写出更加灵活和强大的代码。它可以为特定的 class, strut, enum 或者 protocol 添加新的特性。当你没有权限对源代码进行改造的时候，此时可以通过 extension 来对类型进行扩展。extension 有点类似于 OC 的类别 -- category，但稍微不同的是 category 有名字，而 extension 没有名字。在`Swift 中的可以扩展以下几个：`
　　（1）`定义实例方法和类型方法`
　　（2）`添加计算型属性和计算静态属性`
　　（3）`定义下标`
　　（4）`提供新的构造器`
　　（5）`定义和使用新的嵌套类型`
　　（6）`使一个已有类型符合某个接口`
比如：

```swift
//    添加计算属性
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
}
class Person {
    var name:String
    var age:Int = 0
    init?(name:String) {
        if name.isEmpty {
            return nil
        }
        self.name = name
    }

}

extension Person {
    //添加方法
    func run() {
        print("走了50公里")
    }
}


 let oneInch = 25.4.km
        print("One inch is \(oneInch) meters")
        /**

         输出结果：One inch is 25400.0 meters
         */

        let person = Person.init(name: "xiaom")
        person?.run()
        /**

         输出结果：走了50公里
         */
```

想要了解详情可以去看看这篇文涨（[apple documents - extensions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html)）

#### 16、convenient

swift 中，使用 convenience 修饰的构造函数叫做便利构造函数 。便利构造函数通常用在对系统的类进行构造函数的扩充时使用。便利构造函数有如下几个特点：  
　　（1）便利构造函数通常都是写在 extension 里面   
　　（2）便利函数 init 前面需要加载 convenience   
　　（3）在便利构造函数中需要明确的调用 self.init()  
比如：

```swift
extension UIButton{
    //swit中类方法是以class开头的方法，类似于oc中+开头的方法
    class func createButton(imageName:String)->UIButton{

        let btn=UIButton()
        btn.setImage(UIImage(named:imageName), for: .normal)
        btn.sizeToFit()

        return btn
    }
    /*
     convenience:便利，使用convenience修饰的构造函数叫做便利构造函数
     便利构造函数通常用在对系统的类进行构造函数的扩充时使用。

     */

    convenience init(imageName:String,bgImageName:String){
        self.init()
        if !imageName.isEmpty {
            setImage(UIImage(named:imageName), for: .normal)
        }
        if !imageName.isEmpty {
            setBackgroundImage(UIImage(named:bgImageName), for: .normal)
        }
        sizeToFit()
    }
}


let btn = UIButton.init(imageName: "huanying", bgImageName: "")
        btn.frame = CGRect.init(x: 10, y: 120, width: 100, height: 30)
        btn.backgroundColor = UIColor.red
        self.view.addSubview(btn)
```



#### 17、`deinit`

在 Swift 中，deinit 属于析构函数，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。和 OC 中的 dealloc 一样的,通常在 deinit 和 dealloc 中需要执行的操作有：　　
　　（1）对象销毁
　　（2）KVO 移除
　　（3）移除通知
　　（4）NSTimer 销毁



#### 18、`fallthrough`

swift 语言特性 switch 语句的 break 可以忽略不写,满足条件时直接跳出循环.fallthrough 的作用就是执行完当前 case,继续执行下面的 case.类似于其它语言中省去 break 里，会继续往后一个 case 跑，直到碰到 break 或 default 才完成的效果。比如：

```swift
var myClass = MyClass(parm: 9)
        let index = 15
        switch index {
            case 1  :
                print( "index is 100")
            case 10,15  :
                print( "index is either 10 or 15")
            case 5  :
                print( "index is 5")
            default :
                print( "default case")
        }

        /**
         输出结果：index is either 10 or 15
         */

  let index = 15
        switch index {
        case 100  :
            print( "index is 100")
            fallthrough
        case 10,15  :
            print( "index is either 10 or 15")
            fallthrough
        case 5  :
            print( "index is 5")
        default :
            print( "default case")
        }

/**

打印结果：
    index is either 10 or 15
    index is 5

*/
```



#### 19、protocol

在 Swift 中，protocol 关键字也是属于协议。比如：

```swift
protocol PersonProtocol {
    var height: Int { get set }
    var weight: Int { get }
    func getName()
    func getSex()
    func getAge(age: Int)
}

struct Student: PersonProtocol {
    var height = 178
    var weight = 120
    func getName() {
        print("MelodyZhy")
    }
    func getSex() {
        print("boy")
    }
    func getAge(age: Int) {
        print("age = \(age)")
    }
}

 var stu = Student()
        let height1 = stu.height
        stu.height = 180
        print("height1 = \(height1) height2 = \(stu.height)")

        /**
         输出结果：
         height1 = 178 height2 = 180
         */
```

#### 20、guard 关键字(守护)

```swift
guard expression else {
    //语句
    //必须包含一个控制语句：return，break，continue或throw。
}

这里，expression是一个布尔表达式（返回true或者false）。
如果对表达式求值false，guard则执行代码块内的语句。
如果对表达式求值true，guard则从执行中跳过代码块内的语句
```

\*\*

#### 21、is 用来做类型检查

```swift
let cat = terricole()
        let fish = SeaAnimals()
        let arr = [cat,fish]

        for anima in arr {
            if anima is terricole{
                print("这是陆地动物")
            }else if anima is SeaAnimals{
                print("这是海洋动物")
            }
        }
```

#### 22、as 用来做类型转换(注：如果不确定类型转换能否成功，可以在 as 后面加问号 “?”)

```swift
for animas in arr {
            if let c = animas as? terricole{
                print("这是陆地动物")
            }else if let w = animas as? SeaAnimals{
                print("这是海洋动物")
            }
        }
```

**参考文献:**
[https://www.cnblogs.com/liYongJun0526/p/7522130.html](https://www.cnblogs.com/liYongJun0526/p/7522130.html)

[https://www.hangge.com/blog/cache/detail_524.html#](https://www.hangge.com/blog/cache/detail_524.html#)
