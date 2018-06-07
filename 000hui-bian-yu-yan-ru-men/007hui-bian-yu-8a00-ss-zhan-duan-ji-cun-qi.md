#SS栈段寄存器

##一、栈

- **栈**： 是一种具有特殊的访问方式的存储空间(**后进先出,Last in Out First**)

![栈.png](http://upload-images.jianshu.io/upload_images/2018969-388a09aa732bddb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 8086 会将**CS**作为**代码段**的段地址，将**CS:IP**指向的的指令作为下一条需要取出执行的指令。

- 8086 会将**DS**作为**数据段**的段地址，eg: mov ax ,[偏移地址]就是取出 **DS:偏移地址**的内存数据放到**ax**寄存器中

- 8086 会将**SS**作为**栈段**的段地址，**任意时刻，SS:SP 指向栈顶元素**。

- 8086 提供了**PUSH** （入栈）和**POP**（出栈）指令来操作栈段的数据。
> 比如：
**push ax 是将ax 寄存器中的数据入栈,pop ax 是将栈顶的数据送入 ax** 

&emsp;&emsp;**注意： 8086 push 和 pop 的数据是固定的都是2个字节，即，压入栈2个字节，出栈2个字节**


##二、push ax

![入栈流程.png](http://upload-images.jianshu.io/upload_images/2018969-6878c3d36de9f0f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- push ax 的执行，由以下两个步骤完成
> sp = sp-2 (栈顶指针减2，一定是减2), ss:sp 指向当前栈顶前面的单元，以当前栈顶前面的单元为新的栈顶。
<br>
> 将 ax 中的内容送入ss:sp指向的内存单元处，ss:sp此时指向栈顶。

**注意，入栈时地址是往低地址压**

**push 的概念其实很简单，就是将当前的指针地址减少2 ，并将2个字节的数据压入栈顶对应的地址。**

##三、pop ax 

![pop 的过程.png](http://upload-images.jianshu.io/upload_images/2018969-bdaff5aaa323d9f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- pop ax 的执行，由以下两个步骤完成
> 将栈顶SS:SP 指向的内存数据送入ax
<br>
> 将 SP = SP +2 ,SS:SP 指向当前栈顶小面的单元，以当前栈顶小面的单元为新的栈顶。

**注意： 虽然 pop 时出栈 sp 指针地址加了二，把原来栈顶的数据写入了ax 中了，sp 指针移动后原来的地址内的数据并没有删除还在，只是变成了垃圾数据了（原来的数据没有人改它）**


##思考：

如果将10000H ~ 1000FH 这段空间当做栈，初始状态栈时空的，此时，SS = 1000h ， 那么 SP = ？


![思考.png](http://upload-images.jianshu.io/upload_images/2018969-609aec39ebc8e48a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**结论：如果栈是空的，那么SS:SP 一定指向栈空间最高地址单元的下一个单元**


##四、栈顶越界 - push 越界

![栈顶越界 - push 越界.png](http://upload-images.jianshu.io/upload_images/2018969-c61bb36860ddf9db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**栈顶越界操作，会导致将因错误操作将应该写入栈空间内的数据写入占空间外的地址内，导致其他地址空间内的数据被非法串改，出现数据会乱**


##五、栈顶越界- pop 越界

![栈顶越界- pop 越界.png](http://upload-images.jianshu.io/upload_images/2018969-eb15dd4b00c7a253.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**栈低越界- pop 越界 ，会导致 错误的将非栈内的数据 当做站内的数据操作，造成数据错误**


栈顶越界非常的危险，会导致错误的覆盖了他人的数据，或者错误的使用了他人的数据，在汇编语言中没有对栈顶越界的操作判断，因此对于栈顶越界 需要程序员自己注意 注意。



##六、 PUSH 和POP

- push 和 pop 指令的格式可以是如下的形式：

> push 寄存器  // 将寄存器中的数据入栈

> pop 寄存器  // 将栈中的数据写入寄存器

> push 段寄存器  // 将一个段寄存器中的数据写入栈

> pop 段寄存器   // 将栈中的数据写入段寄存器

> push 内存单元 // 将一个内存单元(**如：ds:[20] **)中的数据写入 栈

> pop 内存单元 // 将当前栈中的数据写入内存中


mov ax ，1000H
mov ds, ax
push [0]
pop [2]



##练习

**编程：
（1）将10000H ~ 1000FH 这段空间当做栈，初识状态栈时空的。
（2）设置 ax = 001aH ,bx = 001bH;
（3）利用栈，将=交换AX，BX 中的数据**


> mov ax, 001aH   
   mov bx, 001bH
   push ax     //写入ax中的数据
   push bx    // 写入bx中的数据
   pop ax      // 取出栈中的数据存入ax, 此时占中的数据时bx中的数据，达到交换的目的了
   pop bx     // 取出栈中的数据存入bx, 此时占中的数据时ax中的数据，达到交换的目的了

**一句话理解push 和 pop ，push 相当于存2个字节的数据，pop 相当于取两个字节的数据**




























