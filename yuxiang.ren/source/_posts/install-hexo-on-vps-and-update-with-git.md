---
title: 在vps上安装hexo并且使用git更新
date: 2016-12-02 11:28:17
categories: 教程
tags: [教程 Hexo]

---


## 一、准备工作
#### VPS&域名
请自行购买服务器以及域名.

## 二、搭建过程
>搭建分为两个部分:一部分在本机(Mac)进行，另一部分则在服务端(VPS)进行，大致需要完成的工作是在本机和服务器各安装一次Hexo和Git，并在服务器上安装Nginx服务器、配置Git Hooks以实现更新.并且全程使用终端命令完成。

<!-- more -->

#### 本地电脑配置
1. 安装Homebrew
* `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
2. 安装Node.js
* `brew install node`
3. 创建网站目录
* 在任意位置创建一个文件夹，作为网站目录，并通过 cd 命令进入文件夹。
4. 本地电脑安装hexo
* `npm install -g hexo-cli`
* `hexo init`
* `npm install`
* `hexo d -fg`
* `hexo serve` *简洁写法:`hexo s`*

然后访问 http://localhost:4000 如果看到网页表示Hexo配置成功

#### 远程服务器配置(VPS)
**下文中的`yuxiang.ren(这是我的域名)`,`45.32.249.14(这是我的主机ip)`...请自行替换**

1. 此处为Debian/Ubuntu在root用户下的操作:
*  执行`ssh root@你的vps地址`进入主机
* 依次执行以下命令 (安装hexo服务)
    ​          
    ```bash
     $apt-get update && apt-get upgrade -y
     $apt-get install git-core -y
     $curl -sL https://deb.nodesource.com/setup | bash
     $apt-get install nodejs -y
     $apt-get install nginx -y
     $cd /etc/nginx/sites-available
     $rm -rf default
     $touch yuxiang.ren #根据自己情况替换
     $vi yuxiang.ren #根据自己情况替换
    ```
2. 上面的最后一条命令输入完成后会进入编辑页面,将下面代码复制到里面(通过按i进入编辑模式)。(host监听)
```ruby
    server {
        listen 45.32.249.14:80; #监听80端口
        server_name yuxiang.ren; # 域名
        access_log  /var/log/nginx/yuxiang_access.log;
        error_log   /var/log/nginx/yuxiang_error.log;
        location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ {
                root /var/www/yuxiang.ren/public;
                access_log   off;
                expires      1d;
            }
        location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
            root /var/www/yuxiang.ren/public;
            access_log   off;
            expires      10m;
        }
        location / {
            root /var/www/yuxiang.ren/public;
            if (-f $request_filename) {
                rewrite ^/(.*)$  /$1 break;
            }
        }
    }
```
 *也可再本地编辑好后上传到服务器*
3. 确认无误后按下esc键,之后再按下:wq!退出. 然后执行下面命令, 注意`yuxiang.ren`的替换。(配置git仓库)
```bash
    ln -s /etc/nginx/sites-available/yuxiang.ren /etc/nginx/sites-enabled/`
    cd ~
    mkdir repos && cd repos
    mkdir yuxiang.ren.git && cd yuxiang.ren.git
    git init --bare
    cd hooks
    touch post-receive
    vi post-receive`
```
4. 在出现的编辑页面中输入以下内容,注意`yuxiang.ren`的替换,然后保存退出。(hexo 部署)
``` bash
    #!/bin/bash -l
    GIT_REPO=$HOME/repos/yuxiang.ren.git
    TMP_GIT_CLONE=$HOME/tmp/git/renyuxiang.ren
    PUBLIC_WWW=/var/www/yuxiang.ren
    rm -rf ${TMP_GIT_CLONE}
    git clone $GIT_REPO $TMP_GIT_CLONE
    rm -rf ${PUBLIC_WWW}/*
    cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
    cd ~
    #cd ${PUBLIC_WWW}
    #hexo d -fg
    cd ~
    exit
```
5. 依次执行下面命令
* `chmod +x post-receive`
* `cd ~`
* `service nginx restart`

#### 回到本地电脑
1. 首先进入网站根目录。
2. 执行以下命令, 注意替换
* `git clone root@45.32.249.14:repos/yuxiang.ren.git`; 把之前再服务器创建的仓库clone到本地
* `git add .`
* `git commit -m "commit message"`
* `push`

## 三、Blog更新
#### 1. 创建`.m`文件
有两种方法
1. 直接创建
2. 通过命令(推荐)
* 进入网站目录
* `hexo new 文章名字`
* 用MarkDown编辑器编辑创建的文件(`/source/_posts/`下)

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
### 发布
进入网站根目录
```ruby
hexo d -fg
git add .
git commit -m "操作内容"
git push
```