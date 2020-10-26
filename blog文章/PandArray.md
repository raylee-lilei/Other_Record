---
title: PandArray
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-03-31 11:46:55
tags:
keywords: 
description:
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/startdash/cat.jpg
---

```bash
char *str1 = "lilei";       //(1)
char str2[] = "lilei";      //(2)
```
这里是定义，不是声明，因为两个字符串都分配了空间
（1）是一个指针变量   保存在**字符串常量区**不能被修改，
（2）是一个数组

```bash
#include "stdio.h"
#include "stdlib.h"
#include "string.h"

int main(void)
{
char *str1 = "lilei";
char *str3 = "lilei";
char str2[] = "lilei";

memcpy(str1,"hello",strlen("hello")+1);   //向一个非法的地址拷贝东西，错误
str1[1] = 'a';   //错误 操作非法的地址
str2[1] = 'a'  //允许

printf("str1:%s str3:%s\n str2:%s\n",str1,str3,str2);

str3 = "world";   //指针变量保存的地址是可以变化的 str3的值会变成world
printf("str1:%s str3:%s str2:%s\n",str1,str3,str2);
printf("hello,world\n");
return (0);
}
```
```bash
#include <stdio.h>
#include "stdlib.h"
#include "string.h"

const int a = 1;
const int a1 = 1;
char * s = "hello";

int main()
{
const int b = 2;
const int b1 = 2;
char * s1 = "hello";

printf("s:%p s1:%p\n",s,s1);
printf("a:%p a1:%p b:%p b1:%p\n",&a,&a1,&b,&b1);

return 1;
}

```

输出

```bash
s:0000000000404008 s1:0000000000404008
a:0000000000404000 a1:0000000000404004 b:000000000062FE14 b1:000000000062FE10

--------------------------------
```
s,s1,a,a1在一个内存区域。这个内存区域的内容是不允许改变的。

对于以上代码
```bash

const int b = 2;

int main()
{
const int b1 = 2;
int *p = <font color=red>&b1</font>;;
printf("b1:%d\n",b1);
*p = 3;
printf("b1:%d\n",b1);

//输出  b1=2   b1 = 3


const int b1 = 2;
int *p = <font color=red>&b</font>;
printf("b:%d\n",b);
*p = 3;   //错误
printf("b:%d\n",b);

//输出  b1=2 

return 1;
}
```