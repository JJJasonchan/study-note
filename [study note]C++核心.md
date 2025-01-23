[TOC]

# [study note]C++核心

## 内存分区模型

> C++程序在运行的时候内存划分为**==四个区域==**

- 代码区:存放二进制代码,由操作系统进行管理
- 全局区:存放*全局变量*和*静态变量*以及*常量*
- 栈区:*编译器*自动分配释放,存放函数的参数值,局部变量等
- 堆区:*程序员*分配释放,程序结束由操作系统回收

### 运行前

> 程序编译后,生成了`.exe`可执行程序,为*执行程序前*分为两个区域

**代码区**:

- **共享**:对于频繁被执行的 程序只需要一份代码即可
- **只读**:防止程序被意外修改

**全局区**:

> ==在程序结束后释放==

- 全局区
  - 全局变量
  - 静态变量(`static`修饰的变量)
  - 字符串常量
  - `const`修饰的全局变量(全局常量)

- 不在全局区
  - 局部变量
  - `const`修饰的局部变量

> 局部变量:在函数中定义的变量(包括在`main`函数中的变量)[栈区]
>
> 全局变量:不在函数中的变量[全局区]
>
> 静态变量:在局部变量前加`static`[全局区]
>
> 常量: 
>
> - 字符串常量 `“helloworld”`
> - 全局常量(在全局变量前加`const`)
> - 局部常量(在局部变量前面加`const`)
>   - 与局部变量放在一起

### 程序运行后

#### 栈区

- *编译器*自动分配释放,存放函数的参数值,局部变量等
- 注意:不要返回 局部变量的地址(函数执行完后内存被释放了)

#### 堆区

> 程序员*分配释放,程序结束由操作系统回收

