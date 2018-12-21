---
title: ubuntu下vultr服务器搭建
date: 2018-04-13 19:09:32
categories:
- Messy article
tags:
- vultr服务器搭建
---
工欲善其事必先利其器。
前一段时间在[陶司机学长](http://logqtainia.github.io/)的帮助下搭了自己的梯子，因为实在是受不了某度的搜索引擎了...，某天电脑出了一个小问题，去某度搜，２个小时过去没有任何头绪，无奈挂了VPN软件搜谷歌，搜索结果的第一条就是解决办法，，，没有对比就没有伤害，google真是一家伟大的公司。
觉得靠免费的VPN软件不是长久之计（一方面是速度不理想，另一方面是考虑安全的问题），于是搭了梯子，终于可以愉快的用google了(•̀⌄•́)，用这篇博客记录一下搭建的过程。
<!-- more -->
## 注册vultr账号
[链接](https://www.vultr.com/)
## 创建vultr主机
点击右上角的蓝色加号，创建新的vultr主机
![p1](/p1.png)
服务器位置选择，推荐选择新加坡，或者美国的服务器(日本服务器已基本被墙)
![p2](/p2.png)
服务器端系统选择，选择ubuntu
![p3](/p3.png)
服务器配置选择,配置越高价格也越高，一般最便宜的2.5刀/month就够用了，但是很难抢到，，，于是就选5刀/month的了。
![p4](/p4.png)
其它的选项不需要填写，点击支付就ok(ps:从2017年10月开始支持支付宝支付！！)
![p5](/p5.png)
接下来等待主机部署完成即可。
## 服务器端配置
服务器端，也就是你刚刚创建的那台主机。
点击主机右侧"..."展开菜单，选择"Server Details"可以看到这台主机的一些信息。包括这台主机的IP地址(IP Address)、用户名(Username)、密码(Password)等，这些信息下面需要用到。
![p6](/p6.png)
打开终端，使用ssh远程登录到主机
```
$ ssh root@你的主机IP地址
```
之后输入你的主机密码进行登录(可以在主机信息页面直接复制，粘贴到终端里时不会显示，但已存在,直接回车即可)。
登录成功后使用一键配置脚本进行配置
```
$ wget -N --no-check-certificate https://softs.fun/Bash/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```
终端中会出现如下界面，输入１选择安装shadowsocksR
![p7](/p7.png)
之后会依次要求设置端口号(推荐设置8000以上)、密码、加密方式、协议、混淆等，下面给出我的几个主要配置选择
```
加密方式(method):none
协议(protocol):auth_chain_a
混淆(obfs):plain
```
服务器端配置完成，终端中输入exit退出ssh登录
```
$ exit
```
## 客户端配置
客户端，也就是你的个人电脑。
在终端中依次输入如下命令安装配置客户端的ssr(shadowsocksR)，注意此时一定要确保已经在终端中退出ssh登录，或者新打开一个终端。
```
$ wget http://www.djangoz.com/ssr
$ sudo mv ssr /usr/local/bin
$ sudo chmod 766 /usr/local/bin/ssr
$ ssr install
$ ssr config
```
配置和服务器端保持一致即可，下面给出示例，只需配置server、server_port、password、method、protocol、obfs这几项
```
    "server": "11.22.33.44",
    "server_ipv6": "::",
    "server_port": 1234,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "123456789",
    "method": "none",
    "protocol": "auth_chain_a",
    "protocol_param": "",
    "obfs": "plain",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, // only works under multi-user mode
    "additional_ports_only" : false, // only works under multi-user mode
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",

```
以后若想修改配置，在终端中输入ssr config即可(服务器端也要对应修改)，还有打开和关闭命令
```
ssr start
ssr stop
```
## 可能的问题
检查自己是否可以登录google、youtube、twitter等网站，若依旧不能可尝试用以下方法解决。
* 依次打开ubuntu系统设置->网络->网络代理，设置如下
![p8](/p8.png)
* 推荐使用chrome浏览器+proxy SwitchyOmega插件(可离线安装)
proxy SwitchyOmega配置如下
![p９](/p9.png)
* 有可能申请的服务器的ip被墙，可通过在线网站验证是否可以ping通，若确认ip被墙，在你的vultr账户中申请更换即可(更换服务器位置，或只更换ip地址)

ps:服务器端配置好后，windows、android端只需下载shadowsocksR软件并进行简单配置即可使用