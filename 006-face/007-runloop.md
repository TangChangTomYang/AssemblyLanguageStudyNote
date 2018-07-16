

####RunLoop 的基本作用
- 保证程序的持续运行
- 处理API中的各种事件(触摸事件、定时事件、 等等)
- 节省CPU资源,提高程序的性能: 该做事做事,该休息休息
- ... ...

####RunLoop 对象

- 1、**iOS 中有2套API来访问和使用RunLoop**
    - Foundation: NSRunLoop
    - Core Foundation:CFRunLoopRef<br><br>
    - NSRunLoop 和CFRunLoopRef 都代表着RunLoop对象,NSRunLoop 是基于CFRunLoopRef 的一层OC包装,CFRunLoopRef 是开源的,地址:(https://opensource.apple/tarballs/CF/)
    
    

####RunLoop 与线程的关系
- 每条线程都有唯一的一个与之对应的 runloop 对象.
- runloop 保存在一个全局的Dictionary 里,线程作为key, Runloop 作为value.
- 线程刚创建时并没有runloop对象,runloop 会在第一次获取它时创建.
- runloop 会在线程结束时销毁.
- 主线程的runloop已经自动获取,子线程的默认没有开启runloop


####获取RunLoop
- 获取当前线程的runloop
```
[NSRunLoop currentRunLoop];
CFRunLoopGetCurrent();
```

- 获取主线程的RunLoop
```
[NSRunLoop MainRunLoop];
CFRunLoopGetmain();
```



####RunLoop 相关的类
- Core Foundation 中关于RunLoop的5个类
    - CFRunLoopRef
    - CFRunLoopModeRef
    - CFRunLoopSourceRef
    - CFRunLoopTimerRef
    - CFRunLoopObserverRef
  ![](/assets/RUNLOOP.png)  ![](/assets/RUNLOOPMODE.png)
   ![](/assets/Snip20180716_1.png) <br>
   简单的说就是,runloop 里面有一堆的mode, 每个mode里面有 source0  source1 observers  timers
   
   
   
   
####CFRunLoopModeRef 
- CFRunLoopRef 代表RunLoop 的运行模式
- 一个RunLoop 包含若干个Mode 又包含若干个Source0/Source1/Timer/Observer
- RunLoop启动时只能选择其中的一个Mode,作为currentMode
- 如果需要切换mode,只能退出当前的Loop,再重新选择一个mode进入.
    - 不同组(不同mode)的Source0/Source1/Timer/Observer 能分隔开来,互不影响.
- **如果Mode里面没有任何的Source0/Souce1/Timer/Observer,runloop 会立即退出**
- 常见的2种Mode
    - kCFRunLoopDefaultMode(NSDefaultRunLoopMode),App 的默认Mode,通常主线程在这个Mode 下运行.
    - UITrackingRunLoopMode,界面跟踪mode,用于ScrollView 追踪触摸滑动,保证界面滑动时不受其他Mode的影响.
    
    

####RunLoop 的运行逻辑

- 1、 Source0  以下这几种情况都是通过 Source0 来处理的
    - 触摸事件处理
    - performSelector:onThread: 
    <br><br>
    **可以通过函数调用栈查看具体细节:如下(看 frame #8)**<br>
    在断点模式下,在lldb 中输入 bt 就可以打印当前的函数调用栈
    ![](/assets/Snip20180717_2.png)
    
    
- 2、Source1  
    - 基于Port的线程间通信
    ![](/assets/Snip20180717_3.png)
    - 系统事件的捕捉(比如触摸事件最开始是通过source1捕捉,在包装成source0来处理的)
    

- 3、Timer 以下都属于timer范畴
    - NSTimer
    - performSelector:withObject:afterDelay:
    
    
- 4、Observers 
    - 主要用于监听RunLoop 的状态
    - autoRelease 自动释放池也是通过observer来进行的.(睡觉前处理)
    - UI刷新(BeforeWating,比如runloop监听到要睡觉了,他就会在runloop睡觉前刷新页面)
    ```
    比如:
    我们在某个方法中写了这行代码
    {
        ... ...
        self.view.backgroundColor = [UIColor redColor];
        ... ...
    }
    并不是代码一过这一行就设置颜色,不是这样的,是先执行了代码,系统记着有这么个操作,当RunLoop 中的Observers 观察到RunLoop 将要睡觉时,在睡觉前就会修改颜色(即UI刷新)
    ```
    

**一句话概括RunLoop运行逻辑:**
一个runLoop 里面有很多的 Modes,但是在一时刻只有一个currentMode(即正在处理的mode),每个Mode里面呢有 source0\ source1\timer\ Observers ,source0\ source1\timer\ Observers 里面都有对应需要处理的各种事件,runloop在运行期间就在不停的处理CurrentMode 里对应的各种事件.
<br><br>
- **RunLoop 处理事件大致流程:**
```
01- 通知Observers: 进入Loop
02- 通知Observers: 即将处理Timers
03- 通知Observers: 即将处理Sources
04- 处理Blocks
05- 处理Source0 (可能会再次处理block)
06- 如果存在Source1,就跳到第8步
07- 通知Observers: 开始休眠(等待消息唤醒)
08- 通知Observers: 结束休眠(被某个消息唤醒)
    001- 处理Timer
    002- 处理GCD Async To Main Queue
    003- 处理Source1
09- 处理Blocks
10- 根据前面的执行结果,决定如何操作
    001- 回到第2步
    002- 退出Loop
11- 通知Observers: 退出Loop
```




