---
title: sscanf & sprintf
date: 2018-09-15 22:32:35
categories:
- C++
tags:
- 总结
---
　　*关于 sscanf 和 sprintf 用法的总结。*
<!--more-->

# sscanf
``int sscanf（const char * str，const char * format，...）``从字符串中读取格式化的输入。

## Example
```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main () {
   int day, year;
   char weekday[20], month[20], dtm[100];

   strcpy( dtm, "Saturday March 25 1989" );
   sscanf( dtm, "%s %s %d  %d", weekday, month, &day, &year );

   printf("%s %d, %d = %s\n", month, day, year, weekday );
    
   return(0);
}
```
## Output
```
March 25, 1989 = Saturday
```

# sprintf
``int sprintf（char * str，const char * format，...）``将格式化的输出发送到str指向的字符串。

## Example
```C
#include <stdio.h>
#include <math.h>

int main () {
   char str[80];

   sprintf(str, "Value of Pi = %f", M_PI);
   puts(str);
   
   return(0);
}
```
## Output
```
Value of Pi = 3.141593
```