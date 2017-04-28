---
title: Swift学习笔记02
date: 2016-04-24 15:34:28
categories: iOS
tags: [Swift]
---

### 一、类型转换
#### 1. is as
* is: 判断是否是某一种类型(`isKindOfClass`)
* as: Swift字符串转OC字符串`str as NSString`

#### 2. as? as!
* as?: 将AnyObject转成可选类型,通过判断可选类型是否有值,来决定是否转换成功
* as!: 将AngObject转成具体类型,但是如果不是该类型程勋会崩溃
  <!-- more -->

```swift
let arr : [AnyObject] = ["ren", 20, 1.77]
let objc = arr.first
// NSObject? 转成 String?
if let name = objc as? String {
print(name)
}
//不推荐使用,转换不成功会崩溃
let name1 = objc as! String
```
---

### 二、函数
#### 1. 函数介绍
* 函数相当于OC中的方法
* func 是关键字, 多个参数列表之间可以使用逗号分开,没有参数之间写()
* 使用箭头`->`指向返回值类型
* 如果没有返回值类型,返回值类型写`Void` 也可以不写

```swift  
   func 函数名(参数列表) -> 返回值类型 {
        代码块...
        return 返回值
   }
```

#### 2. 常见的函数类型

```swift
// 1.没有参数,没有返回值
func about() -> Void {
    print("没有参数,没用返回值")
}
// 调用函数
about()
// 简单写法
func about1() -> () {
    print("如果没用返回值,Void可以写成()")
}
func about2() {
    print("如果没有返回值,后面的内容可以都不写")
}

// 2.有参数,没用返回值
func callPhone(phoneNum : String) {
    
}

// 3.没用参数,有返回值
func readMessage() -> String {
    return "返回值"
}
var str = readMessage()
print(str)

// 4.有参数,有返回值
func sum(num1 : Int, num2 : Int) -> Int {
    return num1 + num2
}
var result = sum(20, num2: 30)
print(result)

// 5.有多个返回值的函数
let nums = [1, 3, 4, 8, 22, 23]
func getNumCount(nums : [Int]) -> (oddCount : Int, evenCount : Int) {
    var oddCount = 0
    var evenCount = 0
    for num in nums {
        if num % 2 == 0 {
            oddCount++
        } else {
            evenCount++
        }
    }
    return (oddCount, evenCount)
}

let result = getNumCount(nums)
result.oddCount
result.evenCount
```

#### 3. 函数的使用注意
* 注意一: 外部参数和内部参数
    * 在函数内部可以看到的参数就是内部参数
    * 在函数外部可以看到的参数就是外部参数
    * 默认情况下,从第二个参数开始,参数既是内部参数也是外部参数
    * 如果第一个参数也想要有外部参数,可以设置标签:`在变量名前添加`
    * 如果不想要外部参数,可以参参数前加`_`

```swift
func sum(num1 num1 : Int, a num2 : Int, _ num3 : Int) -> Int {
    return num1 + num2 + num3
}
sum(num1: 10, a: 20, 30) // 60

// 方法重载:方法名称相同,但是参数不同,可以称之为方法的重载(了解)
func sum(num1 : Int, num2 : Int) -> Int {
    return num1 + num2
}
```
* 注意二: 默认参数
     * 某些情况,如果没有传入具体的参数可以使用默认参数

```swift
func demo(name : String = "默认参数") {}
```
* 注意三: 可变参数
     * swift中函数的参数可以变化, 它可以接受不确定数量的参数
     * 这些参数的类型必须相同
     * 在参数类型后面加`...` 即可表示可变参数

```swift
func sum(nums : Double...) -> Double {
    var total = 0.0
    for num in muns {
        total += num
    }
    return total
}
```
* 注意四: 引用类型(指针类型)
    * 默认情况下, 函数的参数是值传递, 如果想改变外面的变量,则需要传递变量的地址
    * 通过Swift的`inout`关键字可以实现

```swift
var a = 10
var b = 20

func exchange(inout a : Int, inout b : Int) -> Int {
    let temp = a
    a = b
    b = temp
}
// 调用
exchange(&a, b: &b) // a == 20. b == 10
```

#### 4. 函数的嵌套使用
* swift中函数可以嵌套使用,但不推荐

```swift
func test1() {
    func test2() {
        print("test2")
    }
    print("test1")
    test2() // test2 必须在test1中调用 因为test2的作用域只在test1中
}
test1()
// 打印结果: 
// test1
// test2
```

#### 5. 函数的类型
* 每个函数都有属于自己的类型,由函数的参数类型和返回值类型组成
    * 在下面的例子中定义了两个简单的数学函数: `addTwoInts` 和 `multiplyTwoInts`
    * 这两个函数都传入了两个Int类型, 返回一个合适的Int类型
    * 这两个函数的类型是`(Int, Int) -> Int`

