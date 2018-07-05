####Runtime 运行时

- Objective-c 是一门动态性比较强的编程语言,根C/C++ 等语言有着很大的不同.

- Objective-c 的动态性是由Runtime API 来支撑的.

- Runtime API 提供的接口基本都是C语言的,源码由C/C++ 汇编语言编写.


####isa 详解


- 想要学些Runtime , 首先要了解它底层的一些常用数据结构,比如isa 指针.

- 在arm64 架构之前, isa 就是一个普通的指针,存储着Class Meta-Class 对象的内存地址.

- 从arm64架构开始,对isa 进行了优化,变成了一个共用体(union)结构,还使用位域来存储更多的信息.








