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
