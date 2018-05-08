##win32汇编 寄存器

- 如eax、ebx、ecx、edx、eip、esp、ebp、esi、edi等都是32位的寄存器
![win32寄存器](/assets/001.png)

- 段寄存器
    - CPU有两个不同的工作方式：实模式、保护模式            
    - 实模式：使用“段地址:偏移地址”的方式访问内存数据
    - 保护模式：装入段寄存器的不再是段地址，而是段选择符（Segment Selector），在编程过程中，使用偏移地址直接寻址即可
    
***  
***   
<br>

    
##win64汇编 寄存器

![win32寄存器](/assets/002.png)
<br>
![](/assets/003.png)

***  
***   
<br>

    
##AT&T汇编 vs Intel汇编

- 基于x86架构的处理器所使用的汇编指令一般有2种格式
- Intel汇编
    - DOS（8086处理器）、Windows
    - Windows派系 => VC编译器
- AT&T汇编
    - Linux、Unix、Mac OS、iOS（模拟器）
    - Unix派系 => GCC编译器
- AT&T 读作 “AT and T”（American Telephone & Telegraph的缩写）
- 作为iOS开发工程师，最主要的汇编语言是:
    - AT&T汇编 => iOS模拟器
    - ARM汇编 => iOS真机设备
    
####AT&T汇编 vs Intel汇编

![AT&T汇编 vs Intel汇编](/assets/004.png)

####AT&T汇编 vs Intel汇编 寻址方式
![](/assets/005.png)

##64位AT&T汇编的寄存器

- 有16个常用64位寄存器
    - %rax、%rbx、%rcx 、%rdx、%rsi、%rdi、%rbp、%rsp
    - %r8、%r9、%r10、%r11、%r12、%r13、%r14、%r15
- 寄存器的具体用途
    - %rax作为函数返回值使用
    - %rsp指向栈顶
    - %rdi、%rsi、%rdx、%rcx、%r8、%r9、%r10等寄存器用于存放函数参数

####64位AT&T汇编的寄存器

![](/assets/006.png)

####64位AT&T汇编 – 栈帧
![](/assets/007.png)
![](/assets/008.png)
![](/assets/009.png)
***
![](/assets/010.png)
![](/assets/011.png)
![](/assets/012.png)
***
![](/assets/013.png)
![](/assets/014.png)
![](/assets/015.png)


##常见代码反汇编

- sizeof
- a++ + a++ + a++
- if-else
- for
- switch和if效率

##编译器的优化






















