

####一、mach-o的虚拟内存分段

- 1、**首先我们要知道mach-o文件的结构分为3部分：<br>(1)、header<br>(2)、load commands <br> (2)、raw Data（在 raw data 至少主要包含：代码段（__TEXT）和数据段（__DATA））**


- 2、**raw data 内部主要段说明**
VM Address： Virtual Memory Address， 内存地址，在内存中的位置。
VM Size ： Virtual Memory Size， 内存大小，占用多少内存。
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

        

    

        