- C++中利用[`new`](####new操作符)在堆区开辟内存

在堆区开辟数据

```c++
#include<iostream>
using namespace std;
int* function1()
{
    //在堆中开辟出空间存放数据
    //new返回一个地址
    int *p = new int(20);//p指针本质上也是局部变量,放在栈上.指针保存的数据放在堆区
    return p;
}
int main()
{
    //在堆区存放的数据
    int *p = function1();
    cout << *p << endl;
}
```

#### new操作符

> C++中利用`new`操作符在**堆区开辟内存空间**
>
> 堆区开辟的数据手动释放使用操作符`delete`

语法:`new数据类型(数值)`

利用`new`开辟的数据会*返回该数据的指针*

在堆区开辟完**返回指针地址**

```c++
int *p=new int(10);
```

释放堆区内存

```c++
delect p;
```

Eg 

```c++
#include<iostream>
using namespace std;
int* func()
{
    //在堆区创建整型数据类型,返回指针地址
    int *p = new int(10);
    return p;
}
int main()
{
    //读取堆区数据
    int *p = func();
    cout << *p << endl;
    //释放堆区数据
    delete p;
}
```

在堆区创建一个数组空间

Eg 

```c++
int* func()
{
    //在堆区中创建一个数组
    int *arr=new int[10];
    //给数组赋值
    for (int i = 0; i < 10;i++)
    {
        arr[i] = i;
    }
    return arr;
}
int main()
{
    int *arr = func();
    //打印堆区数组
    for (int i = 0; i < 10; i++)
    {
        cout << arr[i] << endl;
    }
    //释放数组时候要使用[]
     delete[] arr;
}
```

## 引用

> 指针的本质是C++内部实现了一个指针常量

### 引用基本

> 引用:给变量起别名

**语法**:数据类型 &别名=原名

```c++
int main()
{
    int a = 10;
    //给a起别名b
    //创建引用
    int &b = a;
    b = 20;
    cout << a << endl;
}
```

**注意**:

- 引用必须初始化(`int &b`错误的)
- 引用在初始化后不可以改变

### 引用做函数参数

> **作用**:函数传递参数的时候,可以使用引用传递实参
>
> ·相当于传递指针·

Eg :

```c++
void exchange(int &a,int &b)//引用传递,变量本身被传递过去
{
    int temp = a;
    a = b;
    b = temp;
}
int main()
{
    int a = 10;
    int b = 20;
    exchange(a, b);
    cout << "a=" << a << endl;
    cout << "b=" << b << endl;
}
```

### 引用做函数返回值

1. 不能返回局部变量的引用

   以下函数返回引用方式**有误**

   ```c++
   int& func()
   {
       int a = 10;//变量放在堆区,在函数运行完毕后被释放
       return a;
   }
   int main()
   {
       int res = func;
       cout << "res=" << res << endl;
   }
   ```

   **正确**返回引用方式:

   ```c++
   int& func()
   {
       static int a = 10;//全局变量,在程序结束后才释放
       return a;
   }
   int main()
   {
       int &res = func();
       cout << "res=" << res << endl;
   }
   ```

2. 函数**返回的是引用**,函数的调用可以==作为左值==,可以直接操作函数中返回的变量.

```c++
#include<iostream>
using namespace std;
int& func()
{
    static int a = 10;//全局变量,在程序结束后才释放
    return a;
}
int main()
{
    int &res = func();
    cout << "res=" << res << endl;
    func() = 2000;//如果函数返回引用,函数调用可以作为左值,可以直接操作返回的变量值
    cout << "res=" << res << endl;
}
```

3. 常量引用

> 用`const`修饰形参,防止修改形参误操作实参

- `const int &a=10`常量引用,加入`const`不能修改变量

- 在形参中加入`const`使实参不发生改变,防止误操作(只读不写)

```c++
void print(const int &a)
{
    //形参被const修饰,函数中不能修改a,防止误操作
    cout << a << endl;
}
int main()
{
    int a = 20;
    print(a);
}
```

- 可以使用`const int &a=10`进行赋值

## 函数plus

### 函数默认参数

> 语法:`返回值类型 函数名 (参数=默认值){}`

注意:

- 函数形参从某个位置开始有了默认参数,则之后的形参都必须有默认值`int func(int a,int b=10,int c=20)`
- 当实参传入值,函数**使用实参传入的值**,不使用默认值
- 函数声明和函数实现只能有一个有默认参数

函数默认参数

```c++
int pl (int a,int b=10,int c=100)
{
    return (a + b + c);
}
int main()
{
    int a = 20;
    int b = 2;
    cout << pl(a) << endl;//结果130
    cout << pl(a, b) << endl;//结果122,函数使用了传入的形参,而非形参默认值
}
```

### 函数占位参数

> 形参列表可以有占位参数,用作占位,调用时必须填补该位置

语法:`返回值类型 函数名(数据类型){}`

```c++
#include<iostream>
using namespace std;
void func(int a,int)//函数占位参数
{
    cout << "a=" << a << endl;
}
int main()
{
    //占位符要传入数据
    func(20, 100);
}
```

- 在主函数调用函数时需要传入占位参数的实参

```c++
 void func(int a,int);
func(30);//不传入占位参数语法错误
func(20, 100);//正确的方式
```

- 占位参数也能有默认值

```c++
void func(int a,int=10);
func(20);//函数调用
```

### 函数重载

> 函数名相同,提高复用性

满足条件:

- 同一个作用域
- 函数名称相同
- 函数**参数**不同,或者**个数**不同,**顺序**不同,*返回类型不能区分*相同函数名的函数

> ~~人话:**参数**不同就可以区分相同名字的函数~~  

注意:当函数重载**存在默认值时会出现二义性**,尽量避免.

## 类和对象

> C++面向对象特征:封装,继承,多态
>
> 对象上有其对应的==**属性**==和==**行为**==
>
> 具有*相同性质的对象*可以抽象的称之为**类**

### 封装

> 三大特性之一

封装的意义:

- 将属性和行为作为一个整体
- 将属性和行为加以权限控制

语法:`class 类名{访问权限:属性/行为};`

类中的属性和成员称为**成员**

属性也称为成员属性;成员变量. 

行为也称为成员函数;成员方法

```c++
#include<iostream>
using namespace std;
#define PI 3.14
// 创建一个类,圆类
class circle
{
    //权限
    //公共权限
    public:
    //属性
        int r;
        //行为
        double calulate()
        {
            return 2 * PI * r;
        }
};
int main()
{
    //通过圆类,创建一个圆
    circle c1;
    c1.r = 10;
    cout << "圆的半径为:" << c1.r << endl;
    cout << "周长为: " << c1.calulate() << endl;
}
```

在类设计时候,可以把属性放在不同的权限下加以控制.访问权限有以下三种:

1. `public` 公共权限    
   - 成员在类内可以访问,类外可以访问
2. `protected`保护权限  
   - 类内可以访问,类外不可以访问
3. `private`私有权限
   - 类内可以访问,类外不可以访问

`struct`和`class`的区别在C++中是**默认的访问权限不同**

- `struct`默认权限是**公共**
- `class`默认权限是**私有**

### 构造函数和析构函数

> 对象的**初始化**和**清理**也是非常重要的事情,一个对象或者变量没有进行初始化,对其使用后果是未知的,同样变量没有及时清理会造成安全问题

C++中利用了**构造函数**和**析构函数**解决上述问题,两个函数被编译器*自动调用*,如果**程序员不提供构造和析构函数,编译器提供的构造和析构函数是空实现**

- 构造函数:作用在创建对象时为对象成员属性赋值
- 析构函数:作用在于对象**销毁前**系统自动调用,执行清理作用

#### 构造函数

> 对象初始化

语法:`类名(){}`

- 没有返回值也不写`void`
- 函数名与类名相同
- 构造函数可以有参数,可以发生重载
- 程序在调用对象的时候会自动调用构造,而且只会调用一次

构造函数分类:

- 按参数分为:有参构造和无参构造
- 按类型分为:普通构造和拷贝构造

有参构造函数:

```c++
person(int a)
{
  age=a;
}
```

拷贝构造函数:把传入的对象属性拷贝到自身上

```c++
//拷贝构造函数
    person(const person &p)
    {
        //将传入对象的属性拷贝过来
        age = p.age;
    }
```

##### 调用方式:

- 括号法

  调用默认构造函数时,不要加`()`,`person p1()`编译器会认为是一个函数声明

  ```c++
  person p1;//默认构造函数调用
  person p2(10);//有参构造函数调用
  person p3(p2);//拷贝构造函数调用 
  ```

- 显示法

  ```c++
  person p1;
  person p2=person(10);//有参构造
  person p3=person(p2);//拷贝构造
  ```

  - `person(10)`是匿名对象,当前执行结束后,系统会立刻回收匿名对象
  -  不要用拷贝构造函数初始化匿名对象,编译器会认为`person (p3)==person p3;`

- 隐式转换法

  ```c++
  person p4=10;//相当于person p4=person(10);
  ```

 拷贝构造函数调用的时机:

1. 使用一个已经创建完毕的对象来初始化一个新对象

   ```c++
   person p1(20);
   person p2(p1);
   ```

2. 值传递的方式给函数参数传值

   ```c++
   void dowork(person p)
   {
    
   }
   void tect()
   {
     person p1;
     dowork(p1);//在值传递的过程中使用了拷贝构造函数
   }
   ```

3. 值方式返回局部对象

   ```c++
   void dowork()
   {
     person p1;//栈上变量,函数结束时会被释放
     return p1;//在释放前拷贝一份数据,使用拷贝构造函数,进行值返回
   }
   void tect()
   {
     person p=dowork();//使用返回值进行初始化
   }
   ```


##### 构造函数调用规则

默认情况下,编译器在编写类会添加3个函数

> 至少会提供一个函数

- 默认构造函数(无参,函数体为空)
- 默认析构函数(无参,函数体为空)
- 默认拷贝构造函数,对属性进行值拷贝

构造函数调用规则如下:

- 如果用户定义有参构造函数,c++不会提供无参构造,但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数,,*c++不会再提供其他构造函数*(包括有参构造和无参构造函数)

#### 析构函数

语法:`~类名(){}`

- 没有返回值也不写`void`
- 函数名与类名相同,在名称前加`~`
- 析构函数**不能**有参数,因此不能发生重载
- 程序在**对象销毁前**的时候会自动调用构造,而且只会调用一次

#### 深拷贝和浅拷贝

> 浅拷贝:简单的赋值拷贝操作
>
> 深拷贝:在堆区重新申请空间,进行拷贝操作

浅拷贝存在问题:当使用析构函数的时候容易**对同一块堆区内存重复释放**,造成违法操作.

编译器提供的构造函数是浅拷贝,在赋值指针类型时容易出现上述浅拷贝的问题

为解决上述浅拷贝,使用深拷贝方法,**重新申请堆区空间进行拷贝操作(深拷贝)**

> 堆区开辟内存关键字`new 数据类型()`
>
> 清空堆区数据使用`deleter`

以下是使用深拷贝的拷贝构造函数

```c++
//拷贝构造函数
    person(const person &p1)
    {
         age = p1.age;
        heighet = new int(*p1.heighet);//深拷贝,重新在堆区开辟一片内存存放拷贝的数据
    }
```

如果不写拷贝构造函数,编译器会自动生成浅拷贝构造函数,直接赋值拷贝操作

```c++
//编译器生成的浅拷贝函数
person(const person &p1)
{
  height=p1.heighet;//直接赋值拷贝
}
```

### 初始化列表

作用：C++提供的初始化列表的语法，用来初始化属性

语法：`构造函数():属性1(值1),属性2(值2),属性3(值3)…..{}`

Eg :

```c++
#include<iostream>
using namespace std;
class person
{
    public:
        int a;
        int b;
        int c;
        person(int p1,int p2,int p3):a(p1),b(p2),c(p3)
        {

        }
};
int main()
{
    person p(100, 2, 3);
    cout << p.a << endl;
    cout << p.b << endl;
    cout << p.c << endl;
}
```

### 类对象作为类成员

在C++中，类的成员可以是另一个类的对象（类的嵌套）

Eg

```c++
#include<iostream>
#include<string>
using namespace std;
class phone
{
    public:
    phone(string name):PhoneName(name)
    {

    }
    string PhoneName;
};
class person
{
    public:
    person(string p_name,string phonename):name(p_name),ph(phonename)//注意这里构造函数中传入的数据要与嵌套的类的成员数据类型相匹配
    {
       
    }
    string name;
    phone ph;
};

int main()
{
    person p("jason", "iphone");
    cout << p.name <<" use "<<p.ph.PhoneName <<endl;
}
```

需要注意的是，当构造函数传入的数据类型要和嵌套的类中成员的数据类型匹配，如这里的`phone`的`PhoneName`数据类型是字符串，那么当`person`的构造函数传入的数据就要和`PhoneName`相匹配.

当其他类对象作为本类的成员时，构造时候**先构造类对象**，再构造自身。**构造与析构顺序相反**

### 静态成员

> 静态成员是在成员变量和成员函数前面加上`static`关键字

- **静态成员变量**特点：
  - 所有成员共享同一份数据
  - 编译阶段分配内存
  - 类内声明，类外初始化

Eg 

```c++
class person
{
    public:
        static int m_a;//类内声明
};
int person::m_a = 10;//此处是类外初始化
```

静态成员变量有两种访问方式：

- 通过对象进行访问

```c++
person p;
cout << p.m_a << endl;
```

- 通过类名进行访问

```c++
cout << person::m_a << endl;
```

所有成员共享同一份数据

```c++
person p2;
    p2.m_a = 20;
    cout << p2.m_a << endl;//输出：20
    cout << p.m_a << endl;//输出：20
```

静态成员变量也可以划分权限

```c++
 private:
        static int m_b;//私有权限外部访问不到
```

- **静态成员函数**的特点
  1. 程序共享一个函数
  2. 静态成员只能访问静态成员变量，不能访问局部成员变量
  3. 静态成员变量是可以设置访问权限的

```c++
class person
{
    public:
        static int m_a;
        static void fuc()//静态全局变量
        {
            m_a = 20000;
            cout << "静态成员函数调用" << endl;
            cout << m_a << endl;
        }
};
```

访问静态成员变量的两种方式：

- 通过对象访问

```c++
person p;
    p.fuc();
```

- 通过类名访问

```c++
person::fuc();
```

  ### 类的成员变量和成员函数的存储

在C++中，类内的成员变量和成员函数是分开存储的

1. **非静态成员变量**储存在类内
2. 静态成员变量/静态成员函数/非静态成员函数都是存储在类外 

#### this指针使用

> C++对象中成员变量与函数是分开储存的，所有对象共用一个成员函数

C++提供特殊的对象指针–`this`指针，通过this指针解决调用函数指向问题，**`this`指针指向被调用的成员函数所属的对象**（谁调用函数就指向谁）`this`指针是隐含每一个非静态成员函数中的一种指针，不需要定义。

> `this`指针本质上是指针常量（`person* const this`),其指向是不能被修改的

