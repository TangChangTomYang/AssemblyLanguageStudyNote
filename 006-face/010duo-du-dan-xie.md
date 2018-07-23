####一  atomic
- 1 **atomic 用于保证属性的 setter getter 的原子性操作,相当于在setter 和 getter 方法内部添加了线程安全的锁.**<br>
(1) 可以参考源码 objc4的objc-accessors.mm<br>
(2) 他并不能保证使用属性的过程是线程安全的.<br>
**问题:**<br>
(1) 既然 atomic  可以保证setter 和 getter 方法内部是线程安全的,为什么在ios 中我们还要使用 nonatomic 呢?<br>
**答:**<br>
1) 因为atomic 虽然是线程安全的,调用频率太高太耗性能了.



####二  iOS 中的读写安全操作
- **思考如何实现以下场景**<br>
(1)同一时间，只能有1个线程进行写的操作<br>
(2)同一时间，允许有多个线程进行读的操作<br>
(3)同一时间，不允许既有写的操作，又有读的操作
- 上面的场景就是典型的“多读单写”，经常用于文件等数据的读写操作，iOS中的实现方案有<br>
(1) pthread_rwlock：读写锁<br>
(2) dispatch_barrier_async：异步栅栏调用
-  pthread_rwlock 的简单使用,pthread_rwlock 在等待锁的时候是休眠的,具体使用如下:


```
#import "ViewController.h"
#import <pthread.h>

@interface ViewController ()
@property (assign, nonatomic) pthread_rwlock_t lock;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 初始化锁
    pthread_rwlock_init(&_lock, NULL);
    
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(queue, ^{
            [self read];
        });
        dispatch_async(queue, ^{
            [self write];
        });
    }
}


// 读操作 使用pthread_rwlock_rdlock 加锁,这样可以让多个线程同时读取
- (void)read {
    pthread_rwlock_rdlock(&_lock);
    sleep(1);
    NSLog(@"%s", __func__);  
    pthread_rwlock_unlock(&_lock);
}

// 写操作的方法 使用 pthread_rwlock_wrlock 加锁解锁,保证只有一个线程写和读
- (void)write{
    pthread_rwlock_wrlock(&_lock);
    sleep(1);
    NSLog(@"%s", __func__);
    pthread_rwlock_unlock(&_lock);
}

// 销毁是要释放读写锁
- (void)dealloc{
    pthread_rwlock_destroy(&_lock);
}

@end
```
**注意:**
虽然 pthread_rwlock 能保证多读 单写,但是有读操作的时候写操作是被锁住的,有写操作读操作是被锁住的,即多读单写的同时保证只有写 或只有读
-  dispatch_barrier_async：异步栅栏调用
 的简单使用,也可以保证多读单写

```
#import "ViewController.h"
#import <pthread.h>

@interface ViewController ()
// 栅栏函数必须使用同一个队列才能实现多读 单写的效果
@property (strong, nonatomic) dispatch_queue_t queue;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
//    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
//    queue.maxConcurrentOperationCount = 5;
    
//    dispatch_semaphore_create(5);
    
    self.queue = dispatch_queue_create("rw_queue", DISPATCH_QUEUE_CONCURRENT);
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(self.queue, ^{
            [self read];
        });
        
        dispatch_async(self.queue, ^{
            [self read];
        });
        
        dispatch_async(self.queue, ^{
            [self read];
        });
        
        dispatch_barrier_async(self.queue, ^{
            [self write];
        });
    }
}


- (void)read {
    sleep(1);
    NSLog(@"read");
}

- (void)write
{
    sleep(1);
    NSLog(@"write");
}

@end
```


