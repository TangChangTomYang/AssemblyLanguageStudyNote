

####RunLoop 的基本作用
- 保证程序的持续运行
- 处理API中的各种事件(触摸事件、定时事件、 等等)
- 节省CPU资源,提高程序的性能: 该做事做事,该休息休息

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