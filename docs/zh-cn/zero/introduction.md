# 介绍

`Vdbench`是一款`I/O`负载生成工具，作者是`Oracle`的 `Henk Vandenbergh`，用于测试和评估块设备和文件系统的基准性能，还可以验证数据完整性。它集成了详细的报告机制，通过浏览器即可查看报告（`HTML`格式）。它使用`Java`语言开发，曾适配过`Solaris`、`各版本Windows`、`HP/UX`、`AIX`、`Linux`、`Mac OS X`、`zLinux`和`RaspBerry Pi`等众多平台。

> 注：上述平台众多，无奈`Henk`老哥手边不可能全有...... 所以版本迭代过程中，新版本的`Vdbench`，并没有适配上述所有平台。但是请放心，它至少包含三个平台（`Windows`、`Linux(x86)`、`Solaris`），如果有其他平台需求，请阅读对应目录中的`readme.txt`文件，按需进行`Java JNI C`编译。

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

如何进行编译呢？见xxx
