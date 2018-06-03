有篇文章关于 Theos，可以[参考下](https://www.jianshu.com/p/307243ea40e4)

##一、相关概念了解

- 1、**什么是theos？**<br>theos 是一个越狱开发工具包，Theos 是越狱开发工具的首先(通过theos 我们可以做一些hook操作)。越狱开发中另一个常用工具是iOSOpenDev。

- 2、**在安装配置过程中，要保证本地已经安装了Homebrew，可以使用brew 命令来安装一些依赖包。**<br> brew 其实类似于Linux中的yum 或者 apt-get ，就是一个包管理工具。


- 3、**dpkg 是theos 依赖的工具之一，dpkg 是Debian Packager 的 缩写，我们可以使用 dpkg 来制作 deb， Theos  开发的插件都会以deb 的格式进行发布的。**<br>所以我们在安装Theos 之前要安装dpkg，当ran我们可以使用强大的brew来完成dpkg的安装。


##越狱插件开发流程

编写hook代码 --> 编译成 deb 安装包（越狱插件） -->安装到手机上



##二、 theos 的使用

- 1、**使用[Homebrew](https://brew.sh/index_zh-cn.html) 安装 idid 签名工具**
```
brew install ldid  
```
**说明：**<br>1、因为很多工具被开发者上传到了brew，所以我们可以通过brew 来安装很多好用的工具。<br>2、ldid 是一个签名工具，theos 中使用ldid 签名，我们平时在 Xcode 用的是codesign 签名工具。<br>3、我们一般最好将ldid 安装在 ~/ 目录下<br>4、ldid 安装完成后后命令（可执行程序）都是放在安装目录下的 theos/bin 目录里面，即：`~/theos/bin` 下
<br>

- 2、**[修改(配置)环境变量](/004huan-jing-bian-liang.md)**
```
vim ~/.bash_profile     //  .bash_profile 是环境变量的配置文件
export THEOS=~/theos    // 添加一个THEOS环境变量，变量的值是`~/theos` 这个路劲
export PATH=$THEOS/bin:$PATH
```




- 3、**[下载theos](https://github.com/theos/theos/wiki)**,参考 wiki 详细操作，里面有说明。
```
git clone --recursive https://github.com/theos/theos.git $THEOS
```
**$THEOS** 表示是取出 环境变量的值，是一个路劲<BR>
**说明：** 这里虽然可以在github 中通过down 下载安装包，但是这里使用clone 可以把代码下的更全。**如果有下面这个文件就要注意了，只能用 git clone --recursive**，--recursive 是递归下载的意思，会把项目中的所有依赖全部递归下载完。 
![](/assets/Snip20180602_1.png)<br>
![](/assets/Snip20180602_2.png)

- 3、**新建项目（tweak项目（相当于hook项目））**
```
nic.pl
```
![](/assets/Snip20180602_3.png)
<br>
![](/assets/Snip20180602_4.png)


- 4、**tweak项目创建完成后直接拖拽到 subline text 中编写项目代码即可**
![](/assets/Snip20180602_5.png)

- 5、**配置 Makefile文件中的 IP 与 端口 环境变量**<br><br>**方式1：WiFi ssh 连接**
    ```
    export THEOS_DEVICE_IP=SSH服务器ip
    export THEOS_DEVICE_PORT=22    // SSH 使用的是22端口
    ```
**方式二：usb ssh 本地连接**

    ```
    export THEOS_DEVICE_IP=127.0.0.1  // 或者 localhost
    export THEOS_DEVICE_PORT=10010    // 本地使用10010和22端口绑定的，所以是10010， 填写绑定22端口的对应端口即可

    ```
    **方式三： 直接在本地mac 的.bash_profile 文件中配置环境变量，这样以后就不用每次都在 makefile中手动配置环境变量了**
```
$ vim ~/.bash_profile 
export THEOS_DEVICE_IP=127.0.0.1
export THEOS_DEVICE_PORT=10010
```
这样就不需要每次都到makefile中配置一次。
![](/assets/Snip20180602_7.png)
    
    
- 6、**编写hook代码**
![](/assets/Snip20180602_12.png)
**注意！！！：**<br> 如果实在 %hook 与 %end 之间写的代码，系统都会默认是覆盖原来的方法，里面的方法不会被认为是新方法。如果要让系统认为是新方法，需要在前面加 %new 来说明
![](/assets/Snip20180603_2.png)


- 6.1、**给hook 代码添加自己的资源文件图片）**<br>资源文件的添加有固定的写法，必须创建一个文件夹名为layout，这个layout 相当于是手机的根路径。![](/assets/Snip20180603_4.png)

- 6.2、**路径宏定义**

    ```
    #define YRFilePath(path) @"abc/abc" #path

    #path 在编译时会在动在 path 前后添加 "
    ```
    调用时
    ```
    直接： YRFilePath(具体path)  // 不用加@  和 ” 号
    ```





- 7、**切换到项目 目录 编译  安装项目**
**方式一：**
```
cd /Desktop/tingweak
make clean  // 清一下
make  //编译
make package //打包
make install
```
**方式二：**<br>**其实几个命令可以一起写：**
```
make clean && make &&  make package && make install 
```
编译打包结果：
![](/assets/Snip20180602_15.png)<br>
**说明：**打包过程中可能会报错，如下：<br>
![](/assets/Snip20180602_14.png)
**报错原因：<br>**是使用的theos工具，压缩的方式有问题，改成gzip 就可以了。注意我们修改了theos 工具中的压缩方式，makefile 中的压缩方式也要改成对应的才行。<br> <br> **(1)修改dm.pl文件，用 # 号注释掉下面2句**

    ```
    $vim $THEOS/vendor/dm.pl/dm.pl

    #use IO::Compress::Lzma;
    #use IO::Compress::Xz;
    ```
**(2)修改dem.mk 文件第6行的压缩方式为：gzip**
```
$ vim $THEOS/makefiles/package/deb.mk
_THEOS_PLATFORM_DPKG_DEB_COMPRESSION ?= gzip  //修改 压缩方式为：gzip
```
<br>

- 8、**删除插件**
![](/assets/Snip20180602_16.png)




        
##三、 theos 安装过程原理

- 1、 编写tweak 代码
- 2、 make  // 编译tweak代码为动态库（*.dylib）
- 3、 make package  // 将dylib 打包成deb文件
- 4、 make install //  将deb 文件传输到手机上，通过Cydia 安装deb
- 5、 插件（deb 安装包和 文件plist）会安装在：**/Library/MobileSubstrate/DynamicLibraries 文件夹中。<br> *.dylib 是tweak编译后的动态库文件<br> *.plist 是存放着需要hook的App ID 
- 6、当打开app 时<br>Cydia Substrate (Cydia 自动安装的插件)会让App 去加载对应的 dylib 动态库文件。<br> 修改App 内存中的代码逻辑，去执行dylib中的函数代码。
- 7、所以，theos 的tweak并不会对App 原来的可执行文件进行修改，仅仅是修改了内存中的代码逻辑而已。
- 8、疑问：<br> 1、未脱壳的App 是否支持tweak？<br>&emsp; 支持。因为tweak是在内存中实现的，并没有修改 .app 包中的可执行文件。<br><br>2、 tweak 效果是否永久有效？<br>&emsp; 取决于tweak 中用到的app代码是否被修改过。<br><br>3、 如过一旦更行app，tweak 会不会失效？<br> &emsp;取决于tweak 中用到的app代码是否被修改过。<br><br>4、未越狱的手机是否支持tweak？<br>&emsp: 不支持。<br><br>5、能不能对Swift、C 函数进行tweak <br>&emsp; 可以，方式和OC 不一样 <br><br> 6、能不能对游戏项目进行tweak？<br>&emsp: 可以，不过游戏大多是通过C++、C#编写的，而且类名、函数名会进行混淆操作。

![](/assets/Snip20180603_5.png)






##四、tweak 多文件开发步骤

**主要有以下2个步骤：**<br>**1、在Makefile 中设置需要编译的文件**(全路径，要保证通过这个路径能找到他)
![](/assets/Snip20180603_7.png)

![](/assets/Snip20180603_8.png)
<br>**2、导入文件**（导入的文件要使用全路径）
![](/assets/Snip20180603_10.png)



##五、Theos 资料查询相关
- 1、[目录结构](https://github.com/theos/theos/wiki/Structure)
- 2、[环境变量](http://iphonedevwiki.net/index.php/Thoes)
- 3、[logos语法](http://iphonedevwiki.net/index.php/Logos)<br>1、**%hook   %end** hook 一个类的开始和结束。<br> 2、**%log** 打印方法调用详情。<br>&emsp; 可以通过Xcode --> Window--> Devices and Simulators 查看日志<br>**3、HBDebulLog**跟NSLog类似 <br>4、**%new** 添加一个新的方法<br>5、**%c(className)**生成一个Class 对象，比如 %c(NSObject),类似于NSStringFromClass()、objc_getClass()<br>5、**%orig** 调用函数原来的代码逻辑 <br>6、**%ctor** 在加载动态库时调用。<br>7、**%dtor**在程序退出时调用 <br>8、**logify。pl** 可有将一个头文件快速转换成已经包含打印信息的xm文件
```
logify.pl xxx.h > xxx,xm
```
9、**如果有额外的资源文件（比如：图片、MP3），请放在项目的layout文件中，对应着手机的 根路径 /**











