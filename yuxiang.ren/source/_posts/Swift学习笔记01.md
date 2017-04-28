---
title: Swift学习笔记01
date: 2016-04-23 22:40:24
categories: iOS
tags: Swift
---

### 一、常量&变量
1. 常量的使用注意
* 优先使用常量
* 常量的本质：保存的是内存地址,不可更改,但可以通过内存地址拿到对应的对象,修改对象的属性.

<!-- more -->

### 二、数据类型
1. 整型与浮点型
* 整型:`Int`
* 浮点型:`Double`
2. 类型推导
* 如果定义一个标识符时,有直接赋值, 那么可以通过赋值的类型推导出标识符的类型.
2. 基本运算
* 相同类型才可以进行运算(没有隐式转换)

### 三、逻辑分支
1. if的使用
* if后面的**()**可以省略
* 没有非0即真的概念,必须有明确的Bool值(flase/true)
2. 三目运算符
* 与OC使用一致
3. `guard`的使用

```swift
    guard 条件语句 else{
        // 条件不满足代码
        break/return/continue/throw
    }
     // 条件满足代码
```

4. switch使用
* ()可以省略
* case结束后可以省略break
* case可以判断多个条件
* 如果希望case有穿透, 加`fallThrough`
* 特殊用法:
     * 判断浮点型
     * 判断字符串
     * 判断区间类型

### 四、循环
1. for循环
*  方式一

    ```swift
    for var i= 0; i< 10; i++ {
        print(i)
    }
    ```
* 方式二

    ```swift
     for i 0..<10 {
        prin(i)
     }
    ```
* 方式三

    ```swift
     for _ 0..<10 {
        
     }
    ```
2. while/dowhile循环
* while后面() 可以省略
* 没有非0即真

### 五、字符串
1. 定义字符串
* 定义可变字符串`var str = "string"`
* 定义不可变字符串`let str = "string"`
2. 获取字符串长度
* `str.characters.count`
3. 遍历字符串

```swift
    for c in str.characters {
        print(c)
    }
```
4. 字符串拼接
* 两个字符串之间:`str1 + str2`
* 字符串与标识符之间:`\(age)`
* 字符串格式化:`String(format:"%d %d",\ argments:[age1,age2])`
5. 字符串截取
* 将String类型转成NSString string as NSString

 ```swift
    let urlString = "www.yuxiang.ren"  
    let header = (urlString as NSString).substringToIndex(3)
    let footer = (urlString as NSString).substringFromIndex(10)
    let range = NSMakeRange(4, 5)
    let middle = (urlString as NSString).substringWithRange(range)
 ```
* swift的截取方式

 ```swift   
    let toIndex = urlString.startIndex.advancedBy(3)
    let header1 = urlString.substringToIndex(toIndex)

    let fromIndex = urlString.endIndex.advancedBy(-3)
    let footer1 = urlString.substringFromIndex(fromIndex)

    let range1 = Range(start: toIndex.advancedBy(1), end: fromIndex.advancedBy(-1))
    let middle1 = urlString.substringWithRange(range1)
 ```

### 六、数组
 1. 定义数组
  * 不可变数组:`let arr = ["www", "shlyren"]`
  * 可遍数组:`var arrM = [String]()`
 2. 遍历数组
  * `for value in arr{print(valur)}`
  * `for (index, value) in array.enumerate() {print(index); print(value)}`
 3. 数组的合并
  * 相同类型的数组可以现价合并

### 七、字典
1. 定义字典
* 不可变字典:`let dict = ["name" : "ren"]`
* 可变字典:`var dictM = [String : NSObject]()`
2. 遍历字典
* `for key in dict.keys {}`
* `for value in dict.values {}`
* `for (key, value) in dict {}`
3. 合并字典
* 相同类型之间的字典也不可以合并

### 八 、元组
1. 使用元组定义数组
* `let info = (“Not Found”, 404) info.0 info.1`
* `let info = (errorInfo : “Not Found”, errorCode : 404) info.errorInfoinfo.errorCode`
* `let (errorInfo, errorCode) = (“Not Found”, 404)`

### 九、可选类型
1. 定义可选类型
* `var name : Optional<String> = nil`
* `var name : String? = nil`
2. 给可选类型赋值
* `name = “renyuxaing”`
3. 从可选类型取值
* `name! ` 强制解包
4. 可选类型注意: 如果可选类型中没有值,强制解包会崩溃
5. 可选绑定
* `if let tempName = name {} `
     * 判断name是否有值
     * 如果有值,对name强制解包并赋值给tempName
* 简单写法 
     * `if let name = name {}`


