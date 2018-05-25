##class-dump
- 顾名思义，他的作用就是把Mach-o文件的class 信息dump出来（把类信息给导出来），生成对应的 .h 头文件。
- [官方地址](http://stevenygard.com/projects/class-dump)
- 下载完工具包后将 class-dump 文件复制到 `/usr/local/bin` 目录，这样在终端才能识别 class-dump 命令。

**说明：**
其实在mac 终端上敲指令时，他去2个路劲查找命令：
 `/usr/bin` 和 `/usr/local/bin`, 注意从Mac 10.11 开始 `/usr/bin`路径是不能再被编写的，有管理员权限也不能写，所以你需要把新的指令，添加到： `/usr/local/bin`目录下。
 ![](/assets/Snip20180526_1.png)
 
 **具体指令语法如下：**
```
class-dump 3.5 (64 bit)
Usage: class-dump [options] <mach-o-file>

  where options are:
        -a             show instance variable offsets
        -A             show implementation addresses
        --arch <arch>  choose a specific architecture from a universal binary (ppc, ppc64, i386, x86_64)
        -C <regex>     only display classes matching regular expression
        -f <str>       find string in method name
        -H             generate header files in current directory, or directory specified with -o
        -I             sort classes, categories, and protocols by inheritance (overrides -s)
        -o <dir>       output directory used for -H
        -r             recursively expand frameworks and fixed VM shared libraries
        -s             sort classes and categories by name
        -S             sort methods by name
        -t             suppress header in output, for testing
        --list-arches  list the arches in the file, then exit
        --sdk-ios      specify iOS SDK version (will look in /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk
        --sdk-mac      specify Mac OS X version (will look in /Developer/SDKs/MacOSX<version>.sdk
        --sdk-root     specify the full SDK root path (or use --sdk-ios/--sdk-mac for a shortcut)
        
        ```
 