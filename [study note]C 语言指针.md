[toc]

# [study note]C 语言指针

> 指针是一个内存地址,指针变量是存放指针的变量
>
> 指针在32位环境下,占四个字节

## 指针基本

基本语法:`指针类型 指针名称=地址`

获取地址可以使用取地址符`&`,如`&a`可以获取变量a的地址,`int* p=&a`

指针p指向了变量a的地址.当需要操作指针指向的地址对应的东西,需要**解引用**`*p`

```c
//指针类型实例:
int* pa;
char* pb;
double* pc;
```

指针类型作用:

1. 决定了指针在解引用的时候可以访问多少个字节,如`int* `可以访问4个字节.
2. 指针类型决定指针进行`p+-1`的步长,`int* `步长为4,`char*`步长为1.

## 野指针

> 野指针:指针指向的空间是不可控的(随机的,不正确的,没有明确限制的)

造成的野指针的原因:

- 指针未初始化

  ```c
  #include<stdio.h>
  int main()
  {
      int *p;//没有初始化指针,默认随机值
      *p = 10;
      return 0;
  }
  ```

- 指针越界访问

  ```c  
  #include<stdio.h>
  int main()
  {
      int arr[10] = {0};
      int *p = arr;
      for (int i = 0; i <= 10;i++)
      {
          *p = i;
          p++;//循环了11次,超出了数组的范围,造成越界访问,指针成为野指针
      }
      return 0;
  }
  ```

- 指针指向的空间被释放

---

**避免野指针:**

在不知道给指针指向何处可以先赋空指针`int* p=NULL`,这里的`NULL`表示0.

```c
int* p=NULL;
if(p!=NULL)//当要使用空指针可以使用当前形式
{
  *p=100;
}
```

注意:不能直接赋值

```c
int* p=NULL;
*p=100;//错误!!!!!
```

## 指针运算

- 指针+/-整数,指针向后移动指针类型所对应的步长

```c
int arr[10]={0};
int* p=arr;
int len=sizeof(arr)/sizeof(int);
for(int i=0;i<len;i++)
{
    *p+i=10;//指针在每次循环之后向后移动int的步长
}//输出:数组都赋值10
```

- 指针-指针

> 指向同一块内存空间才能相减        

指针相减:得到得到指针之间元素的个数

```
int main()
{
  int arr[10]={0};
  int* p=arr;
  printf("%d",&arr[9]-&arr[0]);//输出结果:9
}
```

- 指针的关系运算

### 指针数组

可以通过指针访问数组

> 数组名表示首元素地址
>
> arr[i]=*(arr+i)

```c
int main()
{
    int arr[10] = {0};
    int *p = arr;//指针指向数组,指向数组的首元素地址
    int len = sizeof(arr) / sizeof(int);
    for (int i = 0; i < len; i++)
    {
      //printf("%d",*(arr+i));结果相同,因为数组名表示元素首地址
        printf("%d", *(p + i));//通过指针访问数组
    }
}
```

### 二级指针

> 存放一级指针的地址,指向指针的指针
>
> 语法:`int ** name`,`int*`是指针类型,第二个`*`表明其是个指针

```c
int main()
{
    int a = 10;
    int *pa = &a;//一级指针变量,指向a变量
    int **ppa = &pa;//二级指针变量,指向pa指针
    **ppa = 20;//解引用两次,一次解出来的是一级指针
    printf("%d", **ppa);
}
```

### 指针数组

> 指针数组是存放数组的**地址**

```c
int a[10]={0};
int (*p)[10]=&a;//&a获取数组地址
```

>  温习:整形指针存放的是整形数据地址···

`int(*p)[10]=&arr`指针数组中,`int* []`是指针类型

Eg :

```c
int main()
{
    int a = 10;
    int b = 20;
    int c = 30;
    int (*parr)[3] = {&a, &b, &c};//指针数组,存放指针变量
    for (int i = 0; i < 3;i++)
    {
        printf("%d ", *(parr[i]));//输出:10 20 30
    }
}
```

#### 用指针表示二维数组

> 数组名是首元素的首地址
>
> 二维数组的数组名是第一个一维数组的首地址 
>
> 步长是一个元素的长度,二维数组是一个一维数组的长度

语法:`int (*p)[n]`

```c
 int main()
{
    int arr[3][4] =
     {
        {2, 4, 8, 4},
        {3, 6, 3, 7},
        {9, 5, 4, 7}
    };
    int(*p)[4] = arr;
    for (int i = 0; i < 3;i++)
    {
        for (int j = 0; j < 4;j++)
        {
            printf("%d ", p[i][j]);
        }
        printf("\n");
    }
}
```

当然也可以用以下方式解引用指针    (~~当然你乐意的话~~)

```c
for (int i = 0; i < 3;i++)
{
        for (int j = 0; j < 4;j++)
        {
            printf("%d ", *(*p+i)+j);
        }
        printf("\n");
 }
```

区分两种用法：

```c
*(*p + 1) = 10000;  // 修改 arr[0][1] 的值为 10000
// 修改 arr[1][0] 的值
    *(*(p + 1) + 0) = 9999;
```

