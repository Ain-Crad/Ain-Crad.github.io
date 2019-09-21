---
title: Learn CMake(1)--HelloWorld
date: 2019-04-02 16:38:14
categories:
- Summary
tags:
- CMake
---
之前的博客[C语言编译器工作原理以及.h和.c文件解析](https://ain-crad.github.io/2018/12/02/C%E8%AF%AD%E8%A8%80%E7%BC%96%E8%AF%91%E8%BF%9E%E6%8E%A5%E8%BF%87%E7%A8%8B%E4%BB%A5%E5%8F%8A-h%E5%92%8C-c%E6%96%87%E4%BB%B6%E8%A7%A3%E6%9E%90/)中比较详细的介绍了编译器的编译过程，以及它是如何处理多文件编译问题的。但是在工程项目比较庞大以及需要链接外部库的时候，手动编写Makefile往往比较繁琐，一个比较好的解决方式是使用CMake工具自动生成工程项目的Makefile。
<!--more-->
# CMake介绍
CMake是一个开源的跨平台自动化构建系统。它可以产生不同系统平台的构建文件(e.g., 类Unix系统的Makefile，Window系统的.vcproj)。可以方便的用于工程项目的管理。

*大部分内容来自[Introduction to CMake by Example](http://derekmolloy.ie/hello-world-introductions-to-cmake/)*

# CMake 安装(Ubuntu)
```bash
$ sudo apt-get install cmake    //install CMake
$ cmake -version    //check version of CMake
```

# The Hello World Example
首先创建一个HelloWorld.cpp文件，包含以下代码:
```c++
#include <iostream>

int main(int argc, char *argv[]){
    std::cout << "Hello World!" << std::endl;
    return 0;
}
```
在相同目录下创建一个CMakeLists.txt文件(存放CMake命令的标准文件)，包含一下内容:
```cmake
cmake_minimum_required(VERSION 3.5.1)
project(HelloWorld)
add_executable(hello HelloWorld.cpp)
```
CMakeLists.txt中包含三行：
- 第一行的``cmake_minimum_required()``命令指定了该工程的最小CMake版本，设置为自己系统中的CMake版本即可。
- 第二行的``project()``命令设置了工程的名字。
- 第三行的``add_executable()``命令，第一个参数为生成的可执行文件的名字，第二个参数指定需要编译的源文件。两个参数之间用空格隔开。

接下来在CMakeLists.txt同一目录下执行以下命令：
```bash
$ cmake .
```
"."表示当前目录。

执行完毕后，在同一目录下会生成一些文件，最重要的是生成了Makefile文件，接下来用make命令来编译链接以生成可执行文件：
```bash
$ make
```
之后会在同一目录下生成``hello``可执行文件，运行：
```bash
$ ./hello
```
会在终端中打印出以下信息：
```
Hello World!
```

# 补充
执行``cmake .``命令后我们发现多出的一些文件会使得整个工程看起来比较混乱，而我们只需要用到其中的Makefile文件。所以比较推荐的做法是，在同一目录下新建一个``build``文件夹用来存放cmake命令之后产生的文件，而当我们不需要这些文件的时候只需要将build文件夹下的文件删除。下面是具体的命令：
```bash
$ mkdir build
$ cd build
$ cmake ..
$ make
```
清除cmake文件
```bash
$ sudo rm -rf build/*
```

# 总结
虽然只编译一个HelloWorld.cpp我们完全没有必要使用CMake，但我们通过这个例子解释了CMake的一些基本操作，可以用于后面更复杂的例子。

# 参考链接
[Introduction to CMake by Example](http://derekmolloy.ie/hello-world-introductions-to-cmake/)
[Jermmy's Lazy Blog--cmake学习笔记](http://jermmy.xyz/2017/04/25/2017-4-25-learn-cmake-1/)