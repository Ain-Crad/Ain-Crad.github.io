---
title: new keyword
date: 2018-09-20 13:21:31
categories:
- C++
tags:
- C++
---
使用$new$关键字和不使用$new$关键字创建对象的异同及优劣对比。
<!--more-->
## 参考链接
[When should I use the new keyword in C++](https://stackoverflow.com/questions/655065/when-should-i-use-the-new-keyword-in-c)
顺便安利一波[Stack Overflow](https://stackoverflow.com/) |･ω･｀)
## 使用new关键字
```C++
MyClass* myClass = new Myclass();
myClass->MyField = "Hello World!";
```
特点：
1. 为创建的对象在自由存储区分配内存。
2. 之后需要使用$delete$手动释放分配的内存（否则会造成内存泄露）。
3. 在使用$delete$之前被分配的内存空间持续存在（意味着用$new$关键字创建的对象可以作为指针被函数返回）。

## 不使用new关键字
```C++
MyClass myClass;
myClass.MyField = "Hello World!";
```
特点：
1. 为创建的对象在堆栈段分配内存。
2. 不需要使用$delete$来手动释放。
3. 调用结束时内存空间被清除（意味着用这种方式创建的对象不能作为指针来被函数返回）。

## 拓展
可执行文件由正文段、数据段、和$BSS$段组成。
- 正文段（$Text Segment$）:用于储存指令。
- 数据段（$Data Segment$）:用于储存已初始化的全局变量。
- $BSS$段（$BSS Segment$）:用于储存未赋值的全局变量所需的空间。

调用栈不储存在可执行文件中，而是在运行时创建。调用栈所在的段称为**堆栈段**（$Stack Segment$）。
调用栈：调用栈描述的是函数之间的调用关系。它由多个栈帧（$Stack Frame$）组成，每个栈帧对应着一个未运行完的函数。栈帧中保存了该函数的**返回地址**和**局部变量**，因而不仅能在执行完毕后找到正确的返回地址，还很自然地保证不同函数间的局部变量互不相干－－因为不同函数对应着不同的栈帧。
堆栈段的空间相对于自由空间十分有限（具体大小和操作系统相关，大概几十$M$）。所以建议把较大的数组放在$main$函数外，因为局部变量也是放在堆栈段的。栈溢出（$Segmentation fault$）不一定是递归调用太多，也可能是局部变量太大。只要总大小超过了允许的范围，就会产生栈溢出。
## 总结
如果不想时刻担心要调用$delete$释放内存，那么就不要使用$new$关键字。
如果需要在一个函数中返回对象的指针，那么就一定要使用$new$关键字（局部变量的方式会在函数调用结束时清除分配的空间）。