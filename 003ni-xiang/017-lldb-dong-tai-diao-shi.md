##动态调试

####一、什么是动态调试？
![](/assets/Snip20180605_5.png)
**当我们在Xcode 中打一个断点时，Xcode先将这个断点的信息发送给手机上安装的debugserver 服务器，这个debugserver 服务器将信息转发个App，App 在将信息反馈给debugserver，debugserver 再将信息发送给Xcode ，让后我们在Xcode 上面就能看见调试信息了，这就是就是动态调试的原理。**


####二、Xcode 动态调试的原理
- 1、 **关于GCC、LLVM、GDB、LLDB**<br> Xcode 的**编译器**发展历程：GCC -> LLVM <br> Xcode 的**调试器**发展历程：GDB -> LLDB

- 2、**debugserver（iphone端断点调试服务程序）最初是存放在mac 的Xcode 里面**
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/9.1/DeveloperDiskImage.dmg/usr/bion/debugserver
```


- 3、**当Xcode 识别到手机的设备时(通过Xcode给手机安装app)，Xcode 会自动将debugserver安装到iPhone 上。路劲如下：**

  ```
  /Developer/usr/bin
  ```

- 4、**Xcode 调试的局限性,一般情况下，只能调试通过Xcode安装的App**<br><br>![](/assets/Snip20180605_4.png)

####三、动态调试任意App
- 1、**iPhone上的debugserver 在哪里呢？**
```
/Developer/usr/bin
```
**注意：这个路劲的文件可能会被系统自动删掉，如果没找到用真机调试一下(使用Xcode安装app ，打断点)即可**<br>
<br>

- 2、**debugserver 权限的问题**<br><br>(1)、默认情况下，/Developer/usr/bin 目录下的debugserver 权限是不够的（因为这个debugserver 是Xcode 帮着安装的，默认情况下只能调试Xcode安装的App，只有这个权限，像来自App store 的App 都是调试不了的）。<br>(2)、/Developer 这个文件夹的权限是只读的，(其他地方文件是放不进去的)，需要注意一下。<br>(3)、如果需要调试其他的app，需要对debugServer重新签权限，签上2个调试相关的权限，如下即可：
```
get-task-allow
task_for_pid-allow
```
<br>
- 3、**如何给debugserver签上权限？**<br><br>(1)、iphone上的 **/Developer**目录是只读的，无法直接对**/Developer/usr/bin/debugserver**文件签名，需要先把debugserver复制到mac上，再通过ldid签名（也可以使用Codesign签名）。

    ```
    //1.先将权限导出
    $ ldid -e debugserver > debugserver.entitlements

    //2.debugserver.entitlements文件中在添加2条权限
    get-task-allow = YES
    task_for_pid-allow  = YES

    //3.再将权限签回去即可
    ldid -Sdebugserver.entitlements  debugserver

    //4. 将文件装回iphone 即可(这是 不可能的事 /Developer 文件夹是只读的)
    //5.将修改过权限的debugserver 放在 /usr/bin 路劲下，我们以后会通过终端来使用它。
    ```
    ![](/assets/Snip20180610_18.png)(2)、使用codesign签名
   
 
   ```
  //查看权限信息
  $ codesign -d --entitlements - debugserver

  //签名权限
  $ codesign -f -s - --entitlements debugserver

  //或者简写
  $ codesign -fs- --entitlements debugserver
  ```    
    
- 4、 **注意：**<br>直接将签名回去的debugserver 直接拖回去是不行的。是不能覆盖 **/Developer/usr/bin** 目录的。那怎么办呢？<br> 是这样的，到时我们需要在手机端启动Debugserver 这个服务，所以我们直接将 debugserver 拖到 /usr/bin目录下了，这样我们就可以在手机上通过终端来启动debugserver了。<br>
<br>**debugserver 常用法：**
```
Usage:
  debugserver host:port [program-name program-arg1 program-arg2 ...]
  debugserver /path/file [program-name program-arg1 program-arg2 ...]
  debugserver host:port --attach=<pid>
  debugserver /path/file --attach=<pid>
  debugserver host:port --attach=<process_name>
  debugserver /path/file --attach=<process_name>
  ```
**(iPhone端)最常用的一个指令:**

  ```
  // 表示任何客户端（mac 终端）ip 都可监听10011端口， 都可以连接过来
  debugserver *:10011 -a WeChat       
  // -a 是attachment 附加的意思 ， 表示我们要调试WeChat（进程ID和名字都可以）
  // 端口号，使用iPhone的某个端口启动debugserver（只要不是保留的即可）  
                
  ```
  

####四、动态调试任意App 的主要步骤如下：
**iPhone 端：**
- 1、获取要调试的App 的进程号或者ID
```
ps -A  // 查看所有的进程信息,比如：我们获取的是WeChat
```
- 2、让debugserver 附加到某个App （要动态调试哪个App）
```
debugserver *:10011 -a Wechat
// 我们这里使用10011端口(其他非保留都可以)来监控 Wechat这个 程序
```
**mac端：**
- 1、启动LLDB
```
$ lldb
(lldb)
```
- 2、连接debugserver服务
```
//方式1：
(lldb)
process connect connect://手机ip地址：debug端口号

