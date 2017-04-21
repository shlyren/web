---
title: linux常用命令
date: 2017-03-31 14:32:42
categories: 命令
tags: 命令
---

### 一. 系统清理
1. clean命令删除所有的软件安装包

   * 在网络连接正常的情况下，我们执行软件安装命令，软件安装结束后，以 .deb 为后缀的软件包就不再需要了。所以要对他定期清理

     ```shell
     sudo apt-get clean
     ```

   * 使用上面命令即可上错所有的软件包

2. autoclean 删除不再可用的安装包

   - 与`clean`一样, 只不过它有选择地删除软件包：那些不再可用的安装包(比如，你再也不能够从软件仓库中下载到该软件包的当前版本、或更新版本)。

     ```shell
     sudo apt-get autoclean
     ```

   <!-- more -->

3. remove 删除特定软件

   ```shell
   sudo apt-get remove
   ```

4. purge清除软件残余

   * purge 命令是 remove 命令的另一个版本，它可以在删除软件本身的同时，删除软件包的残余设置。

     ```shell
     sudo apt-get purge 软件名
     ```

5. autoremove 删除不需要的依赖包

   ```shell
   sudo apt-get autoremove
   ```

### 二. 磁盘相关命令

1. `df -lh`

   * 通过这个命令可以查看磁盘的使用情况以及文件系统被挂载的位置

     ![](https://ww4.sinaimg.cn/large/006tNc79gy1fe60rpiwfkj30li0860uk.jpg)

2. `fdisk -l ` 查看分区信息

   * 下图表示一块16G的硬盘

     ![](https://ww2.sinaimg.cn/large/006tNc79gy1fe60v7gzrnj30qu08u0v8.jpg)

3. 查看某目录下占用空间最多的文件或目录

   * 取前10个。需要先进入该目录下

     ```shell
     du -cks * | sort -rn | head -n 10
     ```

4. 查看某一目录大小:

   ```shell
   du -sh
   ```


## 三.  GIT相关

1. 创建一个tag

   ```shell
   git tag <标签名> -m "commit message"
   ```

2. 提交tag

   ```Shell
   git push ---tags #将本地所有标签一次性提交到git服务器
   git push <标签名> #将指定标签提交到git服务器
   ```

3. 删除一个tag

   ```shell
   git tag -d <标签名>
   ```

4. 删除远程标签

   ```shell
   git push origin :<标签名>
   ```

5. 查看所有标签

   ```shell
   git tag
   ```

6. `git warning: LF will be replaced by CRLF in `解决办法

   ```shell
   git config core.autocrlf false
   ```

