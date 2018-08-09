---
title: python脚本-tab转空格
date: 2018-06-12 11:38:20
categories:
- Messy article
tags:
- python
---
有一个一直比较麻烦的问题，平时写代码都是在gedit上写，然后设置了tab键是4个空格的位置，但是代码粘到博客里tab就变成了8个空格的位置，严重破坏了代码的美观！不能容忍！一直没有找到怎么在hexo里面设置，所以之前一直手动把tab换成4个空格QAQ，慢慢的觉得这样很傻，我为什么不写个程序帮我自动改＝＝，首选python！（人生苦短，我用python）。虽然对python不是很熟悉，但是边查边写应该问题不大，最终完成了，从此再也不用傻傻的手动转换了，幸福/(ㄒoㄒ)/~~代码量很小
<!-- more -->
## 简介
就是把文档里的tab全部转换成4个空格，其余不变
效果图：
转换前
![tab](/tab.png)
转换后
![space](/space.png)
## 代码
``` python
#!/usr/bin/python
# -*- coding: UTF-8 -*- 

def T2S():
    str = raw_input("file name:")
    file_path = "/home/aincrad/Desktop/" + str
    f = open(file_path, "r")
    fnew = open("/home/aincrad/Desktop/new", "w")
    data = f.read()
    for c in data:
        if c == '\t':
            fnew.write("    ")
        else:
            fnew.write(c)
    f.close()
    fnew.close()
    print "Done"

if __name__ == "__main__":
    T2S()
```
*说明:默认了需要转换的文件在桌面，运行程序后只需要输入文件名即可，之后会在桌面上生成名为new的转换后的文件。*