应用application：

1. 当函数形参和成员变量相同时，可以用this指针区分

   ```c++
   class person
   {
       public:
           
           person(int age)
           {
               //解决同名变量的问题
               this->age = age;//这里指针指回了调用函数的对象
           }
           int age;
   };
   ```

   当然，推荐成员变量命名遵循`m_xx`的规则。避免出现变量名重复。

2. 在类的非静态成员函数中返回对象本身，可以使用`return *this`

   ```c++
   class person
   {
       public:
           
           person(int age)
           {
               //解决同名变量的问题
               m_age = age;
           }
           int m_age;
           person& Addage(person &p)//通过引用的方式传入对象本身
           {
               m_age += p.m_age;
               return *this;//返回对象本身
           }
   };
   ```

   在C++中，成员函数可以进行链式编程

   ```c++
   p.Addage(p2).Addage(p2).Addage(p2).Addage(p2);
   ```

   这里因为函数返回了对象本身，使用可以重复调用函数。
   
   - 空指针可以访问成员函数。但是当函数调用了成员属性，那么要在函数前面加上判断指针是否为空指针，防止程序崩溃。
     ```c++
      if(this==NULL)
     {
          return 1;
     }
     ```

### const修饰的成员函数（常函数）

在C++中，const表示只读状态，不能被修改。在成员函数后添加`const`修饰，**成员变量的值是不能被修改的。**

