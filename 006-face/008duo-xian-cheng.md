####多线程面试题

- 1 **你理解的多线程?**
![](/assets/Snip20180719_1.png)
其实所有的多线程都是基于pthread 来实现的,我们经常用到的加锁\解锁也会用到pthread.

- 2 ios 中的多线程方案有哪几种? 你更倾向于哪一种?

- 3 你在项目中用过GCD码? 


- 4 GCD的队列类型

- 5 说一下OperationQueue 和 GCD 的区别,以及各自的优势?


- 6 线程安全的处理手段有哪些?


- 7 OC 你了解的锁有哪些? 


- 8 自悬锁和互斥所的区别? 

- 9 使用以上锁需要哪些注意?

- 10 C/C++/OC 任选一,实现自旋锁或互斥锁


- 11 **多线程中几个容易混淆的术语: 同步 异步 并发 串行**<br><br>
(1) **同步** 和 **异步** 主要影响是: 能 不能开启新线程.<br>
&emsp;&emsp;  **同步**: 在**当前**线程中执行任务,**不具备**开启新线程的能力<br>
&emsp;&emsp;  **异步**: 在**新线程**中执行任务,**具备**开线程能力.
(2) **并发**和**串行**主要影响:任务的执行方式<br>
&emsp;&emsp; **并发:** **多个**任务并发(同时)执行<br>
&emsp;&emsp; **串行:** **一个**任务执行完后,在执行下一个任务.


- 12 **请问下面的代码执行结果是什么?**
![](/assets/Snip20180720_2.png)<br>
**答:**<br>
(1) 打印结果是 1  3<br>
(2) 因为 `- (void)performSelector:(SEL)aSelector withObject:(nullable id)anArgument afterDelay:(NSTimeInterval)delay;` 这个方法其实是往当前的runloop 中添加了一个定时器来执行对应的方法. 默认情况下子线程的runloop是没有运行了,自然添加到子线程中的timer 也是不会执行的,因此结果是 1 3<br>
(3) 如果让runloop 跑起来,那么就可以执行,如下如:![](/assets/Snip20180720_3.png)<br>打印结果是 1 3 2 (因为定时器是要等runloop 再次被唤醒才会执行,所以先执行 1 3)<br>
(4)拓展: 也就是说以后凡是看见方法中带有 delay 的,如果是在子线程调用都要注意一下runloop 的问题.





####多线程安全隐患解决方案

- 解决方案: 使用 **线程同步**技术(同步就是协同步调,按预定的先后顺序j进行)



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







