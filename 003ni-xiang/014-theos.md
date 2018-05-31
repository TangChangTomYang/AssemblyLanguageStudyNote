有篇文章关于 Theos，可以[参考下](https://www.jianshu.com/p/307243ea40e4)

##一、相关概念了解

- 1、**什么是theos？**<br>theos 是一个越狱开发工具包，Theos 是越狱开发工具的首先。越狱开发中另一个常用工具是iOSOpenDev。

- 2、**在安装配置过程中，要保证本地已经安装了Homebrew，可以使用brew 命令来安装一些依赖包。**<br> brew 其实类似于Linux中的yum 或者 apt-get ，就是一个包管理工具。


- 3、**dpkg 是theos 依赖的工具之一，dpkg 是Debian Packager 的 缩写，我们可以使用 dpkg 来制作 deb， Theos  开发的插件都会以deb 的格式进行发布的。**<br>所以我们在安装Theos 之前要安装dpkg，当ran我们可以使用强大的brew来完成dpkg的安装。



##一、 theos 的使用

- 1、**安装签名工具**
```
brew install ldid
```


- 2、**修改环境变量**
```
vim ~/.bash_profile
export THEOS=~/theos    // 即theos 文件所在路劲
export PATH=$THEOS/bin:$PATH
```

- 3、**[下载theos](https://github.com/theos/theos/wiki)**,参考 wiki 详细操作，里面有说明。
```
git clone --recursive https://github.com/theos/theos.git $THEOS
```


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

