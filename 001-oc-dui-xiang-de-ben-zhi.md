#Objective-C本质

##一、面试题

**1、一个NSObject 对象占用多少内存？**

- **平时我们编写端 Objective-C 代码，底层实现都是 C/C++ 代码。**
![](/assets/Snip20180527_1.png)
- **所以 Objective-C 的面向对象都是基于 C/C++ 的数据结构实现的**
- **思考：**<br> Objective-C 的对象、类 要是基于C/C++ 的什么数据结构实现？<br> **答：** 结构体。
- **将Objective-C 代码转换为C/C++ 代码**
    
```
clang -rewrite-objc main.m -o main.cpp
// 将main.m 转换成main.cpp c++文件，-o main.cpp表示指定输出文件
// 使用 上面这个指令将 .m 文件转换成 c++ 文件其实是有缺陷的，因为不同平台（mac osx、iOS、windows 、32位、64位）支持的代码不一样，转换的结果就有差异，
因为我们是iOS 开发，所以最好能转换成iOS 指定的平台的C++ 代码。

```
**正确的做法如下：**
```
模拟器（i386）、32位（armv7）、64位（arm64）

xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp

// xcrun -sdk iphoneos 表示借助Xcode 工具运行 iphoneos sdk 
// -arch arm64 表示指定架构是 arm64位的架构
```
可以看出，转换出来的指定架构（arm64）的代码 要小很多。<br><br><br>**我们发现如下信息：**
```
struct NSObject_IMPL {
	Class isa;
};
```
**可以看出，Objective-C 的本质是 C/C++ 的结构体的实现**

![](/assets/Snip20180527_3dfd.png)
**也就是说，一个 NSObjec 对象在内存中其实就是一个结构体**<br>
![](/assets/Snip20180527_5.png)


























