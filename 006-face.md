#一、OC 对象的本质


####一、Objective-C的本质

- 1、我们平时编写的Objective-C代码，底层实现其实都是C\C++代码<br>**Objective-C --> C\C++ --> 汇编语言 --> 机器语言**<br><br>所以，Objective-C 的面向对象的实现都是基于C\C++ 的数据结构实现的.<br><br>**其实Objective-C的 对象、类 主要基于C\C++ 的结构体实现**


<br>
- 2、将Objective-C代码转换为C\C++代码
```
clang -rewrite-objc main.m -o main.cpp
// 将oc 代码转换成C++代码,虽然这样可以直接转成C++，
// 但是这样做不好，因为不同平台的代码是稍微有差异的，
// 比如,汇编代码是和硬件直接相关的
//
// 建议使用下面的指令：
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp
```