```c++
class person
{
    public:
        int m_a;
        person(int a)
        {
            m_a = a;
        }
        void func()const
        {
            m_a = 100//成员变量不能被修改 
        }
};
```

成员函数后面加上`const`,修饰的是`this`的指向，让`this`指向的值不能修改

```c++
this==const person* const this
```

那么如果想要修改成员变量，需要在要修改的成员变量前面加上` mutable`

```c++
 mutable int m_a;
```

### 常对象

> 在对象前面加上`const`变成常对象。常对象不允许修改成员变量

```c++
const person p;
```

常对象可以修改带有`mutable`修饰的成员变量

常对象只能调用常函数，但是不能访问普通的函数

### 友元

> 只允许特定的函数或类访问另一个类的私有成–友元

友元的关键字：`friend`

实现友元的三个方式

1. 全局函数做友元
2. 类做友元
3. 成员函数做友元

---

- 全局函数作友元
  把全局函数在类内声明，并加上`friend`关键字即可

```c++
class building
{
    //通过友元全局函数能访问对象的私有属性
    friend void func(building *Myhome);

public:
    string LivingRoom;
    building()
    {
        LivingRoom = "客厅";
        BedRoom = "房间";
        }

    private:
        string BedRoom;//私有属性
};
//全局函数访问私有权限的属性
void func(building *Myhome)
{
    cout << "现在访问的是： " << Myhome->BedRoom<< endl;
}
```

