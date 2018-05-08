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
    - Windows派系 -> VC编译器
- AT&T汇编
    - Linux、Unix、Mac OS、iOS（模拟器）
    - Unix派系 -> GCC编译器
- AT&T 读作 “AT and T”（American Telephone & Telegraph的缩写）
- 作为iOS开发工程师，最主要的汇编语言是:
    - AT&T汇编 -> iOS模拟器
    - ARM汇编 -> iOS真机设备




