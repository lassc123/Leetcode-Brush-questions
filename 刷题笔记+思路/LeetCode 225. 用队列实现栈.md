# LeetCode [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

## 1.题目

![image-20240510222139611](https://gitee.com/lassc123/imgs/raw/master/image-20240510222139611.png)

## 2.题解

```c
typedef int QueueDataType;
//创建队列节点
typedef struct QueueNode
{
	struct QueueNode* next;
	QueueDataType val;
		
}QNode;
//创建队列
typedef struct Queue
{
	QNode* head;//创建头节点方便头删
	QNode* tail;//创建尾节点方便尾插
	int size;//计队列的节点数

}Queue;
//队列的初始化和销毁
void QueueInit(Queue* q);
void QueueDestroy(Queue* q);
//入队列和出队列
void QueuePush(Queue* q,QueueDataType x);
void QueuePop(Queue* q);
//获取队列的队头元素和队尾元素
QueueDataType QueueFront(Queue* q);
QueueDataType QueueBack(Queue* q);
//队列的判空
bool QueueEmpty(Queue* q);
//队列中有效元素个数
int QueueSize(Queue* q);

void QueueInit(Queue* q)
{
	q->head = q->tail = NULL;
	q->size = 0;
};
void QueueDestroy(Queue* q)
{
	QNode* cur = q->head;
	while (cur != NULL)
	{
		QNode* next = cur->next;
		free(cur);
		cur = next;
	}
	q->head = q->tail = NULL;
	q -> size = 0;
	
};
bool QueueEmpty(Queue* q)
{
	return q->head == NULL &&
		q->tail == NULL;
};
void QueuePush(Queue* q, QueueDataType x)
{
	if (QueueEmpty(q))
	{
		QNode* newnode = (QNode*)malloc(sizeof(QNode));
		newnode->val = x;
		newnode->next = NULL;
		if (newnode == NULL)
		{
			perror("malloc failed");
			return;
		}
		q->head = newnode;
	
		q->tail = newnode;
		q->size++;
	}
	else {
		QNode* newnode = (QNode*)malloc(sizeof(QNode));
		newnode->val = x;
		newnode->next = NULL;
		if (newnode == NULL)
		{
			perror("malloc failed");
			return;
		}
		q->tail->next = newnode;
		q->tail = newnode;
		q->size++;
	}
};
void QueuePop(Queue* q)
{
	assert(!QueueEmpty(q));
	QNode* next = q->head->next;
	q->head = next;
	q->size--;
	//只有单一节点时
	if (q->head == NULL)
	{
		q->tail = NULL;
	}
}
QueueDataType QueueFront(Queue* q)
{
	assert(!QueueEmpty(q));

	return q->head->val;
};
QueueDataType QueueBack(Queue* q)
{
	assert(!QueueEmpty(q));
	return q->tail->val;
}
int QueueSize(Queue* q)
{
	return q->size;
}


typedef struct {
    Queue que1;
    Queue que2;
}MyStack;

bool myStackEmpty(MyStack* obj);

MyStack* myStackCreate() {
    MyStack* ST=(MyStack*)malloc(sizeof(MyStack));
    QueueInit(&ST->que1);
    QueueInit(&ST->que2);
    return ST;
    
}

void myStackPush(MyStack* obj, int x) {
    Queue* QEmpty=&obj->que1;
    Queue* QNoEmpty=&obj->que2;
    if(!QueueEmpty(&obj->que1))
    {
        QEmpty=&obj->que2;
        QNoEmpty=&obj->que1;
    }
    QueuePush(QNoEmpty,x);
}

int myStackPop(MyStack* obj) {
    assert(!myStackEmpty(obj));
    Queue* QEmpty=&obj->que1;
    Queue* QNoEmpty=&obj->que2;
    if(!QueueEmpty(&obj->que1))
    {
        QEmpty=&obj->que2;
        QNoEmpty=&obj->que1;
    }
    while(QueueSize(QNoEmpty)>1)
    {
        QueuePush(QEmpty,QueueFront(QNoEmpty));
        QueuePop(QNoEmpty);
    }
    int ret=QueueBack(QNoEmpty);
        QueuePop(QNoEmpty);
    return ret;

}

int myStackTop(MyStack* obj) {
    if(!QueueEmpty(&obj->que1))
    {
        return QueueBack(&obj->que1);
    }
    else
    {
        return QueueBack(&obj->que2);

    }
}

bool myStackEmpty(MyStack* obj) {
    return QueueEmpty(&obj->que1)&&QueueEmpty(&obj->que2);
}

void myStackFree(MyStack* obj) {
    QueueDestroy(&obj->que1);
    QueueDestroy(&obj->que2);
    free(obj);
}

/**
 * Your MyStack struct will be instantiated and called as such:
 * MyStack* obj = myStackCreate();
 * myStackPush(obj, x);
 
 * int param_2 = myStackPop(obj);
 
 * int param_3 = myStackTop(obj);
 
 * bool param_4 = myStackEmpty(obj);
 
 * myStackFree(obj);
*/
```

