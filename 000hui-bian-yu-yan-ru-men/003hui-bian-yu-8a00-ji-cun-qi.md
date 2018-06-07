#寄存器
## 一、CPU 的典型构成

- CPU 中有很多部件,但一般最主要的有:**寄存器**  **运算器**  **控制器** ,如下图是CPU的主要结构:

![CPU 的典型构成.png](http://upload-images.jianshu.io/upload_images/2018969-1c357e7d020f5f29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**(1)寄存器:** 存东西的,比如我们做加法计算 20 + 30 ,那么数据20 和30 先存在寄存器中,在运算器中计算后再存储到寄存器中.
> CPU 中的寄存器,运算器等部件通过CPU中的**控制器(总线)**与外面的内存等其他部件相连.

- 对于程序员来说,**CPU中最主要的部件是寄存器,可以通过改变寄存器的内容来实现对CPU的控制.**(汇编学的好不好和寄存器学的好不好直接相关)

- 不同的CPU,寄存器的**个数** \ **结构**是不同的,**8086 是16位的结构的CPU**,但是地址总线是20位，可以访问1M的存储空间.

- **8086 有14个寄存器**(都是16位的寄存器(可以存放2个字节))

![8086的14个寄存器.png](http://upload-images.jianshu.io/upload_images/2018969-f5eeb7473ffae0ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##二、通用寄存器

- **AX** **BX** **CX** **DX** 这4个寄存器通常用来存放一般性的数据(eg: int a = 10 , int b = 10 ) 称为通用寄存器(有时也有特殊用途).

- 通常,CPU会先将内存中的数据存储到**通用寄存器中**,然后在对通用寄存器中的数据进行运算.

- 假如,内存中有块**红色内存空间**的值是*3*,现在想把他加*1*,并将结果存储到内存中的**蓝色内存空间**,那么处理流程大致如下:


![数据操作流程.png](http://upload-images.jianshu.io/upload_images/2018969-5255ce2c8de1a25f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



> 1. CPU 首先会将**红色内存空间**中的值放到 AX 寄存器中(通用寄存器)中,即: mov ax , 红色内存空间  *(将右边边红色内存空间的值存到左边AX 中 )*

> 2. 然后让AX 寄存器(通用寄存器)与1相加.即: add ax ,1 *(将右边的值1,与左边AX中的值相加并将结果存入左边AX中)*

> 3. 最后将值(结果)赋值给蓝色内存空间.即: mov 蓝色内存空间, AX *(将 右侧AX 中的值移动到左侧蓝色内存中)*

- **AX**  **BX**  **CX** **DX** 这4个通用寄存器都是16位的,可以存储2个字节,如下如:

![8086通用寄存器.png](http://upload-images.jianshu.io/upload_images/2018969-05f476558eddd159.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 注意: 上一代8086 的寄存器都是8位的,为了保证兼容, **AX** **BX** **CX** **DX** 都可以分为2个8位的寄存器来使用.如下图:

![通用寄存器的拆分.png](http://upload-images.jianshu.io/upload_images/2018969-3fde8180a3ef8908.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![高8位低8位的拆分.png](http://upload-images.jianshu.io/upload_images/2018969-91a96dd4b8d98783.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##三、字节与字

- 在汇编的数据存储中,有两个比较常用的单位:**字节**和**字**. *(相当于高级语言中的 int,long,float 等数据类型).因此我们在汇编中只能定义两种数据类型的数据,* **字节类型(byte类型),字类型(word 类型))**

> **字节**: byte ,1个byte 由8个bit组成,可以存储在1个8位寄存器中.

> **字**:word,1个字由两个字节组成,这两个字节分别称为字的高字节和低字节.

- 比如数据20000 (4E20H,01001110 00100000B),高字节值78,低字节值32.

![字表示.png](http://upload-images.jianshu.io/upload_images/2018969-086098a65e5f8ebe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 1个字可以存储在一个16位寄存器中,这个字的高字节\低字节 分别存储在这个存储器的高8位和低8位寄存器中.



##四、段寄存器

- 8086 在**访问内存**时要由相关部件提供内存单元的**段地址** 和 **偏移地址** 送入地址加法器合成**物理地址**

- 是什么部件提供段地址?  
   答： **段地址** 在8086的**段寄存器**中存放. (段 segment )

> 代码段寄存器: CS (code segment) 存放代码的
> 数据段寄存器: DS (data segment ) 存放数据的
> 堆栈段寄存器: SS (stack segment ) 对象放堆里面,局部对象放栈里面
> 附加段寄存器: ES (Extra segment)

![8086段寄存器.png](http://upload-images.jianshu.io/upload_images/2018969-84a9fbe4b4ba5036.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 8086 有4个段寄存器,**CS** **DS** **SS**  **ES**,当要访问内存时由这4个**段寄存器** 提供内存单元**段地址**.

- 每个段寄存的具体作用是什么呢? 
>   一旦程序运行装载到内存当中,所有的代码\全局变量\局部变量\对象都装载到了内存当中,所以内存当中存在具体的代码和数据,也存在堆栈等等 ,那么CPU想访问内存某段代码,那么他会访问法代码段寄存器,如果CPU想想问堆栈中的数据那么他会访问栈寄存器,依次类推就是这样的.






















