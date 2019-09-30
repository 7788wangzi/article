---
layout: main
title: 基于Ubuntu搭建Shadowsocks服务器
categories: [Ubuntu, Tools]
tags: [翻墙, 代理]
---

# 基于Ubuntu搭建Shadowsocks服务器

## 前提
有一台Ubuntu机器，并且可以通过SSH建立远程连接。

## 安装Shadowsocks服务器

1. 安装更新
使用SSH链接到Ubuntu服务器以后，运行以下命令更新软件包:
```
apt-get update && apt-get upgrade -y
```

1. 安装ShadowsocksR

在Shadowsocks版本选择上，我们建议选择ShadowsocksR (SSR)，SSR的本身安装步骤繁琐，但网友teddysun提供了一个方便的一键安装脚本。

以Root身份连接到主机以后，运行以下命令：
```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
```

```
chmod +x shadowsocks-all.sh
```

```
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

运行完最后的命令行后，会看到一个选项：Which Shadowsocks server you’d select，因为我们要安装ShadowsocksR，就选择ShadowsocksR对应的数字。  

然后会看到“Please enter password for ShadowsocksR”，这时需要设定一个以后翻墙时连接Shadowsocks的密码，请自行设定。   

下一个选项”Please enter a port for ShadowsocksR [1-65535]”是让选择一个Shadowsocks的服务器端口，建议使用：443。   

接下来的选项“Please select stream cipher for ShadowsocksR”，是让选择加密方式，建议选择：chacha20-ietf。   

下一个选项“Please select protocol for ShadowsocksR”，是让选择协议，建议选择：origin。   

下一个选项“Please select obfs for ShadowsocksR”，是让选择混淆模式，建议选择：http_simple_compatible。  

然后出现的指示“Press any key to start…or Press Ctrl+C to cancel”，意思是按任何键开始安装或按“Ctrl+C”取消安装。这时按回车键，就可以开始安装了。安装过程可能需要一段时间，请耐心等待。   

安装完成后，屏幕会显示连接Shadowsocks服务器的所有重要参数，包括：Server IP（服务器IP）、Server Port（服务器端口）、Password、Protocol（协议）、obfs（混淆）、Encryption Method（加密）。一定要把所有参数记下来！

这时ShadowsocksR服务器已经成功安装完成了。如果将来想要更改设置，可以使用编辑器更改“nano /etc/shadowsocks-r/config.json”文件：

```
nano /etc/shadowsocks-r/config.json
```

每次更改完设置后，需要运行以下命令行重启ShadowsocksR:
```
/etc/init.d/shadowsocks-r restart
```

## Shadownsocks客户端下载和配置
Shadowsocks客户端是安装在用户的电脑、手机等设备上的软件或APP，用来连接Shadowsocks服务器以达到翻墙的目的。

[windows 电脑端](files/ShadowsocksR-win-4.9.0.zip)

1. Shadowsocks代理模式的设置：

Shadowsocks（包括SSR）主要有两种代理模式： 

 - 全局模式（Global）：访问所有网站都通过Shadowsocks，如果平时大部分时间访问国外网站，选用这种模式即可。
 - PAC模式（PAC）：通过PAC文件里的规则列表控制哪些流量走SS，哪些不走（比如国内流量不走）。注：在Windows版本的SSR客户端，这个功能已经可以被“代理规则”（Proxy Rule）设置代替（除非使用gfwlist），因此在Windows版本的SSR客户端一般不建议用这一模式。
 - 直连模式（Disable System Proxy）：基本可以理解为关掉Shadowsocks。

