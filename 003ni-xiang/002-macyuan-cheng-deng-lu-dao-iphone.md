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

- **在iPhone 上通过Cydia安装OpenSSH**
    - **添加软件源： http://apt.saurik.com**
    - **搜索OpenSSH并安装**
- **iphone 端OpenSSH的具体使用步骤，参考Cydia 中的OpenSSH的Description**
![](/assets/屏幕快照 2018-05-13 下午4.09.38.png)







####四、使用OpenSSH 远程登录iPhone的具体步骤
- **SSH是通过TCP协议通信的， 所以要确保Mac 和 iphone 在同一个局域网下，比如： 连接的是同一个WiFi，或路由器**
    - **1、在Mac的终端输入： ssh 用户名@服务器主机地址**, 注意，默认情况下root账户和mobile账户的密码都是alpine
    
    ```
    ssh root@10.1.1.168
    ```
        - 用户名，指的是iphone上的用户名，一般来说手机有多个用户名，root 是超级用户
        - 服务器主机地址，指的是手机当前的IP地址
    ![](/assets/屏幕快照 2018-05-13 下午4.37.24.png)

        - 提示要输用户名密码：Cydia 其实在使用步骤中已经明确说明，默认的root账户的密码是 “alpine”
        - 输入密码如果，终端中提示 root# 就表示已经远程登录成功
    - **2、在iOS系统中通常有2个常用的账户： root 和 mobile**
        - **root** 最高权限的账户，$HOME 是 /var/root, 而 mac 上的$HOME 是 /Users/用户名， 可以直接使用命令 **pwd**查看， 我们也可以通过命令 **echo $HOME** 来查看。
            - 说明： **pwd** 命令是查看当前所在路径， 而 **echo $HOME** 是查看当前用户根目录的意思， **echo** 是回显的意思， **$HOME** 代表用户根路劲。
        - **mobile** 普通权限账户，只能操作一些普通文件**（比如：不能在根路径 /  下创建文件，等等）**，不能操作系统级别的文件， $HOME 是/var/mobile
    
    - **3、细节：**
        -  \# 表示的是当前用户是超级权限（比如使用 root 账户登录）![](/assets/Snip20180513_10.png)

        -  $ 表示的是当前用户是普通权限（比如使用 mobile 账户登录）
        
        ![](/assets/Snip20180513_9.png)
    - **4、最好修改一下root 和 mobile y用户的登录密码**
        - 使用root 登录后分别使用 passwd 和 passwd mobile 修改root 账户的密码和 mobile 账户的权限。

        ![](/assets/Snip20180513_14.png)  
        ![](/assets/Snip20180513_16.png)
        如图，直接输入新密码即可修改完成，但是注意，修改了要记住，忘了就找不回来了
    - **4、退出 OpenSSH远程登录**
    ```
    exit
    ```
    
    
####五、SSH的版本
- **SSH 协议一共有2个版本**
    - **SSH-1**
    - **SSH-**
- **现在使用比较多的是SSH-2，客户端必须和服务端的SSH是同一个版本才能通信**
- **查看SSH 的版本信息**，查看配置文件的protocal 字段
    - **客户端：**      
    ```
    cd /etc/ssh
    cat ssh_config
    ```
    - **服务端:**
    ```
    cd  /etc/ssh
    cat sshd_config
    ```  

####六、SSH的通信过程      
- **SSH 通信主要分为3个大的阶段**
    - **建立安全连接**  
        - 在建立安全连接的过程中，服务器会提供自己的身份证明。
        ![](/assets/屏幕快照 2018-05-13 下午9.55.14.png)
        - 如果客户端并没有服务器端的公钥信息，就会询问是否连接此服务器。![](/assets/屏幕快照 2018-05-13 下午9.57.05.png)
        - 客户端清除 SSH服务端 的 RSA 公钥 (仅作了解)
        ```
        ssh-keygen -R 10.1.1.193
        ```
        - 查看客户端存储的SSH的服务器的RSA 公钥
        ```
        cd ~/.ssh
        ls -l
        cat known_hosts
        ```
        ![](/assets/屏幕快照 2018-05-13 下午10.04.37.png)
            
    - **客户端认证** 
        - 账号、密码是否正确等
    - **数据传输**     
                  

      
                  
        
        
        
        

