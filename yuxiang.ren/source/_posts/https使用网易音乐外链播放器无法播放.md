---
title: https使用网易音乐外链播放器无法播放
date: 2016-06-03 01:05:58
categories: 教程
Tags: 教程
---


### 问题描述
`https`协议的网站嵌入网易云音乐外链播放器，无法播放音乐。

<!--more-->

### 分析
无法播放是因为网易云音乐外链播放器的`html`代码你的音乐资源链接默认是`http`协议的，而访问使用`https`协议的网站，浏览器会自动屏蔽掉`http`的资源。

### 解决方法
把网易云音乐外链播放器的`html`代码里的`http`改为`https`就可以解决

### 示例
* 这是网易云音乐默认生成的连接

```html
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=2&id=16232697&auto=1&height=66"></iframe>
```

* 只要将`http`给为`https`即可

```html
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="https://music.163.com/outchain/player?type=2&id=16232697&auto=1&height=66"></iframe>
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="https://music.163.com/outchain/player?type=2&id=16232697&auto=1&height=66"></iframe>
​    



