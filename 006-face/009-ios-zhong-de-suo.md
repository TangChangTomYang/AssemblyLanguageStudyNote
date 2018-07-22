####锁

高级锁 --> 自旋锁 (访问不到就 while(1) ;)
低级锁 --> 互斥锁 (访问不到就线程就睡觉)


- iOS 中的线程同步方案:<br>
(1) OSSpinLock<br>
(2) os_unfair_lock<br>
(3) pthread_mutex<br>
(4) dispatch_semaphore<br>
(5) dispatch_queue(DISPATCH_QUEUE_SERIAL)<br>
(6) NSLock<br>
(7) NSRecursiveLock<br>
(8) NSCondition<br>
(9) NSConditionLock<br>
(10) @synchronized<br>



#### 1、** OSSpinLock** 叫做 `自旋锁` ,等待锁的线程会处于忙等(busy-wait) 状态,一直占着CPU的资源.<br><br>
**自旋锁的主要应用场景: **<br>**存钱/取钱**, 因为存钱和取钱是在不同的方法中直线,当有人取钱时就不能存钱,有人存钱时就不能取前, 也就是说自旋锁,可以办到在 A方法中加锁,在A方法中的锁解锁前, B方法中的锁也是锁着的.<br><br>
**自旋锁的使用:**

 ```
 //导入osspinlock头文件
 #import <libkern/OSAtomic.h>

 // 初始化 OSSPinLock 自旋锁
 self.lock = OS_SPINLOCK_INIT;

 // 自旋锁加锁
 OSSpinLockLock(&_lock);
    
 {
 // 需要锁住的代码

 ... ...
 }
 // 自旋锁解锁
 OSSpinLockUnlock(&_lock);
 ```
 **自旋锁注意点:**<br>
 目前自旋锁已经不再安全,可能会出现反转问题,主要原因如下:<br>
 (1) 多线程的原理是,CPU 在不同的线程之间不停的调度,给人的感觉就像是有多条线程在同时执行一样,因为是多条线程之间不停的调度,CPU 会优先调用线程优先级高的线程, 在一段时间里可能低优先级的线程没有机会被调用.<br>
 (2) 因为自旋锁在访问被锁代码时是处于忙等状态 ,想当与 while(1), 比如一条优先级很高的线程,处在 忙等状态,等着一条优先级很低的线程(正在访问锁住资源的线程)解开自旋锁的锁,由于正在访问资源的现场优先级很低,可能就没机会被CPU 调度,因此有可能锁一直打不开,产生类似于死锁的现象,因此不安全.<br>
 (3) 因为有优先级反转的问题,苹果不推荐使用(其实自旋锁是效率比较高的一种锁).
 
 <br>
 ***
 ####2、os_unfaire_lock (osspinlock 的改进版) 是一种互斥锁 会休眠.
 - os_unfair_lock 是用于取代 osspinlock,从ios 10 起开始支持.<br>
 从底层调用看,等待os_unfair_lock锁的线程会处于休眠状态,而非忙等状态(osspinlock 是处于忙等状态)<br><br>
 **使用:**<br>
    ```
    // 导入os_unfair_lock 的头文件
   #import <os/lock.h>

   // 创建os_unfair_lock
    self.unfairelock = OS_UNFAIR_LOCK_INIT;
 
    //加锁
    os_unfair_lock(&_lock);
    
    {
    // 需要锁住的代码

    ... ...
    }
    // 解锁
    os_unfair_lock(&_lock);
    ```
  
 <br>
***  
 ####3、pthread_mutex  互斥锁
 - mutex 叫做 `互斥所`, 等待锁的线程会处于休眠状态.<br><br>
 **互斥锁主要有2中使用方式:**<br>
 (1) 一般的互斥锁,不能解决像递归调用这个类的锁问题.具体使用步骤如下:<br>
 **step1:初始化锁:(一般锁)**
 
    ```
    #import "MutexDemo.h"
    #import <pthread.h>

    @interface MutexDemo()
    @property (assign, nonatomic) pthread_mutex_t     ticketMutex;
    @property (assign, nonatomic) pthread_mutex_t moneyMutex;
    @end

    @implementation MutexDemo

    - (void)__initMutex:(pthread_mutex_t *)mutex{
    // 静态初始化(不推荐)
    //pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

    // 初始化属性
    pthread_mutexattr_t attr;
    pthread_mutexattr_init(&attr);
    pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_DEFAULT);
    PTHREAD_MUTEX_RECURSIVE
    // 初始化锁
    pthread_mutex_init(mutex, &attr);

    // 初始化锁
    //pthread_mutex_init(mutex, NULL);

    // 销毁属性
    //pthread_mutexattr_destroy(&attr);
}

    - (instancetype)init{
        if (self = [super init]) {
        [self __initMutex:&_ticketMutex];
        [self __initMutex:&_moneyMutex];
        }
        return self;
    }

    ```
