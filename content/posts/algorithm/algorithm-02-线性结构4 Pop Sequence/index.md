---

title: "02-线性结构4 Pop Sequence"
date: 2021-08-16T10:09:58+08:00
draft: true
lastmod: 2021-08-16T10:09:58+08:00

author: "zli2001"
authorLink: "https://zli2001.github.io"
hiddenFromSearch: false
tags: ["C++","code","algorithm"]
categories: ["Algorithm"]


lightgallery: true
---

<!--more-->


## 题目：
![在这里插入图片描述](https://img-blog.csdnimg.cn/3d5faee019ec44deb5b4927a1ee092b3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f4f61fc01b17461799b22077b1a5121d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
# 分析
第一眼看到题其实想的是用一个容器来模拟栈的出栈入栈即可，不需要写栈的结构，但是写到一半就没了头绪。于是重头来过，先写好栈的结构，再写入栈和出栈的函数（本题只需要用到这两个），然后再将题目所给的每一种情况都考虑，就已经能得出结果。总体来说并不复杂，结合栈的特性并认真分析就能覆盖每一种情况。
**注意**：此题的栈是有容量限制M的，只能存放指定的元素,不能溢出。
### 核心算法描述：
扫描输入序列的每一个元素，模拟元素出栈入栈。
a.若此元素比已出栈的最大元素更**大**，则将他们间的元素压入栈，若栈溢出，则终止并返回false
b.若此元素比已出栈的最大元素**小**，当栈顶元素等于此元素，可出栈，否则则无法出栈，返回false。
当程序顺利执行完毕，对每一个元素都满足入栈出栈条件，则返回true
（部分流程示意）
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2280cc5f41c440a84ebf729531b89e7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)

## 代码
```cpp
#include <iostream>
using namespace std;
#define Maxsize 1000
typedef int ElemType;
int M;//capacity
int N;//length
int K;//数据组数
struct SqStack
{
	ElemType data[Maxsize];//
	int top=-1;//栈顶指针
};
//入栈
bool PushStack(SqStack *s, int e)
{
	if (s->top == M - 1) {
		//cout << "Stack Overflow!" << endl; 
		return false;
	}
	s->data[++s->top] = e;
	return true;
}
//出栈
bool PopStack(SqStack* s)
{
	if(s->top==-1)
	{
		//cout << "Empty Stack!" << endl;
		return false;
	}
	s->top--;
	return true;
}
bool IsPossible()
{
	int data[Maxsize];//存放一组输入的序列
	for (int i = 0; i < N; i++)
		cin >> data[i];//读取输入序列
	SqStack s;//模拟栈
	ElemType temp = -1;//当前pop的元素
	ElemType max=0;//pop出的最大元素
	bool flag = true;//溢出、空栈标志
	for (int i = 0; i < N; i++)
	{
		 temp=data[i];//更新当前元素
		if (temp > max)
		{//当前输入元素比出现过的最大元素更大
			int a = max+1;
			while (a <= temp)
			{//将（最大元素，当前元素]间的元素压入栈
				flag = PushStack(&s, a);//若栈溢出则返回false
				if (flag == false)return false;
				a++;
			}flag = PopStack(&s);//当前元素出栈，空栈返回false
			if (flag == false)return false;
			max = temp;//更新最大元素
		}
		else {
			//当前元素小于最大已出栈元素
			
			if (s.data[s.top] == temp)//若栈顶元素等于当前元素则直接出栈
			{
				flag = PopStack(&s);
				if (flag == false)return false;//若为空栈则返回false
			}
			else return false;//若栈顶元素不等于当前元素则返回false
		}
	}
	return true;
}
int main()
{
	cin >> M >> N >> K;
	bool ans[Maxsize];//存放每一组的结果
	for (int i = 0; i < K; i++)
	ans[i] = IsPossible();//对每一组进行判断
	for (int i = 0; i < K; i++)
	{//打印结果
		if (ans[i] == true)cout << "YES";
		else cout << "NO";
		if (i != K - 1)cout << endl;
	}
	return 0;
}
```

