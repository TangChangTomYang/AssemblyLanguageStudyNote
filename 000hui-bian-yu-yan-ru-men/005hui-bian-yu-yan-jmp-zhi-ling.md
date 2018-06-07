#jim指令

##一 jmp 指令,修改 CS:IP 的值

- CPU 从何处执行指令是由**CS:IP**中的内容决定,我们可以通过改变**CS** **IP** 的内容来控制CPU的执行指令.

- 8086 提供了一个**MOV**指令(**传送指令**),可以用来修改大部分寄存器的值,比如:
> mov ax , 10  
   mov bx , 20 
   mov cx , 30 
   mov dx , 40  

- 但是,**mov** 指令不能用于设置**CS** **IP**的值,8086 没有提供这样的功能.

- 8086 提供了另外的指令来修改**CS** **IP** 的值,这些指令统称为**转移指令**,最简单的是**jmp** 指令.

**如果想同时修改 CS IP的内容,可用形如"jmp 段地址:偏移地址" 的指令完成,如下:**

> jmp 2AE3:3 执行后: cs=2AE3H, IP= 0003H,CPU 将从2AE3H处读取指令.

> jmp 3:0B16 执行后: cs=0003H, IP= 0B16H,CPU 将从00B46H处读取指令.

**jmp 段地址:偏移地址**,指令的功能是:用指令中给出的**段地址修改 CS,偏移地址修改IP**

##二 jmp指令,修改 IP 的值



- 若想仅修**IP**的内容,可用形如**"jmp 某一合法寄存器"**的指令完成,如:

> jmp ax ,指令执行前: ax = 1000H,CS = 2000H,IP=0003H.
   指令执行后:ax = 1000H,CS = 2000H, IP = 1000H

> jmp bx ,指令执行前: bx = 0B16H,CS = 2000H,IP=0003H.
   指令执行后:bx = 0B16H,CS = 2000H, IP = 0B16H

&emsp;**"jmp 某一合法寄存器"** 指令的功能为: 用寄存器中的值修改 IP

 &emsp; **jmp ax**,在含义上好似: mov IP ,ax ,但是不允许使用mov修改IP中的值.

- **另外,也可以"jmp 直接值"来修改IP的值,比如:"jmp 0100H"**

![练习1.png](http://upload-images.jianshu.io/upload_images/2018969-e95fb91417316841.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![练习2.png](http://upload-images.jianshu.io/upload_images/2018969-09e0b211f9774ac0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


  