- 类作为友元
  类声明在另一个类并加上`friend`

```c++
lass building
{
    friend class Goodfriend;//通过友元使类可以访问类的隐私权限

public:
    string LivingRoom;
    building()
    {
        LivingRoom = "客厅";
        BedRoom = "房间";
    }

    private:
        string BedRoom;
};
class Goodfriend
{
    public: 
    Goodfriend(string name)
    {
        m_name = name;
    }
    string m_name;
   
    void visit(building *building)
    {
        cout << "good friend is visiting " << building->BedRoom << endl;
    }
};
```

- 成员函数做友元

```c++
// 将 Goodfriend::visit 声明为友元函数
    friend void Goodfriend::visit(building *bd);
```

### 运算符重载

> C++中定义了新的对象，拓展运算符的使用范围，而不局限于编译器原有的操作，可以使用运算符重载。可以拓宽运算符的操作范围

运算符重载可以函数重载（即可以同时支持多种数据类型相加的情况）

#### 加号运算符重载

运算符重载可以有两种方式，一个是全局函数实现运算符重载，一个是成员函数的方式

1. 全局函数实现运算符重载
   ```c++
   class person;
   person operator+(person &p1, person &p2)
   {
       person temp;
       temp.m_a = p1.m_a + p2.m_a;
       temp.m_b = p1.m_b + p2.m_b;
       return temp;
   }
   ```

   重载后就可以实现对象相加`person p3=p1+p2`.本质为：`person p3=operator+(p1,p2)`

