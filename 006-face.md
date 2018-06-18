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
- 4、**一个NSObject 对象占用多少内存空间呢？**
![](/assets/objSize.png)
<br><br>
**其实呢,在创建对象时其实对象应该占用8个字节,但是因为框架设计的因素,一个NSObject 对象至少占用16个字节**
![](/assets/objMinSize.png)
<br>**结论:**<br>(1).系统分配了16个字节给NSObject 对象(通过 <malloc/malloc.h> 下的 malloc_size函数获取)<br>(2).但是NSObject对象内部只使用了8个字节的空间(64位环境,可以通过claa_getInstanceSize 函数获取)












