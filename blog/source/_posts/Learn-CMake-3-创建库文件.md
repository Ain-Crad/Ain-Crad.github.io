---
title: Learn CMake(3)--创建库文件
date: 2019-04-03 15:45:26
categories:
- Summary
tags:
- CMake
---
介绍如何用CMake编译生成动态链接库和静态链接库。
<!--more-->
## 静态库和动态库的区别
[静态链接库(STATIC)和动态链接库(SHARED)](https://ain-crad.github.io/2019/04/03/%E9%9D%99%E6%80%81%E9%93%BE%E6%8E%A5%E5%BA%93-STATIC-%E5%92%8C%E5%8A%A8%E6%80%81%E9%93%BE%E6%8E%A5%E5%BA%93-SHARED/)

## 建立动态链接库(.so/.dll)
我们把上一个示例中的HelloWorld类编译成动态链接库。
文件目录如下：
```bash
aincrad@Inspiron:~/Desktop/HelloWorld$ tree
.
├── CMakeLists.txt
├── include
│   └── HelloWorld.h
└── src
    └── HelloWorld.cpp

2 directories, 3 files
```
由于目标是建立库，而不是生成可执行文件，所以我们不需要``main.cpp``。

下面是CMakeLists.txt中的内容：
```cmake
cmake_minimum_required(VERSION 3.5.1)
project(HelloWorld_lib)
set(CMAKE_BUILD_TYPE Release)

#Bring the headers, such as HelloWorld.h into the project
include_directories(include)

#However, the file(GLOB ...) allows for wildcard additions:
file(GLOB SOURCES "src/*.cpp")

#Generate the shared library from the sources
add_library(HelloWorld SHARED ${SOURCES})

#Set the location for library installation -- i.e., /usr/lib in this case
# not really necessary in this example. Use "sudo make install " to apply
install(TARGETS HelloWorld DESTINATION /usr/lib)
```
解释CMakeLists.txt中新出现的内容：
- ``set(CMAKE_BUILD_TYPE Release)``表示此次编译为正式发布版本。另外还有``Debug``等模式。这个参数决定了在编译时进不进行优化或者包不包含debug信息。详细参见[Build types in CMake](https://codeyarns.com/2015/05/14/build-types-in-cmake/)
- ``add_library()``和之前的``add_executable()``类似。不过是用来创建库文件的，``SHARED``参数表明这是一个动态库（其它选项：STATIC or MODULE）。
- ``install()``指定了库的安装位置。使用``sudo make install``来激发，用来把链接库安装到系统中。

下面是具体的编译命令：
```cmake
$ mkdir build
$ cd build
$ cmake ..
$ make
$ sudo make install
```
成功编译后会在build文件夹里生成库文件libHelloWorld.so，使用``sudo make install``安装成功后的信息：
```bash
aincrad@Inspiron:~/Desktop/HelloWorld/build$ sudo make install
[100%] Built target HelloWorld
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/lib/libHelloWorld.so
```

## 建立静态链接库(.a/.lib)
编译静态库和编译动态库的方式几乎完全一样，唯一的区别就是把``add_library()``中的``SHARED``参数改成``STATIC``。

## 参考链接
[Introduction to CMake by Example](http://derekmolloy.ie/hello-world-introductions-to-cmake/)
[Jermmy's Lazy Blog--cmake学习笔记](http://jermmy.xyz/2017/04/26/2017-4-26-learn-cmake-3/)