####常用LLDB指令
- 打印
    - print 、p  :打印 (打印的是指针地址)
    - po :打印对象,就是 print object 的简称
    
- 读取内存
    - memory read/数量 格式 字节数 内存地址
    ```
    x/3xw 0x10010
    //x 是memory指令的简写
    //3 表示要读取的 x 个单位
    //x 第二个x表示的是读取的格式 （x 表示16进制、f是浮点数、d是10进制）
    //w 表示去取的单位大小 （b:byte 字节、 h：half word2个字节、w：word4字节、 g:giant word 8字节）
    
    如下操作：
    (lldb) x/3xw   //读取 3个 w单位内存  以16进制显示
0x1004363ef: 0xba664e00 0x007fff99 0xe84c3500
(lldb) x/3fw
0x1004363fb: 0.0000000000000000000000000000000000000117548034
0x1004363ff: -0.000859424471
0x100436403: 0.0000000000000000000000000000000000000117547992
(lldb) x/3fd
0x100436407: 846825216
0x10043640b: 8388528
0x10043640f: 256
(lldb) 
    ```
    
