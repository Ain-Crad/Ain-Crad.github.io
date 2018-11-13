---
title: stringstream-类型转换&&字符分割
date: 2018-08-08 22:07:28
categories:
- C++
tags:
- 字符串
---
继续填坑٩(•̤̀ᵕ•̤́๑)，这次是stringstream的内容，最初是看到它可以把字符串直接输出成int类型，惊了，但是一直不是很懂。在网上查了很多资料，才终于差不多理解。stringstream是C++提供的一个字符串流，和cin，cout输入输出流类似，只不过它不是从从键盘或文件输入，也不是输出到屏幕或文件中，而是接受一个对象的输入，或把缓存区的内容输出到一个对象中。具体可以用在类型转换和字符分割中。
<!-- more -->
## 基本方法(操作)
```C++
clear()－用来清空流标志(输入，输出标志)
str()－获得当前流中的内容
str("content")－设置流的内容（或清空流的内容）
operator <<－添加内容到stringstream对象
operator >>－从stringstream流缓存区中输出内容
```
## 应用
### 类型转换
```C++
int a;
string s;
stringstream ss;
//int型转换为字符串类型
ss << a;　//将a输入流中（输入模式）
ss >> s;　//将流中的数值输出到字符串s中（输出模式）

//字符串类型转换为int型
ss << s;　//将字符串输入到流中
ss >> a;　//将流中的字符串输出到int型变量a中

//注意：如果做多次数据转换须调用clear()和str("")来清空输入输出标志和内容
int a = 0, b = 0;
stringstream ss;
ss << "123";
ss >> a;
ss.clear();
ss.str("");
ss << "456";
ss >> b;
cout << "a:" << a << endl;
cout << "b:" << b << endl;
cout << "ss:" << ss.str() << endl;

//运行结果,不加clear()和str("");
a:123
b:0
ss:123
//原因：不加clear()和str("")，流处于输出模式无法接收新内容"456"且未被清空，所以ss仍为"123"，又由于缓存区的内容已输出到a中，所以b中无内容。

//运行结果，加clear()但不加str("");
a:123
b:456
ss:123456
//原因：加入clear()清空模式，可以接收新内容"456"，所以ss:123456。缓存区依次缓存"123"，"456"并分别输出到a, b，所以a:123, b:456

//运行结果，加clear()和str("");
a:123
b:456
ss:456
//原因：clear()清空模式，str("")清空ss对象的内容。
```
### 支持char*的输入和输出
```C++
char ch[20];
stringstream ss;
ss << 2333;
ss >> ch; //直接将数输出到ch字符数组中
```
### 字符分割
```C++
stringstream ss("hello hi hey");
string word;
while(ss >> word){
    cout << word << endl;
}

//运行结果
hello
hi
hey

```
## 三种给stringstream对象赋初值的方式
```C++
stringstream ss("123"); //创建的时候赋初值
ss.str("123"); //str()函数赋值
ss << "123"; //操作符"<<"添加内容（需注意当前模式）
```