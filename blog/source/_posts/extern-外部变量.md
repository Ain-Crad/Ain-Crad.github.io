---
title: extern-外部变量
date: 2018-12-03 06:56:26
categories:
- Summary
tags:
- C语言
---
前两天被同学问起extern的用法，才发现自己对extern的用法也不清晰，囧，整理一下。

## 参考链接
[when to use extern in C++](https://stackoverflow.com/questions/10422034/when-to-use-extern-in-c)

## extern
extern用于多文件编程时，不同源文件之间共享变量。我们在头文件中声明外部变量，这样所有包含该头文件的的源文件就都有了这个外部变量的**声明**，而我们只需要在**其中一个**源文件中**定义**一次。

``extern int x``告诉编译器有一个叫``x``的``int``型变量在某个地方，正如之前[C语言编译连接过程]()里说的那样，此时编译器并不关心``x``的具体实现，即不关心``x``定义在哪个源文件中。编译器只需要知道这个变量的类型和名字，这样就知道了该如何使用它。编译完成生成之后，链接器会将所有**引用**``x``的地方定向到**定义**它的某一个源文件中。
为了使上述过程顺利进行，``x``必须要在某个源文件中被定义为**全局变量**，并且不能包含``static``关键字。

## 例子
print.h
```C
#ifndef _PRINT_H
#define _PRINT_H

extern int global_x;    //任何包含这个头文件的源文件都将可以使用global_x变量
void print_global_x();

#endif

```

print.c
```C
#include <stdio.h>
#include "print.h"

int global_x;   //global_x的具体定义的位置
printf("%d\n", global_x);
```

main.c
```C
#include "print.h"  //头文件中包含global_x的声明

int main(){
    global_x = 5;
    print_global_x();

    return 0;
}
```