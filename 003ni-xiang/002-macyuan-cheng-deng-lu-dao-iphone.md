####一、Mac 远程登录到iphone 
- **我们经常在Mac的终端上，通过敲一些命令来执行一些操作。**
- **iOS 和Mac OSX 都是基于Darwin（苹果的一种基于Unix的开源系统内核），所以iOS中同样支持终端的命令行操作。**
    - **在逆向工程中，我们经常会通过命令行来操纵iPhone**
- **为了能够让Mac终端中的命令行能作用在iPhone上，我们的让Mac和iPhone建立连接**
    - **通过Mac远程登录到iPhone的方式建立连接**
    ![](/assets/屏幕快照 2018-05-13 下午3.49.44.png)
    

####二、SSH、 OpenSSH
- **什么是SSH？ **
    - **SSH 是Secure Shell 的缩写，意味“安全外壳协议”，是一张为远程登录提供安全保证的协议**
    ![](/assets/屏幕快照 2018-05-13 下午3.52.45.png)
    - **使用SSH，可以把所有的传输的数据进行加密，“中间人”攻击方式就不可能实现，能防止DNS欺骗和IP欺骗**，通过SSH方式传输，可以防止sniffer 抓取数据。
    - **sniffer** 是一个软件工具
        - 嗅探器
        - 抓取网络数据，抓取网络数据包
- **什么是OpenSSH？**
    - **首先SSH 是一个协议，要有人去实现它，而OpenSSH 就是SSH协议的免费开源实现。**
    - **我们可以通过OpenSSH的方式让我们的Mac 远程登录到我们的iPhone。**
    
####三、使用OpenSSH远程登录iPhone**
- **首先，我们的Mac默认是支持OpenSSH的，但是我们的iPhone默认是不支持OpenSSH的，为了让我们的iPhone支持OpenSSH，我们需要通过Cydia 在iphone上安装OpenSSH**

