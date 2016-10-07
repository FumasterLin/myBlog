---
title: 线性顺序表的实现（C语言版）
date: 2016-09-09 22:38:39
tags:
- 数据结构
---
------

# **顺序表描述**
去年学习了一些数据结构，其中最简单的就是顺序线性表。目前刚搭建好了这个平台，于是想来记录一番。
> 顺序表是在计算机内存中以**数组**的形式保存的**线性表**，是指用一组地址连续的存储单元依次存储数据元素的线性结构。线性表采用顺序存储的方式存储就称之为顺序表。顺序表是将表中的结点依次存放在计算机内存中一组地址连续的存储单元中。
<!--more-->

# **顺序表实现**

## 顺序表的初始化
```c
typedef int elemType;
typedef int posType;
//初始化一个结构体
struct List
{
	elemType *list;//元素类型指针，类似数组
	int size;//数据个数
	int maxSize;//顺序表大小
};
/*参数ms：初始化顺序表的大小*/
void initList(struct List *L , int ms)//初始化顺序表
{
	if(ms <= 0)//参数合理性判断
	{
		printf("MaxSize 非法！ ");
		exit(1);
	}
	L->maxSize = ms;
	L->size = 0;
	L->list = malloc(ms*sizeof(elemType));//分配内存
	if(!L->list )//分配内存意外情况处理
	{
		printf("空间分配失败！");
		exit(1);

	}
	return;
}
```
## 开辟内存
> realloc()可以对**给定的指针**所指的空间进行扩大或者缩小，无论是扩张或是缩小，`原有内存的中内容将保持不变`.当然，对于缩小，则被缩小的那一部分的内容会丢失.realloc并不保证调整后的内存空间和原来的内存空间保持同一内存地址.相反，`realloc返回的指针很可能指向一个新的地址`.
```c
void againMalloc(struct List*L)
{
    //开辟两倍于当前顺序表大小的内存
	elemType *p = realloc(L->list ,2*L->maxSize *sizeof(elemType));
	if(!p)
	{
		printf("储存空间分配失败！");
		exit(1);
	}
	L->list = p;
	L->maxSize = 2*L->maxSize ;

}
```
## 插入数据操作
### 从表头插入数据

```C
void insertFirstList(struct List *L, elemType x)
{
	int i;
	//判断如果目前表中数据个数达到顺序表的最大size，则再次分配内存
	if(L->size == L->maxSize )
	{
		againMalloc(L);
	}
	//把表中每个数据往后移动
	for(i = L->size-1; i>=0; i--)
	{
		L->list[i+1] = L->list[i];
	}
	L->list[0] = x;//表头插入数据
	L->size ++;//数据个数加1
	return;
}
```
### 从表尾插入数据
```c
void insertLastList(struct List *L, elemType x)
{
    //判断如果目前表中数据个数达到顺序表的最大size，则再次分配内存
	if(L->size == L->maxSize )
	{
		againMalloc(L);
	}
	L->list [L->size ] = x;//表尾插入数据
	L->size ++;//数据个数加1
	return;	
}
```
### 从表中某个位置插入数据