2. 成员函数实现运算符重载
   ```c++
   class person
   {
       public:
           int m_a;
           int m_b;
           
           成员加号运算符重载
           person operator+(person &p)
           {
               person temp;
               temp.m_a = this->m_a + p.m_a;
               temp.m_b = this->m_b + p.m_b;
               return temp;
           }
   };
   ```

   重载后就可以实现对象相加`person p3=p1+p2`.本质为：`person p3=p1.operator+(&p2)`

#### 左移运算符重载<<

   左移运算符一般是用全局函数重载,确保`cout`在左边

```c++
//对左移运算符进行重载
  ostream& operator<<(ostream &cout, const person &p)//引用cout和结构体
  {
      cout << p.m_a << ", " << p.m_b << endl;
      return cout;
  }
```

`cout`的数据类型是`ostream`，函数需要返回`cout`使得输出可以连续链接。一般类的属性是`private`,需要在类内使用友元该全局函数重载运算符。

#### **自增（减）运算符(成员函数重载)**

- 前置运算符,直接对数据本身加减
  ```c++
  myint& operator++()//前置递增
      {
          ++ m_a;
          return *this;//返回数据本身
      }
  ```

- 后置运算符，先返回原本的值在元算
  ```c++
  //后置递增
      myint operator++(int)//后置递增
      {
          //先记录原本数据
          myint temp = *this;
          //再递增
          this->m_a++;
          return temp;//返回数据原本的值就可以了
      }
  ```

  `myint operator++(int)`通过`int`占位参数区分前置/后置占位符。

  #### 赋值运算符重载
  
  C++编译器会默认给类添加4个函数
  
  1. 默认构造函数（无参，函数体为空）
  2. 默认析构函数（无参，函数体为空）
  3. 默认拷贝构造函数，对属性进行值拷贝
  4. 赋值运算符`operator=`对属性进行值拷贝（浅拷贝）

