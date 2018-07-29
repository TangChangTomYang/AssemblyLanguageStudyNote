- file 命令
```
file testfile // 查看文件的信息
```

- lipo 命令

    ```
    lipo -info abc // 查看abc 文件的架构信息
    lipo -thin arm64 

    ```

- otool 命令

    ```
    otool  // 直接在终端上输入 otool 命令查看 otool 命令的常用用法

    otool -l testfile // 查看 mach-o 文件testfile 的load commands 信息
    otool -l testfile | grep crypt // 查看可执行文件（mach-o executable）文件是否被加过壳， 如果 cryptid > 0 说明是加过壳的
    ```  