# cs50 note
## 算法 algorithms
在描述算法的效率是，可以使用*O*和Ω
*o*表示算法速度的上限，Ω表示算法速度的下限
## 数据结构  Data Structure
- array数组
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

