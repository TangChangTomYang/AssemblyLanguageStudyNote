####一、App 从开发到安装到手机的过程

- 开发过程找那个的所有代码和资源文件，最终打包成一个ipa 文件（其实是一个压缩包）
![](/assets/Snip20180525_8.png)
mach-o 是iOS中的可执行文件

- 最终将ipa 安装到手机上，就可以了。将ipa 安装到手机上主要有几种方式：
    - 直接通过 xcode 安装
    - 通过appstore 安装
    - 通过 iTools 、pp助手等工具安装
    注意点： 通过个人免费证书生成的ipa包，有效期只有6天。


####二、代码的编译过程

![](/assets/Snip20180526_4.png)

- 不同的OC代码，编译出的 汇编有可能是一样的
![](/assets/Snip20180526_5.png)


####三、 Mach-o 文件深度学习

- Mach-o 是mach Object 的缩写，是mac\iOS 上用于存储程序、库的标准格式
- 属于Mach-o 格式的文件类型有（主要11个）：
```
#define	MH_OBJECT	     0x1		/* relocatable object file */
#define	MH_EXECUTE	    0x2		/* demand paged executable file */
#define	MH_FVMLIB	     0x3		/* fixed VM shared library file */
#define	MH_CORE		   0x4		/* core file */
#define	MH_PRELOAD	    0x5		/* preloaded executable file */
#define	MH_DYLIB	      0x6		/* dynamically bound shared library */
#define	MH_DYLINKER	   0x7		/* dynamic link editor */
#define	MH_BUNDLE	     0x8		/* dynamically bound bundle file */
#define	MH_DYLIB_STU      0x9		/* shared library stub for static */
/*  linking only, no section contents */
#define	MH_DSYM		   0xa		/* companion file with only debug */
/*  sections */
#define	MH_KEXT_BUNDLE	0xb		/* x86_64 kexts */
```
- 可以在[xnu源码](https://opensource.apple.com/tarballs/xnu/)（xnu 是mac 系统的内核）中，查看到Mach-o 格式的详细定义， 可以在下载文件中查看这2个文件：
```
EXTERNAL_HEADERS/mach-o/fat.h
EXTERNAL_HEADERS/MACH-O/loader.h
```

**常见的mach-o 文件类型：**
- **MH_OBJECT** 类型文件主要有以下几种：<br>(1)目标文件（.o文件，是源文件和可执行文件之间的中间产物）<br>(2)静态库文件(.a文件)，其实静态库就是N个 .o文件合并在一起。

- **MH_EXECUTE**类型文件是， 可执行文件。

- **MH_DYLIB**类型文件是，动态库文件，主要有：<br> (1).dylib 文件<br> (2).framework 文件.

- **MH_DYLINKER**类型文件，动态链接编辑器 <br> /usr/lib/dyld

- **MH_DSYM**类型文件，存储着二进制文件符号 <br>.dSYM/Contents/Resources/DWARF/xx(常用于分析app的崩溃)（release 版本打包时和 xxx.app 放在一起的 ）
![](/assets/Snip20180527_2.png)<br>
![](/assets/Snip20180527_3.png)


**我们其实通过Xcode 也是可以查看文件的类型的**，比如一般的app 项目生成的是  MH_EXECUTE 可执行程序，静态库项目生成的是  MH_DYLIB 静态库文件，但都是mach-o 格式文件
![](/assets/Snip20180527_4.png)


<br><br>**补充知识**：<br> 什么是 .o 文件？<br>.o 文件是源文件和可执行文件的中间产物。
![](/assets/屏幕快照 2018-05-27 上午11.24.40.png)

**了解：**<br>FreeBSD、Unix、Linux、XNU、Darwin 


####四、Mach-o 文件的基本结构

- **官方描述**https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/MachOTopics/0-Introduction/introduction.html

- **一个Mach-o 文件包含3个主要区域：**
    - **Header**（文件类型（动态库、静态库、可执行而文件、动态连接器、dsym 符号文件等）、目标架构（64位、32位，胖的瘦的等）类型等）
    - **Load commands** （描述文件在虚拟内存中的逻辑结构和布局（比如：数据段描述信息、栈段描述信息等等）、以及有没加壳等）
    - **Raw segment data**（在load commands 中定义的各种段（Segment）的原始数据）
    
    
####五、窥探Mach-o文件的结构
- **命令行工具， file ：**查看 Mach-o 的文件结构
```
file 文件路劲 
```

- **otool**，查看Mach-o 特定部分和段的内容
```
cd xxx  // 切换到mach-o 文件路劲
otool   // 查看otool的具体用法
otool -l grep crypt //查看是够加壳
```

- **lipo 常用于多架构Mach-o 文件的处理**
    - 查看架构信息：lipo -info 文件路劲
    - 导出某种特定的架构：lipo 文件路径 -thin 架构类型 -output 输出文件路劲
    - 合并多种架构： lipo 文件路劲1 文件路劲2 -output 输出文件路劲

- GUI工具
    - [MachOView 源码](https://github.com/gdbinit/MachOView) 下载运行安装即可,安装问题，及解决方案：
    ![](/assets/Snip20180609_1.png)<br>
    ![](/assets/Snip20180609_2.png)
    ![](/assets/Snip20180609_3.png)
    
    
####六、dyld 和 Mach-o
- dyld 用于加载以下类型的mach-o 文件
    - MH_EXECUTE 可执行文件
    - MH_DYLIB   动态库
    - MH_BUNDLE  
- app 的可执行文件、动态库都是由dyld负责加载的。

![](/assets/Snip20180527_9.png)










