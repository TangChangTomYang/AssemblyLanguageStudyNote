##Reveal

- **Reveal 是一款调试iOS程序的UI界面的神器**
- **[官网](https://revealapp.com)**
- **[下载地址](https://revealapp.com/download)**
    - 建议下载至少Reveal4版本,支持usb连接调试,速度快.低版本的只能支持wifi连接调试.
    
####调试环境配置
刚刚下载安装的Reveal 一打开是没有任何效果的,我们需要经过配置后才能调试.
- **配置步骤主要如下:**
    - 1、在手机上通过Cydia 安装,软件源:http:apt.so/codermjlee (选择,威锋源-开发工具)
    ![](/assets/Snip20180524_1.png)
    安装完成后在设置中可以看见![](/assets/Snip20180524_2.png)
    - 2、 在手机上安装完成Reveal后，需要打开对应的app 那么调试时才能打开
    - 3、 下载并安装Reveal （下载地址：https://revealapp.com/download ）（破解版下载：http://xclient.info）
    - 3、 打开Reveal 后，点击help --> show Reveal Library in finder --> ios Library --> 拷贝 RevealServer 覆盖手机的 /library/RHRevealLoader/RevealServer文件（主要 需要确保手机端的 文件和电脑端的一样大才是覆盖了） 
    ![](/assets/Snip20180525_3.png)
    - 4、重启SpringBoard或者重启手机，也可以在手机终端输入命令
        - 重启SpringBoard 命令： killall SpringBoard
        - 重启手机命令： reboot
    - **注意：**    
        在电脑端安装好Reveal，在手机端安装好Reveal，将电脑端的RevealServer 拷贝到手机后还需要在手机的setting 中打开Reveal 并开启ReVeal 可以查看的对应的应用才能 调试相应的app 否则不能看见调试页面（这也是很多新手发现各个环节的设置都是正确的但是任然无法查看调试的原因）