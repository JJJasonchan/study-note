# cs50 note
## 算法 algorithms
在描述算法的效率是，可以使用*O*和Ω
*o*表示算法速度的上限，Ω表示算法速度的下限
## 数据结构  Data Structure
- array数组，动态的分配数组内存，可以使用`malloc`函数动态分配内存
```
int *array=(int)malloc(n*sizeof(int));//分配内存地址
if(array==NULL)
{
    printf("分配失败);
    return 1;
}
```
清除内存可以是用`free`
```
free(array);
```
> 注意，使用以上函数需要引入`stdlib.h`头文件
>
- linked lists 链表，链表主要构造是每个节点（node）分配两倍的内存，一个用来存放数据，一个存放下一个数据的指针
> 对比数组能更灵活分配数组大小
> 缺点是无法精准的定位某一位数字，只能一次遍历每一个数字
>
创建链表
```
typedef struct node
{
    int number;
    struct node *next;//下一个节点的内存
}node;
```
定义链表list of size 0
```
node *list=NULL
```
在列表中加入数字
```
node *n=malloc(sizeof(node));
if(n==NULL)
{
    return 1;
}
n->number=1;
n->next=NULL;
list=n;//作为链表的开头
```
链接新加的数字
```
node *n=malloc(sizeof(node));
if(n==NULL)
{
    return 1;
}
n->number=2;
n->next=NULL;
list->next=n;
```
假如继续加节点
```
ode *n=malloc(sizeof(node));
if(n==NULL)
{
    return 1;
}
n->number=3;
n->next=NULL;
list->next->next=n;//这里是链接前面的节点，建议使用循环
```
遍历链表
```
for(node *tmp=list;tmp!=NULL;tmp=tmp->next)
{
    printf("%i",tmp->number);
}
```
释放链表list
```
while(list!=NULL)
{
    node *tmp=list->next;
    free(list);
    list=tmp;
}
```
实际上，我们可以使用循环来完成链表的构建
以下是使用循环的构建
```
#include<stdio.h>
#include<stdlib.h>
typedef struct node
{
    int date;
    struct node *next;
} node;

int main()
{
    struct node *p;
    struct node *head;//头节点
    struct node *tail;//尾节点
    p = (struct node*) malloc(sizeof(node));//给p分配内存空间
    head = p;//头节点指向p
    tail = p;//尾节点指向p
    head->next = NULL;
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n;i++)//创建链表
    {
        p = (struct node*) malloc(sizeof(node));
        scanf("%d", &p->date);
        tail->next = p;//尾节点只想了p
        tail = p;//更新尾节点
        tail->next = NULL;
    }
    //增加节点(假设数据为3)
    struct node *q;
    q = (struct node *)malloc(sizeof(node));
    q->date = 3;
    p = head;
    while(p!=NULL)
    {
        if(p->next->date>q->date)
        {
            q->next = p->next;
            p->next = q;
            break;
        }
        p = p->next;
    }
    //删除节点
    int x = 3;
    p = head;
    while(p!=NULL)
    {
        if(p->next->date==x)
        {
            struct node *tmp;
            tmp = p->next;
            p->next = p->next->next;
            free tmp;
            break;
        }
        p = p->next;
    }
}
```
## python
- 使用`for`循环
```
for i in range(3)
    print("hello world)
```
`range`表示循环的次数
也可以使用以下方式，~~但是不建议使用~~
```
for i in [0,1,2]:
    print("hello world)
```
`[]`里面的数字表示循环的数字，相对繁琐所以建议使用`range`函数

- `if`判断语句格式如下

```python
a=10
b=10
if a>b:
    print("a>b")
elif a<b:
    print("a<b")
else:
    print("a=b")
```

注意，在python中，else if可以直接写成`elif`
- while循环
  语法如下
  
  ```
  a=5
  while a<4:
      print(a)
      a-=1
  ```

