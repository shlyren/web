---
title: Swift学习笔记03
date: 2016-04-26 18:08:29
categories: iOS
tags: [Swift]
---

## 一、三大特性
### 1. 封装，继承， 多态
* 多态产生的三大条件
    * 必须有继承
    * 必须有重写
    * 父类指针指向子类对象
* 如果子类对父类的方法不满意， 可以对福列的方法进行重写
    * 在Swift中如果对弗雷德方法进行个重写,必须在`func`前加`override`

<!-- more -->

### 2. 重载
* 重载的使用场景
    * 参数的类型不同
    * 参数的个数不同

---

## 二、自动引用计数
### 1. 工作机制
* 当有一个强指针指向某一个对象是, 该对象的引用计数会+1
* 当该强指针消失时,引用计数会-1
* 当引用计数为0时,该对象会被系统回收

### 2. 循环引用
* 与OC一样,Swift在开发中经常会出现循环引用的问题
* 解决方法,Swift提供了两种方法
    * `weak`: 和OC中的`__weak`一样,当指向的对象销毁时,会自动将指针指向`nil`
    * `unowned`: 和OC中的`__unsafe_unretained`一样,当指向的对象销毁时,指针依然指向之前的对象,会导致野指针错误

---

## 三、可选链
### 1. 可选链的概念
* 它的可选性体现于请求或调用的目标当前可能为空
    * 如果可选的目标有值，那么调用就会成功
    * 如果可选的目标为空，则这种调用会返回空
* 多次调用被链接在一起形成一个链，如果任何一个节点为空将导致整个链失效。

### 2. 可选链的使用
* 在可选类型后面放一个问号,可以定义一个可选链
* 这一点很像在可选值后面放一个叹号来强制解包
    * 他们的主要区别在于当可选值为空时可选链即刻失效
    * 然而一般的强制解析将会引发运行时的错误
* 因为可选链的结果可能为空,也可能有值,因此它的返回值是一个可选类型
    * 可以通过判断返回值是否有值来判断是否调用成功
        * 有值,表示调用成功
        * 为空,表示调用失败

---

## 四、协议
### 1. 协议的格式
* 协议的定义与类,结构体,枚举的定义都非常相似

```swift
protocol SomeProtocol {
    // 协议方法
}
```
* 遵守协议的格式

```swift
class SomeClass: SomeSuperClass, FirstProtocol,  AnotherProtocol {
    // 类的内容
    // 实现协议中的方法
}
```

### 2. 协议的基本使用
* 定义协议和遵守协议

```swift
// 1.定义协议
protocol SportProtocol {
    func playBasketball()
    func playFootball()
}
// 2.遵守协议 注意:默认情况下在swift中所有的协议方法都是必须实现的,如果不实现,则编译器会报错
class Person : SportProtocol {
    var name : String?
    var age : Int = 0

    // 实现协议中的方法
    func playBasketball() {
        print("人在打篮球")
    }
    func playFootball() {
        print("人在踢足球")
    }
}
```

* 协议之间的继承

```swift
protocol CrazySportProtocol {
    func jumping()
}

protocol SportProtocol : CrazySportProtocol {
    func playBasketball()
    func playFootball()
}
```

### 3. 代理设计模式
```swift
protocol BuyTicketProtocol {
    func buyTicket()
}

class Person {
    // 1.定义协议属性
    var delegate : BuyTicketProtocol
    // 2.自定义构造函数
    init (delegate : BuyTicketProtocol) {
        self.delegate = delegate
    }
    // 3.行为
    func goToBeijing() {
        delegate.buyTicket()
    }
}

class HuangNiu: BuyTicketProtocol {
    func buyTicket() {
        print("买了一张火车票")
    }
}
let p = Person(delegate: HuangNiu())
p.goToBeijing()
```

### 4. 协议中方法的可选
* Swift中的协议默认为必须实现,如果不实现则直接报错
* 要想跟OC一样将协议中的方法设置为可选,则需要特殊操作

```swift
// 1.定义协议
@objc 
protocol SportProtocol {
    func playBasketball()
    optional func playFootball()
}

// 2.遵守协议
class Person : SportProtocol {
    var name : String?
    var age : Int = 0

    // 实现协议中的方法
    @objc func playBasketball() {
        print("人在打篮球")
    }
}
```

---

## 五、闭包
### 1. 闭包的介绍
* 闭包和OC中的block非常相似
    * OC中的block是匿名的函数
    * Swift中的闭包是特殊的函数
    * block和闭包都经常用于回调

### 2. 闭包的写法
* 类型:**(形参列表)->(返回值)**

```swift
// 定义
func test(finished : (parameter : String) -> ()){
   finished(parameter : "block参数")
}
   
// 调用
test({ (parameter) -> () in
    print("block传过来的参数:\(parameter)")
})
```

### 3. 闭包的简写
* 如果闭包没有写参数, 没有返回值,闭包的参数可以不写

```swift
test({
    // 代码块
})
```
* 尾随闭包写法:
    * 如果闭包是函数的最后一个参数,则可以将闭包卸载()后面
    * 如果函数只有一个参数,并且这个参数是闭包,那么()可以不写

