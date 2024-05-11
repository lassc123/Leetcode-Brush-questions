# LeetCode[622. 设计循环队列](https://leetcode.cn/problems/design-circular-queue/)

## 1.题目

![image-20240511200228687](https://gitee.com/lassc123/imgs/raw/master/image-20240511200228687.png)

## 2.解题思路

本题要求实现循环队列，底层可以用链表或数组实现，用链表实现比较简单直接，用数组的话要用到环形数组的技巧。

## 3.实际解题代码

```c
typedef struct CircularQueueNode{
    struct CircularQueueNode*next;
    int val;
}CirQNode;


typedef struct {
    CirQNode* front;
    CirQNode*  rear;
    int size;
    int k;
} MyCircularQueue;

bool myCircularQueueIsEmpty(MyCircularQueue* obj);
bool myCircularQueueIsFull(MyCircularQueue* obj);
MyCircularQueue* myCircularQueueCreate(int k) {
    MyCircularQueue* CQueue=(MyCircularQueue*)malloc(sizeof(MyCircularQueue));
    CQueue->front=(CirQNode*)malloc(sizeof(CirQNode));
    CQueue->rear=CQueue->front;
    CQueue->size=0;
    CQueue->k=k;
    CirQNode* cur=CQueue->front;
    for(int i=0;i<k;i++)
   {
     CirQNode* tmp=(CirQNode*)malloc(sizeof(CirQNode));
    if(tmp==NULL)
    {
        perror("malloc failed");
        return NULL;
    }
    cur->next=tmp;
    cur=cur->next;
   }
   cur->next=CQueue->front;
   return CQueue;
}

bool myCircularQueueEnQueue(MyCircularQueue* obj, int value) {
    if(myCircularQueueIsFull(obj))
    {
        return false;
    }
    obj->rear->val=value;
    obj->size++;
    obj->rear=obj->rear->next;
    return true;
}

bool myCircularQueueDeQueue(MyCircularQueue* obj) {
    if(myCircularQueueIsEmpty(obj))
    {
        return false;
    }
    obj->front=obj->front->next;
    obj->size--;
    
    return 1;

}

int myCircularQueueFront(MyCircularQueue* obj) {
    if(myCircularQueueIsEmpty(obj))
    {
        return -1;
    }
    return obj->front->val;
}

int myCircularQueueRear(MyCircularQueue* obj) {
    if(myCircularQueueIsEmpty(obj))
    {
        return -1;
    }
    CirQNode* cur=obj->front;
    while(cur->next!=obj->rear)
    {
        cur=cur->next;
    }
    
    return cur->val;
    
}

bool myCircularQueueIsEmpty(MyCircularQueue* obj) {
    return obj->size==0;
}

bool myCircularQueueIsFull(MyCircularQueue* obj) {
    return obj->size==obj->k;
    
}

void myCircularQueueFree(MyCircularQueue* obj) {
    int i=obj->k+1;
    CirQNode* tmp=obj->front;
    CirQNode* tmpnext=tmp->next;

    while(i>1)
    {
    free(tmp);
    tmp=tmpnext;
    tmpnext=tmpnext->next;
        i--;
    }
    free(tmp);
    free(obj);

}

```

