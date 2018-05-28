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
**注意：不管是什么平台，像window、android 等 加壳都是这样一个原理**

##三、脱壳
- 1、**什么是脱壳？**<br> 摘掉壳程序，将未加密的Mach-o executable 文件还原出来（有人也称为“砸壳”）。

- 2、**脱壳主要有2种方式：**<br>硬脱壳、动态脱壳。

- 3、**什么是硬脱壳？**<br> 利用算法直接将加密后的 Mach-o executable 文件 解密出来，就是加密的逆运算。硬脱壳不需要运行程序。
![](/assets/Snip20180528_3.png)

- 4、**什么是动态脱壳？**<br> 因为 加壳后的Mach-o没办法直接在运行在内存中，需要连带可程序一起运行，因为可程序一旦运行后会对加密后的mach-o进行解密，这时我们可以直接将解密后的在内存中运行的mach-o 直接导出到硬盘上，称之为动态脱壳，动态脱壳不需要些解密算法，因为可程序运行时已经做了解密操作。![](/assets/Snip20180528_4.png)

- 5、**在iOS上 我们采用的是硬脱壳**<br> **iOS中的脱壳工**具：<br> **[Clutch](https://github.com/KJCracks/Clutch)** 、**[dumpdecrypted](https://github.com/stefanesser/dumpdecrypted)** 、AppCrackr、Crackulous









