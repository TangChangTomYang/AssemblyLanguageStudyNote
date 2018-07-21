####锁

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
 ####2、os_unfaire_lock
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


 
 
 
 































