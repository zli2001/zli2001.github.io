# UVa10391


<!--more-->






遍历所有字符串，两两相加然后匹配的解法TLE

所以采用另一种思路，查看每个单词的拆分组合是否已存在于set中

这里用了str.substr(x,n)函数

>x 指定字符串起始位置
> 
>n 指定复制的字符数长度




TLE代码：
```c
#include <map>
#include<vector>
#include<set>
#include <string>
#include<iostream>
using namespace std;
map<string,int>IDcache;
vector<string>Strcache;
int ID(string s)
{
	if (IDcache.count(s)==1)  return IDcache[s];
	Strcache.push_back(s);
	return IDcache[s] = Strcache.size() - 1;
}

void IsCompound()
{
	
	string temp;
	set<string>ans;
	while (cin >> temp)
	{
		
		int x = ID(temp);
	
		
	}
	int size = Strcache.size();
	
	for (int i = 0; i < size; i++)
		for (int j = i + 1; j < size; j++)
		{
			string temp1 = Strcache[i] + Strcache[j];
			string temp2 = Strcache[j] + Strcache[i];
			int t1 = ID(temp1);
			int t2 = ID(temp2);
			if (t1<=IDcache.size())
			{
				ans.insert(Strcache[IDcache[temp1]]); continue;
			}
			if (IDcache.count(temp2) != 0) {
				ans.insert(Strcache[IDcache[temp2]]);
				
			}
			
		}
	auto it = ans.begin();
	
	while(it!=ans.end())
	{
		
		cout << *it << endl; it++;
	}
}
int main()
{
	IsCompound();
	return 0;
}
```


 


# AC代码：

```c
#include <iostream>
#include <set>
#include <vector>
#include <iostream>
#include<string>
using namespace std;
vector <string> Str;
set<string>s;
int main()
{
	string temp;
	while (cin >> temp)
	{
		Str.push_back(temp);
		s.insert(temp);
		
	}


	for (int i = 0; i < s.size(); i++)
		for (int j = 1; j < Str[i].size(); j++)
			if (s.count(Str[i].substr(0, j)) && s.count(Str[i].substr(j, Str[i].size() - j)))

			{
				cout << Str[i] << endl; break;//若不break将WE,可能会重复输出
			}


	return 0;
}


```

[参考博文](https://blog.csdn.net/iboxty/article/details/45958193?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1328741.49540.16170815072631933&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

