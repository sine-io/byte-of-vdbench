# 介绍

`Vdbench` 是一款 `I/O` 负载生成工具，作者是 `Oracle` 的 `Henk Vandenbergh`，用于测试和评估块设备与文件系统的基准性能，也支持数据完整性验证。它自带较完整的报告输出，执行结束后可直接查看生成的 `HTML` 报告。

> 注：历史版本曾覆盖很多平台，但并非每个新版本都同时维护了这些适配。至少 `Windows`、`Linux(x86)`、`Solaris` 在发布包中是可见的；其他平台需要结合对应目录中的 `readme.txt` 和源码自行确认。

```shell
[root@node1 vdbench50407]# ls -alh 
total 2.7M
drwxr-xr-x  13 root root 4.0K May  7 09:11 .
dr-xr-x---. 17 root root 4.0K May  7 09:11 ..
drwxrwxrwx   2 root root   24 Jun  5  2018 aix
...

# aix系统的共享库没有适配
[root@node1 vdbench50407]# ls -alh aix/
total 8.0K
drwxrwxrwx  2 root root   24 Jun  5  2018 .
drwxr-xr-x 13 root root 4.0K May  7 09:11 ..
-rwxr-xr-x  1 root root  139 Jun  5  2018 readme.txt

# 阅读readme.txt，得知，进行编译即可
[root@node1 vdbench50407]# cat aix/readme.txt 
AIX currently not supported in this release. 
I need a volunteer to do a small C compile and link.
Email me at Henk.Vandenbergh@oracle.com
```

## 读完这一页后做什么

如果你是第一次接触 `Vdbench`，下一步建议直接阅读 [快速开始](quickstart.md)：

- 先确认 `Java` 和 `Vdbench` 是否能正常运行
- 先跑通 `-t` 和 `-tf`
- 再看 `example1` 和 `example7`

如果你已经能跑通基本样例，可以继续看：

- [结果分析](results-analysis.md)
- [参数文件详解](../hero/parameter-file-detail.md)
