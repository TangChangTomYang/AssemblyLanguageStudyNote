####一、KVC 面试题
- 1、**通过 KVC 赋值会不会触发 KVO?**
只要通过KVC 能够赋值成功,不论原来对象中是否有set属性方法,就会触发KVO.

- 2、** KVC 的赋值\取值是怎样的一个过程?**


####二、KVC 详解

- 1、**KVC 全称 key-Value Coding ,俗称"键值编码", 可以通过一个key 来访问某个属性,常见的API 有:**
![](/assets/Snip20180624_7.png)

- 2、 **setValue: forKey:  和 setValue: forKeyPath: 的主要区别:**

    ```
    Person *pson = [[Person alloc]];
    pson setValue:@(20) forKey:@"age"];
    pson setValue:@(20) forKeyPath:@"age"];

    // 以上这种2个方法都可以,但是下面这种操作 setValue: forKey:  不能
    pson setValue:@"张三" forKeyPath:@"father.address"];

    // 只有forKeyPath: 才能使用属性 . 下去,代表的是一个路径
    ```






