####一、学习条件
- **至少1年iOS开发经验**

- **调试设备**
    - 建议至少iphone 5s（因为从5s 开始支持arm64）
    - 或者至少 ipad air，ipad mini2（因需支持arm64）
    
- iOS 系统不要太高，最好是iOS9.1 （完美越狱）


####二、越狱的优缺点
- **什么是越狱（iOS jailbreak）？**
    - 利用iOS系统的漏洞，获取iOS系统的最高权限（root权限），解开之前的各种限制（是合法行文）
    
    ![](/assets/屏幕快照 2018-05-13 上午8.21.53.png)
- **越狱的优点**
    - **打造个性化、与众不同的iphone**
        - 自由安装各种实用的插件、主题、App。
        - 修改系统的App的一些行为。
    - **自由安装非APP store来源的App。**
        - 付费App 秒变 免费App
        - 未越狱iphone 安装app 途径
            - AppStore
            - 真机调试
            - 通过证书打包签名ipa安装
    - **灵活管理文件系统，让iphone 可以像U盘一样使用**
    - **给开发镇提供逆向的工程环境**

- **越狱的缺点**
    - **不予保修。**
    - **费电，越狱后的iOS系统会常驻一些进程，耗电速度提升10%~20%.**
    - **在新的iOS固件版本出来时，不能及时更新。**
        - 每个新版本的固件，都会修复上一个版本的越狱漏洞，使越狱失效。
        - 如果需要保持越狱状态，要等待新等待越狱程序发布时，才能使用新的x相应固件。
    - **不再受iOS系统默认的安全保护。容易被恶意的软件攻击，个人隐私有被窃取的风险。**
    - **如果安装了不稳定的插件，容易让系统变的不稳定、变慢，甚至出现白苹果。**

####三、完美越狱和不完美越狱
- **完美越狱**
    - **完美越狱后的手机可以正常的开关机和重启。**
- **不完美越狱**
    - **iphone一旦关机后再开机，就会一直停留在白苹果状态**
    - **或者能正常开关机，但是越狱失效**
- **[越狱方法推荐,pp助手](http//jailbreak.25pp.com)**

####四、如何判断是否越狱成功？
- **手机桌面是否有Cydia图标**
![](/assets/屏幕快照 2018-05-13 上午8.52.56.png)

- **工具判断（比如pp助手），打开软件，插上手机**
![](/assets/屏幕快照 2018-05-13 上午8.54.04.png)

####五、Cydia？
- **Cydia 你可以理解成 越狱后的 “App store”**
    - **可以在Cydia中安装各种第三方的软件（插件、补丁、app）**
    
    ![](/assets/屏幕快照 2018-05-13 上午8.52.56.png)


- **Cydia 的作者**
    - **jay freeman，网名sauirk**
    
    ![](/assets/屏幕快照 2018-05-13 上午9.01.39.png)
    
    
####六、通过Cydia 安装软件
- **第一步、给Cydia 添加软件源**


![](/assets/添加Cydia软件源.png)
- **从已经添加的Cydia 软件源中选择对应的软件安装即可**

####七、SpringBoard
- **有时通过Cydia安装完插件后，可能会出现以下界面**
![](/assets/屏幕快照 2018-05-13 上午10.47.21.png)
    - SpringBoard 其实就相当于iOS的桌面。
    
    
####八、iPhone 上逆向必备软件安装
- **Apple file conduit "2"（conduit是管道的意思）**
    - **Apple file conduit "2" 补丁的作用**
        - 可以访问整个iOS设备的文件系统，类似的补丁还有： **afc2、afc2add**
    - **安装Apple file conduit 的软件源**
        - http://apt.saurik.com
        - http://apt.25pp.com

- **AppSync Unified**
    - **AppSync unified 补丁的作用**
        - 可以绕过系统的验证，随意安装、运行破解的ipa安装包。
    - **软件源**
        - http://apt.a5pp.com

- **iFile**
    - **iFile 的作用**
        - 可以在iPhone上自由访问iOS文件系统
        - 类似的还有Filza file manager、 File Browser
    - **软件源**
        - http://apt.thebigboss.org/repofiles/sydia
        
- **PP助手**
    - **可以利用pp助手安装海量App**
    - **软件源**
        - http://apt.25pp.com
        
####九、Mac 端逆向必备软件安装
- **PP助手**

####十、关于必备软件安装顺序
- **iphone端**
    - **Cydia**
    - **Apple file conduit**
    - **AppSync unified**
    - **iFile**
    - **pp助手**
- **Mac端**
    - **iFunBox**
    - **pp 助手**
    
####十一、关于安装包说明
- **通常情况下**
    - **通过 Cydia 安装的安装包是 deb 格式的 （结合 包管理工具  apt）
    - **通过 pp助手 安装的安装包格式是 ipa 格式的

- **如果通过Cydia 源安装 deb 失败**
![](/assets/屏幕快照 2018-05-13 上午11.21.45.png)
    - **可以通过从网上（比如：搜 iFile deb ）下载 deb 格式的安装包（安装包可能是多个 deb 格式的文件包）**
    ![](/assets/Snip20180513_5.png)![](/assets/Snip20180513_7.png)
    
    - **让后直接将下载的 deb 文件安装包 直接放到文件路径： /var/root/Media/Cydia/AutoInstall **
    - **重启手机，Cydia 就会自动安装 deb **
    
####十二、 代码判断手机是否越狱

- **通过代码判断手机是否有安装Cydia **
    ```
    if ([[NSFileManager defaultManager] fileExistsAtPath:@"/Applications/Cydia.app"]
        ) {
        NSLog(@"手机已经 越狱");
    }
    else{
        NSLog(@"手机未 越狱");
    }
    ```

- **判断手机是否已经越狱， 在网上一搜有很多**


####逆向环境搭建
- **提高效率的工具**
- **Alfred**
    - **便捷搜索**
    - **工作流**
- **XtraFinder**
    - **增强型finder**
    
- **iTerm2**
    - **完爆terminal 的命令行工具**
- **Go2Shell**
    - **从finder快速定位到命令行工具**

        
        
        
        

  
    
    























    
    

    