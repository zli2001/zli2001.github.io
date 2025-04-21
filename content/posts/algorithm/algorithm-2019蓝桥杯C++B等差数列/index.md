

---

title: "2019蓝桥杯C++B等差数列"
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


```cpp
#include <iostream>
#include<vector>
#include <set>

using namespace std;
int N;
multiset<int>input;
set<int> d;
int zero=0;;
int gcd(int i,int j)
{
    if(i>j)swap(i,j);
    int temp=0;
    while(i!=0)
    {
        temp=i;
        i=j%i;
        j=temp;

    }
    return temp;
}
int main()
{
    cin>>N;
   for(int i=0;i<N;i++)
   {
       int val=0;
       cin>>val;
       input.insert(val);
   }

   set<int>::iterator it;

it=input.begin();
int last=*it;
   for(++it;it!=input.end();it++)
   {
        d.insert(*it-last);
        if(*it-last==0){zero=1;break;}
       last=*it;
   }
if(zero==1){cout<<N;}//常数数列
else
    {it=d.begin();

   int GCD=*it;
   for(++it;it!=d.end();it++)
   {
       GCD=gcd(GCD,*it);
   }

   int n=(*(--input.end())-*input.begin())/GCD+1;
   cout<<n;
}
       return 0;
}


```

