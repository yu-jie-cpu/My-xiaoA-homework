# **我的C语言学习笔记**

## *前言：*

​	从2019年高中毕业的那个暑假开始，我就了解到c语言是与我专业相关性很高的一种编程语言，也正是从那时候开始，我第一次地接触到了编程语言，当我写下人生中的第一个c语言程序"Hello,world."的时候，我真的十分地激动，我的c语言学习之路就从此处开开始了。

## *3月1日-3月5日的学习笔记：*

### 大体内容：A.掌握单向链表的各种操作	B.用c语言实现长度可以自动增长的数组



**First.关于链表的具体学习内容：**

1.参考网站：[关于C语言链表](https://blog.csdn.net/morixinguan/article/details/68951912"关于C语言链表")

2.具体内容：a.制作链表	b.实现利用链表储存一些数据的函数	c.实现打印链表数据的函数	d.实现链表中数据搜索的函数	e.实现链表中部分数据删除的函数	f.实现将整个链表删除的函数

3.相关结构：

A.表示链表的内容的结构

typedef struct _node {
	int value;
	struct _node* next;
} Node;

B.定义自己的数据结构类型来表示整个链表，能够使得链表内容具有可扩充性

typedef struct _list {	//定义了自己的数据结构来代表整个链表，其中有着链表的											头部 
	Node* head;		//定义在结构里面的指向另外的结构的指针 
} List;



4.相关函数:

a.将数值填入链表的函数

void add (List *plist, int number)
{
			

	    Node *p = (Node*)malloc(sizeof(Node)); //动态申请空间 
		p->value = number;
		p->next = NULL;		//将数据填入新申请的空间 
		// find the last
		Node *last = plist->head;	//为遍历链表做准备 
		if( last )
		{
			while ( last->next )	//当到达链表的最后一个的前一个时，离开循环 
			{
				last = last->next;
			}
			// attach
			last->next = p;
		}
		else
		{
			plist->head = p;
		}
}			

b.打印链表内所有数据的函数

void print(List *plist)	//打印链表的函数原型2
{
		Node *p;
		for (p = plist->head; p; p = p->next) //常见的遍历链表的方法 
		{
			if( p->value==-1 )	//防止输出最后的一个number-1 
				break;
			printf("%d", p->value);
		 } 
		printf("\n");
	}

c.删除链表中部分节点的函数

void	delete（List* plist）

Node*p,Node* q; 	//链表中部分节点的删除 
	for (q = NULL, p == list.head; p; q = p, p = p->next)
	{
		if (p->value = number)		//其中p是有保障的，一定不会指向NULL 
		{
			if (q != NULL)
			{
				q->next = p->next;		//当找的是链表的首个时，q指向NULL 
			}							//此时通过q去访问里面的数据时会出错 
			else
			{
				list.head = p->next;			//此时便有了保障q的机制，防止了
												//因q指向NULL所带来的错误使用q访问数据的错误 
			}
			free(p);
			break;
		}
	}



**Second:关于可增长数组的学习内容**

1.参考网站：[关于C语言的可增长数组](https://www.icourse163.org/learn/ZJU-200001#/learn/content?type=detail&id=1212809728&sm=1)	[C语言实现可扩充的数组](https://blog.csdn.net/qq_35025383/article/details/80845139)

2.具体内容： a.确定每一次数组所需要增长的长度  b.实现利用动态内存申请相应的内存空间的函数  c.实现将申请内存空间删除的函数  d.实现计算数组长度函数  e.实现判断数组长度是否够用的函数，并且在内存不够用时，能够调用整长数组的函数f.实现返回数组某个下标中内容的函数 g.实现将数据填入数组的函数 h.（重点）实现使得数组增长的函数

3.相关函数：

a.利用动态申请内存创造数组的函数

 Array array_create(int init_size)	//动态内存申请一个数组的函数，需要输入想要创建的数组大小
{
	Array a;
	a.size = init_size;
	a.array = (int*)malloc(sizeof(int)*a.size);
	return a;
}

b.将数组所申请的内存释放的函数

void array_free(Array *a)	//将数组所申请来的内存释放 
{
	free(a->array);
	a->array=NULL;
	a->size=0;
}

c.封装，返回数组的长度函数

void array_free(Array *a)	//将数组所申请来的内存释放 
{
	free(a->array);
	a->array=NULL;
	a->size=0;
}

d.判断数组长度，并且在长度不够时可以调用增长函数的函数

 int* array_at(Array *a,int index)
 {
 	if( index >= a->size){	//判断数组长度是否不够用了 
 		array_inflate(a,(index/BLOCK_SIZE+1)*BLOCK_SIZE-a->size);	//调用增长函数 
 	}
 	return &(a->array[index]); //返回数组在某个下标上的元素的指针（地址） 
}

e.返回数组某个下标上所表示元素的值的函数

int array_get (const Array*a,int index)
{
	return a->array[index];	//返回数组在某个指数的值 
}

f.将数值填入数组某个下标上元素的函数

void array_set(Array *a,int index,int value) 
{
	a->array[index] = value;	//把value填入数组的index位置 
}

g.(重点)增长数组的函数

void array_inflate(Array *a,int more_size)	//增长数组的函数 
{
	int *p =(int*)malloc(sizeof(int)(a-> size+ more_size));	//申请空间 
	int i;
	for( i=0; i<a->size;i++){
		p[i] = a->array[i];		//复制数组 
	}
	free(a->array);	//释放原本所占用的空间 
	a->array=p;	//生成新的数组，得到地址 
	a->size += more_size;	//新的数组长度 
}






