---
title: Learn CMake(2)--多文件/目录
date: 2019-04-03 10:16:23
categories:
- Summary
tags:
- CMake
---
随着工程的不断扩大，我们会将工程分成不同的子模块，每个子模块用一个子文件夹管理。传统的编译方法是在每一个子目录下创建一个Makefile，然后这些Makefile由父目录中的Makefile调用。使用CMake可以有效的简化这种情况。
<!--more-->
## A Project with Directories
我们把HelloWorld封装成一个类，头文件``HelloWorld.h``放在``include``文件夹里，源文件``HelloWorld.cpp``和``main.cpp``放在``src``文件夹里。(文件中具体代码在最后给出)。目录结构如下：
```bash
aincrad@Inspiron:~/Desktop/HelloWorld$ tree
.
├── CMakeLists.txt
├── include
│   └── HelloWorld.h
└── src
    ├── HelloWorld.cpp
    └── main.cpp

2 directories, 4 files
```
这时候CMakeLists.txt中的内容如下：
```cmake
cmake_minimum_required(VERSION 3.5.1)
project(HelloWorld)

#Bring the headers, such as HelloWorld.h into the project
include_directories(include)

#Can manually add the sources using the set command as follows:
#set(SOURCES src/main.cpp src/HelloWorld.cpp)

#However, the file(GLOB ...) allows for wildcard additions:
file(GLOB SOURCES "src/*.cpp")

add_executable(hello ${SOURCES})
```
解释一下CMakeLists.txt中新出现的内容：
- ``include_directories()``命令用来把头文件加入环境中，它的参数是包含头文件的目录。另外还有一个``target_include_directories()``可以在添加头文件目录的同时指定头文件作用的目标(target)。
- ``set()``命令可以用来设置变量，这里我们把``SOURCES``变量设置为``main.cpp``和``HelloWorld.cpp``，表示包含这两个源文件。但是因为需要手动添加所有的源文件，比较繁琐，所以这条命令被注释了，用下一行的命令代替。
- ``file()``命令将所有的源文件添加到工程中。``GLOB``用来创建一个文件列表，读取了``src/*.cpp``中的所有源文件，并将它们赋值给``SOURCES``变量。
- ``add_executable()``和上一个例子中直接显式使用源文件不同的是使用了``SOURCES``变量作为参数，来生成``hello``可执行文件。

下面是用CMake进行编译的命令：
```bash
$ mkdir build
$ cd build
$ cmake ..
$ make
$ ./hello 
```
成功编译、运行后终端打印出结果：
```
Hello World!
```

## 文件中代码
**HelloWorld.h**
```c++
#ifndef _HelloWorld_H
#define _HelloWorld_H

#include <iostream>

class HelloWorld{
public:
    void sayHello(void);
};

#endif
```

**HelloWorld.cpp**
```c++
#include "HelloWorld.h"

void HelloWorld::sayHello(){
    std::cout << "Hello World!" << std::endl;
}
```

**main.cpp**
```c++
#include "../include/HelloWorld.h"

int main(int argc, char* argv[]){
    HelloWorld hi;
    hi.sayHello();
    return 0;
}
```

## 参考链接
[Introduction to CMake by Example](http://derekmolloy.ie/hello-world-introductions-to-cmake/)
[Jermmy's Lazy Blog--cmake学习笔记](http://jermmy.xyz/2017/04/26/2017-4-26-learn-cmake-2/)