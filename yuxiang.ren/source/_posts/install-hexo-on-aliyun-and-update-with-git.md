---
title: 在阿里云上安装hexo并且使用git更新
date: 2017-4-24 11:28:17
categories: 教程
tags: 教程

---

* 本文在 ******8 基础上加以修改
* 本文所使用的阿里云服务器系统为: `Ubuntu 16.04.2 LTS`
* 本地测试环境为: `MacOS 10.12.4`
* 本文为本人自己搭建博客环境时所记录的步骤, 

 # 一. 准备工作

1. 云服务器: 这里我使用的阿里云ECS, 需要配置web环境等
2. 本地测试环境: 需要安装hexo, git, 等必备工具
3. 域名(非必须)



<!-- more -->

# 二. 配置服务器端

* 说明: 为方便,**下文中的`yuxiang.ren(域名)`,`106.14.9.43(主机ip)`...请自行替换**

1. 登录服务器
   * `ssh root@ip`

2. 安装所需服务

   * 依次执行下面命令(过程有点漫长, 耐心等待)

     ```shell
      apt update && apt upgrade -y # 14.04 使用apt-get
      apt install git -y
      curl -sL https://deb.nodesource.com/setup | bash
      apt install nodejs -y
      apt install nginx -y
      cd /etc/nginx/sites-available
      rm -rf default
     ```

   * 访问ip`106.14.9.43`出现nginx默认页面表示nginx安装成功,后面还需要对ngin进一步配置

3. 安装git并且初始化git仓库

   * 依次执行一下命令

     ```shell
     cd ~
     mkdir repos && cd repos
     mkdir yuxiang.ren.git && cd yuxiang.ren.git #yuxiang.ren.git 为存放博客的git仓库(.git后缀) 
     git init --bare
     cd hooks
     touch post-receive
     vi post-receive #此命令是编辑`post-receive`文件, 也可在本地编辑完上传到对应目录替换
     ```

4. 在出现的编辑页面中输入以下内容,注意`yuxiang.ren`的替换,然后保存退出。(hexo 部署)

   ```bash
   #!/bin/bash -l
   GIT_REPO=$HOME/repos/yuxiang.ren.git
   TMP_GIT_CLONE=$HOME/tmp/git/renyuxiang.ren
   PUBLIC_WWW=/var/www/yuxiang.ren
   rm -rf ${TMP_GIT_CLONE}
   git clone $GIT_REPO $TMP_GIT_CLONE
   rm -rf ${PUBLIC_WWW}/*
   cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
   cd ~
   exit
   ```

5. 执行下面命令

   * `chmod +x post-receive`

6. 配置nginx.conf文件

   * `vi /etc/nginx/nginx.conf ` (也可在本地编辑完上传到对应目录替换)

   * 在`http`添加一下内容

     ```
     server {
     	 listen 80;
          server_name yuxiang.ren www.yuxiang.ren;
     	 root /var/www/yuxiang.ren/public/; 
     	 index index.html index.htm index.txt;
     }
     ```

   * 保存

   * 重启nginx : `service nginx restart`

     * 如果重启失败 可通过`nginx -t`错误路径以方便处理错误

   * 这时候访问`106.14.9.43` 出现的应该是nginx 404页面



# 三. 配置本地环境

1. 安装 `Homebrew`

   - `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

2. 安装`Node.js`

   - `brew install node`

3. 选择博客存放目录我放在文档目录下

   * `cd ~/Documents`

4. 将刚刚再服务器上创建的仓库clone到文档目录下

   - `git clone root@106.14.9.43:repos/yuxiang.ren.git`
     - `106.14.9.43` 为主机ip地址
     - `repos/yuxiang.ren.git` 为博客的仓库路径
   - 会让你输入服务器登录密码
   - 然后被clone一份空的git仓库

5. 给本地电脑安装`Hexo`

   - `npm install -g hexo-cli`

6. 初始化Hexo

   - 进入刚刚从服务器clone下的`yuxiang.ren`文件夹

   - 依次执行下面命令

     ```shell
     hexo init
     npm install
     hexo d -fg
     hexo s
     ```

   然后访问 http://localhost:4000 如果看到网页表示Hexo配置成功




# 四. 更新

1. git的更新命令如下

   * 进入git本地仓库

   * 执行以下命令

     ```shell
     git add . #把需要更新的所有文件添加到本地仓库
     git commit -m "update message" # 提交到本地仓库
     git push # 提交到远程仓库 这里只服务器
     ```

     ​