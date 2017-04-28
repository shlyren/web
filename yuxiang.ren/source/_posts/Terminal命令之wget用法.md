---
title: Terminal命令之wget用法
date: 2017-04-27 16:09:45
categories: 命令
tags: 命令
---



 wget : 一款下载命令

1. 支持断点下传功能
2. 同时支持FTP和HTTP下载方式
3. 支持代理服务器
4. 设置方便简单
5. 程序小，完全免费
6. 安装: `brew install wget`

wget语法: wget<参数> 文件url

<!-- more -->

# 下载http/ftp站点

1. `wget url`: 下载url页面
2. `wget -r url`: 递归下载url页面
3. `wget -p url`: 下载url页面下的所有资源

# 下载文件

1. `wget -c url`: 断点续传下载文件
2. 批量下载:
   * 如果有多个文件需要下载，那么可以生成一个文件，把每个文件的URL写一行，例如生成文件**download.txt**,
   * `wget -i download.txt`: 下载文件用所有的url
3. 选择下载
   * `wget -m -regect=gif url`
   * 上面命令表示:  下载url下所有文件, 胡虐.git类型文件






