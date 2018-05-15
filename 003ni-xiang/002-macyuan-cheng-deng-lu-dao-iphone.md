##一、SSH 远程登录了解

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
        -  \# 表示的是当前用户是超级权限（比如使用 root 账户登录）![](/assets/Snip20180513_9.png)

        -  $ 表示的是当前用户是普通权限（比如使用 mobile 账户登录）
        
        ![](/assets/Snip20180513_10.png)
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
    - **SSH-2**
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
<br>




##二、基于网络的SSH连接
####一、SSH的通信过程      
- **SSH 通信主要分为3个大的阶段**
    - **建立安全连接**  
    - **客户端认证，主要有2中方式** 
        - 方式1，基于密码的客户端认证
        - 方式2，基于秘钥的认证方式
    - **数据传输**   

####二、SSH 通信之 &emsp;建立安全连接过程详解

- 在建立安全连接的过程中，服务器会提供自己的身份证明(如： RSA公钥)。
    ![](/assets/屏幕快照 2018-05-13 下午9.55.14.png)
- 如果客户端没有存储过服务器端的公钥信息（没有和该SSH服务器建立过连接，通常建立连接后会存储服务端的RSA公钥），就会询问是否连接此服务器。![](/assets/屏幕快照 2018-05-13 下午9.57.05.png)
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
    - 查看SSH服务端的RSA公钥
    ```
    cd /etc/ssh
    ls -l
    cat ssh_host_rsa_key.pub
    ```

####三、SSH 通信之 &emsp;客户端认证
- **方式1**，基于密码的客户端认证
    - 使用账号和密码即可认证
        - 基于账号和密码的认证的弊端
            - 每次都要输入密码
            - 不安全
- **方式2**，基于秘钥的认证方式
    - 免密码认证
    - 最安全的一张认证方式
    - SSH 默认会优先采用这种认证方式
    
- **免密码认证, 过程详解**
    ![](/assets/屏幕快照 2018-05-14 上午6.40.35.png)
    - **密钥认证需要在客户端生成一对 rsa 公私钥，并将 rsa 公钥追加到SSH服务端的 authorized_keys文件中**
        - **在客户端生成rsa 公私钥对**，客户端公私钥，存储地址： **~/.ssh**
        ```
ssh-keygen -t rsa //指定算法并生成一对rsa公私钥
// openssh 默认采用的是 rsa 可以不指定，直接使用 
ssh-keygen
```
        - **.ssh 文件夹中有很多文件，现在对id_rsa、id_rsa.pub、known_hosts 这3个文件进行说明：**
                - **id_rsa** 存储的是当前用户生成的 rsa 私钥
                - **id_rsa.pub** 存储的是当前用户生成的 rsa 公钥，和id_rsa 是一对钥匙，在进行ssh 通信时会把 id_rsa.pub 发送给ssh 服务端 用作身份验证。
                - **known_hosts** 存储的是 SSH 服务端的rsa 公钥，公钥用来验证要建立连接的服务端是否是合法的（服务端合法验证至少包含2部分： IP 地址正确、rsa 公钥正确）
    - **将客户端 RSA 公钥追加到服务端authorized_keys文件中，只要在服务器端的authorized_keys文件中存储了RAS公钥，服务器就知道将来会有个客户端要连过来。** 
        - **追加方式1：**使用 ssh-copy-id 将客户端的公钥内容自动的追加到服务器的授权文件的尾部

        ```
         ssh-copy-id root@10.1.1.193
         // 这个指令会自动的将 客户端的RSA 公钥复制并追加到服务器端的authorized_keys文件中
        ```
        - **追加方式2：**手动操作。复制客户端的公钥到服务器某路径
            - 复制客户端的公钥到服务器某路径
            ```
            scp ~/.ssh/id_rsa.pub root@10.1.1.193:~/.ssh
            // 从 a 主机拷贝到b 主机的 ~/.ssh 目录
            // scp 是secure的缩写,是基于SSH登录进行安全的远程文件拷贝命令,把一个文件拷贝到远程的另外一台主机上.

            ``` 
            - 将一个文件中的内容追加到另一个文件
            ```
            cd .ssh
            cat id_rsa.pub >> authorized_keys
            // 注意如果 authorized_keys 文件不存在会自动创建这个文件
            ```
    - **注意:** 如果在服务器端中如果有存储客户的 RSA 公钥,还是需要输入 账号和密码要求连接,那么可能是服务器端的 authorized_keys 文件权限不够.
    ```
    chmod 755 ~
    chmod 755 ~/.ssh
    chmod 644 ~/.ssh/authorized_keys
    ```
