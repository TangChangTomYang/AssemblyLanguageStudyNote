####一、汇编语言的后缀名
- 在windows 平台下汇编语言的后缀名是 .asm
- 在mac Xcode 平台中汇编的后缀是 .s

<br>
####二、使用汇编语言实现C环境的函数:

- **c 语言实现**

    - **.h头文件**
    ```
    int sum(int a, int b)
    ```
    - **.c文件**
    ```
    int sum(int a, int b){

        return (a + b);
    }
    ```

- **汇编语言实现**
    - **.h头文件**
   
     ```
    int sum(int a, int b)
   
     ```
    - **.s 汇编语言实现**
    
    ```
    .global _sum    //说明这个汇编函数是全局的(这是汇编的声明部分)
    
    _sum:
        movq %rdi, %rax   // 默认情况下函数参数是依次放在 %rdi %rsi %rdx %rcx %r8 %r9 %10 等10 寄存器中的,专门存放函数参数的寄存器数量不足才会将函数参数入栈
        addq %rsi, %rax   // 函数的返回值一般都是存在%rax 寄存器中的
        retq
        
        ```
        
- **其他地方调用函数**
    ```
   int result = sum(10,20);
    ```
    
- **内联汇编函数**

```
int num1 = 10;
int num2 = 20;
int result;

__asm__(
    "addq %%rbx, %%rax"  // 将2个寄存器数据相加。
    : "=a"(result)    // =a 的意思是取出a %rax 寄存器中的值，存入 result 中
    : "a"(num1), "b"(num2)   // 这一行是输入， a 就是 %rax 寄存器， b 就是%rbx 寄存器 ，"a"(num1) 的意思是将 num1 的数值存入对应寄存器
);

NSLog(@"%ld",result);
```
    


