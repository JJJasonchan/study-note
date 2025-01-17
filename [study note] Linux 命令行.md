# [study note] Linux 命令行

## ls用法

- `ls`是**列出**文件或者目录

Eg .

````
jasonchan@MacBook-Pro ~ % ls
Applications			Movies
Desktop				Music
Documents			Pictures
Downloads			Public
Google Drive			Sites
Library				iCloud雲碟（封存）
````

- `ls -l`列出每个文件的属性

````
jasonchan@MacBook-Pro ~ % ls -l
total 0
drwxr-xr-x@  7 jasonchan  staff   224 Jan 13 23:56 Applications
drwx------+ 17 jasonchan  staff   544 Jan 15 00:19 Desktop
drwx------@  4 jasonchan  staff   128 Dec 30 22:37 Documents
drwx------@ 37 jasonchan  staff  1184 Jan 13 23:56 Downloads
````

在这个列表中，`drwxr`中的`d`表示这个是个目录，`-`则是文件；`rwx`表示权限。 

- `ls -a`是列出隐藏的文件

````
jasonchan@MacBook-Pro ~ % ls -a
.				Desktop
..				Documents
.CFUserTextEncoding		Downloads
.DS_Store			Google Drive
.Trash				Library
.hidpi-disable			Movies
````

以`·`开头的文件是隐藏文件

- `cd**`**进入文件/切换目录**. 用法：`cd 文件名`

```
jasonchan@MacBook-Pro ~ % cd desktop
jasonchan@MacBook-Pro desktop % 
```

`cd ..**`回到上级目录**。当然，也可以套用两层（返回两层目录）:`cd ../..`

- `pwd`**打印当前绝对路径**

````
jasonchan@MacBook-Pro music % pwd
/Users/jasonchan/music
````

- `cat`**读取文件** `cat 文件名`即可打印文件名
  tips：当文件名与其他文件不尽相同，可以通过输入关键词+TAP键补全

- ⬆和⬇可以查看历史命令

- `head`**查看文件开头**

  ```
  jasonchan@MacBook-Pro study note % head head C++核心.md
  head: head: No such file or directory
  ==> C++核心.md <==
  [TOC]
  
  # [study note]C++核心
  
  ## 内存分区模型
  
  > C++程序在运行的时候内存划分为**==四个区域==**
  
  - 代码区:存放二进制代码,由操作系统进行管理
  - 全局区:存放*全局变量*和*静态变量*以及*常量*
  ```

  若只查看开头的某n行，可以输入`head --line=n 文件名`

  eg 我要查看文件前4行

  ```
  jasonchan@MacBook-Pro study note %  head --line=4 C++核心.md
  
  [TOC]
  
  # [study note]C++核心 
  ```

- `tail`查看文件尾部，逻辑与`head`类似

- `less`可以通过键盘⬆️⬇️键读取文件，`q`键结束读取
  `more`相似

- `nano 文件`即可编辑文件了

- `file`查看文件属性,说明该文件是什么类型的文件 

  ```
  jasonchan@MacBook-Pro study note % file C++核心.md 
  C++核心.md: C++ source text, Unicode text, UTF-8 text
  ```

- `where`可以查找文件的位置
- `echo`打印

```
echo hello\ world
hello world
```

- 定义变量：`名称=内容`
  ```
  % h="hello"
  jasonchan@MacBook-Pro ~ % echo $h
  hello
  jasonchan@MacBook-Pro ~ % echo haha${h}ahah
  hahahelloahah
  jasonchan@MacBook-Pro ~ % echo haha-${h}=300
  haha-hello=300
  ```

  可以随时插入在任意地方，若要使用变量要在前+`$`符号

- `mv`修改文件名称，可以就该文件名称