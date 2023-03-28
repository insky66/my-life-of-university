#include "stdio.h"
#include <cstdlib>
#include <iostream>
using namespace std;
struct LinkNode
{
	int coef;
	int exp;
	struct LinkNode* next;
};
void Linkcreate(LinkNode*& L,int c, int e)
{
	LinkNode* s,*p;
	s = (LinkNode*)malloc(sizeof(LinkNode));
	if (s == nullptr)exit(0);
	p = L; 
	s->coef = c;
	s->exp = e;
	if(L->next==NULL)
	{
		L->next = s;
		s->next = NULL;
	}
	else
	{
		while (p->next->exp > s->exp)
		{
			p = p->next;
			if (p->next == NULL)break;
		}
		if (p->exp == s->exp)cout << "重复输入请退出重进" << endl;
		if (p->next == NULL) { p->next = s; s->next = NULL; }
		if(p->next->exp < s->exp)
		{
			s->next = p->next;
			p->next = s;
		}
	}
}
void Linkadd(LinkNode*& L, LinkNode* s)
{
	LinkNode* p = L, * q = s; int a;
	if (s == NULL) { cout << "第二个多项式不可为空"; cin >> a; }
	while(1)
	{
		if (p->next->exp == s->next->exp)
		{
			p->next->coef += s->next->coef;
			s = s->next;
			p = p->next;
		}
		else if(p->next->exp < s->next->exp)
		{
			s = s->next;
		}
		else
		{
			p = p->next;
		}
		if (s->next == NULL)break;
	}
	while (q->next!=NULL)
	{
		Linkcreate(L, q->next->coef, q->next->exp);
		q->next = q->next->next;
	}
	
}
void DispList(LinkNode* L)
{
	LinkNode* p = L->next;
	while (p != NULL)
	{
		if(p->next==NULL)cout << p->coef << "x^" << p->exp<< endl;
		else cout << p->coef<<"x^" << p->exp<<"+" << endl;
		p = p->next;
	}
	cout << endl;
}
int main()
{
	LinkNode* L,*s;
	L = (LinkNode*)malloc(sizeof(LinkNode)); if (L == nullptr)exit(0);
	L->next = NULL;

	s = (LinkNode*)malloc(sizeof(LinkNode));
	s->next = NULL;
	int coef, exp;
	while (1)
	{
		cout << "请输入d=第一个多项式的项系数和底数(输入0结束）" << endl;
		cin >> coef;
		if (coef == 0)break;
		cin>> exp;
		Linkcreate(L, coef, exp);
	}
	while (1)
	{
		cout << "请输入第二个多项式的项系数和底数(输入0结束）" << endl;
		cin >> coef;
		if (coef == 0)break;
		cin >> exp;
		Linkcreate(s, coef, exp);
	}
	Linkadd(L, s);
	DispList(L);
	return 0;
}
