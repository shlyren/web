---
title: install VPN on your vps
date: 2017-01-16 15:25:07
categories: 教程
tags: [教程 翻墙]

---

### 一、安装环境

1. ubuntu 14.04

### 二、检查ppp/tun环境

```shell
cat /dev/net/tun
```

* 必须返回是:`cat: /dev/net/tun: File descriptor in bad state`

```shell
cat /dev/ppp
```

* 必须返回是`cat: /dev/ppp: No such device or address`

如果不是, 请联系你的vps提供商

<!-- more -->

### 三、安装脚本

1. 下载脚本:

   ```shell
   wget --no-check-certificate https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh
   ```

2. 安装

   ```shell
   chmod +x one-key-ikev2.sh
   bash one-key-ikev2.sh
   ```

   这里需要注意的是让你选择vpn类型: **OpenVZ还是Xen、KVM**, 自己可询问vps提供商, 或者通过其他途径, 其他选项 一路回车就可以.

   如图情况表示配置完成, 可根据自身情况修改用户名、密码、密钥等

   ![installed](https://ww2.sinaimg.cn/large/006y8lVagw1fbsi9xr8x7j30w6092adm.jpg)

3. 修改/添加用户

   ```shell
   vi /usr/local/etc/ipsec.secrets
   ```

   内容如下:

   ```ruby
   : RSA server.pem 
   : PSK "myPSKkey" #表示贡献密钥
   : XAUTH "myXAUTHPass"
   myUserName1 %any : EAP "myUserPass1" #用户名以及密码
   myUserName2 %any : EAP "myUserPass2" #用户名以及密码
   ```

### 四、开机启动(ubuntu)

1. ubuntu系统

   ```shell
   chmod +x /etc/rc.local && echo "/usr/local/sbin/ipsec start" >> /etc/rc.local
   ```


2. 打开sysctl文件:

   ```shell
   vi /etc/sysctl.conf
   ```

   - 找到 `net.ipv4.ip_forward=1`，删除前面的“#”注释符（按` I键 `进入编辑模式），并保存退出（按` Esc键 `退出编辑模式，然后输入` :wq `保存并退出）。

3. 使用以下指令刷新sysctl：

   ```shell
   sysctl -p
   ```

### 五、卸载

```shell
cd /root/strongswan-5.3.5
make uninstall
```

### 六、其他命令

```shel
ipsec start # 启动pvn
ipsec status #vpn状态
ipsec restart #重启vpn
ipsec stop #停止vpn
```



