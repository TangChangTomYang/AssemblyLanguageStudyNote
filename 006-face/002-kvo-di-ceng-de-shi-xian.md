####一、iOS 用什么方式实现一个对象的KVO?(KVO 实现的本质)

- 1、**当给类添加kvo后系统会利用runtime API 动态生成一个该类的子类(比如:NSKVONotifying_类名),并让该实例对象(instance 对象)的isa 指向这个子类.(如果移除了kvo后,又会让isa 指向会 原来的类)**

- 2、**当我们给某个类的 age 属性 添加了kvo后系统会自动在新的子类中添加4个方法:(setAge:,class,dealloc,_isKVOA)**

- 3、**当修改instance 对象的 age属性 时,会调用Foundation的 - NSSetxxxValueAndNotify函数,在此函数内会调用以下方法:**<br>(1)、willChangeValueForKey:<br>(2)、父类原来的setter方法<br>(3)、didChangeValueForKey:, 且在didChangeValueForKey: 方法内部会调用 监听者的 observeValueForKeyPath:ofObject:change:context:方法,触发监听方法.


- 2、**如何手动触发KVO?**<br>(1)、调用实例对象的 willChageVAlueForKey:<br>(2)、调用实例对象的 didChangeValueForKey: <br> (3)、 之后系统就会触发观察者的监听方法了

 
- 3、**直接修改成员变量的值会触发KVO吗?**<br> 直接修改成员变量的值(比如: _age=20)是不会触发kvo 的,只有通过set方法和setValue:forKey: 才会触发KVO
 
 
 
 ####二、KVO 详解
 - 1、 **KVO 的全称是Key-Value_Observeing,俗称"键值监听", 可以用来监听某个对象的属性的改变**
 ![](/assets/Snip20180624_3.png)
 
 
 - 2、**未使用KVO 时,实例对象(instance对象)的isa指针 指向的 class 对象结构**
 ![](/assets/Snip20180624_2.png)
 
 
 - 3 **使用KVO 后,实例对象(instance对象)的isa指针 指向的 class 对象结构(发生了变化,指向了另一个class 对象(原来类的子类))**
 ![](/assets/Snip20180624_5.png)
 
 
 


