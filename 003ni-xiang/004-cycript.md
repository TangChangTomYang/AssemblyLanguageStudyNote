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
    
    
    
##三、封装Cycript 文件 （cy 文件）


####1、编写一个 test.cy  文件
```
(function(exports){
    exports.sum = funtion(a,b){
        return a + b;
    }
    
    exports.minus = function(a,b){
        return a - b;
    }
    
    exports.age = 18;
    
})(exports);
```
![](/assets/屏幕快照 2018-05-17 下午11.36.43.png)




####2、调用test.cy 中的方法
```
cy# test.sum(20,10);  // 通过test.cy的名称调用里面的方法，调用时会通过test 传给文件中的exports

```
![](/assets/屏幕快照 2018-05-17 下午11.43.21.png)




####2、在手机终端引用cycript 文件的使用步骤：
- **1、拷贝cy 文件到手机端的库文件,手机库cycript 库文件目录： /usr/lib/cycript0.9**

```
yang$   scp -P 10010 ~/Desktop/test.cy root@localhost:/usr/lib/cycript0.9

```
- **2、通过 打开的app 进程名（或者进程号）进入对应cycript 编辑页面**
```
root#  cycript -p ting
```
- **3、直接应用导入cycript0.9 目录中的cycript 库文件即可**
```
cy# @import test.cy
```
- **4、 直接调用 test.cy 中的方法即可**
    




