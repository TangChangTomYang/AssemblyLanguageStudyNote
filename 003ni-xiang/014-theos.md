有篇文章关于 Theos，可以[参考下](https://www.jianshu.com/p/307243ea40e4)

[yenei](#bottom)
[fs](#jump)
##一、相关概念了解

- 1、**什么是theos？**<br>theos 是一个越狱开发工具包，Theos 是越狱开发工具的首先。越狱开发中另一个常用工具是iOSOpenDev。

- 2、**在安装配置过程中，要保证本地已经安装了Homebrew，可以使用brew 命令来安装一些依赖包。**<br> brew 其实类似于Linux中的yum 或者 apt-get ，就是一个包管理工具。


- 3、**dpkg 是theos 依赖的工具之一，dpkg 是Debian Packager 的 缩写，我们可以使用 dpkg 来制作 deb， Theos  开发的插件都会以deb 的格式进行发布的。**<br>所以我们在安装Theos 之前要安装dpkg，当ran我们可以使用强大的brew来完成dpkg的安装。


##越狱插件开发流程

编写hook代码 --> 编译成 deb 安装包（越狱插件） -->安装到手机上



##一、 theos 的使用

- 1、**使用[Homebrew](https://brew.sh/index_zh-cn.html) 安装 idid 签名工具**

```
brew install ldid  

// ldid 是一个签名工具，theos 中使用ldid 签名
//在 Xcode 里面有个codesign 签名工具
```
补充：因为很多工具被开发者上传到了brew，所以我们可以通过brew 来安装很多好用的工具。


- 2、**修改环境变量**
```
vim ~/.bash_profile     //  .bash_profile 是环境变量的配置文件
export THEOS=~/theos    // 即theos 文件所在路劲
export PATH=$THEOS/bin:$PATH
```
**说明：** `export` 是导出的意思，`export THEOS=~/theos` 可以这样理解，导出一个环境变量 为  `THEOS`， 环境变量的值是 `~/theos`.


- 3、**[下载theos](https://github.com/theos/theos/wiki)**,参考 wiki 详细操作，里面有说明。
```
git clone --recursive https://github.com/theos/theos.git $THEOS
// $THEOS 表示是取出 环境变量的值，是一个路劲
```
**说明：** 这里虽然可以在github 中通过down 下载安装包，但是这里使用clone 可以把代码下的更全。**如果有下面这个文件就要注意了，只能用 git clone --recursive**，--recursive 是递归下载的意思，会把项目中的所有依赖全部递归下载完。 
![](/assets/Snip20180602_1.png)<br>
![](/assets/Snip20180602_2.png)

- 3、**新建项目**
```
nic.pl
```



- 4、**安装过程中遇到的问题**
```
vim $THEOS/vendor/dm.pl/dm.pl
# use ...:Lzma
# use ...:Xz

vim $THEOS/makefiles/package/deb.mk
?= gzip
```

<span id="jump">wp ssf </span>
<p id="bottom">wo shi dib </p>



