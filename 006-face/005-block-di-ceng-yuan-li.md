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

- **什么是捕获呢?**<br>**捕获就是block 内部会新增一个变量来记录外面的变量的值(具体是记录值 还还记记录 指针就看外面变量的情况了,具体如下图:)**
![](/assets/图片 1.png)

<br>
![](/assets/Snip20180703_2.png)

- **为什么局部变量block 会捕获呢?**<br><br>**答:**<br>**这个需要从 block 的具体实现本质来解释.<br>(1)首先 block 也是一种对象,是一种分装了函数调用和函数调环境的对象,即 block 有自己的调用函数,调用参数(即,当外面调用 block时,其实是在让 block对象 执行其内部的函数方法).<br>(2) 我们在外面封装函数时 函数内部有自己的 局部变量,block 等等,当我们在当我们调用封装的 block时,因为block内部的函数不能访问外部的局部变量(跨作用域了,a 函数不能访问b函数的局部变量)因此要捕获外部的局部变量**,如下图说明:<br><br>oc实现
![](/assets/Snip20180704_3.png)
C/C++ 底层具体对应实现(解释了为什么要捕获)
![](/assets/Snip20180704_1.png)






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

- **对象类型的 auto 变量 block 具体实现如下如:**
![](/assets/Snip20180703_17.png)



####六 __block  修饰符auto 变量(基本变量,对象变量)

- **__block 可以用于解决 block 内部无法修改 auto 变量的问题 ** <br><br>**注意:**<br> block 不能修饰全局变量 和 静态变量(static) 

- **__block 为什么能解决 block 内部无法修改 auto 变量的问题? **<br><br>**答:**
<br> **(1)编译器会对 __block 修饰的 auto 变量(基本变量,对象变量) 进行对象包装, 即block 内部会自动生成一个对象来持有 __block 修饰的auto变量, 在block 销毁前, block 内部会强引用着这个自动生成的包装对象,直到block 销毁,因此 被__block 修饰的 auto 变量在block 内部可以修改**.<br>(2)**block 内部自动生成的对象(持有对象) 具体对 __block 修饰的 auto变量, 是强引用还是弱引用,的看外面__block 修饰的对象前有无 __weak 修饰符,即 和外面引用类型一样(外面强引用,里面就是强引用.外面弱引用,里面就是弱引用)**<br>(3)**如果 __block 修饰的是基本变量 就不存在强弱引用之说了**

<br>
- **__block 修饰 auto 基本变量的 Block 具体实现**
![](/assets/Snip20180703_22.png)

<br>
- **__block 修饰的 auto 对象的block 具体实现**<br><br>**注意:** **self 也是(一种) auto 对象,因为 oc 的方法调用内部采用的是消息机制 ,即:<br> objc_msgSend(xxx对象,xxx方法,xxx参数...) **
![](/assets/Snip20180703_28.png)





####七 __block  的内存管理

- **当 block 在栈上时,并不会对 __block 变量产生强引用**
- **当block 被拷贝到堆上时**<br>(1)会调用block 内部的copy函数<br>(2)copy 函数内部会调用 _Block_object_assign 函数<br>(3) _Block_object_assign 函数会对__block 修饰的变量(强引用变量)形成强引用
![](/assets/Snip20180703_29.png)

- **当block 从堆中移除时**<br>(1) 会调用block 内部的dispose 函数<br>(2)dispose 函数内部会调用 _Block_object_dispose 函数<br> _Block_object_dispose 函数会自动释放引用的__block 变量.
![](/assets/Snip20180703_30.png)










