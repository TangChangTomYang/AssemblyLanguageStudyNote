##一、没有使用ASLR的情况

####一、mach-o的虚拟内存分段
![](/assets/Snip20180609_17.png)

- 1、**首先我们要知道mach-o文件的结构分为3部分：<br>(1)、header<br>(2)、load commands <br> (2)、raw Data（在 raw data 至少主要包含：代码段（__TEXT）和数据段（__DATA））**


- 2、**raw data 内部主要段说明**<br>
VM Address： Virtual Memory Address， 内存地址，在内存中的位置。<br>
VM Size ： Virtual Memory Size， 内存大小，占用多少内存。<br><br>
    - __PAGEZERO 
        - VM Address : 0X00
        - VM Size:     0x1 0000 0000
    - __TEXT
        - VM Address : 0x1 0000 0000
        - VM Size:     0x380 C000
    - __DATA
        - VM Address : 0x1 0380 C000
        - VM Size:     0xD4 C000
    - __LINKEDIT
        - VM Address : 0x1 0455 8000
        - VM Size:     0x2C 8000
    
    
####二、ASLR 

- 1、**什么是ASLR？**<br>**(1)、Address Space Layout Randomization， 地址空间布局随机化**<br>**(2)、是一种针对缓冲区溢出的安全保护技术，通过对堆、栈、共享库映射等线性区布局的随机化，通过增加攻击者预测目的地址的难度，防止攻击者直接定位攻击代码的位置，达到阻止溢出攻击的目的的一种技术**<br> **(3)、iOS4.3 开始引入了ASLR技术**
    

        

    

        




