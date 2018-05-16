##一、sh 脚本

#### sh 脚本文件
- **我们可以将经常执行的一系列终端命令行放到 sh 脚本文件中（shell），然后执行脚本文件**

- ** sh 脚本文件** 其实就是一个 后缀为 .sh 的文本文件，文本文件中每一行文字代表一条需要执行的 终端命令

**比如：** 在Desktop文件路劲内创建一个abc.sh 脚本文件,使用abc.sh 脚本在Desktop的Test 文件夹内创建一个abc文件夹，具体操作如下：

```
cd ~/Desktop
vim abc.sh  //在当前路径（Desktop文件夹内）创建abc.sh 脚本文件打开
i // 按i 进入 vim 的编辑模式
cd Test
mkdir abc
:wq  // 保存并退出
```

- **执行脚本文件,有3中方式：**
    - **方式1： sh   abc.sh**
    - **方式2： bash abc.sh**
    - **方式3： source abc.sh, source 也可以用 . ,比如： . abc.sh 是一样的效果**
    
- **注意：** 通过 sh、bash、source（.） 这3个命令都可以执行脚本文件,但是是有差异的，通过sh、 bash 执行 sh脚本文件时， 当前的shell 环境（命令终端）会启动一个子进程来执行脚本文件，执行完后返回到父进程的shell环境（命令终端）。执行 cd 时，在子进程中会进入到cd目录，但是在父进程中环境并没有改变，也就是说父进程目录不会变化。通过 source 执行 sh 脚本文件时，实在当前环境（当前命令终端）下执行， source 可以使用 . 来代替，比如： . abc.sh  和 source abc.sh 是一样的。



##二、iOS 终端的中文乱码问题
- **默认情况下，iOS 终端不支持中文输入和显示**
- **解决方案： 新建一个 “~/.inputrc”文件，文件内加绒编写完毕后，重启终端即可生效，文件内容是：**
    - **set convert-meta off** （不将中文字符转化为转义序列）
    - **set output-meta on** （允许向终端输出中文）
    - **set meta-flag on **
    - **set input-meta on** （允许向终端输入中文）
    
```
set convert-meta off
set output-meta on
set meta-flag on
set input-meta on
```

- **如果是想在终端编辑文件内容，可以通过Cydia 安装一个 vim ，软件源（http://apt.saurik.com）**
![](/assets/屏幕快照 2018-05-16 下午10.13.05.png)
    




