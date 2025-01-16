# [Study Note]cs 61A

## Functions

- Defining Functions

  ```python
  def <name> (<formal parameters>):/定义函数名字，传递参数
  return <return expresstion>//return是返回的表达式
  ```

  eg.

  ```python
  def square(x): //square二次幂
   return mul(x,x)
  ```

  1. 加入局部框架，创建一个新的框架
  2. 把这个函数的主要参数绑定去新的框架
  3. 在新环境中运行这个函数主体

- Pure Functions: *just return values*（纯函数）
  - eg.   `abs` `pow`只会返回数值

- Non-Pure Function: *have side effects* （非纯函数）
  - eg.    `print`return`none`的同时显示输出的值

> ps : in `python`中，`none`是不包含任何东西，不会输出任何东西
>
> otherwise,`print(none)`得到结果是`none`

## Control

用户定义函数步骤：

- def statement: 

```
square (x)://x:formal parameter形式参数
return mul(x,x)
```

- call expression:

```
square(2+2)//运算2+2，结果4作为函数参数
```

- applying

![image-20241031092439832](D:\Jason\assets\image-20241031092439832.png)

运算符

- `add`加法，也可以用速写`2+2`
- `mul`乘法，也可以用速写`2*2`
- `truediv`真除法，也可以用速写`2013/10`结果:`201.3`
- `floordiv`除法，也可以用速写`2013//10`结果:`201`(没有小数)

- `mod`取余，也可以用速写`2013%10`结果为:`3`

`python`可以定义返回多个值的函数

```python
def divide_exact(n,d):
   return n//d,n%d
   //quotient 商  remainder 余数
    quotient,remainder=divide_exact(2013,10)
quotient
201
remainder
3
```

