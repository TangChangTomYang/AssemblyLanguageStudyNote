##一、Makefile 文件的语法

- **001、什么是Makefile ？**<br>Makefile 就相当于我们项目的管理文件（用Makefile文件来管理我们的文件），在我们在终端 执行命令 `make`时，会在当前的文件目录去查找Makefile 这个文件。<br> 最简单的Makefile



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