```swift
func addTwoInts(a : Int, b : Int) -> Int {
    return a + b
}

func multiplyTwoInt(a : Int, b : Int) -> Int {
    return a * b
}
```

* 抽取两个函数的类型并使用

```swift
// 定义函数的类型
var mathFunction : (Int, Int) -> Int = addTwoInts
// 使用函数的名称
mathFunction(10, 20) // 30
// 给函数的标识符赋值
mathFunction = multiplyTwoInt
mathFunction(10, 20) // 200
```

* 函数的类型作为方法的参数

```swift
func result(a : Int, b : Int, method : (Int, Int) -> Int) {
    print(method(a, b)) // 打印结果
}
//  调用
result(10, b: 20, method: addTwoInts) // 30
result(10, b: 20, method: multiplyTwoInt) // 200
```

* 函数作为方法的返回值

```swift
func test2() -> ((Int, Int) -> Int) {
    return addTwoInts
}
let tmpFunc = test2()
tmpFunc(20, 30) // 50
```
---

### 三、 枚举
#### 1. 基本概念
* 概念介绍
    * 枚举定义了一个通用类型的一组相关的值,使你可以在你的代码中以一个安全的方式来使用这些值.
    * 在C/OC中枚举的值只能为整型
    * Swift中的枚举更加灵活,它可以提供一个值是字符串,字符,整型或浮点值
* 枚举语法

   ```swift
   enum 枚举名 {
        枚举值
   }
   ```

#### 2. 枚举定义
* 以下是指南针四个方向的一个例子
    * `case`关键字表明新的一行成员值将被定义
    * 与C 和 OC 不一样, Swift的枚举成员在被创建的时候不会被赋一个默认值

   ```swift
   enum CompassPoint {
        case North
        case South
        case East
        case West
   }
   ```

* 方式二: 多个陈远志可以出现在同一行上

   ```swift
   enum CompassPoint {
        case North, South, East, West
   }
   ```

#### 3. 给枚举类型赋值
* 枚举类型赋值可以是字符串/字符/整型/浮点型
    * 注意如果有给枚举类型赋值,则必须在枚举类型后面明确说明具体的类型

 ```swift
 // 方式一
 enum CompassPoint : Int {
      case North = 1
      case South = 2
      case East = 3
      case West = 4
 }

 // 方式二
 enum CompassPoint : Int {
      case North = 1, South, East, West
 }

 // 赋具体值
 let point = CompassPoint(rawValue : 1)
 ```
---

### 四、结构体
#### 1. 结构体介绍(struct)
* 基本概念
    * 结构体是由一系列具有相同类型或不同类型的数据构成的数据集合
    * 结构体指的是一种数据结构
    * 结构体是值类型,在方法中传递时是值传递
* 结构体的定义格式

 ```swift
 struct Location {
      var x : Double
      var y : Double
 }
 ```

#### 2. 结构体增强
* 扩充构造函数
     * 默认情况下创建的Location时使用Location(x : x值, y : y值)
     * 但是为了让我们在使用该结构体是更加灵活, swift还可以对构造函数进行个扩充
     * 扩充的注意点
         * 在扩充的构造函数中必须保证成员变量是有值的
         * 扩充的构造函数会覆盖原有的构造函数

 ```swift
 struct Location {
      var x : Double
      var y : Double

      // 原有的构造函数
      init() {
      }
    
      // 扩充的构造函数
      init(x : Double, y : Double) {
          self.x = x
          self.y = y
      }
 }
 let location = Location(x: 100, y: 100)
 ```

* 扩充方法
     * 为了让结构体使用更加灵活,swift的结构体中可以扩充方法
     * 例子:为了Location结构体扩充两个方法
         * 向水平方向移动的方法
         * 向垂直方向移动的方法

 ```swift
 struct Location {
      var x : Double
      var y : Double

      init(x : Double, y : Double) {
          self.x = x
          self.y = y
      }

      mutating func moveH(x : Double) {
          self.x += x
      }

      mutating func moveV(y : Double) {
          self.y += y
      }
   }
 ```
 ---

### 五、类的使用
#### 1. 类的介绍和定义
* `class`是Swift中的关键字,用于定义类
* 注意:
    * 定义的类可以没有父类,那么该类是rootClass
    * 通常情况下,定义类是,继承NSObject

    ```swift
    class 类名 : superClass {
        // 类的属性和方法
    }
    ```

#### 2. 类的属性
* swift中类的属性有多种
    * 存储属性: 存储实例的常量和变量
        * 可以给存储属性提供一个默认值，也可以在初始化方法中对其进行初始化
