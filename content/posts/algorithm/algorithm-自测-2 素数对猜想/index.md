---

title: "自测-2 素数对猜想"
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
![在这里插入图片描述](https://img-blog.csdnimg.cn/139a897c5f2846d79419b5b5f50f53db.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f3441956e2a4d01b01cf27a431229e8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 思路
便于分析，我们先列出一些质数：
2，3，4，5，7，11，13...
**求素数对：**
当N小于等于4，无满足条件的素数对。
当K大于4，从5开始的偶数一定不是素数，因此依次加2循环，判断每个数是否为素数，若为素数则与前一个素数相减，结果为2则满足条件素数对个数加1；
**对于素数的判断：**
对于大于2的数N，对从2开始的数每次加1，直到除数等于N为止。若有能整除的数，即有除N外的因数，则不为素数；若没有则为素数；
思路很简单，**但此方法的时间复杂度为O(n²）**，因此需要减小复杂度。
## 超时代码:

```cpp
#include <iostream>
using namespace std;
bool IsPrime(int n);
int PairNumber(int N)
{
	if (N <= 4)return 0;//小于等于4时结果为0
	int last = 0;//上一个素数
	int cnt = 0;//素数对个数
	for (int i = 3; i <= N; i += 2)
	{
		if (IsPrime(i))
		{//若i为素数
			int now = i;
			if (now - last == 2)
				cnt++;//当now与上一个素数相差为2，计数加一
			last = now;//更新上一个素数
		}
	}
	return cnt;
}
bool IsPrime(int n)
{
	if (n == 1)return false;
	if (n == 2)return true;
	for (int i = 2; i < n; i++)
	{

		if (n % i == 0)return false;//若n有因数，不为素数
	}
	return true;//除1和本身外无因数，为素数
}
int main()
{
	int N; cin >> N;
	cout << PairNumber(N);
}
```
### 结果
当n过大时，超时
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2fe66244abf4a9993309ee8ca7e0768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 优化
- 方法1.
（待完善）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20dcb13e57b141cea697c924f9a1d99c.png)
- 方法2.
为减小时间复杂度，使用筛选法，开辟数组空间，所有数默认为素数。
NotPrime[17]=0;//表示17为素数
NotPrime[16]=1;//表示16为非素数
对前N个数进行遍历，每遍历一个数，若此数已被设为非素数，则跳过。否则将这个数的所有倍数设为非素数，最终得到N个数中每一个数的属性。
再遍历数组，若前后素数相差为2，则素数对的个数加1；

```cpp
#include <iostream>
using namespace std;
int NotPrime[100005];//存放素数标志，1表示非素数，0表示素数，默认全为0
int PairNumber(int N)
{
	if (N <= 4)return 0;//小于等于4时结果为0
	int cnt = 0;
	for (int i = 5; i <= N; i+=2)
	{
		if (NotPrime[i] == 0 && NotPrime[i - 2] == 0)cnt++;//前后素数相差2则为满足条件的素数对
	}
	return cnt;
}
void SetNotPrime(int n)
{//筛选素数
	NotPrime[0] = NotPrime[1] = 1;//设置0和1都为非素数
	for (int i = 2; i <= n; i++)
	{	
		if (NotPrime[i] == 1)continue;//若已标记为非素数，直接跳过
		int j = 2;
		int temp = j*i;//将i的倍数全部标记为非素数
		while (temp <= n)
		{//此循环是先判断temp是否越界，再赋值，否则会出现段错误
			NotPrime[temp] = 1;//i的j倍，设为非素数
			j++;
			temp = j * i;
			
		}
	}
}
int main()
{
	
	int N; cin >> N;
	SetNotPrime(N);
	cout << PairNumber(N);
}
```
## 结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/a3b1ffb83cef47aea5c0a560b8cb15c0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
## 总结
筛选法这一种快速求某区间中素数的方法十分巧妙，能够有效地减小时间复杂度，但是也牺牲了一定的空间。
