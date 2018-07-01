- **重写 OC 代码为C++ 代码**
```
xcrun sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp
```

####一 block 的本质

- 1 **其实Block本质上是一个OC对象,只是这个对象里面封装了函数调用和函数调用环境.**<br><br>(1) **为什么说block 是一个oc 对象呢?**<br><br> **因为block 里面有 isa 指针**


![](/assets/Snip20180628_2.png)
___
![](/assets/Snip20180628_1.png)
