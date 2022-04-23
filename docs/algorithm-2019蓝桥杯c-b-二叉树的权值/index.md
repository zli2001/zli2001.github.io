# 2019蓝桥杯C++B 二叉树的权值


<!--more-->

## 二叉树的权值

```cpp
#include <iostream>
#include<vector>
#include <math.h>
using namespace std;

int main()
{
    pair<int, int>max(0,0);//shendu/quanzhi
    vector<int>ans;
    ans.push_back(0);
    int N;//个数
    cin >> N;

    int x;//最大层数
    x = ceil(log(N) / log(2));
    if(log(N)/log(2)==x)x+=1;
    for (int i = 1; i <= x; i++)
    {
        int val = 0;
        ans.push_back(0);
       if(i!=x)
        for (int j = 0; j < pow(2, i-1); j++)
        {

            cin >> val;
            ans[i] += val;

        }
       else {

               int n = N - pow(2, x - 1) + 1;

               int cnt = 0;
               while (cnt < n) {
                   cin >> val;
                   ans[x] += val;
                   cnt++;
               }


       }
        if (ans[i] > max.second || (ans[i] == max.second && i < max.first))
        {
            max.first = i; max.second = ans[i];
        }
    }
    if(N==1)max.first=1;

   cout<<max.first;
    return 0;
}


```


