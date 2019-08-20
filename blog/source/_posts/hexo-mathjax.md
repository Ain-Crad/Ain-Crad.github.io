---
title: hexo中mathjax使用坑点
date: 2018-04-09 15:29:50
categories:
- Messy article
tags:
- mathjax
- hexo插入数学公式
- 总结
---
刚刚开始自己的博客之旅，开始真是无比艰难啊，各种被坑。过程中博客需要插入数学公式，下载了mathjax插件(安装之后就可以使用Latex定义的宏写数学公式了)，结果被坑惨(也不算是mathjax的锅，markdown自己也要背)，卡了2个小时，气愤(╯‵□′)╯︵┻━┻
这里记录一下在mathjax里踩到的坑。
mathjax的安装配置和使用大家可以参照[Steven's space的博文](http://stevenshi.me/2017/06/26/hexo-insert-formula/)，不再赘述了。
<!-- more -->
# 坑点
markdown本身的特殊符号和Latex中的符号会出现冲突。
- 下划线\_在markdown中是斜体，但是Latex中下划线是下标的意思，所以这里会冲突。
- 双斜线\\\\在markdown中会被转义为\，所以Latex的\\\\换行功能也会不正常。
- 乘法符号\*在markdown中是粗体＝＝(就是在这被卡了)，所以Latex在写两个数相乘时不能直接写
```
$a*b$
```
以上情况都会导致无法在mathjax中渲染出来。

# 解决方法
手动转义：
比如需要在公式中写乘法*可以写成
```
$a\*b$
```
下标写成
```
$a\_1$
```
换行写成
```
$\\\\$
```
解决，散花。

更多解决方法可以参考[hexo下mathjax的转义问题](http://shomy.top/2016/10/22/hexo-markdown-mathjax/)
