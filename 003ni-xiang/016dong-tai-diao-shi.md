##动态调试

####一、什么是动态调试？
![](/assets/Snip20180605_5.png)
当我们在Xcode 中打一个断点时，Xcode先将这个断点的信息发送给手机上安装的debugserver 服务器，这个debugserver 服务器将信息转发个App，App 在将信息反馈给debugserver，debugserver 再将信息发送给Xcode ，让后我们在Xcode 上面就能看见调试信息了，者就是动态调试的原理。


####二、Xcode 动态调试的原理
- 1、 **关于GCC、LLVM、LLDB**<br> Xcode 的**编译器**发展历程：GCC -> LLVM <br> Xcode 的**调试器**发展历程：GDB -> LLDB
- 2、**debugserver 一开始存放在mac 的Xcode 里面**<br>/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/9.1/DeveloperDiskImage.dmg/usr/bion/debugserver
- 3、当Xcode 识别到手机的设备时，Xcode 会自动将debugserver安装到iPhone 上。
- 4、Xcode 调试的局限性<br>一般情况下，只能调试通过Xcode安装的App<br><br>![](/assets/Snip20180605_4.png)

####三、动态调试任意App
- 1、iPhone上的debugserver 在哪里呢？
```
/Developer/usr/bin
```
**注意：这个路劲的文件可能会被系统自动删掉，如果没找到用真机调试一下即可**

- 2、**debugserver 权限的问题**<br>(1)、默认情况下，/Developer/usr/bin 目录下的debugserver 权限是不够的（因为这个debugserver 是Xcode 帮着安装的，默认情况下只能调试Xcode安装的App，只有这个权限，像来自App store 的App 是调试不了的）。<br>(2)、/Developer 这个文件夹的权限是只读的，需要注意一下。<br>3、如果需要调试其他的app，需要对debugServer重新签名权限，签上2个调试相关的权限
```
get-task-allow
task_for_pid-allow
```

- 3、**如何给debugserver签上全向**<br>iphone上的 **/Developer**目录是只读的，无法直接对**/Developer/usr/bin/debugserver**文件签名，需要先把debugserver复制到mac上，在通过ldid签名。

    ```
    //1.先将权限导出
    $ ldid -e debugserver > debugserver.entitlements

    //2.debugserver.entitlements文件中在添加2条权限
        get-task-allow = YES
    task_for_pid-allow  = YES

    //3.在将权限签回去即可
    ldid -Sdebugserver.entitlements  debugserver

    //4. 将文件装回iphone 即可(这是 不可能的事 /Developer 文件夹是只读的)
    ```
    
- 4、 注意直接将签名回去的debugserver 直接拖回去是不行的。是不能覆盖 **/Developer/usr/bin** 目录的。那怎么办呢？ 是这样的，到时我们需要在手机端启动Debugserver 这个服务，所以我们直接将 debugserver 拖到 /usr/bin目录下了，这样我们就可以在手机上通过终端来启动debugserver了。
debugserver 常用法：
```
Usage:
  debugserver host:port [program-name program-arg1 program-arg2 ...]
  debugserver /path/file [program-name program-arg1 program-arg2 ...]
  debugserver host:port --attach=<pid>
  debugserver /path/file --attach=<pid>
  debugserver host:port --attach=<process_name>
  debugserver /path/file --attach=<process_name>
  ```
最常用的一个指令
  ```
  // 表示监听10011端口，任何ip 都可以连接过来
  debugserver *:10011 -a WeChat       // -a 是attachment 附加的意思 
```
  








####四、常用的LLDB

- 1、**LLDB 指令格式**
```
<command> [<subcommand> [<subcommand> ...]] <action>  [-options  [option-value]] [argment [argment ...]]
```
**说明：**<br>(1)、凡是在指令说明书里面 看到的 `[]` 表示这部分是可以省略的。 <br>(2)、一般的指令格式都是： 命令、子命令、动作(比如： 向设置东西呢、删除东西呢、还是添加东西 等)、选项(比如： -n、-d、-i)、参数。 比如：<br><br>**给 test方法设置断点：**
```
breakpoint set -n test
或者
breakpoint set --name test
```
**breakpoint 是：**`<command>`     <br> **set是：**`<action>`        <br> **-n是：**`<options>`    <br>**test是：**`<arguments>  <br> <br> **hlep 指令的用法：** <br>直接在命令的签名加 help 来查看命令帮助说明，如：

  ``` 
  help breakpoint  // 查看breakpoint 命令的帮助信息

  help breakpoint set // 查看breakpoint set  命令的帮助信息
  依次类推，一层一层的查看帮助

  ```















































