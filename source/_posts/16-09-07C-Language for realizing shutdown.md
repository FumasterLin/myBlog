---
title: C语言实现定时关机小程序
date: 2016-09-07 22:38:39
tags:
- C语言
---
------

### 程序功能说明
> - 输入1可实现自定义10分钟内的定时关机
> - 输入2可实现立即关机
> - 输入3可实现注销计算机
> - 输入0可退出程序
<!--more-->

### 程序运行截图
![](http://od191c801.bkt.clouddn.com/C%E5%85%B3%E6%9C%BA%E7%A8%8B%E5%BA%8F.png)

### 代码
```c
/**********关于system("color 参数")配置从控制台颜色************
0 = 黑色    8 = 灰色    1 = 淡蓝      9 = 蓝色
2 = 淡绿    A = 绿色    3 = 湖蓝      B = 淡浅绿  
C = 红色    4 = 淡红    5 = 紫色      D = 淡紫  
6 = 黄色    E = 淡黄    7 = 白色      F = 亮白
****************************************************************/
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

int main()
{
    char cmd[20]="shutdown -s -t ";
    char t[5]="0";
    int c;

    system("title C语言关机程序");  //设置cmd窗口标题
    system("mode con cols=48 lines=25");  //窗口宽度高度 
    system("color f0");  ////f为背景色，0为前景色
    system("date /T");//显示系统日期
    system("TIME /T");//显示系统时间

    printf("----------- C语言关机程序 -----------\n");
    printf("1.实现10分钟内的定时关闭计算机\n");
    printf("2.立即关闭计算机\n");
    printf("3.注销计算机\n");
    printf("0.退出系统\n");
    printf("-------------------------------------\n");

    scanf("%d",&c);
    switch(c) {
        case 1:
            printf("您想在多少秒后自动关闭计算机？（0~600）\n");
            scanf("%s",t);
            system(strcat(cmd,t));
            break;
        case 2:
            system("shutdown -p");
            break;
        case 3:
            system("shutdown -l");
            break;
        case 0:
            break;
        default:
            printf("Error!\n");
    }
    system("pause");
    return 0;
}
```
### 小总结
> - 要点就是system()函数的运用和cmd命令的了解
> - 函数原型int system(const char * string);
>- strcat()函数原型char *strcat(char *dest, const char *src);
>> strcat() 会将参数 src 字符串复制到参数 dest 所指的字符串尾部；dest 最后的结束字符 NULL 会被覆盖掉，并在连接后的字符串的尾部再增加一个 NULL。

### 联系 ###
> - 博客：[fumasterlin.com](www.fumasterlin.com)
> - 邮箱：[fumasterlin@163.com](fumasterlin@163.com)

-------------------------2016.09.07--------------------------------
