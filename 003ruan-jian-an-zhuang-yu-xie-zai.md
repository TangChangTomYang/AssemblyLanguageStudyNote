
####/etc/apt/sources.list 详解（了解）
- 1、**/etc/apt/source.list** 是包管理工具 **apt** 所用的记录软件包仓库位置的配置文件。
- 2、**source.list** 文件中的**条目**一般都是形如：
```
deb http://site.example.com/debian distribution component1 component2 component3
deb-src http://site.example.com/debian distribution component1 component2 component3
```
条目中的**第一个词**  **deb** 或 **deb-src** 表明了所获取的**软件包档案类型。**<br> **deb** 表示档案类型为二进制预编译软件包，一般我们所用的档案类型.<br> **deb-src** 表示档案类型为用于编译二进制软件包的源代码。<br><br> 条目中的**第二个词** 表示软件包所在的仓库的地址，我们可以更换仓库地址为其他地理位置更靠近自己的镜像来提高下载速度。
```
http://site.example.com/debian // 表示软件仓库地址
```
条目中的**第三个词** 跟在仓库地址后的是发行版。<br>发行版有两种分类方法:<br>一类是发行版的具体代号，如 xenial,trusty, precise 等；<br>还有一类则是发行版的发行类型，如oldstable, stable, testing 和 unstable。<br>另外，在发行版后还可能有进一步的指定，如 xenial-updates, trusty-security, stable-backports 等。<br><br>跟在发行版之后的就是软件包的具体分类了，可以有一个或多个。


<br><br><br><br>

##软件的安装于卸载
- **主要有3种2方式：** <br> **1、apt-get 方式的安装与卸载。**<br> **2、deb 包方式的安装** <br> **3、源码安装**

##一、 apt-get 软件安装与卸载
**apt-get 是ubuntu 提供的一种软件安装卸载机制，是一种服务器客户端模型，他把常用的软件（比如： tree命令、vsftpd 命令等等）放在服务器上，服务器有官方的和非官方的服务器，官方的服务器在欧洲，非官方的服务器(网易、搜狐、cn99等)在国内，一般最新的软件在官方服务器上，会被非官的服务器down到非官方的服务器上，本地的速度要快些。这种方式安装要求必须联网。**<br><br> **在客户端首先我们要选择服务器，然后通过 apt-get 命令抓取到本地， apt-get 安装软件的步骤： **<br><br> **step1:**<br> 更新源服务器列表（即选择服务器） 
```
sudo vi /etc/apt/sources.list   // 更新服务器
```
**step2:**<br>更新下载源（即将服务器端的软件列表（软件信息）下载到本地）
```
sudo apt-get update  // 更新软件源（把可以下载的软件信息下载到客户端），更新完成就可以查看那些软件可以下载了
```
**step3：**<br>查找某个软件
```
sudo apt-cache search xxx软件
```
**step4:**<br> 下载具体的软件
```
sudo apt-get install xxxx软件
```
**step5:**<br> 删除某个软件
```
sudo apt-get remove  xxxt软件
```
**step6:**<br>获取显示安装的软件的信息，说明、大小、版本等信息
```
sudo apt-cache show xxx软件
```
**step7:**<br>重新安装软件
```
sudo apt-get intall xxx软件  --reinstall
```
**step8：**<br>修复安装
```
sudo apt-get -f install
```
**step9:**<br> 删除软件，包含配置文件
```
sudo apt-get remove xxx软件 --purge
```
**step10:**<br> 更新已经安装的软件
```
sudo apt-get upgrate
```
**step11:**<br> 下载该包的源码
```
sudo apt-get source xxx软件
```
![](/assets/Snip20180601_2.png)








##二、deb 包安装
deb 安装就相当于把一个软件打好包，把这个包拷贝过来


- 1、**安装deb 软件包命令**(即安装软件)
```
sudo dpkg -i xxx.deb  // -i 表示的是install 的意思
```
- 2、**删除软件包命令**(即 卸载软件)
```
sudo dpkg -r xxx.deb  // -r 是remove 的意思
```
- 3、**连同配置文件一起删除**(即 连同配置文件一起卸载)
```
sudo dpkg -r --purge xxx.deb
```
-4、**查看软件包信息**
```
sudo dpkg -info xxx.deb
```
- 5、**查看文件拷贝详情命令**
```
sudo dpkg -L xxx.deb
```
- 6、**查看系统中已经安装软件包信息命令**
```
sudo dpkg -l
```
- 7、**重新配置软件包**
```
sudo dpkg-reconfigure xxx
```




##三、源码安装
源码安装，即给你的是.c 、.h源文件，你通过源码文件来安装软件

```
1、 解压缩源代码（比如有 xxx.tar.gz文件，tar zxvf xxx.tar.gz）
2、 cd dir
3、 .configure  //检测文件是否缺失、编译环境等，创建makefile，检测编译环境
4、 make  // 编译源码，生成库和可执行程序
5、 sudo make install // 把库和可执行程序，安装到系统路劲下

```









































