**step2:使用互斥锁(场景1)锁住1处代码**

    ```
    // 应用场景1: 卖票
    - (void)__saleTicket{
        pthread_mutex_lock(&_ticketMutex);
    
        // 需要锁住的代码
        [super __saleTicket];
    
        pthread_mutex_unlock(&_ticketMutex);
    }
    ```
**step2:使用互斥锁(场景1)锁住A处代码,解锁前其它地方也不能访问**

    ```
    // 在存钱时就不能取钱, 在取钱时就不能存钱
    // 存钱
    - (void)__saveMoney{
        pthread_mutex_lock(&_moneyMutex);
        // 需要锁住的代码    
        [super __saveMoney];
        pthread_mutex_unlock(&_moneyMutex);
    }

    // 取钱
    - (void)__drawMoney{
        pthread_mutex_lock(&_moneyMutex);
        [super __drawMoney];
        pthread_mutex_unlock(&_moneyMutex);
    }
    ```
    **step3: 释放互斥锁**
    ```
    // 使用互斥锁,在对象销毁前需要释放锁
    - (void)dealloc{
        pthread_mutex_destroy(&_moneyMutex);
        pthread_mutex_destroy(&_ticketMutex);
    }

    @end
    ```
(2)互斥锁,递归锁的使用,可以解决一个方法被递归调用时,死锁的现象<br>
**step1: 创建互斥锁 递归所**

    ```
    - (void)__initMutex:(pthread_mutex_t *)mutex{
        // 静态初始化(不推荐)
        //pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

        // 初始化属性
        pthread_mutexattr_t attr;
        pthread_mutexattr_init(&attr);
        // 注意,类型必须 是PTHREAD_MUTEX_RECURSIVE
        pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_RECURSIVE);
        // 初始化锁
        pthread_mutex_init(mutex, &attr);

        // 销毁属性
        //pthread_mutexattr_destroy(&attr);
    }
    ```
**step2 使用互斥锁 递归所**

    ```
    // 递归方法 使用互斥锁
    - (void)saleTicket{
        pthread_mutex_lock(&_ticketMutex);
        NSLog(@"%s",__func__);
        [self saleTicket];
        pthread_mutex_unlock(&_ticketMutex);
    }
    ```
    
(3)互斥锁, 条件互斥锁 (主要应用场景: 生产者 -- 消费者 模式)
    
**step1:  创建 互斥锁 和 互斥锁条件**

``` 
#import "MutexDemo3.h"
#import <pthread.h>
@interface MutexDemo3()
// 互斥锁
@property (assign, nonatomic) pthread_mutex_t mutex;
// 互斥锁 条件
@property (assign, nonatomic) pthread_cond_t cond;
@property (strong, nonatomic) NSMutableArray *data;
@end

@implementation MutexDemo3

- (instancetype)init{
if (self = [super init]) {
    // 初始化属性
    pthread_mutexattr_t attr;
    pthread_mutexattr_init(&attr);
    pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_RECURSIVE);
    // 初始化锁
    pthread_mutex_init(&_mutex, &attr);
    // 销毁属性
    pthread_mutexattr_destroy(&attr);
        
    // 初始化条件
    pthread_cond_init(&_cond, NULL);
        
    self.data = [NSMutableArray array];
}
return self;
}

```
    
**step2 测试生产者 消费者 模式**
    
```
- (void)otherTest{
   [[[NSThread alloc] initWithTarget:self selector:@selector(__remove) object:nil] start];
    
   [[[NSThread alloc] initWithTarget:self selector:@selector(__add) object:nil] start];
}
```


