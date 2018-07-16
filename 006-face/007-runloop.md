

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
    - UITrackingRunLoopMode,界面跟踪mode,用于ScrollView 跟踪触滑动,保证界面滑动时不受其他Mode的影响.

   




