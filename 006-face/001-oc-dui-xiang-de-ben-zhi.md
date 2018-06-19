#一、OC 对象的本质


####一、Objective-C的本质

- 1、**我们平时编写的Objective-C代码，底层实现其实都是C\C++代码**<br><br>**Objective-C --> C\C++ --> 汇编语言 --> 机器语言**<br><br>所以，Objective-C 的面向对象的实现都是基于C\C++ 的数据结构实现的.<br><br>**其实Objective-C的 对象、类 主要基于C\C++ 的结构体实现**


<br>
- 2、**将Objective-C代码转换为C\C++代码**

```
clang -rewrite-objc main.m -o main.cpp
// 将oc 代码转换成C++代码,虽然这样可以直接转成C++，
// 但是这样做不好，因为不同平台的代码是稍微有差异的，
// 比如,汇编代码是和硬件直接相关的
//
// 建议使用下面的指令：
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp
// xcrun -sdk iphoneos 表示使用Xcode的工具
// -arch arm64 表示指定架构

```
<br>
- 3、**NSObject 的底层实现**
![](/assets/objc.png)
**其实 Class 就是一个指向结构体的指针**
![](/assets/class.png)
**既然Class 是一个指向结构体的指针，而一个NSObject 对象内部只有一个isa 的指针，**


<br>
- 4、**一个NSObject 对象占用多少内存空间呢？**<br><br>
![](/assets/objSize.png)
<br><br>
**其实呢,在创建对象时其实对象应该占用8个字节,但是因为框架设计的因素,一个NSObject 对象至少占用16个字节**
![](/assets/objMinSize.png)
<br><br>**结论:**<br>(1).系统分配了16个字节给NSObject 对象(通过 <malloc/malloc.h> 下的 malloc_size函数获取)<br>(2).但是NSObject对象内部只使用了8个字节的空间(64位环境,可以通过claa_getInstanceSize 函数获取)

<br>
- 5、**一个自定义对象占用多少个字节**
![](/assets/Snip20180619_1.png)
如图所示,一个自定义的student 对象占用16个字节,具体细节如下图:<br><br>
![](/assets/Snip20180619_2.png)



<br>
- 6、** 一个Person对象,一个student 对象占用多少内存空间?**
![](/assets/Snip20180619_3.png)
如上图所示:<br>(1)、一个Person 对象占用16个字节,其中isa 占用8个字节,_age 占用4个字节,因为n内存对齐的关系,一个Person对象最终占用16个字节.<br>(2)、 一个student 对象占用的内存大小其实就是 里面的成员变量的大小的总和,因为Person对象占用的是16字节,且其中有4个字节正好和_no的大小一样,且没有用,因此_no 使用的是Person 内没有使用的内存空间,因此student对象也是占用16个字节.<br><br>
**注意点:**<br>**结构体内存对齐: 结构体的大小必须是最大成员变量的倍数,ios 中一般是8字节的倍数(isa 大小8个字节)**<br><br>
![](/assets/Snip20180619_5.png)<br><br>
**注意2:**<br> 我们通过alloc init 方法创建出来的实例对象中是不包含方法的.因为同一种类的实例对象可能有多个,但方法只有一个.









####二、常用LLDB指令

print\ p :打印
po :打印对象,print object

**读取内存:**<br> memory read/(数量格式块大小) 内存地址<br>**可以简写**<br> x/(数量格式字节数) 内存地址<br><br>**格式说明:**<br>x 是16进制\f是浮点\d是10进制<br> b byte1字节\h half word 2字节\w word 4字节\g giant word 8 字节



**eg:**<br> x/3xw 0x10000<br>表示的是从内存地址xxx开始,以某种数据格式,读取几块大小yyy的内存数据<br> 
![](/assets/readeg.png)



<br>**修改内存中的值**<br> memory write 内存地址 数值<br> **eg: **<br> memory write 0x10000 10


<br><br><br>


####三、2个容易混淆的函数

- 1、**创建一个实例对象,至少需要多少内存?**

        ```
# import <objc/runtime.h>
class_getInstanceSize([NSObject class]);
        ```

- 2、**创建一个实例对象,实际分配多少内存空间?**

        ```
# import <mallac/malloc.h>
malloc_size((__bridge const void * )obj);
        ```
**注意:**<br> **在对象实际分配内存时,实际内存的大小是16的倍数,这就是我们所谓的内存对齐**,常用内存大小如下:<br>Buckeets sized {16, 32, 48, 64, 80,96, 112, 128, 256}.
<br>
**注意:**<br>结构体内存对齐是最大成员变量占用内存的倍数.

<br><br><br>



####四、OC对象的分类


##### Objective-C中的对象,简称OC对象,主要可以分为3种
<br><br>
- 1、**instance对象(实例对象)**<br><br>(1)、 instance 对象就是通过alloc 出来的对象,每次调用alloc 都会产生一个新的instance 对象
![](/assets/Snip20180619_4.png)
<br>(2)、object1、object2 是NSObject的instance对象(实例对象),他们是不同的2个对象,分别占用不同的内存空间.<br><br>(3)、instance 对象在内存中存储的信息包括:<br>**isa 指针**<br>**其它成员变量**
![](/assets/Snip20180620_3.png)


<br><br><br>
- 2、**class 对象(类对象)**

```
#import <Foundation/Foundation.h>
#import <objc/runtime.h>
int main(int argc, const char * argv[]) {

NSObject *obj1 = [[NSObject alloc]init];
NSObject *obj2 = [[NSObject alloc]init];
// 以下都是类对象,一个类的类对象在内存中是唯一的
Class cls1 = [obj1 class];
Class cls2 = [obj2 class];
Class cls3 = object_getClass(obj1);
Class cls4 = object_getClass(obj2);
Class cls5 = [NSObject class];
NSLog(@"cls1:%p, cls2:%p, cls3:%p, cls4:%p, cls5:%p ",cls1,cls2,cls3,cls4,cls5);
// 打印结果:
// cls1:0x7fffa4c5c140, cls2:0x7fffa4c5c140, cls3:0x7fffa4c5c140, cls4:0x7fffa4c5c140, cls5:0x7fffa4c5c140

return 0;
}
```
(1)、cls1、cls2、cls3、cls4、cls5 都是NSObject 的class 对象(类对象).<br>(2)、他们是同一个对象,每个类在内存找那个有且只有一个class 对象.<br>(3)、class 对象在内存中存储的信息主要有:
<br> **isa 指针**<br> **superclass指针**<br>**类的属性信息(@property)**<br>**类的对象方法信息(instance method)**<br>**类的协议信息(protocal)**<br>**类的成员变量(变量类型,变量名等描述信息)**<br><br>





- 3、**meta-class对象(元类对象)**
