```swift
// 闭包是函数的最后一个参数
test(){
    // 代码块
}

//函数只有一个参数,并且这个参数是闭包
test {
    // 代码块
}
```

### 4. 闭包导致的循环引用解决方案
* 方案一:
    * 使用`weak` 对当前控制器使用弱引用
    * 但是因为`self`可能有值也可能没有值,因此`weakSelf`是一个可选类型,在真正使用时可以对其强制解包(该处强制解包没有问题,因为控制器一定存在,否则无法调用所在函数)

```swift
 weak var weakSelf = self
```
* 方案二(常用):
    * 和方案一类型,只是书写方式更加简单
    * 可以写在闭包中,并且在闭包中用到的self都是弱引用

```swift
test({[weak self] (parameter) -> () in
    print("block传过来的参数:\(parameter)")
})
```

* 方案三:
    * 使用关键字`unowned`
    * 从行为上来说 `unowned` 更像OC中的 `unsafe_unretained`
    * `unowned`表示:即使它原来引用的对象被释放了，仍然会保持对被已经释放了的对象的一个 "无效的" 引用，它不能是`Optional`值，也不会被指向nil

```swift
test({[unowned self] (parameter) -> () in
    print("block传过来的参数:\(parameter)")
})
```

---

## 六、懒加载
### 1. 懒加载的介绍
* 苹果的设计思想:希望所有的对象在使用时才真正加载到内存中)
* 和OC不同的是Swift有专门的关键字来实现懒加载
* `lazy`关键字可以用于定义某一个属性懒加载

### 2. 懒加载的使用
* 格式: 在属性前用`lazy`修饰

```swift
// 苹果官方写法: 创建的时候可以设置一些属性
lazy var btn1 : UIButton = {
    let tempBtn = UIButton()
    tempBtn.setTitle("按钮" forState:.Normal)
    return tmpBtn
}()
    
// 简单写法: 只是创建对象
lazy var btn1 : UIButton = UIButton()
```

---

## 七、常见注释
### 1. 单行注释
```swift
// 注释
```

### 2. 多行行注释
* Swift中`/* */`可以嵌套使用

```swift
/*
  注释
   /* 注释 */
*/
```

### 3. 文档注释
```swift
/// 注释
```

### 4. 分组注释
```swift
// MARK:- 带有分割线
// MARK: 不带分割线
```

---

## 八、访问权限
### 1. Swift中的访问权限
* Swift中的访问控制模型基于模块的源文件这两个概念
    * `internal`: 在本模块中可以进行访问
    * `private`: 在当前源文件(项目)可以访问
    * `public`: 在其他模块中可以访问

---

## 九、异常处理
### 1. 异常的介绍
* 只要在编程,就一定会遇到错误处理的问题
* Swift在设计的时候就尽可能的让开发者感知错误,明确处理错误
    * 比如: 只有使用个`Optional`才能处理空值
* 描述一个错误
    * 在Swift中, 任何一个遵守`ErrorType protocol`的类型，都可以用于描述错误。
    * `ErrorType`是一个空的`protocol`, 它唯一的功能，就是告诉Swift编译器，某个类型用来表示一个错误。
    * 通常，我们使用一个`enum`来定义各种错误的可能性

### 2. 异常的示例
* 假如想要读取一个文件中的内容,按照OC的逻辑我们可以这样来模拟
    * 当调用方法获取结果为`nil`时,你并不能确定到底参数了什么错误得到了`nil`

```swift
func readFileContent(filePath : String) -> String? {
    // 1.filePath为""
    if filePath == "" {
        return nil
    }
    // 2.filePath有值,但是没有对应的文件
    if filePath != "/User/Desktop/123.plist" {
        return nil
    }
    // 3.取出其中的内容
    return "123"
}

readFileContent("abc")
```

* 使用异常对上述方法的改进

```swift
// 1.定义异常
enum FileReadError : ErrorType {
    case FileISNull
    case FileNotFound
}

// 2.改进方法,让方法抛出异常
func readFileContent(filePath : String) throws -> String {
    // 1.filePath为""
    if filePath == "" {
        throw FileReadError.FileISNull
    }

    // 2.filepath有值,但是没有对应的文件
    if filePath != "/User/Desktop/123.plist" {

        throw FileReadError.FileISNull
    }

    // 3.取出其中的内容
    return "123"
}
```

### 3. 异常处理的三种方式
```swift
// 1. try方式,需要手动处理异常
do {
    let result = try readFileContent("abc")
} catch {
    print(error)
}

// 2. try?方式,不处理异常,如果出现了异常,则返回一个nil.没有异常,则返回对应的值
// 最终返回结果为一个可选类型
let result = try? readFileContent("abc")

// 3. try!方法,告诉系统该方法没有异常.
// 注意:如果出现了异常,则程序会崩溃
try! readFileContent("abc")
```

---

## 十、OC、Swift混编
### 1. Swift调用OC
* 创建桥接文件
* 配置桥接文件
    * 步骤图
      ![](https://yuxiang.ren/images/images/配置桥接文件.png)

### 2. OC调用Swift代码
* 导入**项目名称-Swift.h**
* Swift中的所有类以及方法和属性必须使用`public`修饰