* 计算属性: 通过某种方式计算出来的属性
    * 计算属性并不存储实际的值，而是提供一个getter和一个可选的setter来间接获取和设置其它属性
        * 计算属性一般只提供getter方法
        * 如果只提供getter，而不提供setter，则该计算属性为只读属性,并且可以省略get{}
    * 类属性: 与整个类自身相关的属性
        * 所有的类和实例都共有一份类属性.因此在某一处修改之后,该类属性就会被修改
        * 类属性的设置和修改,需要通过类来完成
* 监听属性的改变
    * 在OC中我们可以重写`set`方法来监听属性的改变
    * Swift中可以通过属性观察者来监听和响应属性值的变化
    * 通常是监听存储属性和类属性的改变.(对于计算属性，我们不需要定义属性观察者，因为我们可以在计算属性的`setter`中直接观察并响应这种值的变化)
    * 我们通过设置以下观察方法来定义观察者
        * `willSet`：在属性值被存储之前设置。此时新属性值作为一个常量参数被传入。该参数名默认为`newValue`，我们可以自己定义该参数名
        * `didSet`：在新属性值被存储后立即调用。与`willSet`相同，此时传入的是属性的旧值，默认参数名为`oldValue`
        * `willSet`与`didSet`只有在属性第一次被设置时才会调用，在初始化时，不会去调用这些监听方法
    * 监听的方式如下:

   ```swift
   class Person : NSObject {
      var name : String? {
          // 可以给newValue自定义名称
          willSet (new){ // 属性即将改变,还未改变时会调用的方法
              // 在该方法中有一个默认的系统属性newValue,用于存储值
          }
          // 可以给oldValue自定义名称
          didSet (old) { // 属性值已经改变了,会调用的方法
              // 在该方法中有一个默认的系统属性oldValue,用于存储旧值
          }
      }
   }
   ```

#### 3. 类的构造函数
* 构造函数的介绍
    * 构造函数类似于OC中的初始化方法:`init`方法
    * 默认情况下载创建一个类时,必然会调用一个构造函数
    * 即便是没有编写任何构造函数，编译器也会提供一个默认的构造函数。
    * 如果是继承自`NSObject`,可以对父类的构造函数进行重写
* 基本使用
    * 类的属性必须有值
    * 如果不是在定义时初始化值,可以在构造函数中赋值
* 初始化时给属性赋值
    * 很多时候,我们在创建一个对象时就会给属性赋值
    * 可以自定义构造函数
    * **注意**:如果自定义了构造函数,会覆盖init()方法.即不在有默认的构造函数
* 字典转模型(初始化时传入字典)
    * 真实创建对象时,更多的是将字典转成模型
    * 注意
        * 去字典中取出的是`NSObject`,任意类型.
        * 可以通过`as!`转成需要的类型,再赋值(不可以直接赋值)

     ```swift
      class Person: NSObject {
              var name : String
              var age : Int
          
              // 自定义构造函数,会覆盖init()函数
              init(dict : [String : NSObject]) {
                  name = dict["name"] as! String
                  age = dict["age"] as! Int
              }
       }

          // 创建一个Person对象
          let dict = ["name" : "why", "age" : 18]
          let p = Person(dict: dict)
     ```

* 字典转模型(KVC)
    * 利用KVC字典转模型会更加方便
    * 注意
        * KVC并不能保证会给所有的属性赋值
        * 因此属性需要有默认值
            * 基本数据类型默认值设置为0
            * 对象或者结构体类型定义为可选类型即可(可选类型没有赋值前为nil)

      ```swift
       class Person: NSObject {
            // 结构体或者类的类型,必须是可选类型.因为不能保证一定会赋值
            var name : String?
        
            // 基本数据类型不能是可选类型,否则KVC无法转化
            var age : Int = 0
        
            // 自定义构造函数,会覆盖init()函数
            init(dict : [String : NSObject]) {
                // 必须先初始化对象
                super.init()
        
                // 调用对象的KVC方法字典转模型
                setValuesForKeysWithDictionary(dict)
            }
        }
        
       // 创建一个Person对象
       let dict = ["name" : "why", "age" : 18]
       let p = Person(dict: dict)
      ```

#### 4. 类的析构函数
* Swift 会自动释放不再需要的实例以释放资源
    * Swift 通过自动引用计数（ARC）处理实例的内存管理
    * 当引用计数为0时,系统会自动调用析构函数(不可以手动调用)
    * 通常在析构函数中释放一些资源(如移除通知等操作)
* 析构函数的写法

```swift
deinit {
// 执行析构过程
}
```


