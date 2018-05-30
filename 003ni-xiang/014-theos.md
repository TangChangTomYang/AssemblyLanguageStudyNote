##一、 theos 的使用

- 1、**安装签名工具**
```
brew install ldid
```


- 2、**修改环境变量**
```
vim ~/.bash_profile
export THEOS=~/theos
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


