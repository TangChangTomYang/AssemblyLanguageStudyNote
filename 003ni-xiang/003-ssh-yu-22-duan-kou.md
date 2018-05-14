####一、22端口
- **端口就是设备对外提供服务的窗口,每个端口都有个端口号(0~65535,g共计 6^16 个)**
- **有些端口是保留的,已经规定了用途,如如:**
    - 21端口提供FTP服务
    - 80端口提供HTTP服务
    - 22端口提供给SSH服务(可以查看 /etc/ssh/sshd_config 的port字段)
    - [更多保留端口号参考](https://baike.baidu.com/item/端口号/10883658#4%203)
- **iPhone 默认采用22端口进行SSH通信,采用的是TCP协议**
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
    
####三、usbmuxd 的使用一
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
  
          


      
    
    
    



