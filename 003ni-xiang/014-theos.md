有篇文章关于 Theos，可以[参考下](https://www.jianshu.com/p/307243ea40e4)

##一、相关概念了解

- 1、**什么是theos？**<br>theos 是一个越狱开发工具包，Theos 是越狱开发工具的首先(通过theos 我们可以做一些hook操作)。越狱开发中另一个常用工具是iOSOpenDev。

- 2、**在安装配置过程中，要保证本地已经安装了Homebrew，可以使用brew 命令来安装一些依赖包。**<br> brew 其实类似于Linux中的yum 或者 apt-get ，就是一个包管理工具。


- 3、**dpkg 是theos 依赖的工具之一，dpkg 是Debian Packager 的 缩写，我们可以使用 dpkg 来制作 deb， Theos  开发的插件都会以deb 的格式进行发布的。**<br>所以我们在安装Theos 之前要安装dpkg，当ran我们可以使用强大的brew来完成dpkg的安装。


##越狱插件开发流程

编写hook代码 --> 编译成 deb 安装包（越狱插件） -->安装到手机上



##一、 theos 的使用

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

- 5、**配置 Makefile文件中的 IP 与 端口**<br><br>**方式1：WiFi ssh 连接**
    ```
    export THEOS_DEVICE_IP=SSH服务器ip
    export THEOS_DEVICE_PORT=22    // SSH 使用的是22端口
    ```
**方式二：usb ssh 本地连接**

    ```
    export THEOS_DEVICE_IP=127.0.0.1  // 或者 localhost
    export THEOS_DEVICE_PORT=10010    // 本地使用10010和22端口绑定的，所以是10010， 填写绑定22端口的对应端口即可

    ```
    ![](/assets/Snip20180602_7.png)
    
- 6、**编写hook代码**
![](/assets/Snip20180602_12.png)














- 4、**安装过程中遇到的问题**

```
vim $THEOS/vendor/dm.pl/dm.pl
# use ...:Lzma
# use ...:Xz

vim $THEOS/makefiles/package/deb.mk
?= gzip
```
安装完成后，就可以完成了app 修改了

- 5、卸载




