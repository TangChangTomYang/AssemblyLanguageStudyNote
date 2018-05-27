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








<br><br><br>


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







<br><br><br>

##三、 sh 脚本基本语法

- 1、**开头**<br><br>
程序必须一下面一行开始（必须写在文件的第一行）：
```
#!/bin/sh
```
符号`#!` 用来告诉系统他后面的参数是用来执行该文件的程序。<br>在这个例子中我们使用 `/bin/sh` 来执行程序。<br>当编写完脚本时，如果要执行该脚本，还必须使其可执行。<br> 要使编写脚本可执行，需要给脚本文件添加执行权限，这样脚本才可执行。
```
chomd +x filename  // 给 fileName 文件添加执行权限
```


- 2、**注释** <br><br>在进行shell 编程时， 以`#` 开头的句子表示注释，直到这一行的结束。

- 3、**变量**<br><br>在shell 编程中，所有的变量都是由字符串组成的，并不需要对变量进行声明。要赋值给一个变量，可以这样写：<br>
```
#!/bin/sh
#对变量赋值
a="helloworld"
#现在打印a的内容
echo "A is:"
echo $a
```
有时候变量名很容易和其他文字混淆，比如：
```
num=2
echo "this is the $numdn"
```
这并不会打印出“this is the 2nd” ，而 仅仅打印出 “this is the ”，因为shell 回去搜索变量numnd 的值，但是这个变量是没有值得，可以使用 `{}` 来告诉shell 我们要打印的是num变量。
```
num=2
echo "this is the ${num}nd"
```
这样就可以打印出： this is the 2nd

- 4、**环境变量**<br><br>
由`export`关键字处理过的变量叫做环境变量。我们不对环境变量进行讨论，因为通常情况下仅仅是在登录脚本中使用环境变量。
- 5、**Shell 命令和控制流程**<br><br>
在shell 脚本中可以使用3类命令：<br> 1> **Unix 命令**<br> 虽然在Shell脚本中可以使用任意的unix命令，但是还是有一些相对更常用的命令。<br>**这些命令通常是用来进行文件和文字操作的。**<br><br>**常用明林个语法及功能**

```
echo "some text" //将文本内容打印在屏幕上

ls // 文件列表

wc -l filewc -w filewc -c file // 计算文件的行数、单词数、字符个数

cp sourcefile desfile // 文件拷贝

mv oldname newname  // 重命名文件 或 移动文件

rm file // 删除文件

grep ‘pattern’ file // 在文件内搜索字符串比如： grep ‘searchstring’ file.text

cut -b colnum file //指定欲显示的文件内容范围， 并将它们输出到标准输出设备比如： 输出每行第5个到第9个字符 `cut -b5-9 file.text`, 注意不要写成cat 命令了。

cat File.txt // 输出文件内容到标准输出设备（屏幕）上

file somefile  // 得到文件类型

read -p "请输入你的名字和地址：" name  address //提示用户输入，并将输入赋值给变量 name 和 address 


```

- **流程控制**<br>
```
if ... ;then
...
elif ...;then
...
else
...
fi
```
**通常用‘[]’来表示条件测试。**注意这里的空格很重要。要确保方括号的空格<br> `[-f "somefile"]` 判断是否是一个文件<br> `[-x "/bin/ls"]` 判断/bin/ls 是否存在并有可执行权限<br> `[-n "$var"]` 判断$var变量是否有值<br> `["$a"= "$b"]` 判断$a和$b 是否相等。
















































































    




