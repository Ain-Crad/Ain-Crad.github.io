---
title: next_permutation解析
date: 2018-12-18 10:34:22
categories:
- C++
tags:
- 排列
---
next_permutation是C++的STL中提供的一个库函数，声明在algorithm头文件中，用来求下一个排列。好奇内部是怎么实现的，在Stack Overflow上看到了精彩的回答。

## 链接
参考链接[std::next_permutation Implementation Explanation](https://stackoverflow.com/questions/11483060/stdnext-permutation-implementation-explanation)

## 内部实现
```C++

template<typename It>
bool next_permutation(It begin, It end)
{
    if (begin == end)
        return false;

    It i = begin;
    ++i;
    if (i == end)
        return false;

    i = end;
    --i;

    while (true)
    {
        It j = i;
        --i;

        if (*i < *j)
        {
            It k = end;

             while (!(*i < *--k))
                /* pass */;

            iter_swap(i, k);
            reverse(j, end);
            return true;
        }

        if (i == begin)
        {
            reverse(begin, end);
            return false;
        }
    }
}

```

## 解析
```
1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2
1 4 3 2 
2 1 3 4
...
```
要得到一个排列的下一个排列，**我们可以把排列中的每一个元素看做数字中的某一位，整个排列看做一个具体的数**。这样求下一个排列的问题就变成了求比当前数字的大的最小的那个数（上确界）。例如排列组成的所有可能的数中，比1234大的数中最小的那个就是1243。
在上面的例子里我们看到前几个排列都是1作为排列的首部，当1作为首部的情况被改变的时候是1后面的三个数字降序排列的时候。
所以算法设计的关键是**从后向前找到第一个打破降序的数字，将它和它右边降序排列中它的上确界调换位置，之后再将原本的降序序列部分反转**。之所以反转是因为调换位置后已经是一个新的排列，而反转后的升序序列是原本降序序列的最小状态。

核心代码：
```
while (true)
    {
        It j = i;
        --i;

        //找到最先打破降序的元素
        if (*i < *j)
        {
            It k = end;

            //找到这个元素在它右边降序序列的上确界
             while (!(*i < *--k))
                /* pass */;

            iter_swap(i, k);//交换元素
            reverse(j, end);//降序序列部分反转
            return true;
        }

        //表示整个序列已经是降序序列
        if (i == begin)
        {
            reverse(begin, end);
            return false;
        }
    }
```

