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


<br><br>

- **2、什么是Makefile ？**<br>Makefile 就相当于我们项目的管理文件（用Makefile文件来管理我们的文件），在我们在终端 执行命令 `make`时，会在当前的文件目录去查找Makefile 这个文件。<br> **最简单的Makefile**
```
all:
    gcc add.c  sub.c main.c -o app
```

- **1、Makefile的语法：**<br>



- 1、我们在命名Makefile 文件时有个规范，通常第一个M为大写，其它的为小写。（即Makefile）



- 2、Makefile 文件编写的语法：

```
#目标：依赖（条件）
#    命令

all：add.c sub.c  dive.c  mul.c  main.c
    gcc add.c sub.c dive.c mul.c main.c -o app
```
// all 就是目标， add.c sub.c  dive.c  mul.c  main.c 这几个文件就是依赖（条件）
// gcc add.c sub.c dive.c mul.c main.c -o app 就是要执行的命令



