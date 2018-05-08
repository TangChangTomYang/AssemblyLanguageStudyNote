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

![](/assets/003.png)




