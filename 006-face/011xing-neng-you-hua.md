##APP 性能优化


####一 APP 的启动

- 1 **App 的启动分为22种:**<br>
(1) 冷启动(cold launch): 从0开始启动App<br>
(2) 热启动(warm launch): App 已经在内存中,在后台存活着,再次点击图标启动.


- 2 **App 启动时间的优化,是主要针对冷启动进行优化的.**


- 3 **通过`添加环境变量`可以打印出App的启动时间分析**<br>
(1) DYLD_PRINT_STATISTICS 设置1, 一般大概启动时间<br>
(2) DYLD_PRINT_STATISTICS_DETAILS 设置1, 详细启动时间
![](/assets/Snip20180728_3.png)

