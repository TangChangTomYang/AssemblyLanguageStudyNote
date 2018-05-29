##一、Makefile 项目管理工具

- **1、Makefile 的用途**<br><br> **.项目代码编译管理**<br>我们在集成项目管理工具(Xcode\VS)中经常会使用 Build 命令，集成工具自动的为我们已经编写好了Makefile工具，但是在像linux、unix 环境下没有集成环境时就需要我们自己手动编写Makefile来管理项目（将.c 文件 编译成可执行文件，即 .c -> .o -> exec）<br><br>**.节省编译项目时间**<br> 在.c文件被修改后智能地识别那些.c文件需要重新编译成.o文件，那些不需要，最终将所有的.o 链接成可执行文件，节约编译时间，再一个每次在终端上敲 gcc xx.c yy.c ... ... 很麻烦，浪费时间
![](/assets/Snip20180529_9.png)

 <br><br>**.一次编写终身受益**<br>一个项目编写一个Makefile就可以了，甚至通用性强的Makefile还可以跨项目使用.。<br><br>**.Makefile 示例**
 ```
 vim Makefile   //规范，第一个M用大写
 // 以下是Makefile 中填写的具体内容
 all:
     gcc add.c sub.c main.c -o app
     
 //保存文件，在终端敲 make 执行， 当执行make时会自动的去当前目录查找Makefile 文件
 ```
 一般，Makefile的语法由3部分构成：<br> **目标：依赖** <br>&emsp;&emsp;**命令**<br><br>写全的化就是这样：
 ```
 all: add.c  sub.c main.c 
     gcc add.c sub.c main.c -o app
 ```
** 可以这样来理解：**<br><br>我的目标是  `all` <br>目标的依赖（条件）是 `add.c  sub.c main.c ` <br>如何达成我的目标呢， 执行命令 `gcc add.c sub.c main.c -o app` 来达成目标。<br><br> 当我执行 `make`命令时，make 就会来加载我的目标 `all`, 达成我的目标的条件是`add.c  sub.c main.c `，如何执行目标，就是执行`gcc add.c sub.c main.c -o app` 这个命令来达成。<br>
**注意：**<br> 目标必须顶格写后边加 `:` 再跟依赖（条件），命令必须 必须换行且在命令前面要有一个tap 空格键，在Makefile 中 `#` 表示的是注释。<br><br> **改进写法：(阶段二)**

    ```
    app: add.o sub.o main.o
        gcc add.o sub.o main.o -o app

    add.o:add.c
        gcc -c add.c   // -c add.c 表示的是只编译成 .o 不链接
    
    sub.o:sub.c
        gcc -c sub.c
    
    main.o:main.c
        gcc -c main.c
    ```
**GCC 工作分2个阶段**<br> 阶段1，从上往下建立关系树，阶段2， 从下往上执行命令。



 








<br><br><br><br>
- **2、基本规则**


