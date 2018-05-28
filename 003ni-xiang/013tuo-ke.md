##一、Mach-o 可执行文件分析

- 使用class-dump 工具将 mach-o 文件的所有头文件导出，以便分析
```
class-dump -H sourceFile -o Headers
// sourceFile 需要分析的 mach-o 可执行文件
// -o Headers 指定输出到Headers 文件夹
```

- 使用Hopper disassembler 反汇编分析Mach-o 可执行文件
```
直接将 Mach-o 文件拖到Hopper 中即可
```


##二、加壳
- 1、 **什么是加壳？** <br> **理解1：**<br>只要是上传到 app store 的ipa 包，苹果都会自动对可执行文件（Mach-o 可执行文件）进行加壳操作，可以理解为，苹果会对 Mach-o 可执行文件进行加密。加壳后的mach
-o可执行文件，使用class-dump 和 hopper 是不能直接分析的。<br> **理解2：**<br>利用特殊的算法，对可执行文件的编码进行改变（比如： 压缩、加密），以达到保护程序的目的。<br>**理解3：**<br>Mach-o executable 文件加壳（加密）前后运行状态：<br> **加壳（加密）前**，启动程序是直接通过dyld文件(一种mach
-o动态链接编辑器)将Mach-o executable链接加载到内存中
![](/assets/Snip20180528_1.png)
**加壳（加密）后：**因为加壳后的mach-o 文件不能再被 dyld 直接链接了，在加壳后的mach-o 外面会有一个**壳程序** dyld 直接将可程序链接运行到内存中。
![](/assets/Snip20180528_2.png)