```c
/*
pos:要插入数据的位置；
x  :要插入的数据
插入成功返回1，失败返回0；
*/
int insertPosList(struct List *L,int pos, elemType x)
{
	int i;
	if(pos < 1 || pos > L->size )//输入位置合理性判断
	{
		return 0;
	}
	//判断如果目前表中数据个数达到顺序表的最大size，则再次分配内存
	if(L->size == L->maxSize )
	{
		againMalloc(L);
	}
	//把要插入数据的位置上数据以及之后的数据，都往后移动一个位置
	for(i = L->size-1; i>=pos-1; i--)
	{
		L->list [i+1] = L->list [i];
	}
	L->list [pos-1] = x;//当前位置插入数据
	L->size ++;//数据个数加1
	return 1;//插入成功则返回1
}
```
## 删除数据操作
### 从表头删除数据
从表头删除数据的操作即：把第一个数据之后的数据都往前移动一个位置，由此把表头的数据给顶出去
```c
/*删除成功则返回被删除的数据*/
elemType deleteFirstList(struct List *L)
{
	elemType temp;
	int i;
	if(L->size == 0)//合理性判断
	{
		printf("线性表为空，不能进行删除操作！");
		exit(1);
	}
	temp = L->list [0];
	//把第一个数据之后的数据都往前移动一个位置
	for(i=1 ; i < L->size ; i++)
	{
		L->list [i-1] = L->list [i];
	}
	L->size --;//数据个数-1
	return temp;
}
```
### 从表尾删除数据
```c
/*删除成功则返回被删除的数据*/
elemType deleteLastList(struct List *L)
{
	elemType temp;
	if(L->size == 0)//合理性判断
	{
		printf("线性表为空，不能进行删除操作！");
		exit(1);
	}
	temp = L->list[L->size ];//删除表尾数据
	L->size --;//数据个数-1
	return temp;
}
```
### 删除表中某个位置的数据
```c
/*
pos：要删除数据的位置
说明：此位置已经转换成我们日常所习惯的位置，从1开始，而不是0开始
*/
elemType deletePosList(struct List *L, int pos)
{
	elemType temp;
	int i;
	if(pos < 1 || pos > L->size )//合理性判断
	{
		printf("pos越界，不能进行删除操作！");
		exit(1);
	}
	//把pos之后的所有数据都往前移动一个位置，由此把pos上的数据踢掉
	for(i = pos-1; i < L->size; i++)
	{
		L->list[i] = L->list [i+1];
	}
	temp = L->list [pos-1];
	L->size --;//数据个数-1
	return temp;//返回删除的数据	
}
```
### 删除表中具体值的数据
```c
/*
x：要删除的数据
删除成功返回1
*/
int deleteValueList(struct List *L, elemType x)
{
	int i,j;
	//遍历顺序表，找出与x相等的数据所在的位置i
	for(i = 0 ; i < L->size; i++ )
	{
		if(L->list [i] == x)
			break;
	}
	//合理性判断
	if(i == L->size )
	{
		return 0;
	}
	//把i之后的每个数据往前移动一个位置，由此把i上的数据踢掉
	for(j = i+1; j < L->size ; j++)
	{
		L->list [j-1] = L->list [j];
	}
	L->size --;//数据个数-1
	return 1;//删除成功返回1
} 
```
## 单个数据操作
### 替某个位置的换数据
```c
/*
把第pos个的数据替换成x
替换成功返回1，否则返回0
说明：此位置已经转换成我们日常所习惯的位置，从1开始，而不是0开始
*/
int updatePosList(struct List *L, int pos, elemType x)
{
    //合理性判断处理
	if(pos < 1 || pos > L->size )
	{
		return 0;
	}
	L->list[pos-1] = x;//替换第pos个数据
	return 1;
}
```

### 获取数据位置
```c
/*
返回数据x所在的位置，失败返回0
说明：此位置已经转换成我们日常所习惯的位置，从1开始，而不是0开始
*/
int findList(struct List *L, elemType x)
{
	int i;
	for(i = 0; i < L->size ; i++)
	{
		if(L->list[i] == x)
		{
			return i+1;
		}
	}
	return 0;
}
```
### 获取相应位置的数据
```c
/*
返回第pos个数据，失败退出
说明：此位置已经转换成我们日常所习惯的位置，从1开始，而不是0开始
*/
elemType getElem(struct List *L, int pos)
{
	elemType temp;
	//合理性判断处理
	if(pos<1 || pos > L->size )
	{
		printf("元素序号越界！");
		exit(1);
	}
	temp = L->list [pos-1];
	return temp;
}
```

## 整个顺序表操作
### 遍历顺序表（打印表中所有数据）
```c
void traverseList(struct List *L)
{
	int i;
	printf("当前顺序表中的数据为：");
	//循环遍历顺序表并打印
	for(i = 0; i<L->size; i++)
	{
		printf("%d  ", L->list[i]);

	}
	printf(" \n");
	return;

}
```
### 打印表中数据个数
```c
int sizeList(struct List *L)
{
	printf("表中数据个数为: %d\n " , L->size );
	return L->size ;
}
```
### 清除表中所有数据
```c
void clearList(struct List *L)
{
	if(L->list != NULL)
	{
		free(L->list );//回收分配的内存
		L->list =0;//置0
		L->size = L->maxSize = 0;//置0
	}
	return;
}
```

### 联系 ###
> - 博客：[fumasterlin.com](www.fumasterlin.com)
> - 邮箱：[fumasterlin@163.com](fumasterlin@163.com)


