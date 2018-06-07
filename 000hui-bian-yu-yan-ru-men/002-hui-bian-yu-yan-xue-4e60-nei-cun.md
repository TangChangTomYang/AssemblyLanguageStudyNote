##一、汇编语言学前须知
- 想要学好汇编语言,首先要对**CPU等硬件结构**有一定的了解.
- **软件** \ **程序** 的执行过程
![软件\程序的执行过程.png](http://upload-images.jianshu.io/upload_images/2018969-31372fc4b82a42b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 最为关键的是要了解**CPU** 和 **内存**.
- 在学习汇编语言的过程中,遇到的**绝大部分指令都是跟内存CPU相关的指令**.

##二、总线
- 每一个CPU芯片都有许多的管脚,这些管脚和总线相连,**CPU通过总线和外部器件进行交互.**
- 总线: 一根根的导线的集合.
- 总线的分类:

| 地址总线 |  数据总线 |  控制总线 |
|----------|-----------|-----------|

![总线.png](http://upload-images.jianshu.io/upload_images/2018969-29097131e917166b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**CPU 从内存的3号单元读取数据的过程.**
![CPU数据操作过程.png](http://upload-images.jianshu.io/upload_images/2018969-2c2ae59a91495367.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 地址总线
>(1)它的宽度决定了CPU的**寻址能力**
  (2)8086的地址总线宽度是**20**,所有他的寻址能力是**1M** (2^20^)

- 数据总线
 >(1)他的宽度决定了CPU单次数据传送量,也就是数据的传送速度.
 (2)8086 的数据总线宽度是**16**,所以单次最大传送**2个字节的数据**.

- 控制总线
 >它的宽度决定了CPU对其他器件的**控制能力**,能有多少种控制.

##三、数据总线

- 8088的数据总线宽度是8, 8086的数据总线宽度是16,分别向内存中写入89D8H.
8088CPU 分两次传送89D8H,第一次传送D8,第二传入89.

![8088CPU.png](http://upload-images.jianshu.io/upload_images/2018969-57f1f246bde2326d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![8086CPU.png](http://upload-images.jianshu.io/upload_images/2018969-0a47c1fb98f4a24e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##四、思考
- 1个CPU的寻址能力为8KB,那么他的地址总线宽度为 **13**
- **8080**  **8088** **80286** **80386** 的地址总线宽度分别为: 16根\20根\24根\32根,则他们的寻址能力分别为: *64KB* *1MB* *16MB*  *4GB*
- **8080** **8088** **8086** **80286** **80386**  的数据总线宽度分别为: 8根\8根\16根\16根\32根,则他们一次的数据传送能力为: *1B* *1B* *2B*  *2B* *4B*

-  从内存中读取 1024 字节的数据,8086至少读** 512** 次,80386 至少读取**256**次.


##五、内存

- 各种存储器的逻辑连接情况
![存储器逻辑结构.png](http://upload-images.jianshu.io/upload_images/2018969-c307dd6b441284c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 所有的内存单元都有唯一的地址,叫做物理地址
- 各类存储器的物理地址情况
   - 内存地址空间的大小受CPU地址总线宽度的限制.8086的地址总线宽度为20,可以定位 2^20^ 个不同的内存单元(内存地址范围: 0x00000 ~ 0xfffff),所以8086的内存空间大小为1MB.
  - 0x00000 ~0x9ffff: 主存储器,可读可写
  - 0xAffff ~ 0xBffff: 向显存中写入数据,这些数据会被显卡输出到显示器.可读可写.
  - 0xc0000 ~ 0xfffff : 存储各种硬件\系统信息. 只读.

![8086内存地址划分.png](http://upload-images.jianshu.io/upload_images/2018969-d27574cbd9a10bdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Snip20180130_8.png](http://upload-images.jianshu.io/upload_images/2018969-dc21f236a3b88801.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##六 8086的寻址方式

**古怪的寻址方式**
- CPU 访问内存单元时,要给出内存单元的地址
- 8086 有20位(根)地址总线,可以传送20位的地址,即1M的寻址能力.
- **但是,但是** 8086 又是16位的CPU结构,他的内存能一次性处理\传输\暂时存储的地址为**16位**.如果将地址从内部简单的发出,那么他只能送出**16位**的地址,表现出来的寻址能力只有64KB.
- 8086 采用一种在内部用**2个16位地址**合成的方法来生成**一个20位的物理地址**
![8086CPU地址合成原理.png](http://upload-images.jianshu.io/upload_images/2018969-550bde9b2c0e5484.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 地址加法器采用 **物理地址= 段地址x16+偏移地址**的方法用段地址和偏移地址合成物理地址.例如: 8086CPU 要访问地址为123C8H 的内存单元,此时地址加法器的工作过程:

![地址加法器的工作过程.png](http://upload-images.jianshu.io/upload_images/2018969-fd967f2e9eec4d5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


地址合成:
段 地 址:      1200H   (16位)
偏移地址: 1000H (16位)
物理地址 = 段地址 x 16 + 偏移地址
物理地址 = 段地址 x 10H + 偏移地址
物理地址 = 1200H x 10H + 1000H = 12000H +  1000H = 13000H

- 疑问
![段地址与偏移地址的组合.png](http://upload-images.jianshu.io/upload_images/2018969-c947a9a3ad158d4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##七 内存的分段管理

- 8086 是用*起始地址 (端地址 X 16 ) + 偏移地址 = 物理地址* 的方式给出物理地址.
- 为了开发方便,我们可以采用分段的方式来管理内存,比如: 
   -地址为10000H ~ 100FFH的内存单元组成一个段,该段的**起始地址**为10000H, **段地址**为 1000H 大小为 100H. *(00H ~ FFH 大小就是100H)*

![内存地址的分段.png](http://upload-images.jianshu.io/upload_images/2018969-c366ca6039adeac0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **偏移地址**为16位,16位地址的寻址能力是64kB ,所以一个段的最大是64KB.

![Snip20180130_23.png](http://upload-images.jianshu.io/upload_images/2018969-1145880e0fc21d5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
















































