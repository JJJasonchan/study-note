# [study note]C++

## C++入门基础

### 编写简单的程序

> 在终端中输出`hello_world`

eg：

```c++
#include<iostream>
using namespace std;
int main()
{
  //输出helloworld
  cout <<"helloworld"<< endl;
  system("pause");
  return 0;
 
}
```

### 变量

> 可被修改的数据

- 语法：`数据类型 变量名=初始值`

Eg.

```c++
#include<iostream>
using namespace std;
int main()
{
  int a=10;//定义变量
  cout <<"a="<<a<<endl;
  system("pause");
  return 0;
  
}
```

### 常量

> 不可更改的数据

定义的方式：

1. `#define` 宏常量 `#define 常量名 常量值`
2. `const`修饰的变量 `const 数据类型 常量名=常量值`
   - 在定义前加const，**不可修改**

 Eg .

```c++
#include<iostream>
using namespace std;
#define day 7 //宏常量
int main()
{
    cout <<"one week have "<<day<<" days"<<endl;
    const int mouth=12;//常量
    cout <<"one year have "<<mouth<<" mouths"<<endl;
    system("pause");
    return 0;
}
```

### 关键字

> 在定义变量或常量时，不要用关键字

summarize：

![img](https://img-blog.csdn.net/20161216190705732)

### 标识符命名规则

- 标识符不能是关键字
- 标识符只能由字母，数字，下划线组成
- 第一个必须是字母或下划线（不能是数字）
- 字母区分大小写

> 尽量起名字见面名知意

## 数据类型

### 整形

> **整数**类型的数据

- `short` 短整形
  - 占用空间：2字节
- `int`   整形
  - 占用空间：4字节
- `long`  长整型
  - 占用空间：4字节（Windows/Linux 32bit）/8字节（Linux 64bit）
- `long long`  长长整形
  - 占用空间：8字节

### sizeof关键字

> 统计数据类型所占用的内存大小

语法：`sizeof(数据类型/变量)`

Eg :

````
int main()
{
  int a=10; 
  cout <<"int占用："<<sizeof(int)<<"个字节"<<endl;
}
````

> short<int<=long<=long long

### 浮点型

- `float`  单精度（有效范围：7位有效数字）
  - 占用空间：4个字节

- `double` 双精度（有效范围：15-16位有效数字）
  - 占用空间：8个字节

> 有效数字判定示例：3.14的有效数字为3

### 数据类型

> 显示单个字符

**语法**:`char name='a';`

- `char`字符占用1个字节
- 字符型变量内存中存储ASCII编码到存储单元

输出字符串对应的ASCII值：

> 常见的值：a-97；A-65；

```
#include<iostream>
using namespace std;
int main()
{
  char a='b';
  cout <<"char="<<a<<endl;
  cout <<(int)a<<endl;//输出字符串对应的ASCII值
}
```

### 转译字符

> 输出特殊的字符

![img](https://bkimg.cdn.bcebos.com/pic/3bf33a87e950352ab1edf5555043fbf2b3118bdb?x-bce-process=image/format,f_auto/watermark,image_d2F0ZXIvYmFpa2UyNzI,g_7,xp_5,yp_5,P_20/resize,m_lfit,limit_1,h_1080)

> `\t`制表符，一次占8个空格，不够就会不够空格
>
> > 用于对齐
>
> `\b`退格

### 字符串

> 用于表示字符串

语法：

1. **C语言风格字符串**：`char str[]="字符串值";`

Eg :

```c++
#include<iostream>
using namespace std;
int main()
{
    char str[]="helloworld";
    cout <<str<<endl;
}  
```

2. **C++风格字符串**:`string 变量名="字符串值"`

> 使用前要先引用`#include<string>`头文件

Eg 

```
#include<iostream>
using namespace std;
int main()
{
    string a="helloworld";
    cout <<a<<endl;
    char b[]="hello";
    cout <<b<<endl;
} 
```

### 布尔类型bool

作用：代表真或假的值

bool类型：

1. true（本质是1）
2. false（本质是0）

占用内存大小：**一个字节**

Eg 

```c++
#include<iostream>
using namespace std;
int main()
{
    bool flag=true; //true表示真
    cout<<flag<<endl; //true本质就是1
    flag=false;//false表示假
    cout<<flag<<endl; //false本质上是0
} 
```

> 非0的数都代表真

### 数据输入

> 键入数据

**语法**：`cin>>变量`

```c++
#include<iostream>
using namespace std;
int main()
{
 int a;
 cin>>a;
 cout<<a<<endl;
}
```

## 运算符

### 算数运算符

> 四则运算符的计算

四则运算符的符号：

![image-20241121094858471](/Users/jasonchan/Library/Application Support/typora-user-images/image-20241121094858471.png)

> 整数相除结果是整数，小数相除结果是小数
>
> 取余不可以两个小数运算，相除可以两个小数相除

---



`++a`和`a++`的区别：

- `++a`先自增，在进行表达式运算
- `a++`先进行表达式运算，在自增

### 赋值运算符

> 变量本身运算与赋值的符号

赋值运算符主要的符号：

![image-20241121100509817](/Users/jasonchan/Library/Application Support/typora-user-images/image-20241121100509817.png)

### 比较运算符

> 表达式之间的比较，并返回一个真假值

常见的符号：

![image-20241121100702480](/Users/jasonchan/Library/Application Support/typora-user-images/image-20241121100702480.png)

### 逻辑运算符

> 根据表达式的判断

逻辑运算符：

![image-20241121101308983](/Users/jasonchan/Library/Application Support/typora-user-images/image-20241121101308983.png)

## 程序流程结构

> - **顺序结构**：按顺序执行
> - **选择结构**：根据条件是否满足判断是否运行
> - **循环结构**：根据条件是否满足从而循环多次某段代码

### 选择结构

##### if语句

> 判断表达式是否满足然后决定是否运行程序

- 单行格式if语句`if(表达式){运行程序}；`
- 多行代码if语句`if(表达式){运行程序}else{运行程序}；`
- 多条件if语句`if(表达式){运行程序}else if(表达式){运行程序}.....else{表达式}`

> 多条件的运行：从上到下寻找符合条件的表达式，当符合条件则运行，并结束if语句

##### 三元运算符

> 判断两个表达式的关系，并输出对应的结果
>
> 返回对应的**变量**

语法：`a>b?a:b`

逻辑：

`a`与`b`判断大小，如果成立，输出`a`，如果不成立，则输出`b`

特殊：在c++中三元运算符返回的是**变量**，可以继续赋值

```c++
int a=10;
int b=20;
(a>b?a:b)=100;//此时返回的是b变量，然后给b变量赋值100
```

#### switch语句

> 执行多条件的分支语句

语法：

```c++
switch(表达式)
{
  case 结果1:
    执行语句;
      break;
    case 结果2:
    执行语句;
      break;
    .......
      default:
    执行语句;
    break;
}
```

> 将表达式与下面的结果进行匹配，然后运行其中的执行语句，遇到`break`结束。如果都没有匹配条件；则运行`default`中的执行语句。
>
> 特别的: 当其中一个执行语句之后没有`break`，***要继续运行之后条件的执行语句直到遇到`break`才结束***。

### 循环

#### for循环

- 详情参考[study note]c.md

#### while循环

- 详情参考[study note]c.md

#### do-while循环

- 详情参考[study note]c.md

## 数组

#### 一维数组

**定义**：

- `数据类型 name[len]={a1,a2....};`

**获取数组的值**

`int 变量=arr[索引];`//索引从0开始，小于`len`。

**元素个数**

`int len=sizeof(arr)/sizeof(数据类型);`

**长度**

`sizeof(arr)`

```c++
#include<iostream>
using namespace std;
int main()
{
  int arr[]={1,2,3,4,5};
  cout<<sizeof(arr)/sizeof(int)<<endl;//len的大小
  cout<<sizeof(arr)<<endl;//整个数组内存长度
  cout<<arr<<endl;//打印首地址
} 
```

#### 冒泡排序

> 原理：比较相邻两个元素，如果第一个比第二个大，就进行交换。对每一对相邻元素进行同样的工作，**执行完毕后找到最大值**。然后重复以上步骤，**每一次比较数减1**，直到不需要比较。

Eg.

```c++
#include<iostream>
using namespace std;
int main()
{
  int arr[]={4,2,8,0,5,7,1,3,9};
  int len=sizeof(arr)/sizeof(int);
  //进行冒泡排序
  for(int i=0;i<len-1;i++)
  {
    for(int j=0;j<len-1-i;j++)//每次比较少一次
    {
      if(arr[j]>arr[j+1])
      {
         int temp=arr[j];
         arr[j]=arr[j+1];
         arr[j+1]=temp;
      }
    }
  }
  for(int i=0;i<len;i++)
  {
   cout<<arr[i]<<endl;
  }
} 
```

#### 二维数组

> 定义二维数组：`int arr[行数][列数]={{数组1},{数组2},...};`

名称用途：

- 查看占用内存大小
- 查看二维数组占用的空间大小

## 函数

> 模块化的程序，避免重复

**定义函数**:

```c++
返回值类型 函数名(参数1，参数2...)
{
 函数体语句；
 return语句；
}
```

**函数调用**：

具体参考c语言

**值传递**：

- 形参发生改变，实参不会改变
- 值传递传递实参的值

函数的具体知识点与c语言类似

## 指针

> 可以通过指针访问内存

 
