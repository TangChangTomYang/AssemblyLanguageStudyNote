##Cycript

##1、Cycript 简介
- **Cycript 是 Objective-C++ 、ES6（javaScript）、java等语言的混合物**
- **Cycript 可以用来探索、修改、调试正在运行的Mac、iOS App**
- **[Cycript官网](http://www.cycript.org)**
- **[Cycript文档](http://www.script.org/manual)**
![](/assets/屏幕快照 2018-05-17 上午7.22.02.png)


##2、手机端安装Cycript
- **通过Cydia 安装Cycript，即可在iphone上调试运行中的app**
![](/assets/屏幕快照 2018-05-17 上午7.24.37.png)


##3、手机端安装 adv-cmds  查看手机进程
- **安装adv-cmds**
![](/assets/屏幕快照 2018-05-17 上午7.45.51.png)
- **ps命令是process status 的缩写（进程状态），使用ps命令可以列出系统当前的进程**
    - **列出所有进程：ps-A**
    - **搜索关键字：ps-A|grep 关键字**

##4、Cycript 的开启与关闭
- **开启Cycript，直接在手机终端 输入 `cycript` 命令即可，也可以使用其它命令，比如：**
    - cycript -p 进程ID
    - cycript -p 进程名
    ![](/assets/屏幕快照 2018-05-17 上午7.34.24.png) ![](/assets/屏幕快照 2018-05-17 下午9.06.35.png)
- **退出Cycript， 在手机终端 按 `ctr + d`， 即可**

##二、Cycript 常用语法
####1、常用语法1
- **获取当前运行的app对象，有2种方式：**
    - **UIApp**
    - **[UIApplication sharedApplication]**
    
- **定义变量**
    - **var 变量名 = 变量值**
    
- **用内存地址获取对象**
    - **#内存地址**

![](/assets/屏幕快照 2018-05-17 下午9.33.35.png)

- **查看当前App 用到的所有的类**
    - **ObjectiveC.class**
    
    
- **查看对象的所有成员变量**
    - **\*对象**
    
- **对象查看View的所有子控件（跟 LLDB 一样的函数）**
    - **UIApp.keyWindow.recursiveDescription().toString()**
    
- **筛选出某种类型的对象**
    - **choose(UiViewController)**
    - **choose(UITableViewCell)**
    




