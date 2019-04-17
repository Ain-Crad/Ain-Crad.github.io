---
title: Learn CMake(4)--链接库文件
date: 2019-04-03 17:38:42
categories:
- Summary
tags:
- CMake
---
很多时候我们都要在程序中链接外部库文件，不论是我们自己创建的，还是第三方的（例如OpenCV）。这里介绍如何使用CMake来方便的在项目中链接库文件。
<!--more-->
## 链接自己的创建的库文件
沿用上一个示例中创建的库文件，文件目录：
```bash
aincrad@Inspiron:~/Desktop/HelloWorld$ tree
.
├── CMakeLists.txt
├── include
│   └── HelloWorld.h
├── lib
│   ├── libHelloWorld.a
│   └── libHelloWorld.so
└── src
    └── main.cpp

3 directories, 5 files
```
将库文件放在了lib文件夹中，因为有了库文件所以我们不再需要HelloWorld.cpp。

CMakeLists.txt中的内容：
```cmake
cmake_minimum_required(VERSION 3.5.1)
project(HelloWorld_lib)

#For the shared library:
set(PROJECT_LINK_LIBS libHelloWorld.so)
link_directories(lib)

#For the static library:
#set(PROJECT_LINK_LIBS libHelloWorld.a)
#link_directories(lib)

include_directories(include)

add_executable(hello src/main.cpp)
target_link_libraries(hello ${PROJECT_LINK_LIBS})
```
源文件main.cpp中的内容：
```c++
#include "../include/HelloWorld.h"

int main(int argc, char* argv[]){
    HelloWorld hi;
    hi.sayHello();
    return 0;
}
```

## 链接第三方库文件
CMake提供了一个``find_package()``命令来搜寻这些依赖库。比如，如果我们需要链接OpenCV库文件，可以加入以下几条命令：
```cmake
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
```
然后在``add_executable()``之后链接库文件：
```cmake
target_link_libraries(hello ${OpenCV_LIBS})
```
``<NAME>_FOUND``，``<NAME>_INCLUDE_DIRS``和``<NAME>_LIBS``是相应库的.cmake文件中定义好的。

``find_package()``并不能保证一定可以找到相应的库，如果找不到，就需要我们手动添加路径了，类Unix系统一般都在``/usr/local/include``和``/usr/local/lib``路径下。在CMakeLists.txt文件中添加以下路径。
```cmake
include_directories(/usr/local/include)
link_directories(/usr/local/lib)
```
或者如果我们知道库源文件的具体位置(以OpenCV为例)，但其没有安装到系统目录中，添加以下命令：
```cmake
set(OpenCV_DIR /home/aincrad/opencv/_build)
```
其中第二个参数是对应库在自己计算机上的路径。

## OpenCV示例
文件目录：
```bash
aincrad@Inspiron:~/Desktop/opencv$ tree
.
├── CMakeLists.txt
├── lena_color.png
└── main.cpp

0 directories, 3 files
```
CMakeLists.txt中的内容：
```cmake
cmake_minimum_required(VERSION 3.5.1)
project(HelloWorld_lib)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(img main.cpp)
target_link_libraries(img ${OpenCV_LIBS})
```
main.cpp中的内容：
```c++
#include <stdio.h>
#include <opencv2/opencv.hpp>

using namespace cv;

int main(int argc, char** argv ){
    if ( argc != 2 )
    {
        printf("usage: DisplayImage.out <Image_Path>\n");
        return -1;
    }
    Mat image;
    image = imread(argv[1], 1);
    if ( !image.data )
    {
        printf("No image data \n");
        return -1;
    }
    namedWindow("Display Image", WINDOW_AUTOSIZE );
    imshow("Display Image", image);
    waitKey(0);
    return 0;
}
```
终端中执行以下命令后：
```bash
$ mkdir build
$ cd build
$ cmake ..
$ make
$ ./img ../lena_color.png
```
显示出lena的图片：
![lena](/lena_color.png)

## 参考链接
[Introduction to CMake by Example](http://derekmolloy.ie/hello-world-introductions-to-cmake/)
[Jermmy's Lazy Blog--cmake学习笔记](http://jermmy.xyz/2017/04/26/2017-4-26-learn-cmake-3/)
[Using OpenCV with gcc and CMake](https://docs.opencv.org/3.3.0/db/df5/tutorial_linux_gcc_cmake.html)
[CMake如何查找链接库---find_package的使用方法](https://blog.csdn.net/u011092188/article/details/61425924)
[CMake之find_package](https://www.jianshu.com/p/46e9b8a6cb6a)