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



