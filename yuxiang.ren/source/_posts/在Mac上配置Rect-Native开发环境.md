---
title: 在Mac上配置Rect Native开发环境
date: 2017-03-30 15:06:25
categories: 教程
tags: [教程 MacOS]
---



## 一. 环境需求

1 .  安装Homebrew(可选)

```shell
ruby -e "$(curl -fsSL <a href="https://raw.githubusercontent.com/Homebrew/install/master/install">https://raw.githubusercontent.com/Homebrew/install/master/install</a>)
```


2. 安装`watchman` (用于监控bug文件)

   ```Shell
   brew install watchman
   ```

3. 安装`flow`(用于找出代码中可能存在的类型错误)

   ```shell
   bew install flow
   ```


<!-- more -->

### 二. ios环境

* Xcode7及以上版本

### 三. 安装`React Native`

1. 安装命令

   ```shell
   npm install -fg react-native-cli
   ```

   * 出现下图情况需要用管理员权限
   * ![690](https://ww4.sinaimg.cn/mw690/6a80ef0fgw1f4a5eimqh5j214m0t6ti8.jpg)

### 四. 创建`RReact Native`工程

* react-native init demo

  ```
  react-native init demo
  ```

* 可能需要等待一段时间

  ![](https://ww1.sinaimg.cn/mw690/6a80ef0fgw1f4a4q7xtxij20v404ywfr.jpg)

* 出现下图所示表示初始化成功

  ![](https://ww2.sinaimg.cn/mw690/6a80ef0fgw1f4a4q94jjsj20us0e60wh.jpg)

* 目录结构分析

  * 默认生成`iOS`和`Android`两个平台的项目
  * 其中, `index.android.js`和`index.ios.js`是`Android`和`iOS`的空壳应用文件
  * `node_modules`文件夹是为`Node.js`存放和管理npm包资源,也包含`react-native`框架文件

* 运行`iOS`程序

  * 打开`ios`文件夹下的`demo.xcodeproj`,`command+r`运行

  * 启动`React native`服务器,不要关闭此窗口

    ![](https://ww4.sinaimg.cn/mw690/6a80ef0fgw1f4a57e98fwj20vo0lojvz.jpg)

  * 客户端运行界面

    ![](https://ww4.sinaimg.cn/mw690/6a80ef0fgw1f4a58cn2ukj20ku12a75q.jpg)