---
title: 栈的实现及其小应用-括号匹配（C++版）
date: 2016-09-10 22:38:39
tags:
- 数据结构
---
------

## 1. 栈的概念和特点

> - 定义：栈是限定仅在表头进行插入和删除操作的线性表

> - 栈作为一种数据结构，是一种特殊的线性表，它的插入和删除运算均在`同一端`进行。
- 这一端被称为`栈顶(top)`，另一端为`栈底`，插入称为`进栈(push)`，删除称为`出栈(pop)`。
- 有`后进先出`的性质。
- 栈顶top相当于顺序表中的size，即元素个数。
- 关于顺序表，可参照我的上一篇文章：[线性顺序表的实现（C语言版）](http://fumasterlin.com/2016/09/09/%E7%BA%BF%E6%80%A7%E9%A1%BA%E5%BA%8F%E8%A1%A8%E7%9A%84%E5%AE%9E%E7%8E%B0%EF%BC%88C%E8%AF%AD%E8%A8%80%E7%89%88%EF%BC%89/#more)
<!--more-->

## 2. 栈的操作及实现

> - 1、类的定义
> - 2、初始化
> - 3、判断是否为空
> - 4、取得栈顶值
> - 5、入栈操作
> - 6、出栈操作
> - 7、打印栈的内容

### 头文件的定义

```C++
#ifndef SEQSTACK_H_
#define SEQSTACK_H_
#include <stdlib.h>
#define LENGTH 100
typedef char DATA_TYPE;
//栈类的定义
class Stack
{
public:
	DATA_TYPE data[LENGTH];
	int top=0;
};
/*
实现栈操作函数的定义，从Stack公有继承过来
ps：也可以直接把Stack里的成员变量直接放到这个类里，直接封装起来
*/
class seqStackFun:public Stack
{
public:
	seqStackFun();//构造函数，初始化在这里做
	~seqStackFun();
	bool isEmpty();//判断栈是否为空
	DATA_TYPE getTop();//取得栈顶值
	bool push( DATA_TYPE pushData);//入栈操作
	DATA_TYPE pop();//出栈操作
	void displayseqStack();//打印栈的内容
};
#endif // !SEQSTACK_H_
```
### 具体函数的实现
#### 1. 初始化即构造函数实现
```C++
seqStackFun::seqStackFun()
{
	this->top = 0;//top置0
}
//析构函数为空
seqStackFun::~seqStackFun()
{
	
}
```
#### 2. 判断栈是否为空
```C++
bool seqStackFun::isEmpty()
{
	return (this->top>0 ? 0 : 1);
}
```
#### 3. 获取栈顶数据
```C++
DATA_TYPE seqStackFun::getTop()
{
	if (0 == this->top)
	{
		cout << "栈为空，没有数据" << endl;
	}
	else 
		return (this->data[this->top - 1]);
}
```
#### 4. 入栈
```C++
bool seqStackFun::push(DATA_TYPE pushData)
{
	if (this->top == LENGTH)
	{
		cout << "栈已经满了" << endl;
		return false;
	}
	else
	{
		this->data[this->top] = pushData;
		this->top++;
		return true;
	}
}
```
#### 5. 出栈
```C++
DATA_TYPE seqStackFun::pop()
{
	if (0 == this->top)
	{
		cout << "栈中无数据，为空" << endl;
	}
	else
	{
		this->top--;
		return this->data[this->top];
	}
		
}
```
#### 6. 打印栈中的内容
```C++
void seqStackFun::displayseqStack()
{
	if (0 == this->top)
	{
		cout << "栈中无数据，为空！" << endl;
	}
	else
	{
		for (int loop = 0; loop < this->top; loop++)
		{
			cout << this->data[loop] << " ";
		}
		cout << endl;
	}
}
```
## 3. 栈的小应用-括号的匹配
### 匹配函数
```C++
/*
判断栈顶数据和x是否相同，是返回真，否返回假
*/
bool compare(seqStackFun stack, DATA_TYPE x)
{
	if (x == stack.getTop())
	{
		return true;
	}
	else
		return false;
}
```
### 主函数实现

```C++
bool compare(seqStackFun stack, DATA_TYPE x);
int main()
{
	DATA_TYPE data;
	seqStackFun *myStack = new seqStackFun();
	cout << "请输入一串带括号的的字符串：";
	while ((data = getchar() )!= '\n')//回车结束
	{
		switch (data)
		{
		case '{':
		case '(':
		case '[':
			myStack->push(data);		
			break;
		case '}':
			if (compare(*myStack, '{'))
				myStack->pop();
			break;
		case ']':
			if(compare(*myStack, '['))
				myStack->pop();
			break;
		case ')':
			if (compare(*myStack, '('))
				myStack->pop();
			break;
		default:
			break;
		}
	}
	if (myStack->isEmpty())
		cout << "括号匹配正确" << endl;
	else
		cout << "括号匹配不正确" << endl;
	delete myStack;
	system("pause");
	return 0;
}
```
运行结果如下：

![](http://od191c801.bkt.clouddn.com/%E6%A0%88%E6%96%87%E7%AB%A02.png)

![](http://od191c801.bkt.clouddn.com/%E6%A0%88%E6%96%87%E7%AB%A01.png)

### 参考
> 参考自：[http://blog.csdn.net/fansongy/article/details/6784919](http://blog.csdn.net/fansongy/article/details/6784919)

### 联系 ###
> - 博客：[fumasterlin.com](www.fumasterlin.com)
> - 邮箱：[fumasterlin@163.com](fumasterlin@163.com)