方式2：
python tcppython -t 10011：10012
// 使用mac端的10012端口绑定iPhone端的10011端口
process connect connect:localhost:10012
```
- 3、使用LLDB命令让程序继续运行
```
(lldb)continue
```









####四、常用的LLDB

- 1、**LLDB 指令格式**
```
<command> [<subcommand> [<subcommand> ...]] <action>  [-options  [option-value]] [argment [argment ...]]
```
**说明：**<br>(1)、凡是在指令说明书里面 看到的 `[]` 表示这部分是可以省略的。 <br>(2)、一般的指令格式都是： **命令、子命令、动作**(比如： 向设置东西呢、删除东西呢、还是添加东西 等)、**选项**(比如： -n、-d、-i)、**参数**。 
<br>
<br>

- 2、**给 test方法设置断点：**
```
breakpoint set -n test
或者
breakpoint set --name test
// test 是要打断点的方法名
```
**breakpoint 是：**`<command>`     <br> **set是：**`<action>`        <br> **-n是：**`<options>`    <br>**test是：**`<arguments>` 
 <br> <br> 

- 3、**hlep 指令的用法：** **help** `<command>`  <br>即: 直接在命令的前面加 help 来查看命令帮助说明，<br>如：

  ``` 
  help breakpoint  // 查看breakpoint 命令的帮助信息

  help breakpoint set // 查看breakpoint set  命令的帮助信息
  依次类推，一层一层的查看帮助

  ```
  <br>
<br>


  
- 4、**express 命令的使用： ** **express** `<cmd -options> - <expr>` <br>这个命令就 🐂 B 了，可以在调试时动态插入执行一个表达式 （调用一个方法，设置一个参数 等等） <br><br> `<cmp-options>`命令的选项 <br> `-- ` 命令选项结束符，表示所有的命令选项已经设置完毕，如果没有命令选项， `--` 可以省略 <br> `<expr>` 就是需要执行的表达式，比如：
```
// 一旦在LLDB 中执行了这句代码，就会在短点的下一执行命令前先执行 这个表达式
express self.view.backgroundColor = [UIColor redColor];
```
**express -o -- ** 和 指令**po**的效果是一样的。

- 5、**thread backtrace** <br> 打印线程的堆栈信息。<br> 可以用简写：**bt** 代替，效果一样的，都是打印堆栈信息。

- 6、**thread return [<expr>]**<br> 让函数直接返回某个值，断点后面的代码不会执行执行，直接跳过
```
thread return 10;
```

- 7、**thread variable [<variable-name>]**<br>打印当前栈帧的变量
```
thread variable // 将会打印出当前的所有的局部变量的值
又或者
thread variable a // 打印出变量a 的值
```

- 8、**thread continue** <br> 让当前断点断住的程序继续执行<br> 简写 **continue** 或者 **c**

- 9、**thread step-over**<br>单步运行（一次运行一行代码，把子函数当成整体一步执行）<br> 简写 **next**或者**n**


- 10、**thread step-in**<br>单步运行（一次运行一行代码，遇到子函数会进入子函数）<br>简写 **step** 或者**s**


- 11、**thread step-out**<br> 执行完当前函数的所有代码，返回到调用该代码处<br>简写**finish**

- 12、**thread step-inst-over** 指令级别单步执行（汇编函数调用当成一步执行）<br>简称 **nexti** 或者**ni**

- 13、**thread step-inst** 指令级别单步运行（遇到汇编函数调用会跳进汇编函数）<br>简称**stepi** 或者**si**

- 14、 **ni**、**si** 和 **n**、**s** 类似<br> **ni**、**si** 是汇编指令级别的<br> **n**、**s** 是源码级别的。


- 15、**breakpoint set -a 函数地址**
- 16、**breakpoint set -n 函数名**

```
breakpoint set -n test  //给所有的test 设置断点，有很多个（这个是C语言的方法）

breakpoint set -n touchesBegan：withEvent：  // 给很多个touchesBegan：withEvent： 设置断点（这个是oc 的方法）

breakpoint set -n “ViewController touchesBegan：withEvent：” 给当前控制器的 touchesBegan：withEvent： 设置断点
```

- 16.1、 **breakpoint set -r 正则表达式**，给匹配正则的所有方法加断点
```
breakpoint set -r "abc" //给所有包含abc 的方法设置断点
```

- 16.2、**breakpoint set -s 动态库 -n 函数名**
```
breakpoint set -s lib.dylib -n abc  //-s是shlib 的意思， lib.dylib 是动态库的名称
```

- 17、**breakpoint list**<br> 列出所有的断点（每个断点都有自己的编号）

- 18、**breakpoint disable 断点号**，禁用某个断点

- 19、**breakpoint enable 断点号**， 启用某个断点

- 20、**breakpoint delete 断点号**，删除某个断点

- 21、**breakpoint command add 断点编号**， 给某个断点添加一些执行命令（执行语句、方法）相当于是，执行到这个断电后多执行几条语句
```
breakpoint set -n "-[ViewConreoller test]"  // 给控制器的test 方法增加断点， 加入返回的断点号是 12

breakpoint command add 12 // 给 断点号是12 的断点增加几条命令（语句）,以下三句是新增的断点命令，DONE 是结束指令
self.age = 32;
self.view.backgroundcolor = [UIColor redColor];
DONE  

```















































