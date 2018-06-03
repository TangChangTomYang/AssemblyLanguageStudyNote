**[Hopper官网](https://www.hopperapp.com)**
**[参考文章](https://www.jianshu.com/p/5b465c9e2fb2)**


####一、什么是Hopper Disassmbler

- Hopper Disassmbler 能够将Mach-o 文件的机器语言代码反编译成汇编代码、OC 伪代码或者Swift 伪代码。

- 通过Hopper Disassembler 工具 我们可以还原代码的大概结构（比如：查看 xxx方法大概长什么样， 方法的实现细节（方法的编写和调用流程））

- Hopper Disassembler 的使用流程： 打开 把需要分析的框架的Mach-o 文件拽进去即可。

- 常用的快捷操作
    - shift + option + X
    找出哪里引用了这个方法 
    
    
####二、 使用Hopper Disassembler 分析具体的框架实战

&emsp;**Hopper分析的主要步骤：**
- 1、**找到要分析的框架的 Mach-o文件，主要有几种方式：**<br>方式1：直接在ipa 包中查找Mach-o文件<br>方式2：通过 Cycript 在手机上动态安装对应的库 ： YRLoadFramework('UIKit')， 安装成功后会显示对应的库文件，在手机上就可以找到了。

- 2、**将Mach-o文件拖拽到Hopper disassembler 中即可**












