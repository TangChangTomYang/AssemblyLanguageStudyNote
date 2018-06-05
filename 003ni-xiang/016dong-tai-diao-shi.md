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
**注意：**<br>1、/Developer 这个文件夹的权限是只读的。<br>2、默认情况下，/Developer/usr/bin 目录下的debugserver 权限是不够的（因为这个debugserver 是Xcode 帮着安装的，默认情况下只能调试Xcode安装的App，只有这个权限）。



####四、常用的LLDB













































