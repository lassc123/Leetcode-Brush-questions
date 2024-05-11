# LeetCode[232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

## 1.题目

![image-20240511163702583](https://gitee.com/lassc123/imgs/raw/master/image-20240511163702583.png)

## 2.题目思路

栈实现队列，按照队列实现队列的思路，让一个栈作为空栈（sta1），==用于倒换数据==，另一个栈做非空栈（sta2），==用于存储数据==。

但是当我们实际去画图看这个倒换数据的过程就会发现![IMG_0166](https://gitee.com/lassc123/imgs/raw/master/IMG_0166.PNG)

倒出非空栈（sta2)来出队列元素*1*时,暂时进入到空栈sta1的元素顺序发生了倒换，而且这时出栈的顺序就是元素出队列的顺序，所以我们不妨改变sta1和sta2的功能，让sta1仅作为**入队列的栈**，sta2仅作为**出队列的栈**。

```c
typedef int STDataType;
typedef struct Stack
{
	STDataType* a;
	int size;
	int top;
	int capacity;
}ST;
//栈的初始化 栈的销毁
void StackInit(ST* s);
void StackDestroy(ST* s);
//入栈 出栈
void StackPush(ST* s, STDataType x);
void StackPop(ST* s);
//栈的取顶和判空
STDataType StackTop(ST* s);
bool StackEmpty(ST* s);


void StackInit(ST* s)
{
	s->a = NULL;
	s->top = -1;
	s->size = s->capacity = 0;
};
void StackDestroy(ST* s)
{
	s->size = s->capacity = 0;
	s->top = -1;
	free(s->a);
	s->a = NULL;
};
//入栈 出栈
void StackPush(ST* s, STDataType x)
{
	//检查容量是否满了
	if (s->capacity == s->size)
	{
		int newcapacity = s->capacity == 0 ? 4 : s->capacity * 2;
		STDataType* tmp = (STDataType*)realloc(s->a, newcapacity * sizeof(STDataType));
		if (tmp == NULL)
		{
			perror("realloc failed");
			return;
			//gpt给出的建议
			//exit(EXIT_FAILURE); 如果realloc失败，退出程序
		}
		s->a = tmp;
		s->capacity = newcapacity;
	}
	s->top++;
	s->a[s->top] = x;
	s->size++;

};
void StackPop(ST* s)
{
	assert(!StackEmpty(s));
	s->top--;
	s->size--;
};
//栈的取顶和判空
STDataType StackTop(ST* s) {
	assert(!StackEmpty(s));
	return s->a[s->top];

};

bool StackEmpty(ST* s)
{
	if (s->size == 0)
		return true;
	else
		return false;
	//更好，更简洁的方式
	//return s->size == 0;
};



typedef struct {
    ST sta1;//入队列的栈
    ST sta2;//出队列的栈

} MyQueue;


MyQueue* myQueueCreate() {
    MyQueue* QStack=(MyQueue*)malloc(sizeof(MyQueue));
    StackInit(&QStack->sta1);
    StackInit(&QStack->sta2);
    return QStack;
}

void myQueuePush(MyQueue* obj, int x) {
    assert(obj);
    StackPush(&obj->sta1,x);
}

int myQueuePop(MyQueue* obj) {
    if(StackEmpty(&obj->sta2))
    {
        while(obj->sta1.size>0)
        {
            StackPush(&obj->sta2,StackTop(&obj->sta1));
            StackPop(&obj->sta1);
        }
    }

    assert(obj);
    
    int ret=StackTop(&obj->sta2);
    StackPop(&obj->sta2);
    return ret;
}

int myQueuePeek(MyQueue* obj) {
    if(StackEmpty(&obj->sta2))
    {
        while(obj->sta1.size>0)
        {
            StackPush(&obj->sta2,StackTop(&obj->sta1));
            StackPop(&obj->sta1);
        }
    }

    assert(obj);
    
    int ret=StackTop(&obj->sta2);
    
    return ret;
}

bool myQueueEmpty(MyQueue* obj) {
    return StackEmpty(&obj->sta2)&&StackEmpty(&obj->sta1);
}

void myQueueFree(MyQueue* obj) {
    free(obj);
    
}

/**
 * Your MyQueue struct will be instantiated and called as such:
 * MyQueue* obj = myQueueCreate();
 * myQueuePush(obj, x);
 
 * int param_2 = myQueuePop(obj);
 
 * int param_3 = myQueuePeek(obj);
 
 * bool param_4 = myQueueEmpty(obj);
 
 * myQueueFree(obj);
*/
```

