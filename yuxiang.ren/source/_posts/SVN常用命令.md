---
title: SVN常用命令
date: 2017-05-16 08:56:21
categories: 命令
tags: 命令
---



1. 将文件checkout到本地

   `svn checkout <path>`;

   `svn co <path>`

2. 添加新的文件

   `svn add <file>`: 添加名为file的文件

   `svn add *.m`: 添加后缀为.m的所有文

3. 提交到版本库

   `svn commit -m 'message'`

   `svn ci`

4. 更新到某个版本

   `svn update -r m <path>`

   `svn update`:将当前目录以及子目录下的所有文件都更新到最新版本。 

   `svn up`

<!-- more -->

5. 查看文件或者目录状态
   * `svn status`: 【?：不在svn的控制中；M：内容被修改；C：发生冲突；A：预定加入到版本库；K：被锁定】
   * `svn st`: 简写
6. 删除文件
   * `svn delete <file name>`: 删除
   * `svn ci "message"`: 提交
7. 查看历史
   * `svn log`
8. 查看文件详情
   * `svn info`