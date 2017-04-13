---
title: 重写UIScrollView分类dealloc错误
date: 2016-12-06 09:25:01
categories: error
tags: [Xcode, error]
---

> [UIScrollView _systemGestureStateChanged:]: message sent to deallocated instance> 错误解决方法
> [UITableViewWrapperView _systemGestureStateChanged] 解决亦是如此

今天一个朋友自己的程序出现了一个bug, 不知道怎么解决, 他把报错内容给我, 报错内容如下:

<img src="https://ww4.sinaimg.cn/large/65e4f1e6gw1fagsd983rsj21g60ggjyt.jpg" width = "600"/>
就是普通的僵尸对象错误, 可是解决就不是那么简单了...

我翻边了各大国内过外网站, 终于再一个不显眼的地方看到了大致是因为在`UIScrollView`分类重写了`dealloc`方法导致.

<!-- more -->

于是我去项目里看一下`UIScrollView`的分类. 果真, 这个第三方重写了`dealoc`, 把它注释掉果然没有出现僵尸对象了.

可是问题又来了,这个分类是在`dealloc`里移除监听的
<img src="https://ww4.sinaimg.cn/large/65e4f1e6gw1fagsjlodobj20qw04ywfe.jpg" width = "500"/>

我想到了`MJRefresh`使用的也是同样的原理, 于是我去看了他的代码,他是再这里移除监听的
<img src="https://ww4.sinaimg.cn/large/65e4f1e6gw1fagsl6oopij210a07yacr.jpg" width = "500"/>

那我们就学着他
<img src="https://ww4.sinaimg.cn/large/65e4f1e6gw1fagslu9vecj20zk0ba41t.jpg" width = "500"/>

重新运行程序, 问题解决.

然而, 又有新的问题出现, 就是通知移除的问题, 为了彻底解决自己写了个.
代码地址:https://github.com/shlyren/ZYScaleHeader