<br>





##三、基于USB端口的SSH连接
####一、22端口
- **端口就是设备对外提供服务的窗口,每个端口都有个端口号(0~65535,g共计 6^16 个)**
- **有些端口是保留的,已经规定了用途,如如:**
    - 21端口提供FTP服务
    - 80端口提供HTTP服务
    - 22端口提供给SSH服务(可以查看 /etc/ssh/sshd_config 的port字段)
    - [更多保留端口号参考](https://baike.baidu.com/item/端口号/10883658#4%203)
- **iPhone 默认采用22端口进行SSH通信,采用的是TCP协议，也就是说，客户端是同SSH服务器的22端口建立的TCP连接**
![](/assets/屏幕快照 2018-05-14 下午10.39.31.png)



####二、SSH 通过 USB连接
- **默认情况下，由于SSH走的是TCP协议，mac 是通过网络连接的方式 SSH登录到iPhone，要求iphone 和 mac 连接同一个WiFi**
![](/assets/屏幕快照 2018-05-14 下午10.39.31.png)
- **为了加快传输速度，也可以通过USB连接的方式进行SSH登录**
    - **Mac 上有个服务程序 usbmuxd （开机会自动启动），可以将Mac的数据通过USB传输到iPhone**
    ```
    /System/Library/PrivateFrameworks/MobileDevice.framework/Resources/usbmuxd
    // 也可以这样查看
    cd  /System/Library/PrivateFrameworks/MobileDevice.framework/Resources
    ls -l 
    ```
    ![](/assets/屏幕快照 2018-05-14 下午10.52.40.png)
    
- **Mac 通过 USB 与 iPhone建立 SSH 主要步骤如下：**
    - **1. Mac 通过SSH的方式和自己的10010 建立SSH通信**
    - **2. Mac 直接把消息发送到自己的10010 端口，这样 usbmuxd 自己会把消息传递给 iPhone的 22端口通信**
    
####三、usbmuxd 的使用
- **step1、下载usbmuxd 工具包（下载 v1.0.8版本，主要用到里面的python 脚本： tcprelay.py）**
    - **[下载地址](https://cgit.sukimashita.com/usbmuxd.git)**
    - **我们只需要下载文件中的python client中的2个文件**![](/assets/屏幕快照 2018-05-15 上午6.46.59.png)
- **step2、将iPhone 的22端口（SSH端口）映射到Mac本地的10010 端口**（其实不一定是10010 端口，只要不是被保留的端口都可以）
    ```
    1. 切换到tcprelay.py 文件目录
    2. 运行 tcprelay.py 脚本 将服务器的 端口 与 客户端的端口映射
   
     即, 
    phtopy tcprelay.py -t 22:10010
    ```
  ![](/assets/屏幕快照 2018-05-15 上午6.58.42.png)
      
      - **说明：** python xxx.py 是执行python脚本的意思， 22：10010 是指映射服务端的22 端口 到客户端的10010 端口， -t 参数表示可以建立多条SSH连接的意思，如若只需要一条，可以不要。
      - **注意：** 在执行映射脚本后，终端会出现卡死状态，是对的，只需要开启新的终端工作即可
 
- **step3、 在客户端本机 建立SSH连接**
  ```
  ssh root@localhost -p 10010
  或者
  ssh root@127.0.0.1 -p 10010
  ```
          - **注意1：** 千万不要在使用 ssh root@服务器地址 这种方式连接了，因为如果你使用这种方式连接的话，还是走 wifi 网络的方式连接，不会走usb
          - **注意2：** 如果你是没有运行 python 脚本映射服务器的端口与客户端的22端口， 是不能ssh 连接到本地的![](/assets/屏幕快照 2018-05-15 上午7.13.01.png)
  - **只要本机的 SSH 建立后，以后就可以往本机的 对应端口写数据，自动就通过usb传到了服务器**
  
  
  
##四、文件拷贝

```
将数据拷贝到服务器（这种是通过网络传输）
scp ~/.ssh/id_rsa.pub root@10.0.0.193:~/.ssh

将数据拷贝到服务器（这种是通过usb传输,借助usbmuxb）
scp -P 10010 ~/.ssh/id_rsa.pub root@localhost:~/.shh 
```

##五、公钥 >> 授权文件
- **可以使用 ssh-copy-id root@主机地址，将客户端的公钥内容自动追加到服务器的授权文件的为尾部，也可以手动复制**
    - **客户端秘钥对 ~/.ssh/id_rsa.pub 和 ~/.ssh/id_rsa**
    - **客户端验证服务器身份文件 ~/.ssh/known_hosts**
    - **服务器端秘钥对地址：/etc/ssh/ssh_host_rsa_key.pub  和 /etc/ssh/ssh_host_rsa_key**
    - **服务器授权文件地址： ~/.ssh/authorized_keys**
    
- **手动复制客户端公钥到服务器某文件路径：scp ~/.ssh/id_rsa.pub root@192.168.1.105:~/.ssh, scp 是 secure copy 的缩写，是基于ssh 登录进行安全远程文件拷贝命令，把一个文件拷贝到远程另一台电脑上，上面 “~/.ssh/id_rsa.pub” 是要拷贝的文件路劲，“192.168.1.105”指的是远程主机，“:~/.ssh” 指的是远程主机的具体目标文件路劲 **
    - **注意：**在拷贝动作开始前有个类似建立 ssh 通信的过程，要密码，如果是密钥授权就不用

- **追加公钥内容到授权文件尾部： cat ~/id_rsa.pub >> ~/.ssh/authorized.keys**

- **创建.ss隐藏文件夹， mkdir .ssh**
- **删除文件， rm ~/.ssh/id_rsa.pub**

- **同一台电脑上拷贝文件，cp 源文件  目标路劲**




  
   
    
     
      
       
        
         
          
                                    
                                      
 
 ##六、服务器身份信息变更
 
 - **在建立安全连接的过程中，可能会遇到以下错误信息： 提示服务器的身份信息发生了变更**  
 ![](/assets/屏幕快照 2018-05-13 下午10.12.12.png)
     - **什么情况下，会出现这种错误**
        - ssh 客户端在和服务端建立连接时，服务器端会把RSA 公钥发送给客户端，客户端如果没和这个服务器端建立过SSH连接，就会将这个公钥保存，如果有链接过就会比较这次的公钥和上次存储的公钥是不是一样的，如果不是一样的（这次的公钥有变更）说明可能这次的服务器可能是其他人伪造的，也有可能是真的服务器出现了变更（服务器端换电脑了，但是IP地址设置的是同一个）。   
- **解决方案：** 
    - **方式1，删除存储的服务器端的公钥 （IP -> 公钥）**   
    ```
    cd ~/.ssh
    ls -l
    vim known_hosts
    ``` 
    - **方式2,重置服务器端的公钥信息（删除公钥信息）** 
    ```
    ssh-kengen -R 10.1.1.193
    ```     
    



      
                  
        
        
        
        

