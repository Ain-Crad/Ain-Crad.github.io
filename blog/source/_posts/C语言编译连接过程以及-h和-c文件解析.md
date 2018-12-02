---
title: C语言编译器工作原理以及.h和.c文件解析
date: 2018-12-02 12:41:00
categories:
- 总结
tags:
- C语言
---
在进行多文件编程的时候，虽然很多IDE都可以帮我们完成预处理、编译、链接等过程，但深入理解编译器的这几个过程可以避免很多"undefined"，"undeclared"以及"redefined"的问题。而比较好的实践方式是自己写Makefile（IDE实际上完成的就是帮我们弄清文件依赖关系，然后生成一个Makefile，调用编译器去编译链接生成可执行文件）。

## 参考链接
[C语言中.h和.c文件解析（很精彩）](https://www.cnblogs.com/laojie4321/archive/2012/03/30/2425015.html)
[Purpose of #ifndef and #define](https://community.arduboy.com/t/purpose-of-ifndef-and-define/3592/2)

## 编译器工作原理
1.预处理阶段：
实际上是处理的是"define"，"include"等宏命令，进行宏替换。例如#include "xx.h"实际意思是把当前这一行删掉，把xx.h中的内容原封不动的插入在当前行位置。

2.编译阶段：
**以.c文件为基本单位进行**，也就是说如果项目中一个.c文件都没有，那么项目无法编译。这一阶段为所有.c文件中的变量、函数分配空间，并将各个全局变量、函数进行符号描述，编译、汇编成二进制码从而生成.o目标文件。
**需要注意的是这个过程并不关心变量的具体定义和函数的具体实现，只要存在变量和函数的相关声明就可以使得编译通过。**

3.链接阶段:
**以.o文件为基本单位进行**，主要的工作是重定位各个目标文件的函数、变量。这个过程主要是为上一个编译过程中变量和函数的声明找到他们具体的定义和实现。

*下面是一个具体的解释例子：*

add.h文件
```C
#ifndef _ADD_H
#define _ADD_H

int add(int a, int b);

#endif
```

add.c文件
```C
#include "add.h"

int add(int a, int b){
    return a + b;
}
```

main.c文件
```C
#include <stdio.h>
#include "add.h"

int main(){
    int a = 1, b = 2;
    int ans = add(a, b);
    printf("%d\n", ans);

    return 0;
}
```

1.预处理阶段：编译器会把main.c和add.c中的``#include "add.h"``替换成add.h中的全部内容。
2.编译阶段：为main.c和add.c中的各变量和函数分配内存空间并汇编成目标文件。可以发现在main.c中并没有关于add函数的实现只有由``#include "add.h"``替换成的这个函数的声明，但这并不影响编译过程的进行。
3.链接阶段：经过编译后生成了main.o和add.o文件。因为main.o中用到了add函数但没有它的具体实现，所以编译器会去其它.o文件中寻找该函数的具体实现。在这个例子中.o提供了add函数的具体实现，链接过程顺利进行，生成可执行文件。

Ps.关于add.h中#ifndef、#define和#endif的解释：它们的作用是防止头文件在一个.c文件中被重复包含。

*一个具体的例子：*

a.h文件
```C
struct Node{
    int a;
    int b;
};
```

b.h文件
```C
#include "a.h"
```

c.h文件
```C
#include "a.h"
```

main.c文件
```C
#include <stdio.h>
#include "b.h"
#include "c.h"

int main(){
    print("hello\n");
    return 0;
}
```
a.h中有结构体struct Node的定义，b.h和c.h都包含了a.h，main.c中又包含了b.h和c.h这两个头文件。这会导致a.h中的内容被两次复制到main.c中，产生struct Node的重复重复定义错误，而如果a.h遵循下面的格式：
```C
#ifndef _A_H
#define _A_H

struct Node{
    int a;
    int b;
};

#endif
```
那么a.h在main.c中只会被替换一次，这样就避免了头文件的重复包含。

## .h和.c文件解析
**一般情况下我们在.h文件中进行变量、函数和宏的声明，在.c文件中进行变量和函数的具体实现。**
但这只是我们默认的一种规范的做法，实际上我们舍弃.h文件，在.c文件中包含变量、函数的声明以及实现也没有问题，但如果在其它.c文件中用#include去包含这个.c文件就会出现问题。

*具体例子：*

add.c文件
```C
int add(int a, int b);

int add(int a, int b){
    return a + b;
}
```

main.c文件
```C
#include "add.c"

int main(){
    int a = 1, b = 2;
    int ans = add(a, b);
    printf("%d\n", ans);

    return 0;
}
```
链接过程会出现add函数重复定义的错误，因为编译后生成的main.o和add.o中都含有add函数的具体实现。这种情况下可以对main.c做如下修改:
```C
int add(int a, int b);

int main(){
    int a = 1, b = 2;
    int ans = add(a, b);
    printf("%d\n", ans);

    return 0;
}
```
这样编译后生成的main.o文件中便不再包含add函数的实现，而只包含声明。但是如果add.c中的函数很多，以及调用这些函数的.c文件也很多，这样在每个.c文件中手动挨个添加这些函数的声明会十分不方便，**而可以理解为.h文件就是为了声明函数和变量方便而创建的**，因为这样把所有的变量、函数声明都写在.h中，其它.c文件只需要#include相应的.h文件就好。

## 问答专区
```
Q：ifndef后面的标识必须是_ADD_H这种头文件名大写形式加下划线的形式吗？
A：不必，你可以自定义，这也是一种习惯性写法，因为一个工程中头文件名是不会重复的，所以这样定义的话标识也不会重复。

Q：.h文件必须和相应的.c文件同名吗？
A：不必，但是通常我们都这样做，因为比较这样比较清晰。

Q：头文件必须是.h后缀吗？
A：不必，因为头文件只是其中的内容被#include宏命令替换，所以.txt、.doc等作为头文件的后缀都可以，但我们习惯用.h作为头文件后缀。

Q：.c文件必须包含自己的.h文件吗？
A：不一定，在最初解释编译器工作原理的例子中add.c就可以不包含add.h文件，因为add.h中只是对add函数的声明，而add.c不需要这个声明也可以编译通过。
```