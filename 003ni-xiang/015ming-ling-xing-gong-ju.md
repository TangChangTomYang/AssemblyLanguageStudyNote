##iOS 开发命令行工具

####一、命令行工具的本质
- 其实命令行工具就是一个 mach-o executable 文件。只是不同平台的命令行工具的架构不一样，比如，在iphone上用到的命令行工具其实和iOS App一样是Arm 架构的只是没有配置文件、没有资源文件、不能显示，而在mac 上使用的是x86 架构的。
- 因此我们在开发命令行工具时，要首先明确平台的架构（用在哪个系统上 MacOS 、 还是iOS 等等）
- 一句话概括，其实命令行工具和App的可执行文件 差不多。
<br><br>参考代码如下：

```

#import <UIKit/UIKit.h>

// 导入Mach-o 的头文件
#import <mach-o/fat.h>
#import <mach-o/loader.h>



// 关于命令行工具的参数说明:
// int argc 是执行命令行工具的参数的总和
// char * argv[] 是传入的参数的数组，其中第argv[0] 是当前可执行文件的路径，从argv[1] 开始才是传入的参数
int main(int argc, char * argv[]) {
    @autoreleasepool {
        
    //  如果要对传入的参数做处理可以这样写
        if (argc == 1) {
            NSLog(@"请传入额外参数----");
            
             NSLog(@"-i  查询列表");
            return 0;
        }
        
        
        if (strcmp(argv[1], "-l") != 0) {
            NSLog(@"参数有误");
            NSLog(@"-i  查询列表");
            return 0;
        }
        
        NSString *filePath = @"/var/mobile/Containers/Bundle/Application/BBA92172-46FA-4A53-A490-B1D57CD10406/OneTravel.app/OneTravel";
        NSFileHandle *fileHandle = [NSFileHandle fileHandleForReadingAtPath:filePath];
        
        // 获取magic num mach-o 文件类型的数据长度
        int len = sizeof(uint32_t); // 4字节
        NSData *magicData =  [fileHandle readDataOfLength:len];//标识文件类型
        uint32_t magicNum = 0;
       
        [magicData getBytes:&magicNum range:NSMakeRange(0, len)];
        
        
        if (magicNum == FAT_CIGAM || magicNum == FAT_MAGIC) { //大端 小端
            NSLog(@"64为 FAT 文件");
        }
        else if (magicNum == MH_MAGIC || magicNum == MH_CIGAM) { //大端 小端
            NSLog(@"非64位 架构 文件");
        }
        else if (magicNum == MH_MAGIC_64 || magicNum == MH_CIGAM_64) { //大端 小端
            NSLog(@"64位 架构 文件");
        }
        else{
            NSLog(@"读取失败 0x%x",magicNum);
            
        }
        
        // 句柄操作完成要关闭
        [fileHandle closeFile];
        return 0;
    }
}

```
调试命令行工具参数
![](/assets/Snip20180606_2.png)


####二、命令行工具的权限问题


####三、Mach-o 识别

