# 顺序表的增删查

~~~C++
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>

typedef int ElemType;
typedef struct
{
    ElemType* data;
    int length;
    int MaxSize;
}SqList;

//顺序表初始化
/*
	1.动态分配内存
	2.判断动态分配内存是否成功
	3.将表的大小设为100
	4.表中有效元素个数置零
*/
void InitList_Sq(SqList &L)
{
    L.data = (ElemType*)malloc(sizeof(ElemType));
    if(!L.data)
    {
        printf("动态内存分配失败！\n");
        exit(-1);
    }
    L.MaxSize = 100;
    L.length = 0;
}

//顺序表的插入
/*
	判断插入位置是否合理
	判断顺序表是否已经满溢
	循环 
		注意循环结束的标志
	将元素插入对应位置
	表长+1
*/
bool ListInsert_Sq(SqList &L,ElemType element,int position)
{
    if(position<0 || position>L.length+1)
        return false;
    if(L.length==L.MaxSize)
        return false;
    for(int i=L.length;i>=position;i--)
    {
        L.data[i]=L.data[i-1];
    }
    L.data[position-1]=element;
    L.length++;
    return true;
}

//顺序表的删除
/*
	判断删除位置是否合理
	判断顺序表是否为空
	记录被删除元素
	循环
		注意循环结束条件
	表长-1
*/
bool ListDelete_Sq(SqList &L,ElemType &element,int position)
{
    if(position<0 || position>L.length)
        return false;
    if(L.length==0)
        return false;
    element = L.data[position-1];
    for(int i=position;i<L.length;i++)
    {
        L.data[i-1]=L.data[i];
    }
    L.length--;
    return true;
}

//顺序表的按值查找
/*
	判断顺序表是否为空
	循环
		注意循环结束条件
	查找成功：返回其所在位置
	查找失败：返回0
*/

int LocateElement_Sq(SqList L,ElemType element)
{
    if (L.length==0)
        return 0;
    int position = 0;
    for (int i=0;i<L.length;i++)
    {
        if(L.data[i]==element)
        {
            position = i;
        	break;
    	}
    }
    return position;
}

//合并有序表
/*
	已知LA，LB是非递减的顺序表，将其归并为一个新的顺序表LC中，且LC也是非递减顺序表
	1.新建顺序表LC，长度为LA,LB长度之和
	2.循环
*/

void MergeList_Sq(SqList LA,SqList LB,SqList &LC)
{
    LC.data = (ElemType*)malloc(sizeof(ElemType));
    LC.length = LA.length+LB.length;
    int i = 0;
    int j = 0;
    int k = 0;
    while(i<LA.length && j<LB.length)
    {
        if(LA.data[i]<LB.data[j])
        {
            LC.data[k]=LA.data[i];
            i++;
            k++;
        }
        else
        {
            LC.data[k]=LB.data[j];
            j++;
            k++;
        }
        
    }
    while (i<LA.length)
    {
        LC.data[k]=LA.data[i];
        i++;
        k++;
    }
    while (j<LB.length)
    {
        LC.data[k]=LB.data[j];
        j++;
        k++;
    }
    
}

int main(void)
{
	int element;
	SqList LA,LB,LC;
	InitList_Sq(LA);
	InitList_Sq(LB);
	ListInsert_Sq(LA,1,1);
	ListInsert_Sq(LA,5,2);
	ListInsert_Sq(LA,8,3);
	ListInsert_Sq(LA,4,2);
	if (ListInsert_Sq(LA,5,3))
		printf("插入成功！\n");
	else
		printf("插入失败！\n");
	if(ListDelete_Sq(LA,element,4))
	{
		printf("删除成功，删除的元素为%d\n",element);
	}
	else
		printf("删除失败！\n"); 
	ListInsert_Sq(LB,2,1);
	ListInsert_Sq(LB,10,2);
	ListInsert_Sq(LB,15,3);
	ListInsert_Sq(LB,4,2);
	MergeList_Sq(LA,LB,LC);
	for(int i=0;i<LC.length;i++)
	{
		printf("%d\n",LC.data[i]);
	}
	
	return 0;
 } 
~~~

