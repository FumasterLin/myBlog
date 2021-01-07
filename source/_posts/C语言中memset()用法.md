---
title: C语言中memset()用法
date: 2016-09-12 20:38:39
tags:
- C语言
---
------

### memset()功能

> **功 能:** 将s所指向的某一块内存中的每个字节的内容全部设置为ch指定的ASCII值,

> 块的大小由第三个参数指定,这个函数通常为新申请的内存做初始化工作
<!--more-->

### 用法示例

1. **原型:** void *memset(void *s, char ch, unsigned n);

2. **程序例:**

```c
#include <string.h>
#include <stdio.h>
#include <memory.h>
int main(void)
{
    char buffer[] = "Hello world/n";
　　printf("Buffer before memset: %s\n", buffer);
　　memset(buffer, '*', strlen(buffer) );
　　printf("Buffer after memset: %s\n", buffer);
　　return 0;
}
```
> **输出结果：**
>>  Buffer before memset: Hello world
>>  Buffer after memset: \*\*\*\*\*\*\*\*\*\*\*


### memset函数详细说明

**（1）void *memset(void *s,int c,size_t n)**

> **总的作用：**将已开辟内存空间 s 的首 n 个字节的值设为值 c。

**（2）例子：**
```c
void main()
{
　　char *s="Golden Global View";
　　clrscr();
　　memset(s,'G',6);//貌似这里有点问题//
　　printf("%s",s);
　　getchar();
　　return 0;
}　
```
> 【这个问题相当大，程序根本就运行不下去了，你这里的S指向的是一段`只读`的内存，而你memset又试图修改它，所以运行时要出错，修改办法char *s修改为char s[ ]】

**（3）memset() 函数常用于`内存空间初始化`。如：**

> char str[100];
> memset(str,0,100);

**（4）memset()的深刻内涵：**用来对一段内存空间全部设置为某个字符，一般用在对定义的字符串进行初始化为：memset(a, '\0', sizeof(a));

--------------------------
### 联系 
> - 博客：[fumasterlin.com](www.fumasterlin.com)
> - 邮箱：[fumasterlin@163.com](fumasterlin@163.com)


