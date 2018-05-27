####一、 动态库 共享缓存（dyld shared cache）
- 从 iOS3.1开始，为了提高性能，绝大部分的系统动态库文件都打包存放到了一个缓存文件中（dyld shared cache）<br>
缓存路径：
```
/System/Library/Caches/com.apple.dyld/dyld_shared_cache_armX
```
iphone CPU 的架构是ARM的。<br>不同的ARM处理器提供的指令集（可以认为是二进制或者是汇编指令集，因为机器指令和汇编指令是互逆的）是不一样的。<br> 主要的有： armv6、armv7、armv7s、arm64（比较新的）<br>从5s 开始都是arm64架构的。<br>**注意： 所有的指令集，原则上都是向下兼容的**
- 动态库共享缓存一个非常明显的好处是节约空间
- 现在的ida、Hopper 反汇编编译工具都可以识别动态库共享缓存文件。

- **[dyld源码](https://opensource.apple.com/tarballs/dyld)**


####二、加载动态库
- 在Mac、iOS 中，当我们Bundle来加载动态库时，其实内部是使用了 /usr/lib/dyld 程序来加载动态库<br> **dyld** (dynamic link editor 动态链接编辑器，dynamic loader 动态加载器)


####三、 使用命令终端将C语言代码编译成终端命令

- **C++ 编译器：** g++ 、clang++, 可以在 `/usr/bin` 目录下查看到![](/assets/屏幕快照 2018-05-27 上午9.27.01.png)<br><br> **补充编译C语言文件知识：**<br>

1、 编写c语言文件，内容如下

```
#include <stdio.h>

int main(){
    printf("测试编译c语言文件\n");
    return 0;
}
```

2、编译c 语言文件 生成 `.out` 文件
```
cd   c语言文件目录    // 切换到c语言文件夹
clang abc.c         // 编译c语言文件生成 a.out 文件（成功编译后会在 c语言文件目录 下生成一个 a.out文件）

// 如果需要给输出的文件指定文件名则这样写：clang -o abc abc.c  
// 编译abc.c 文件并生成一个abc.out 输出文件


```
3、执行 a.out 文件，相当于是运行程序
```
./a.out     // 找到执行文件并赋予执行权限（就执行了）

// 或者直接将 可执行文件拖入 终端 回车也可执行
// 即 exec可执行文件 需要全路径才能执行 
```

4、说明： 经过clang 编译自动生成的文件是 固定的名字 `a.out` 其实是一个exec 可执行文件，如果给编译的文件指定文件名，则生成的文件不会添加`.out` 也是一个exec 可执行文件,也就是说和后缀关系不大。



####四、借助苹果开源的 dyld 工具 将 苹果的 动态库共享缓存（dyld shared cache)中的各个框架的 Mach-o 文件抽取出来

- **下载[dyld源码](https://opensource.apple.com/tarballs/dyld)**
- **将dyld 源码中的 dsc_extractor.cpp 编译成可执行（exec）文件**
```
clang++ -o dsc_extractor  dsc_extractor.cpp
```
**注意：**<br> 在编译 dsc_extractor 时需要将 条件编译 改为 1 ，因为默认是不会编译的， 可以把条件编译及前面代码去掉，因为没有用
- **执行命令抽取即可**

```
cd  dsc_extractor目录 // 切换到可执行文件目录
./dsc_extractor  dylb_shared_cache_armv7s armv7s  
// dylb_shared_cache_armv7s armv7s架构的共享缓存库文件， 抽取后的文件全部放入armv7s文件
```
![](/assets/屏幕快照 2018-05-27 上午10.18.59.png)









