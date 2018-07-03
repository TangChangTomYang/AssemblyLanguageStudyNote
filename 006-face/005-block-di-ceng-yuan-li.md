- **重写 OC 代码为C++ 代码**
```
xcrun sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp
```

- **运行时重写oc 代码为C++代码**

    ```
    xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc -fobjc-arc -fobjc-runtime=ios-8.0.0 main.m
    ```
<br><br>
####一 block 的本质

- 1 **其实Block本质上是一个OC对象,只是这个对象里面封装了函数调用和函数调用环境.**<br><br>(1) **为什么说block 是一个oc 对象呢?**<br><br> **因为block 里面有 isa 指针**


![](/assets/Snip20180628_2.png)
___
![](/assets/Snip20180628_1.png)


####二 block 的变量捕获(capture)

- **为了保证block 内部能够正常访问外 部的变量,block 有个变量捕获机制**
![](/assets/图片 1.png)

<br>
![](/assets/Snip20180703_2.png)


####三 block 的类型

- **block 有3种类型,可以通过调用 class 方法,或者isa指针查看具体的类型,最终都是继承自NSBlock 类型**<br>(1)__NSGlobalBlock__ (_NSConcreteGlobalBlock)<br>(2)__NSStackBlock__ (_NSConcreteStackBlock)<br>(3)__NSMallocBlock__ (_NSConcreteMallocBlock)

![](/assets/Snip20.png)
![](/assets/Snip20180703_3.png)


- **每一种block 调用copy 后的结果如下:**
![](/assets/Snip20180703_5.png)

####四 block 的copy
- **在ARC 环境下,编译器会根据情况自动将栈上的block 复制到堆上,比如以下情况:**<br>(1)block 作为函数返回值<br>(2)将block 赋值给 strong 指针时<br>(3)block 作为Cocoa API 中方法名含有usingBlock 的方法参数时.<br>(4)block 作为GCD API 的方法参数时.


- **MRC 下block 属性的建议写法:**<br>@property(copy,nonatomic)void (^block)(void);

- **ARC 下block 属性的建议写法:**<br>(1)@property(strong,nonatomic)void (^block)(void);<br> 或者<br>(2)@property(copy,nonatomic)void (^block)(void);


####五 对象类型的 auto 变量

- **当block 内部访问了对象类型的 auto 变量**<br> (1)如果block 实在栈上,将不会对 auto 变量产生强引用.

- **如果 block 被拷贝到了堆上**<br> (1)会调用block 内部的copy 函数<br>(2)copy 函数内部会调用 _Block_object_assign 函数<br>(3)_Block_object_assign 函数会根据 auto 变量的修饰符(__strong __weak __unsafe_unretained) 做出相应的操作,形成强引用(retain)或者弱引用

- **如果block 从堆上移除**<br>(1)会调用block 内部的dispose 函数<br>(2)dispose 函数内部会调用_Block_object_dispose 函数<br>(3) _block_object_dispose 函数会释放应用的auto 变量(release)
![](/assets/Snip20180703_6.png)










