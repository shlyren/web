---
title: Ubuntu更新Nginx服务
date: 2017-03-30 15:25:03
categories: 教程
tags: [教程]
---

Ubuntu 官方自带的 nginx 版本非常低。我安装的默认版本是1.4.6。如果需要更好的性能和功能，那么需要将 nginx 升级到最新版。目前最新版稳定版为 1.10.1，mainline 开发版是 1.11。

<!-- more -->

### 一. 确定Ubuntu版本号

1. 登录服务器

2. 使用`lsb_release -a`查看版本号 返回结果如下

   ```ruby
   No LSB modules are available.
   Distributor ID:	Ubuntu
   Description:	Ubuntu 14.04.4 LTS
   Release:	14.04 #ubuntu 的版本
   Codename:	trusty #代码号
   ```

   * 常用的 14.04 是 trusty ，12.04 是 precise ，14.10 是 utopic ，16.04 是 xenial。

### 二. 下载安装 nginx 官方服务器的验证 key

1. `wget http://nginx.org/keys/nginx_signing.key`
2. `apt-key add nginx_signing.key`

### 三. 添加官方 nginx 地址源

1. 编辑 `/etc/apt/sources.list.d/nginx.list` 文档。

2. 在里面添加或修改:

   ```ruby
   #开发版
   deb http://nginx.org/packages/mainline/ubuntu/ `代码号` nginx
   deb-src http://nginx.org/packages/mainline/ubuntu/ `代码号` nginx

   #稳定版
   deb http://nginx.org/packages/debian/ `代码号` nginx
   deb-src http://nginx.org/packages/debian/ `代码号` ngin
   ```

   ​

   * 其中**代码号**部分换成第一步查到的参数，mainline 根据你的需要选择 `mainline` 或是 `stable`（如果你更看重稳定性的话),比如我的 **Ubuntu 14.04**版安装

     * **mainline开发版**为：

       ```ruby
       deb http://nginx.org/packages/mainline/ubuntu/ trusty nginx
       deb-src http://nginx.org/packages/mainline/ubuntu/ trusty nginx
       ```


     * **Stable 稳定版**为：

       ```ruby
       deb http://nginx.org/packages/ubuntu/ trusty nginx
       deb-src http://nginx.org/packages/ubuntu/ trusty nginx
       ```

### 四. 更新Nginx

1. 安装命令:

   ```Shell
   sudo apt-get update && apt-get install nginx
   ```

2. 升级命令

   ```shell
   sudo apt-get update && apt-get upgrade nginx
   ```

   * 如果出错，则可能你需要先删除旧的 nginx，再进行上一步骤新装处理。
     删除旧 nginx 的命令：

     ```shell
     apt-get remove nginx nginx-common nginx-core
     ```

* 如果出现以下提醒:

  ```ruby
  Configuration file '/etc/nginx/nginx.conf'
   ==> Modified (by you or by a script) since installation.
   ==> Package distributor has shipped an updated version.
     What would you like to do about it ?  Your options are:
      Y or I  : install the package maintainer's version
      N or O  : keep your currently-installed version
        D     : show the differences between the versions
        Z     : start a shell to examine the situation
   The default action is to keep your current version.
  *** nginx.conf (Y/I/N/O/D/Z) [default=N]
  ```

  * 选择N(默认)即可

最后使用命令`nginx -v`查看当前的nginx版本如果是`nginx version: nginx/1.10.1`表示升级成功