当类中属性指向堆区，赋值操作会出现[深浅拷贝](#深拷贝和浅拷贝)的问题

要解决深浅拷贝的问题，可以对赋值运算符进行重载

```C++
class person
{
    public:
        int *m_age;
    person(int age)//构造函数
    {
        m_age = new int(age);//在堆区开辟一块内存
    }
    ~person()//析构函数
    {
        if(m_age!=NULL)
        {
            delete m_age;
        }
    }
    person& operator=(person &p)
    {
        if(m_age==NULL)
        {
            delete m_age;
            m_age = NULL;
        }
        //要用深拷贝
        m_age = new int(*p.m_age);//在堆区重新开辟一个区域
        return *this;
    }
};
```

#### 关系运算符重载

重载关系元算符，判断对象之间的关系

```C++
bool operator==(person &p)
        {
            if(this->m_age==p.m_age&&this->m_name==p.m_name)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
bool operator!=(person &p)
        {
            if (this->m_age == p.m_age && this->m_name == p.m_name)
            {
                return false;
            }
            else
            {
                return true;
            }
        }
```

#### 函数调用运算符()

函数调用运算符重载后调用方式很像函数调用，因此被称为**仿函数**。仿函数没有固定的写法。`类名()`

> 函数调用重载可以简单理解是调用函数的快捷指令，可以在（）内填入形参（如果有）

eg

- 打印类型：

```C++
void operator()()
        {
            cout << this->m_age << endl;
            cout << this->m_name << endl;
        }
```

调用函数时

```C++
person p;
p();
```

- 计算类型：

```C++
int operator()(int num1,int num2)
        {
            return num1 + num2;
        }
```

调用函数时

```C++
int res=p1(10, 21);
```

> 当看见`类型()()`匿名函数对象，`类型()`时匿名对象，使用后自动销毁

### 继承

> 面向对象的三大特性之一

当定义了一个类之后，与下一级的类有共同特性，就可以使用继承减少代码的重复性（比如猫类可以细分各种品种的猫）

继承的语法： `class 子类：继承方式 父类`

> 子类也叫派生类；父类也叫基类

```C++
class BacePage//基类
{
    public:
        void homepage()
        {
            cout << "主页，登陆，注册" << endl;
        }
};
//派生类
class CPP:public BacePage//继承基类
{
    public:
    void CPP_Page()
    {
        cout << "CPP's note" << endl;
    }
};
int main()
{
    BacePage BP;
    CPP cpp;
    cpp.homepage();
    cpp.CPP_Page();
}
```

 派生类的成员包含两部分

1. 继承基类的成员
2. 自己增加的成员

#### 继承方式

> `class 派生类：继承方式 基类`

继承方式：`public公共   protected保护   private私有 `，基类的private权限的成员**一律访问不到**

派生类继承了基类的成员后，存储在派生类的内存中。基类的私有成员也会一并存储到派生类，但是不能被访问。

- public公共权限
  基类原本是权限设置完整继承下来（该是公共的是公共，该是保护的是保护）
  基类

  ```c++
  class father
  {
      public:
          int m_a;
      protected:
          int m_b;
      private:
          int m_c;
  };
  ```

  派生类

  ```c++
  class son:public father
  {
      public:
          int m_a;
  
      protected:
          int m_b;
      //m_c无法访问
  };
  ```

  

- protected保护
  基类公共权限和保护权限下的成员都继承为派生类的保护(protected)权限

  ```c++
  class son:protected father
  {
      protected:
          int m_a;
          int m_b;
          // m_c无法访问
  };
  ```

- private私有
  基类公共权限和保护权限下的成员都继承为派生类的私有(private)权限

  ```c++
  class son:protected father
  {
      private:
          int m_a;
          int m_b;
          // m_c无法访问
  };
  ```

#### 继承中析构函数和构造函数

- 构造函数
  基类的先构造函数，然后再调用派生类的构造函数
- 析构函数
  派生类先析构，然后基类再析构