**step3 条件互斥锁实现**
```
// 生产者-消费者模式

// 线程1
// 删除数组中的元素
- (void)__remove
{
    pthread_mutex_lock(&_mutex);
    NSLog(@"__remove - begin");
    
    if (self.data.count == 0) {
        // 等待
        pthread_cond_wait(&_cond, &_mutex);
    }
    
    [self.data removeLastObject];
    NSLog(@"删除了元素");
    
    pthread_mutex_unlock(&_mutex);
}

// 线程2
// 往数组中添加元素
- (void)__add{
    pthread_mutex_lock(&_mutex);
    
    sleep(1);
    
    [self.data addObject:@"Test"];
    NSLog(@"添加了元素");
    
    // 信号
    pthread_cond_signal(&_cond);
    // 广播
//    pthread_cond_broadcast(&_cond);
    
    pthread_mutex_unlock(&_mutex);
}

```
**step4 条件互斥锁 销毁**
```
- (void)dealloc
{
    pthread_mutex_destroy(&_mutex);
    pthread_cond_destroy(&_cond);
}

@end
```
**怎样来理解 条件互斥锁呢?**<br>
其实他的运行逻辑是这样的:<br>
(1) 当我们的程序(A线程)在调用方法: `- (void)__remove`时, 如果执行到下面这行
```
if (self.data.count == 0) {
    // 等待
    pthread_cond_wait(&_cond, &_mutex);
}
```
如果判断到 `if (self.data.count == 0)` 条件成立,就会执行 `pthread_cond_wait(&_cond, &_mutex);` 这一句代码, 这句代码一执行,这时 自旋锁会 先解锁且让当前线程(A线程)睡眠在这里,程序暂时不往下执行.<br>
(2) 当我们的程序(B线程)在调用方法: `- (void)__add`时, 发现当前的 互斥锁是解开的,就会往下执行,并且执行到 `pthread_cond_signal(&_cond);
` 这句代码时 就会给互斥锁发一个信号消息,这时 睡眠在 `pthread_cond_wait(&_cond, &_mutex);` 处的A线程就会被唤醒,唤醒后`pthread_cond_wait(&_cond, &_mutex);` 同时会对当前线程进行加锁,代码继续往前执行. 这样就完成了 生产者 --- 消费者模式的加锁.



<br>
***
####4、 NSLock 
- NSLock 是对 pthread_mutex 互斥锁的封装.
![](/assets/Snip20180722_1.png)

<br>
***
####5、 NSRecursiveLock 
- NSRecursiveLock 是对 pthread_mutex的递归锁封装,API跟 NSLock 基本一致,只是他是一种递归所.


<br>
***
####6、 NSCondition
- NSCondition 是对pthread_mutex  和 pthread_mutex condition  的封装,即包含了锁和条件锁的封装.因此 NSCondition 具备互斥锁 和 互斥条件的功能. (主要功能: 加锁 解锁 等待)
 ![](/assets/Snip20180722_2.png)
 比如:wait 和 signal 的封装如下:<br><br>
 wait 是当条件不满足时,休眠 等待条件的意思
 ![](/assets/Snip20180722_5.png)<br><br>
 signal 是当条件满足通知wait 的线程唤醒,其实还有个broadcast也是唤醒的意思,signal 是唤醒一条睡眠的,broadcast是唤醒多个睡眠的. 
 ![](/assets/Snip20180722_3.png)
 
 
 
 <br>
***
####7、 NSConditionLock
是一种 对 NSCondition 的封装,能办到 让多个子线程按照某种顺序(某种条件), 顺序的执行的.具体使用如下: 

```

#import "NSConditionLockDemo.h"

@interface NSConditionLockDemo()
@property (strong, nonatomic) NSConditionLock *conditionLock;
@end

@implementation NSConditionLockDemo

- (instancetype)init
{
    if (self = [super init]) {
        self.conditionLock = [[NSConditionLock alloc] initWithCondition:1];
    }
    return self;
}

- (void)otherTest
{
    [[[NSThread alloc] initWithTarget:self selector:@selector(__one) object:nil] start];
    
    [[[NSThread alloc] initWithTarget:self selector:@selector(__two) object:nil] start];
    
    [[[NSThread alloc] initWithTarget:self selector:@selector(__three) object:nil] start];
}

- (void)__one{
    // 当锁的条件==1 时才会执行加锁,才会执行下面的代码,从而达到控制线程的执行效果
    [self.conditionLock lockWhenCondition:1];
    
    NSLog(@"__one");
    sleep(1);
    
    [self.conditionLock unlockWithCondition:2];
}

- (void)__two{
    // 当锁的条件==1 时才会执行加锁,才会执行下面的代码
    [self.conditionLock lockWhenCondition:2];
    
    NSLog(@"__two");
    sleep(1);
    
    [self.conditionLock unlockWithCondition:3];
}

- (void)__three{
    // 当锁的条件==1 时才会执行加锁,才会执行下面的代码
    [self.conditionLock lockWhenCondition:3];
    
    NSLog(@"__three");
    
    [self.conditionLock unlock];
}

@end
```


<br>
***
####8、semaphore 信号量

- semaphore 可以用来控制一个方法,在同一个时间段内最多允许几条线程访问,以达到数据安全的目的.



 



 


 
 
 
 































