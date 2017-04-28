---
title: 'const,static,extern简介'
date: 2016-03-26 19:30:15
categories: iOS
tags: Objective-C
---

## const与宏的区别(面试题):
1. `const`简介:之前常用的字符串常量,一般是抽成宏,但是苹果不推荐我们抽成宏,推荐我们使用const常量.  
   主要是因为以下四点:
   * `编译时刻`:**宏**是预编译,**const**是编译阶段.
      * `编译检查`:**宏**不做检查,不会报编译错误,只是替换,**const**会编译检查,会报编译错误.
      * `宏的好处`:宏能定义一些函数,方法,const不能.
      * `宏的坏处`:使用大量红,容易造成编译时间久,每次都需要重新替换.

<!-- more -->
　　**注意:**很多blog都说使用宏会消耗很多内存,但我这验证并不会生成很多内存,宏定义的是常量,常量都放在常量区,只会产生一份内存.
​    
## const
1. **const作用**:const的作用是用来限制类型的.
* const仅仅用来修饰右边的变量(基本数据类型p,指针变量*p);
* 被const修饰的变量是只读的.
2. const基本使用

 ```objc
 - (void)viewDidLoad {
    [super viewDidLoad];
    int a = 1;
    a = 20;
    const int b = 20; // b:只读变量
    int const b = 20; // b:只读变量
    b = 1;  // 不允许修改值
    
    // const:修饰指针变量*p，带*的变量，就是指针变量.
    // 定义一个指向int类型的指针变量，指向a的地址
    int *p = &a;
    int c = 10;
    p = &c;// 允许修改p指向的地址，
    *p = 20;  // 允许修改p访问内存空间的值
    
    // 两种方式一样,const修饰指针变量访问的内存空间，修饰的是右边*p1，
    const int *p1; // *p1：常量 p1:变量
    int const *p1; // *p1：常量 p1:变量

    // const修饰指针变量p1
    int * const p1; // *p1:变量 p1:常量

    // 两种方式一样,第一个const修饰*p1 第二个const修饰 p1
    const int * const p1; // *p1：常量 p1：常量
    int const * const p1;  // *p1：常量 p1：常量
 }
 ```

3. const开发中的使用场景
* 当一个方法参数只读
* 定义只读全局变量

 ```objc
    // 定义只读全局常量
    NSString * const str  = @"123";
    // 当一个方法的参数，只读.
    - (void)test:(NSString * const)name;
    // 指针只读,不能通过指针修改值
    - (void)test1:(int const *)a;
    // 基本数据类型只读
    - (void)test2:(int const)a;
 ```

## static和extern的简单使用
1. **static作用**
* 修饰局部变量:
     1. 延长局部变量的什么周期:程序结束才会销毁
     2. 局部变量只会生成一份内存,只会初始化一次.
* 修饰全局变量
     1. 只能在本文件中访问,修改全局变量的作用域,生命周期不会改.
2. **extern作用**
* 只是用来获取全局变量(包括全局静态变量)的值, 不能用于定义变量.
3. **entern原理**
* 现在当前文件查找有没有全局变量,没有找到,才会其他文件查找.
4. 代码演示
 ```objc
    int a = 20;// 全局变量：只有一份内存，所有文件共享，与extern联合使用。
    static int age = 20;// static修饰全局变量

    - (void)test
    {
         // static修饰局部变量
        static int age = 0;
        age++;
        NSLog(@"%d",age);
    }

    - (void)viewDidLoad {
        [super viewDidLoad];
        [self test];

        extern int age;
        NSLog(@"%d",age);
    }
 ```

## static与const联合使用
1. **static与const作用:**声明一个只读的静态变量
2. **开发场景:**在`一个文件中`经常使用的字符串常量,可以使用static与const组合.
* 开发中常用static修饰全局变量,只改变作用域
* 为什么要改变全局变量作用域? 防止重复声明全局变量。
* 开发中声明的全局变量，有些不希望外界改动，只允许读取。比如一个基本数据类型不希望别人改动
 ```objc
 // 声明一个静态的全局只读常量
 static const int a = 20;
 // 开发中经常拿到key修改值，因此用const修饰key,表示key只读，不允许修改。
 static  NSString * const key = @"name";
 // 如果 const修饰 *key1,表示*key1只读，key1还是能改变。
 static  NSString const *key1 = @"name";
 ```

## extern与const联合使用
1. 开发使用场景:
* 在**多个文件中**经常使用的同一个字符串常量,可以使用extern与const组合
2. 原因:
* **static与const组合**:在每个文件中都需要定义一份静态全局变量.
* **entern与const组合**:只需定义一份全局变量,多个文件共享.
3. 示例代码
 ```objc
 // .h文件中
 extern NSString * const nameKey = @"name";
 //.m文件中
 NSString * const nameKey = @"name";
 ```


