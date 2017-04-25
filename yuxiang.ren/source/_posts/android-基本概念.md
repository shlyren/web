---
title: android-基本概念
date: 2017-03-24 09:23:22
categories: Android
tags: Android
---



Android的一些基本的概念, 名词解释等.....

## 1. 安卓的五种布局

1. 线性布局, 分水平布局和垂直布局, 
2. 相对布局, 
3. 帧布局
4. 表格布局
5. 绝对布局

<!-- more -->

## 2. 数据存储的几种方式

1. 保存到文件
2. SQLite数据库存储
3. 内容提供者
4. sharedpreferences保存数据
5. 网络

## 3. 文件的权限

1. `MODE_PRIVATE`: 私有模式, 只能被应用本身访问,写入内容会覆盖远内容, 如果要追加, 可以使用`MODE_APPEND`, 为默认操作模式

2. `MODE_APPEND`: 该模式下会检测文件是否存在, 存在就往文件里最佳, 否则就穿件新的文件

3. `MODE_WOELD_READABLE`、`MODE_WORLD_WRITEABLE`用来控制其他应用是否有权限读写该文件

   * `MODE_WORLD_READABLE`: 表示当前文件可以被其他应用读取;

   * `MODE_WORLD_WRITEABLE`: 表示当前文件可以被其他应用写入;

   * 如果希望文件被其他应用读写,可以传入:

     ```java
     openFileOutput("文件名", Context.MODE_WORLD_READABLE + Context.MODE_WORLD_WEITEABLE);
     ```

* Android 有一套自己的安全模型, 但应用程序(.apk)再安装时系统会分给他一个userid, 当该应用要去访问其他应用资源文件时, 就需要userid匹配. 默认情况下,任何应用穿件的文件 sharedpreferences,数据库,都应该试试有的(位于/data/data/<package name>/files), 其他程序无法访问, 除非再创建的时指定了`Context.MODE_WORLD_READABLE`或者`Context.MODE_WORLD_WRITEABLE`.

## 4. 四大组件

1. Activity: 用于表现功能。
2. Service: 后台运行服务，不提供界面呈现
3. BroadcastReceiver: 用于接收广播。
4. 内容提供商:支持在多个应用中存储和读取数据，相当于`数据库`