#Objective-C本质

##一、面试题

**1、一个NSObject 对象占用多少内存？**

- **平时我们编写端 Objective-C 代码，底层实现都是 C/C++ 代码。**
![](/assets/Snip20180527_1.png)
- **所以 Objective-C 的面向对象都是基于 C/C++ 的数据结构实现的**
- **思考：**<br> Objective-C 的对象、类 要是基于C/C++ 的什么数据结构实现？<br> **答：** 结构体。
- **将Objective-C 代码转换为C/C++ 代码**

**不恰当的做法如下：** 
  
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
发现，转换出来的指定架构（arm64）的代码 要小很多。<br><br><br>**查看代码可以看出，Objective-C 的本质是 C/C++ 的结构体的实现**

![](/assets/Snip20180527_3dfd.png)
**也就是说，一个 NSObject 对象在内存中其实就是一个结构体**<br>
![](/assets/Snip20180527_5.png)

**验证测试：**
```
#import <Foundation/Foundation.h>
#import <objc/runtime.h>
#import <malloc/malloc.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        NSObject *obj = [[NSObject alloc]init];
        // 1个NSObject 对象占用16个字节
        
        // 获取NSObject 实例对象（成员变量）的大小
        NSLog(@"NSObject 实例对象: %zd",class_getInstanceSize([NSObject class]) );
        
        // 获取 obj 指针所指向的内存大小
        NSLog(@"obj 指针所指向: %zd",malloc_size((__bridge void *)obj));
        
    }
    return 0;
}
```

**打印输出**
```
NSObject 实例对象: 8    // 说明一个NSObjective 对象大小的确是8 个字节
obj 指针所指向: 16       // 说明 obj 对象指针占用的是 16个字节的空间
```
源码发现:
```
size_t class_getInstanceSize(Class cls)
{
    if (!cls) return 0;
    return cls->alignedInstanceSize(); // aligned 是对齐的意思
}

// Class's ivar size rounded up to a pointer-size boundary.
uint32_t alignedInstanceSize() {
    // 字对齐
    return word_align(unalignedInstanceSize());
}

// 发现实例大小最小是16
size_t instanceSize(size_t extraBytes) {
    size_t size = alignedInstanceSize() + extraBytes;
    // CF requires all objects be at least 16 bytes.
    if (size < 16) size = 16;
    return size;
}
```


<br><br>**结论：**
**Class 是一个指针，在64位中一个指针占8个字节，一个NSObject 对象的大小是8个字节，但是占用的大小是16字节 （因为有内存对齐的问题）<br>官方的解释：
// CF requires all objects be at least 16 bytes.**


- 2、**对象的isa 指针指向哪里？**
指向一个Class结构体


- 3、**OC的类信息存放在哪里？**

























