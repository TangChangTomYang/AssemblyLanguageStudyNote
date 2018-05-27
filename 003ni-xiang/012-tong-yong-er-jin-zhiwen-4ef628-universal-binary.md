####一、通用二进制文件（Universal Binary）
    - 同时使用于多种架构的二进制文件，Xcode 生成的mach-o 文件就是个通用二进制文件
    - 包含了多种不同架构的独立的二进制文件
![](/assets/Snip20180527_7.png)
具体来说，怎样才能使 Xcode 生成的Mach-o文件是 通用二进制文件呢？ 是由Xcode 的build setting 中的  **Architecture** 设置的参数决定的。
![](/assets/Snip20180527_8.png)
- 因为需要存储多种架构的代码，通用二进制文件通常比单一平台的二进制的文件大。
- 由于两种架构有共通的一些资源，所以并不会达到单一版本的两倍之多。
- 由于执行过程中，只调用一部分的代码，运行起来也不需要额外的内存。
- 因为文件比原来的要大，因此也被称为“胖二进制文件” fat Binary



####二、 Mach-o 文件瘦身
- 我们可以通过 File 命令来查看 mach-o 文件的具体类型， 也可以通过 lipo 命令来查看Mach-o 支持的架构
```
file test  // 查看具体的mach-o 文件的类型
```
输出
```
abc: Mach-O universal binary with 2 architectures: [i386: Mach-O executable i386] [x86_64: Mach-O 64-bit executable x86_64]
abc (for architecture i386):	Mach-O executable i386
abc (for architecture x86_64):	Mach-O 64-bit executable x86_64
```
说明abc 这个mach-o文件是一个通用二进制文件（胖二进制文件）
<br>
```
lipo -info abc
```
输出
```
Architectures in the fat file: abc are: i386 x86_64 
```

- 使用 lipo 命令给 mach-o 文件瘦身
```
lipo abc -thin i386 -output abc_i386   // 瘦身 为i386架构的文件
lipo -info abc_i386  
```
输出

```
Non-fat file: abc_i386 is architecture: i386

```

####三、将多个Mach-o文件合并成一个 胖二进制文件(通用二进制文件)
```
lipo -create abc_x86_64 abc_i386 -output abc_fat
// 将abc_x86_64 和 abc_i386 两个 mach-o 文件合并成一个 fat 二进制文件
```





