---
title: 利用pods给你的程序添加第三方库
date: 2016-03-21 14:31:45
categories: 教程
tags: CocoaPods
---

## CocoaPods的安装
* 打开Terminal执行:
  `$ sudo gem install cocoapods`
* 执行完这句如果报告以下错误：

        ERROR: Could not find a valid gem 'cocoapods' (>= 0), here is why:
        Unable to download data from https://rubygems.org/ - Errno::ETIMEDOUT: Operation timed out - connect(2) (https://rubygems.org/latest_specs.4.8.gz)
        ERROR: Possible alternatives: cocoapods

<!-- more -->
* 这是因为ruby的软件源rubygems.org因为使用亚马逊的云服务，被我天朝屏蔽了，需要更新一下ruby的源，过程如下：

```
$ gem sources -l (查看当前ruby的源)
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
```
* 如果gem太老，可以尝试用如下命令升级gem
    `$ sudo gem update --system`
* 升级成功后会提示:**RubyGems system software updated**
* 然后重新执行安装下载命令:
  `$ sudo gem install cocoapods`
* 接下来进行安装，执行：:
  `$ pod setup`
* Terminal会停留在 Setting up CocoaPods master repo 这个状态一段时间,是因为要进行下载安装,而且目录比较大,需要耐心等待一下.如果想加快速度,可使用cocoapods的镜像索引.
* 至此,cocoapods已经成功

## 利用cocoapods给你的程序添加第三方库
* 打开Terminal,cd到你程序目录
* 创建**Podfile**文件(一定要创建在跟.xcodeproj同级的目录下):
  `$ touch Podfile`
* 以安装AFN为例:
  `$ pod search AFNetWorking`
* 找到**-> AFNetworking**,如下显示,复制`pod 'AFNetworking', '~> 3.0.4'`
```
-> AFNetworking (3.0.4)
A delightful iOS and OS X networking framework.
pod 'AFNetworking', '~> 3.0.4'
- Homepage: https://github.com/AFNetworking/AFNetworking
- Source:   https://github.com/AFNetworking/AFNetworking.git
```
* 编辑Podfile文件,推荐使用`vi`命令或Xcode编辑。不建议其他编辑器编辑，不然后面更新pods会有警告的。Terminal命令 :
    `$ vim Podfile`
* 将下面代码粘进去,然后保存退出 ,`#`为注释
```
use_frameworks! #用于swift
platform :ios, '8.0' # 支持的ios版本
#下面放需要导入的第三方库的名字
pod 'AFNetworking', '~> 3.0.4'
```
* 安装:
  `pod install --verbose --no-repo-update`
* 安装成功后项目文件夹里会出现`.xcwoekspace`的文件.以后只要写代码就打开xcworkspace这个文件写就好了。 打开之前的那个xocdeproj写的话会出现问题。
* 新建的项目添加的话 会遇到搜索不到头文件。。需要配置点东西
    点击项目->Build->Settings,搜索`header`,找到`User Header Search Patchs`,添加参数**${SRCROOT}**并将后边的属性改为**recursive**。
* 或者通过`#import <>`导入


## 其他命令
* 卸载原有的CocoaPod:
  `sudo gem uninstall cocoapods`
* 重新安装cocoapod:
  `sudo gem install -n /usr/local/bin cocoapods`

## 常见问题
1. 卡在Updating local specs repositories:**pod install**被墙了，请大家换成:
    `pod install --verbose --no-repo-update`
    `pod update --verbose --no-repo-update`
2. 出现这种警告:

        Your Podfile has had smart quotes sanitised. To avoid issues in the future, you should not use TextEdit for editing it. If you are not using TextEdit, you should turn off smart quotes in your editor of choice.
* **解决方法**:不要使用文本编辑去编辑Podfile，使用Xcode编辑，或者使用终端敲命令去编辑。或者输入格式错误，没输入运行版本：`$platform:ios, ‘8.0‘`
3. 使用cocoapods导入第三方类库后头文件没有代码提示**解决方法**:
    选择Target -> Build Settings 菜单，找到`User Header Search Paths`设置项，新增一个值**${SRCROOT}**，并且选择**Recursive**
4. `pod setup` 报**CocoaPods was not able to update the `master` repo** 错误**解决方法**
    * 先删除全局的缓存：
       `$ sudo rm -fr ~/Library/Caches/CocoaPods/`
        `$ sudo rm -fr ~/.cocoapods/repos/master/`
    * 还不行的话就把当前 Pods 目录清空：
       `$ sudo rm -fr Pods/`
    * 再执行
      `$ sudo gem install cocoapods`
    * 重新执行
      `$ pod setup`


