```
ps -A //查看当前的进程
ps -A | grep 关键字  // 查看带关键字的进程
/var/mobile/Containers/Bundle/Application/      iphone 上应用程序安装路劲   
```


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







##三、脱壳 (介绍)
- 1、**什么是脱壳？**<br> 摘掉壳程序，将未加密的Mach-o executable 文件还原出来（有人也称为“砸壳”）。

- 2、**脱壳主要有2种方式：**<br>硬脱壳、动态脱壳。

- 3、**什么是硬脱壳？**<br> 利用算法直接将加密后的 Mach-o executable 文件 解密出来，就是加密的逆运算。硬脱壳不需要运行程序。
![](/assets/Snip20180528_3.png)

- 4、**什么是动态脱壳？**<br> 因为 加壳后的Mach-o没办法直接在运行在内存中，需要连带可程序一起运行，因为可程序一旦运行后会对加密后的mach-o进行解密，这时我们可以直接将解密后的在内存中运行的mach-o 直接导出到硬盘上，称之为动态脱壳，动态脱壳不需要些解密算法，因为可程序运行时已经做了解密操作。![](/assets/Snip20180528_4.png)

- 5、**在iOS上 我们采用的是硬脱壳**<br> **iOS中的脱壳工**具：<br> **[Clutch](https://github.com/KJCracks/Clutch)** 、**[dumpdecrypted](https://github.com/stefanesser/dumpdecrypted)** 、AppCrackr、Crackulous (AppCrackr、Crackulous 已经过时了)








##四、鉴定Mach-o 文件是否被加壳（加密）
**主要方法有：**<br> **方式一：**通过 class-dump 和 Hopper Disassembler 只能根据有没有解析出需要的信息来查看Mach-o文件是否被加密。<br>**方式二：**使用MachOView 工具查看load Commands的cryptid 字段，如果大于0 说明加密了。<br>**方式三：**通过otool 命令终端查看
```
otool // 直接在终端输入，打印出 otool 的用法
otool -l mach-o文件  // 列出当前mach-o文件的 所有load commands 信息
otool -l mach-o文件 | grep 关键字 //将所得结果 过滤关键字
otool -l testfile | grep crypt
```
![](/assets/Snip20180529_2.png)


##五、脱壳实践

- 5.1、**使用Clutch工具脱壳**（clutch 是离合器的意思）<br><br>**主要步骤如下：<br>** 1、下载**[Clutch可执行程序](https://github.com/KJCracks/Clutch/releases)**,并修改文件名为Clutch<br>![](/assets/Snip20180529_4.png)<br>2、将 Clutch 可执行程序copy 到手机的 `/usr/bin`目录，以便在手机上通过命令操作Clutch 工具<br>3、修改手机端Clutch文件权限保证，保证可执行
```
root# chmod +x /usr/bin/Clutdh  // 增加可执行权限
```
4、Clutch -i  列出手机上已经安装的需要脱壳的app 信息![](/assets/Snip20180529_5.png)<br>5、输入App 的序号或者bundleId 来脱壳
```
Clutch -d 9 // 根据序号脱壳
或者
Clutch -d com.tencent.QQMusic // 根据bundleID 脱壳
```
![](/assets/Snip20180529_6.png)
6、将手机上使用 api 头文件导出到Mac 并 找到其中的Mach-o executable 可执行程序
7、在mac 端使用class-dump 将mach-o 文件中的所有头文件全部导出
```
class-dump -H  QQMusic -o Headers 
// -H  QQMusic 指定要dump 的mach-o 文件，-o Headers 指定dump 出的头文件存放在Headers 中
``` 

**Clutch 常用操作说明**
    
```
-b --binary-dump     Only dump binary files from specified bundleID
-d --dump            Dump specified bundleID into .ipa file
-i --print-installed Print installed application
--clean              Clean /var/tmp/clutch directory
--version            Display version and exit
-? --help            Display this help and exit

```

<br>
- 5.2、**使用dumpdecryped工具脱壳**<br>**使用步骤如下：**<br>1、[下载dumpdecrypted源码](https://github.com/stefanesser/dumpdecrypted)![](/assets/Snip20180529_7.png) <br>2、将下载下来的源码编译成动态库（直接在终端执行 make 命令）<br>3、将dylib 文件拷贝到iphone 上（如果是 root 用户，建议方 /var/root 目录）
![](/assets/Snip20180530_1.png)<br> 4、终端进入手机的/var/root 目录。 <br>5、使用环境变量**DYLD_INSSERT_LIBRARIES** 将dylib 注入需要脱壳的可执行文件（可执行文件可路径可通过： ps -A 查看获取）

    ```
    root# cd /usr/root // 切换到 dumpdecrypted.dylib 动态库所在文件目录
    root# DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/mobile/Containers/Bundle/Application/6CEF92A9-AC6D-4F56-8D62-58F5D33B04E9/To-Do.app/To-Do
    // 指定环境变量 DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib ，和应用程序的路径
   
    ```
    ![](/assets/Snip20180530_3.png)
    
    6、使用otool 查看 Mach-o executable 文件的加密状态

    ```
    otool -l To-Do.decrypted | grep crypt
    ```
输出

    ```
    To-Do.decrypted:
    cryptoff 16384
    cryptsize 1081344
    cryptid 0
```  
说明 脱壳成功    



















