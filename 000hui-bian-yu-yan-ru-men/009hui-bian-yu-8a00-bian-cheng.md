##一、 使用汇编语言编写一个完整的程序，步骤大致入下：

(1) 编写源代码，文件拓展名为 **.asm**
(2)编译、连接(可使用微软的MSAM编译器)
(3)调试、运行
![Snip20180209_2.png](http://upload-images.jianshu.io/upload_images/2018969-941d858fa7eaebeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意： 汇编语言文件拓展名时.asm**



## 二、汇编语言的组成

```objc
assume cs:code // 说明 code 范围内的是 cs 代码段的代码，这行只对程序员有意义，对于计算机可以不用写
code segment // 汇编段开始
mov ax， 1122h
mov bx，3344h
add ax, bx

; 正常的退出指令
mov ah,4ch // 写成 mov ax,4c00h 都可以，只要 ah == 4ch 即可，退出只看ah == 4ch
int 21h
code ends // 汇编段结束
end // 表示编译结束，不往下面执行了
```

##三、汇编语言由两类指令组成

- 汇编指令
eg: mov add sub 等
**有对应的机器指令指令，可以编译为机器指令，最终被cpu执行**

- 伪指令
eg: assume segment ends end 等
**没有对应的机器指令，由编译阶段解析，最终不被cpu 执行**

**在汇编语言中注释不是使用 "//" ,而是使用 分号 ";"**

![汇编语言指令的差异.png](http://upload-images.jianshu.io/upload_images/2018969-edbdaacb91eb959b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 伪指令-- segment 、ends、end
```objc
assume cs:code
code segment
mov ax, 1122h
mov bx,3344h
add ax,bx

mov ah,4ch
int 21h
code ends
end
```

- segment 和ends 的作用是一个定义一个段，segment 代表一个段的开始，ends 代表一个段的结束，使用格式为：

**段名 segment**


**段名 ends**


- 一个有意义的汇编语言程序中，至少要有一个段作为代码段存放代码。

-assume 将用作代码段的code 段和cpu 中的cs 寄存器关联起来

- end 编译器遇到end 时，就结束对源程序的编译










##四、HelloWorld 汇编程序

- 书写方式1
```objc

assume cs:code ds:data

;数据段
data segment
db 'Hello world!$' //美元符$在汇编中 表示字符串结束
data ends


;代码段
code segment

start: //表示代码段开始执行
;设置数据段的值
mov ax , data
mov ds, ax

;打印数据段的字符串
mov dx,0h // 从数据段的开始位置
mov ah，9h // 中断 21h 指令执行的是打印程序
int 21h // 调用系统中断
code ends
end start

```


- 书写方式2

```objc
; 这个仅仅是提醒开发者没个段的含义，没有具体的其它含义。
assume cs:code , ds:data

;-----数据段 begin---------------------
data segment
age db 20h // 定义一个字节的数据，age 表示的是别名
no dw 30h // 定义一个字的数据，no表示的是别名
db 10 dup(6) // 定义10个字节的数据，每个字节内都存放的是6
string db 'Hello world!&' // 定义一个字符串 string表示的是字符串的别名
data ends
;-----数据段 end---------------------




;-----代码段 begin---------------------
code segment

start:

mov ax ,data // 获取代码段的地址
mov ds,ax // 存取代码段的地址

mov dx, offset string // 获取字符串的偏移地址

mov ax, 9h
int 21h // 调用系统中断，中断码ah = 9h 表示的是打印

mov ax,4c00h
int 21h // 调用系统中断，结束程序
code ends
;-----代码段 end---------------------
start end
```


**数据段的作用**

- 数据段一般来说是放全局变量的，也就是说**C语言** 中的全局变量就对应汇编中的数据段。

##五、多段汇编程序
**数据段、代码段、栈段**

```objc
assume cs:code, ds:data, ss:stack // 代码段、数据段、栈段


;栈段
stack segment
db 200 dup(0) // 分配200个字节
stack ends


;数据段
data segment
db 300 dup(0) // 分配300 个字节
data ends


;代码段
code segment
start：// 代码段中的这个标记必须要写，否则程序不知道入口从第一行开始执行
； 手动说明ds
mov ax,data
mov ds,ax

； 手动说明ss
mov ax,stack
mov ss,ax
； 使用栈
mov sp,200 // 栈针指向栈顶

push ax,1122h
push bx,3344h
pop ax
pop bx

； 程序退出
mov ax， 4c00h
int 21h
data ends
end start
```



