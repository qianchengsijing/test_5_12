# test_5_12
#include <stdio.h>
#include <stdlib.h>
//树的存储结构
//双亲表示法
#define MAX_TREE_SIZE 100
typedef struct{
	ElemType data;
	int parent;
}PTNode;
typedef struct{
	PTNode nodes[MAX_TREE_SIZE];
	int n;
}PTree;
//孩子表示法
struct CTNode{
	int child;
	struct CTNode *next;
};
typedef struct{
	ElemType data;
	struct CTNode *Firstchild;
}CTBox;
typedef struct{
	CTBox nodes[MAX_TREE_SIZE];
	int n,r;
}CTree;
//孩子兄弟表示法
typedef struct CSNode{
	ElemType data;
	struct CSNode *Firstchild,*nextsibling;
}CSNode,*CSTree;
//并查集
#define SIZE 13
int UFSets[SIZE];
//初始化并查集
void Initial(int S[]){
	for(int i=0;i<SIZE;i++)
		S[i] = -1;
}
//Find找到x所在的集合（返回x所在集合根结点）
int Find(int S[],int x){
	while(S[x] >= 0)
		x = S[x];
	return x;
}
//Union把root1和root2合并为一个集合
void Union(int S[],int Root1,int Root2){
	if(Root1 == Root2)
		return;
	else
		S[Root2] = Root1;
}
//Union操作优化(小树合并到大树）
void Union(int S[],int Root1,int Root2){
	if(Root1 == Root2)
		return;
	if(Root1 < Root2)
	{
		S[Root1] += S[Root2];
		S[Root2] = Root1;
	}
	else
	{
		S[Root2] += S[Root1];
		S[Root1] = Root2;
	}
}
//Find操作优化（压缩路径）
int Find(int S[],int x){
	int root = x;
	while(S[root]>=0)
		root = S[root];
	while(x != root)
	{
		int t = S[x];
		S[x] = root;
		x = t;
	}
	return root;
}
//二叉排序树的查找
typedef struct BiTNode{
	ElemType key,data;
	struct BiTNode *lchild,*rchild;
}BSTNode,*BiTree；
BSTNode *BST_Search(BiTree T,ElemType key){
	while(T != NULL && key != T->data){
		if(key<T->data)
			key = T->lchild;
		else
			key = T->rchild;
	}
	return T;
}
//二叉排序树的插入
int BST_Insert(BiTree &T,KeyType k){
	if(T == NULL)
	{
		T = (BiTree)malloc(sizeof(BiTNode));
		T->key = k;
		T->lchild = T->rchild = NULL;
		return 1;
	}
	else if(k == T->key)
		return 0;
	else if(k<T->key)
		return BST_Insert(T->lchild,k);
	else
		return BST_Insert(T->rchild,k);
}
