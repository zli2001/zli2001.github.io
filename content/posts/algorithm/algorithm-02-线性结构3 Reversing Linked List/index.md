

---

title: "02-线性结构3 Reversing Linked List"
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



## 题目
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac888283cc1348f98a31a07fe0f89413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/2872fa91c2474576ae30c33d248e2d37.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 思路：
利用数组Data[Address]存储数据，再将原始数据的地址信息按顺序存储在数组OriAdr中。再对每K个数字进行反转，将反转后及余下不足K个数的数据的地址存储进容器Ans中，最后遍历打印结果。

## 错误代码：
只反转了数据，没有重新建立新结点的关系即Address，导致出错。

```cpp
#include <iostream>
using namespace std;
#define maxsize 100005
int Data[maxsize];
//int Address[maxsize];
int Next[maxsize];
int Ans[maxsize];//存储反转后的Address
void Read(int *Data,int *Next,int N)
{
	for (int i = 0; i < N; i++)
	{
		int address, data, next;
		cin >> address >> data >> next;
		Data[address] = data;
		Next[address] = next;
	}
}
void Print_Order(int* Data, int* Next,int first)
{
	int temp = first;
	while (temp != -1)
	{
		cout << Data[temp] << " ";
		temp = Next[temp];
		
	}
}
void Print_Format(int num)
{
	if (num == -1) {
		cout << -1; return;
	}
	int d = 10000;
	for (int i=0;i<5;i++)
	{
		int temp = num / d;
		while (temp / 10 != 0)
			temp = temp % 10;
		cout << temp;
		d /= 10;
	}

}
void Reverse(int* Data, int* Next, int first,int K)
{
	int temp = first;
	int cnt = 0;
	//计算有效结点
	
	while (temp != -1)
	{
		
		cnt++;
		Ans[cnt] = temp;
		temp = Next[temp];
	}
	for (int i = 0;; i += K)
	{	//若剩余结点为0，则结束循环
		
		//若剩余未反转结点不足K个，则顺序输出
		if (i + K > cnt)
		{
			cout << endl;
			i++;
			while (i <= cnt)
			{
				Print_Format(Ans[i]);
				cout << " ";
				cout << Data[Ans[i]] << " ";
				Print_Format(Next[Ans[i]]);
				if (i == cnt);
				else cout << endl;
				i++;
			}
			break;
		}
		int j = i+K;
		if (i != 0)cout << endl;
		while (j > i)
		{
			
			Print_Format(Ans[j]);
			cout << " " << Data[Ans[j]] << " ";
			Print_Format(Next[Ans[j]]);
			if (j == i + 1);
			else cout << endl;

			j--;
		}
		if (i + K == cnt)break;
	}
	
}
int main()
{
	
	int first, N, K;//首地址，节点个数，反转个数
	cin >> first >> N >> K;
	Read(Data, Next, N);
	//Print_Order(Data, Next, first);
	Reverse(Data, Next, first, K);
	return 0;
}
```
### 结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/aedf6474eecb4ccda786df653782c07e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 更改后的代码：
创建一个vector容器，将反转后的结点依次加入，再遍历容器打印结果。

```cpp
#include <iostream>
#include<vector>
using namespace std;
#define maxsize 100005
int Data[maxsize];
//int Address[maxsize];
int Next[maxsize];
int OriAdr[maxsize];//按顺序存储有效节点的Address
//int Ans[maxsize];//反转后的结点Address
vector<int>  Ans;
void Read(int *Data,int *Next,int N)
{
	for (int i = 0; i < N; i++)
	{
		int address, data, next;
		cin >> address >> data >> next;
		Data[address] = data;
		Next[address] = next;
	}
}
void Print_Format(int num)
{//格式化输出五位地址数
	if (num == -1) {
		cout << -1; return;//值为-1，则输出-1
	}
	int d = 10000;
	for (int i=0;i<5;i++)
	{//对于五位地址，每一位分别输出
		int temp = num / d;//求出当前位及之前的数字
		while (temp / 10 != 0)//求出当前位的数字，丢弃前面的数字
			temp = temp % 10;
		cout << temp;
		d /= 10;
	}

}
void Reverse(int* Data, int* Next, int first,int K)
{
	int temp = first;
	int cnt = 0;
	//计算有效结点，题目所给有些结点是多余的
	
	while (temp != -1)
	{
		cnt++;
		OriAdr[cnt] = temp;//OriAdr顺序存储未反转前的链表，起始下标为1
		temp = Next[temp];//按链表地址顺序索引
	}
	for (int i = 0;; i += K)
	{
		//若剩余未反转结点不足K个，则将余下结点按顺序加入Ans中
		if (i + K > cnt)
		{
			i++;//从第i+1个节点开始
			while (i <= cnt)
			{	//依次加入
				Ans.push_back(OriAdr[i]);
				i++;
			}
			break;//结束循环
		}
		//对于第i至第j个节点——共K个
		int j = i+K;
		//反转后加入Ans
		while (j > i)
		{	//注意OriAdr下标是从1开始的
			Ans.push_back(OriAdr[j]);
			j--;
		}
		//若剩余结点为0，则结束循环
		if (i + K == cnt)break;
	}
	//遍历Ans，打印结果
	for (auto it = Ans.begin(); it != Ans.end(); it++)
	{
		Print_Format(*it);
		cout << " " << Data[*it] << " ";
		if (it+1 == Ans.end())cout << -1;//若当前节点为最后一个有意义的节点，则输出结束标志-1
		else {
			Print_Format(*(it+1)); cout << endl;//否则输出下一个节点的数据并换行
		}
	}
}
int main()
{
	
	int first, N, K;//首地址，节点个数，反转个数
	cin >> first >> N >> K;
	Read(Data, Next, N);
	//Print_Order(Data, Next, first);
	Reverse(Data, Next, first, K);
	return 0;
}
```
### 结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/3140d3954fff4bc3ae337c2616b208a1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 总结：
题目虽然是反转链表，但尝试用链表解决此题似乎很麻烦。用数组应该是比较容易理解和实现的方法。此外在审题方面仍然做的不够好，导致浪费了时间。
