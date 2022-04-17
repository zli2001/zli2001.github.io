# 03-树1 树的同构


<!--more-->

## 题目
![在这里插入图片描述](https://img-blog.csdnimg.cn/592acbcdb0804330a1bec60a6da3db68.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5YWw5bee5omL5Y23,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9ecaac6ce142431e8b9e1ff55ce14c25.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5YWw5bee5omL5Y23,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0a5ab40c2e44ca18617805ce79c328c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5YWw5bee5omL5Y23,size_20,color_FFFFFF,t_70,g_se,x_16)



## 思路
用链表连接每一个结点，对每一个结点存入其孩子结点信息。


```cpp
#include <iostream>
#include<set>
using namespace std;
typedef struct Node* PNode;
char Name[100];
struct Node
{	//树的结构
	char data;
	char Rchild;
	char Lchild;
	PNode next;
};
typedef Node* Bintree;


Bintree Create()
{//建立树
	Bintree T = (Bintree)malloc(sizeof(Node));
	PNode tail = T;
	if (T == NULL) { cout << "Malloc Failure"; return 0; }
	int N; cin >> N;
	for (int i = 0; i < N; i++)
	{
		char c; char right, left;
		cin >> c>>left>>right;
		Name[i] = c;
		PNode temp = (PNode)malloc(sizeof(Node));
		//尾插法
		temp->Lchild = left;
		temp->Rchild = right;
		temp->data = c;
		temp->next = NULL;
		tail->next = temp;
		tail = temp;
		free(temp);
	}
	PNode p = T->next;
	while (p)
	{//遍历树，添加左右子树
		p->Lchild =Name[p->Lchild-'0'];
		p->Rchild = Name[p->Rchild - '0'];
		p = p->next;
	}
	
	return T;
}
void Print(Bintree T)
{
	Bintree p = T->next;
	while (p)
	{
		cout << p->data;
		cout << p->Lchild;
		cout << p->Rchild;
		cout << endl;
		p = p->next;
	}
}

bool Is_Isomorphic(Bintree t1, Bintree t2)
{
	PNode p  =t1->next;
	while (p)
	{	
		set<char> s1;
		s1.insert(p->Lchild);//t1当前元素的孩子插入s1
		s1.insert(p->Rchild);
		PNode q = t2->next;
		while (p->data != q->data)
		{
			if (q->next)
				q = q->next;//找到t2中与t1相同的元素
			else return false;//若t2中无t1当前元素，则两树不同构
		}
		set<char>s2;
		s2.insert(q->Lchild);//t2当前元素的孩子插入s2
		s2.insert(q->Rchild);
		if (s1 != s2)return false;//若两集合不等则两棵树不同构
		p = p->next;//相等则比较下一元素
	}

	return true;
}
void Release(Bintree t)
{
	PNode p= t;
	PNode q = p;
	while (p)
	{
		q = p->next;
		free(p);
		p = q;
	}

}
int main()
{
	Bintree t1 = (Bintree)malloc(sizeof(PNode));
	Bintree t2 = (Bintree)malloc(sizeof(PNode));
	t1 = Create();
	t2 = Create();
	if (Is_Isomorphic(t1, t2))cout << "Yes";
	else cout << "No";
	Release(t1);
	Release(t2);
}
```


