---
title: 开源的NES游戏模拟器
date: 2018-12-03 00:28:38
categories:
- Messy article
tags:
- Game
---
偶然发现一个博主分享的开源NES游戏模拟器，安装后只要再去下载rom游戏就可以重温小时候的小霸王游戏机上的游戏！安装之后迫不及待的下载了超级马里奥，还是当年的感觉！激动(≧▽≦)
![SuperMario](/SuperMario.png)
<!--more-->
# 参考链接
[WTF Daily Blog](http://blog.topspeedsnail.com/archives/4249)
感谢博主！下面只列出了开源的NES游戏模拟器，更多类型的模拟器请转到博主博客原文。

[NES游戏下载地址](http://nesyouxi.net/)

# NES模拟器

## FCEUX
[FCEUX](http://www.fceux.com/web/home.html)是开源的NES模拟器，使用C++编写，大概也是Linux上最好用的NES模拟器。它同样支持Windows、Mac os x。如果你使用的是Ubuntu，执行``sudo apt-get install fceux``安装fceux。如果你使用其他Linux发型版，可以从[这里](http://www.fceux.com/web/download.html)下载源代码编译安装 。

Ps.你可能会遇到无法解析安装包的情况，可以``sudo apt-get update``之后重新安装。如果依旧安装失败，可以尝试更新软件源。

## Nestopia
[Nestopia](http://nestopia.sourceforge.net/)另一个开源的NES模拟器，貌似很久没有更新了，它支持Windows、Linux、Mac os x系统。Nestopia也是使用c++编写，并且使用了大量的模版（难读）。它的执行效率还是挺高的。去年，我（指WTF Daily Blog的博主）把它移植到了andorid上，在低端手机上可以流畅运行。

## My Nes
[My Nes](https://sourceforge.net/projects/mynes/)是使用C#编写的开源NES模拟器，它只支持Windows系统。My Nes的代码非常易读，也许是因为这个原因，导致执行效率不高。