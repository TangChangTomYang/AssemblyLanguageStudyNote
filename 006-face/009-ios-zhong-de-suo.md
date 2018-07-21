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



-  1** OSSpinLock** 叫做 `自旋锁` ,等待锁的线程会处于忙等(busy-wait) 状态,一直占着CPU的资源.<br>自旋锁的主要应用场景: 存钱/取钱, 因为存钱和取钱是在不同的方法中直线,当有人取钱时就不能存钱,有人存钱时就不能取前, 也就是说自旋锁,可以办到在 A方法中加锁,在A方法中的锁解锁前, B方法中的锁也是锁着的.

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































