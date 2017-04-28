---
title: 在阿里云上使用hexo并且用git更新
date: 2017-4-24 11:28:17
categories: 教程
tags: [教程,命令]

---

本文基于https://hexo.io 博客框架. 基于Node.js

* 本文在<a href="/2016/12/02/install-hexo-on-vps-and-update-with-git/">原文</a>基础上加以修改完善
* 本文所使用的阿里云服务器系统为: `Ubuntu 16.04.2 LTS`
* 本地测试环境为: `MacOS 10.12.4`
* 本文为本人自己搭建博客环境时所记录的步骤, 他人按照此步骤 可能会出现我没有遇到的问题, 请自行百度/google
* 本文非图文并茂, 全程都是命令行, 也没有什么图可以贴的
* 为了防止命令输入错误, 可以直接`command+c`
* 大致为: 在本地电脑上使用hexo撰写文章, 部署并测试成功后, 通过git提交到服务器上.完成博客的更新
  1. 首先在服务器上部署web环境,以及git等
  2. 然后在本地电脑上安装hexo, git等所需环境
  3. 撰写文章并且更新
  4. 其他完善: 主题, 插件,SEO等...
* 初来乍到, 勿喷......

# 一. 准备工作

1. 云服务器: 这里我使用的阿里云服务器ECS, 需要配置web环境,git等
2. 本地测试环境: 需要安装hexo, git等
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
     mkdir yuxiang.ren.git && cd yuxiang.ren.git #yuxiang.ren.git 为存放博客的git仓库(.git后缀) 名称并没有什么要求, 我只是为了方便才这样写的
     git init --bare
     cd hooks
     touch post-receive
     vi post-receive #此命令是编辑`post-receive`文件, 也可在本地编辑完上传到对应目录替换
     ```

4. 在出现的编辑页面中输入以下内容,注意`yuxiang.ren`的替换,然后保存退出。

   * 这是一个自动更新博客源文件的脚本, 将仓库里的源文件拷贝到对应博客的源文件目录

   ```bash
   #!/bin/bash -l
   GIT_REPO=$HOME/repos/yuxiang.ren.git
   TMP_GIT_CLONE=$HOME/tmp/git/renyuxiang.ren
   PUBLIC_WWW=/var/www/yuxiang.ren #网站的根目录 如果`wwww/` 下没有该文件夹 需要手动创建
   rm -rf ${TMP_GIT_CLONE}
   git clone $GIT_REPO $TMP_GIT_CLONE
   rm -rf ${PUBLIC_WWW}/*
   cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
   cd ~
   exit
   ```

5. 执行下面命令

   * `chmod +x post-receive`

6. 配置`nginx.conf`文件(配置nginx服务器)

   * `vi /etc/nginx/nginx.conf ` (也可在本地编辑完上传到对应目录替换)

   * 在`http` 里添加一下内容

     ```bash
     server {
     	 listen 80; # 监听80端口(http), 如需配置https 可自行百度
          server_name yuxiang.ren www.yuxiang.ren; # URL名字, 需要去域名服务商解析, 如何解析 自己百度
     	 root /var/www/yuxiang.ren/public/; # 这个是网站的根目录 要与上面`PUBLIC_WWW`一致
     	 index index.html index.htm index.txt; #这个是让nginx默认读取的文件名
     }
     ```

   * 保存退出

   * 重启nginx : `service nginx restart`

     * 如果重启失败 可通过`nginx -t`错误路径以方便处理错误

   * 这时候访问`106.14.9.43` 出现的应该是nginx 404页面


# 三. 配置本地环境

* 可能需要安装Xcode命令行工具, 我也不清楚, 因为我电脑本就安装了Xcode, 所以可以直接使用

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
   - 然后clone一份空的git仓库

5. 给本地电脑安装`Hexo`

   - `npm install -g hexo-cli`

6. 初始化Hexo

   - 进入刚刚从服务器clone下的`yuxiang.ren`文件夹

   - 依次执行下面命令

     ```shell
     hexo init #初始化 一个博客环境, 需要下载相关文件,耐心等待
     hexo d -fg #部署
     hexo s #开启本地预览服务
     ```

   然后访问 http://localhost:4000  (本地的4000端口)如果看到网页表示Hexo配置成功


# 四. 提交到服务器

1. git的更新命令如下

   * 进入博客目录

   * 执行以下命令更新

     ```shell
     git add . #把需要更新的所有文件添加到本地仓库
     git commit -m "update message" # 提交到本地仓库
     git push # 提交到远程仓库 这里只服务器
     ```

   * 如果提示需要输入密码 输入服务器密码即可

2. 如果提交的时候提示` cp: target '* * *' is not a directory`意思是这不是一个目录, 所以要去创建一个目录

   * 上文`PUBLIC_WWW`表示网站的根目录, 
   * 直接执行`mkdir /var/www/yuxiang.ren` 创建网站根目录
   * 再次提交
     * 提示`Everything up-to-date`
     * 那就随便改一下重新部署并提交呗


# 五. 博客的配置, 主体更换,

 插件: https://hexo.io/plugins/

主题: https://hexo.io/themes/

…...

自行 百度/Google

# 六. 发布文章 

1. 进入网站根目录

2. `hexo new 文章名字`

3. 编辑`/source/_posts/文章名字.md` 文件使用的是MarkDown语法

   ```markdown
   	title: title  #文章的标题
   	date: yyyy-mm-dd #创建时间
       categories: category #分类
       tags: tag #标签
          
       #多标签请这样写：tags: [tag1,tag2,tag3]
       #或者这样写： 
        #tags: 
          #- tag1
          #- tag2 
          #- tag3 
       ---  
         
       #正文
   ```

4. 正文写完后可以再本地此时下

   * `hexo d -fg`
   * `hexo s`
   * 然后访问 http://localhost:4000 

5. 提交到服务器

   ```shell
   git add .
   git commit -m "操作内容"
   git push
   ```


# 七. 域名绑定

自行 百度/Google

# 八. 最后

当然你也可以放在Github上, 请自行 百度/Google