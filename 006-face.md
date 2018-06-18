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
如上图所示:<br>(1)、一个Person 对象占用16个字节,其中isa 占用8个字节,_age 占用4个字节,因为n内存对齐的关系,一个Person对象最终占用16个字节.<br>(2)、 一个student 对象占用的内存大小其实就是 里面的成员变量的大小的总和,因为Person对象占用的是16字节,且其中有4个字节正好和_no的大小一样,且没有用,因此_no 使用的是Person 内没有使用的内存空间,因此student对象也是占用16个字节.<br>
**注意点:**<br>**内存对齐: 结构体的大小必须是最大成员变量的倍数**









####二、常用LLDB指令

print\ p :打印
po :打印对象,print object

**读取内存:**<br> memory read/(数量格式块大小) 内存地址<br>**可以简写**<br> x/(数量格式字节数) 内存地址<br><br>**格式说明:**<br>x 是16进制\f是浮点\d是10进制<br> b byte1字节\h half word 2字节\w word 4字节\g giant word 8 字节



**eg:**<br> x/3xw 0x10000<br>表示的是从内存地址xxx开始,以某种数据格式,读取几块大小yyy的内存数据<br> 
![](/assets/readeg.png)



<br>**修改内存中的值**<br> memory write 内存地址 数值<br> **eg: **<br> memory write 0x10000 10














