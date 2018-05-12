####汇编语言的后缀名
- 在windows 平台下汇编语言的后缀名是 .asm
- 在mac Xcode 平台中汇编的后缀是 .s


**使用汇编语言实现C环境的函数:**

- c 语言实现

    - .h头文件
    ```
    int sum(int a, int b)
    ```
    - .c文件
    ```
    int sum(int a, int b){

        return (a + b);
    }
    ```

- 汇编语言实现
    - .h头文件
   
     ```
    int sum(int a, int b)
   
     ```
    - .s 汇编语言实现
    
    ```
    .global _sum    //说明这个汇编函数是全局的(这是汇编的声明部分)
    
    _sum:
        movq %rdi, %rax   // 默认情况下函数参数是依次放在 %rdi %rsi %rdx %rcx %r8 %r9 %10 等10 寄存器中的,专门存放函数参数的寄存器数量不足才会将函数参数入栈
        addq %rsi, %rax   // 函数的返回值一般都是存在%rax 寄存器中的
        retq
    


