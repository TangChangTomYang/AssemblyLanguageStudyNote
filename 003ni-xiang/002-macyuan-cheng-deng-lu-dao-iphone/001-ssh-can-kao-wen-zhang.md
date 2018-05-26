[参考文章](https://blog.csdn.net/langzxz/article/details/79133985)

##iOS逆向学习之 Mac 登录到 iPhone

####1、登录
-  Mac 登录到 iPhone 是通过终端的命令行，iOS和Mac 都是基于 Darwin（苹果的一个基于Unix的开源系统内核），所以iOS同样支持终端操作，上次越狱的时候就在手机上安装Terminal,用来执行了一下命令。在逆向工程中，经常会通过命令行来操作iPhone，但是在手机上打命令太费劲了，所以就有了Mac登录到iPhone的需求。
 Mac 登录到 iPhone 是通过 SSH(Secure Shell) 协议进行登录, OpenSSH是SSH协议的免费开源实现，可以通过OpenSSH的方式让Mac远程登录到iPhone。
 在iPhone上通过Cydia 安装 OpenSSH 工具，软件源 http://apt.saurik.com/
<p></p>
![](https://img-blog.csdn.net/20180122214435864?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGFuZ3p4eg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<p></p>


- 画红框的里面有写怎么应用这个工具。
红框下面有一个 Root Password How-To 里面有怎么修改默认密码的教程，为啥要改密码呢，因为2009年 Ikee 蠕虫利用安装了SSH服务器却没有修改默认root密码这个事儿，感染了好好多iOS手机。
言归正传，开始登录，首先你要查到iPhone的WiFi地址，假如是192.168.1.100，然后打开Mac的终端，输入命令后回车

 ```
ssh root@192.168.1.100  
```
- 会有提示如下：

 ```
The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.  
RSA key fingerprint is SHA256:L7lgqr7eQfYWlZ8UjGSquTD95ofoyQ1mswmhjQa8Urc.  
Are you sure you want to continue connecting (yes/no)?  
```
- 询问你是否继续连接 RSA key fingerprint 为 SHA256:L7lgqr7eQfYWlZ8UjGSquTD95ofoyQ1mswmhjQa8Urc 的服务器， 这个就是SSH公钥，那SSH公钥是什么呢，在下面会有简单说明，如果用过Git或者懂HTTPS的人应该会懂的。
输入命令回车

 ```
yes  
```

- 再输入yes之前可以先打开Mac上的文件 ~/.ssh/known_hosts 文件，等输入yes之后可以看看这个文件会在文件的结尾追加类似下面这段

 ```
192.168.1.100 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJKgxE2QDn5EEUE9ITtyl2lJ23C3gNxonvgSiRGuQVNCk2cX6/bvvNoIBfzLN5jPGUiR7qzYIcz1J82file09S4Qfyf42sC9EttR+Roy3Jy6a8/jpN9+fJIMi6RvfYKmVsJFJ5WaRryMSMU1WAcRYq3UnOskwtHHZfUHkXthF223og5ReL81IEcLU5xfq6u+Ft7+qxltJ9ACQsvxDKLDWzuE0iNyz3nDKiIuj6CDOjBfYaGU1k2MIp74zXT8ixrODtc9CR/XpLR1fu5ME9oPY2HGUgMVdHD6Bj6l7AdpJXyyg05Feab1ZEBf29t2/zxKjGYStqRehKaIy8qJRgmwJf 
```
- 上面这段内容就和手机上/etc/ssh/ssh_host_rsa_key.pub 文件里的内容一样，如何查看文件呢，等通过命令行连接上手机，然后用vim查看，下面会有vim的简单应用，或者通过Cydia下载iFile查看，这是一个老牌的第三方文件管理app。
输入yes回车后就会让输入密码提示 root@192.168.1.100's password: ，然后输入下面命令后回车

 ```
alpine 
```
- （记得要修改密码）
会出现 yande-iPhone:~ root# 
说明已经进入root账号的$HOME目录，，输入命令

 ```
pwd
```
- 就会打印出当前目录 /var/root，当前用户的权限是最高的，可以在任意地方增删改查文件，也可以调用reboot命令重启iOS。还有一个mobile用户，普通权限，无法调用reboot命令，只能操作一些普通文件，不能操作系统级别的文件。
输入下面命令退出

 ```
 exit
 
 ```
 
- 然后输入
```
ssh mobile@192.168.1.100 
```
- 输入密码
````
alpine  
```
就进入了mobile账户的$HOME目录 /var/mobile。


####2、免密登录

 - SSH的通信过程可以分为3大主要阶段：建立安全连接，客户端认证，数据传输
 -  建立安全连接就是前面输入yes的那个阶段，服务器（iPhone）发送公钥等信息给客户端（Mac），就是/etc/ssh/ssh_host_rsa_key.pub文件信息，如果客户端并无服务端的公钥信息，就会询问是否连接此服务器，然后输入yes，安全连接建立了。
 - 然后就是客户端认证，客户端认证有两种方式，
  - 一种就是基于密码的客户端认证，就像上面的那种方式。
  - 还有一种就是基于秘钥的客户端认证，免密码认证，也是最安全的一种认证方式。
方法是在客户端生成SSH的公钥私钥，然后将公钥追加到授权文件（~/.ssh/authorized_keys）的尾部,就像上面服务端的公钥追加到客户端~/.ssh/known_hosts文件的尾部一样。

- 首先生成一对向管理的秘钥：公钥（public key）、私钥（private key），在Mac的终端输入命令行

 ```
生成rsa 密钥对
ssh-keygen 
也可以使用命令 
ssh-keygen -t rsa // 在使用ssh 时默认使用的是rsa 加密，所以-t rsa 有时也是可以不写的表示默认， -t rsa的意思是指定算法

 ```
一路敲回车即可，最终
生成的公钥：~/.ssh/id_rsa.pub
生成的私钥：~/.ssh/id_rsa

- 将公钥追加到authorized_keys文件尾部,执行命令
```
这时一种快捷方式，也可以使用手动追加方式
ssh-copy-id root@192.168.1.100  
```
- 然后输入密码
```
alpine 
```
追加成功再次执行登录命令  ssh root@192.168.1.100 就不用输入密码直接登录了。<br>
如果追加命令出错，也可以手动的将公钥内容粘贴到~/.ssh/authorized_keys文件的尾部，如果没有这个文件，那就用命令创建它们，如果还是有问题，那就有可能是文件权限问题，修改文件权限就好了。
```
chmod 755 ~  
chmod 755 ~/.ssh  
chmod 644 ~/.ssh/authorized_keys 
```

####3、通过USB进行SSH登录

- 默认情况下，由于SSH走的是TCP协议，Mac是通过网络连接的方式SSH登录到iPhone的，要求iPhone必须连接WiFi，但是有的时候网络连接不是很好，在终端上敲一个字符要反应半天，所以为了加快传输速度，我们可以通过USB连接的方式进行SSH登录。
- Ma上有个服务程序 usbmuxd （开机自动启动），可以将数据通过USB传输到手机上，该程序的位置在

 ```
/System/Library/PrivateFrameworks/MobileDevice.framework/Versions/A/Resources/usbmuxd
```
- 默认情况下iPhone是使用22端口进行SSH通信，就是客户端（Mac）通过WiFi连接到服务端（iPhone）的22端口，
而通过USB连接呢，就是客户端SSH登录数据先发到自己的端口上，然后利用usbmuxd程序通过usb连接线转发到服务端的22端口上。自己的端口选择哪个呢？去百度搜，选择一个不被占用的端口，有好多端口都是保留端口。比如说我这里用10001端口
- 那如何使用usbmuxd这个程序呢，首先去下载一个工具包
[点击下载](http://cgit.sukimashita.com/usbmuxd.git/snapshot/usbmuxd-1.0.8.tar.gz),然后解压，里面会有一堆文件，咱只用到python-client目录下的tcprelay.py和usbmuxd.py两文件，复制到工作目录下，然后cd到工作目录，
```
python tcprelay.py -t 22:10001 
```
- 然后会输出 Forwarding local port 10001 to remote port 22，这就说明端口映射完毕，想和手机的22端口通信，直接跟Mac 本地的10001端口通信就可以了
这个终端不要关，然后咱在另起一中端，输入以下命令，二选一

 ```
ssh root@localhost -p 10001  
ssh root@127.0.0.1 -p 10001  
```
- 然后会出现
```
The authenticity of host '[localhost]:10001 ([127.0.0.1]:10001)' can't be established.  
RSA key fingerprint is SHA256:L7lgqr7eQfYWlZ8UjGSquTD95ofoyQ1mswmhjQa8Urc.  
Are you sure you want to continue connecting (yes/no)?  
```
- 这个和通过WiFi连接的时候一样，也是需要将服务端（iPhone）的SSH公钥追加到~/.ssh/known_hosts 文件中，有兴趣可以看看两次追加的文本有啥不一样。
然后就是客户端认证，基于密码的客户端认证或者是基于秘钥的客户端认证。
-  还有一点，每次USB登陆都需要执行上面的命令，一模一样，现在我们可以执行将上面的命令写成sh文件，不需要去记端口号了，直接执行sh文件就好了。<br><br>
创建一个文本文件  usb.sh, 里面的内容是 python tcprelay.py -t 22:10001<br><br>
再创建一个文本文件 login.sh 里面的内容是 ssh root@localhost -p 10001
- 要确保 shell文件和 tcprelay.py 、usbmuxd.py文件在同一目录下，
然后开启两个终端，一个终端执行 
```
sh usb.sh  
```
- 另一个终端执行
```
sh login.sh  
```
就登录成功了。


####4、最后补充两个小知识点
- 1 将文件拷贝到iPhone上，执行命令
```
scp -P 10001 文件.pdf root@localhost:~/文件.pdf  
```
注意P是大写，这样就将文件拷贝到手机上了。<br>

- 2 要在终端编辑文本内容，可以通过Cydia安装一个vim，名字叫 Vi IMproved的插件。创建一个~/.inputrc 文件
```
set convert-meta off  
set output-meta on  
set meta-flag on  
set input-meta on  
```
将上面的内容写进去。
然后重启终端，再连接到iPhone上，就可以在iOS终端输入中文了。
- 2018.02.10添加
再添加一个使用usbmuxd通过USB进行SSH的方式，上面那个是使用python，下面这个通过一个可执行文件exec
从[Google Code](https://code.google.com/archive/p/iphonetunnel-usbmuxconnectbyport/downloads)下载[itnl_rev8.zip](://blog.csdn.net/langzxz/article/details/79133985)解压,然后在终端cd到itnl_rev8目录下，执行命令
```
./itnl --iport 22 --lport 10001  
```
然后再另起一终端执行login.sh脚本就好了












