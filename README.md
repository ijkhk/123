# 123
上课的作业
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#include <string.h>
#define MAX 1024

char a[MAX],b[MAX];

typedef struct node
{ 
    char data[MAX];
    int no;
    struct node *next;
}linknode;

typedef struct
{
    linknode *front;
    linknode *rear;
}listqueue;
listqueue *xa;           //A消息队列 
listqueue *xb;           //B消息队列

void setnull(listqueue *q)          //建立队列 
{
    q->front=q->rear=(linknode *)malloc(sizeof(linknode));
    q->rear->next=NULL;
}

int empty(listqueue *q)             //消息队列是否为空 
{
    if(q->front==q->rear)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}

void enqueue(listqueue *q,char a[])             //发送一条消息 
{
    q->rear->next=(linknode *)malloc(sizeof(linknode));
    q->rear=q->rear->next;
    strcpy(q->rear->data,a);
    q->rear->next=NULL;
}

void dequeue(listqueue *q,char b[])               //接收一条消息
{
    if(empty(q))
    {
        printf("队列已空\n");
        return;
    }
    linknode *p=NULL;
    p=q->front;
    q->front=q->front->next;
    free(p);
    p=NULL;
    strcpy(b,q->front->data);
}

void show_queue(listqueue *q){                    //打印消息队列 
	linknode *p=q->front->next;
	if(p==NULL) printf("队列空\n");
	while(p!=NULL){
		puts(p->data);
		p=p->next;
	}
	printf("\n"); 
}

void deall(listqueue *q)                    //接收所有消息 
{
	while(!empty(q))
	{
		dequeue(q,b);
		puts(b);
	}
}

void print_menu()
{
	printf("按1键A进程给B进程发送消息 \n");
	printf("按2键B进程给A进程发送消息\n");
	printf("按3键显示A的消息队列\n");
	printf("按4键显示B的消息队列\n");
	printf("按5键A接收一个消息\n");
	printf("按6键B接收一个消息\n");
	printf("按7键A接收所有消息\n");
	printf("按8键B接收所有消息\n");
	printf("按9键显示A和B的消息队列\n");
	printf("按0键退出\n");
	printf("==================================================\n"); 
}

int main()
{
    
    int i,s;
    xa = (listqueue *)malloc(sizeof(listqueue));          //A消息队列 
	xb = (listqueue *)malloc(sizeof(listqueue));          //B消息队列 
    setnull(xa);
    setnull(xb);  
	printf("\n谭霁峰            数据18-1班            2018015303\n\n\n");
	print_menu();
	while(1)
	{
		printf("您的选择： \n");	 
		scanf("%d", &s);
		switch(s)
		{
			case 1:{
				fflush (stdin);                             //刷新缓冲区
				printf("请输入A进程给B进程发送消息：");
				gets(a);
				enqueue(xb,a);
				break;
			}
			case 2: {
				fflush (stdin);                            //刷新缓冲区
				printf("请输入B进程给A进程发送消息:") ;
				gets(a);
				enqueue(xa,a);
				break;
			}	
			case 3: {
				printf("A的消息队列为（从头到尾）:\n"); 
				show_queue(xa);
				break;
			} 
			case 4: {
				printf("B的消息队列为（从头到尾）:\n");  
				show_queue(xb);
				break;
			} 
			case 5: {
				dequeue(xa,b);
				printf("A接收一个消息为:");
				puts(b);
				break;
			}
			case 6: {
				dequeue(xb,b);
				printf("B接收一个消息为:"); 
				puts(b);
				break;
			}
			case 7: {
				printf("A接收所有消息为：\n");
				deall(xa);
				break;
			}
			case 8: {
				printf("B接收所有消息为：\n"); 
				deall(xb);
				break;
			}
			case 9: {
				printf("A的消息队列为（从头到尾）:\n");
				show_queue(xa);
				printf("B的消息队列为（从头到尾）:\n");  
				show_queue(xb);
				break;
			}
			case 0: 
				return 0;
				break;
		}
	}



    return 0;
}
