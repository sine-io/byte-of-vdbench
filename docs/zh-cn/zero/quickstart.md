# 快速开始

## 安装部署

从[Oracle网站](https://www.oracle.com/downloads/server-storage/vdbench-downloads.html)下载`Vdbench (5.04.07)`安装包（压缩包，`zip`格式），进行解压，解压后的目录长这样：

```shell
# 测试环境OS：CentOS Linux release 7.9.2009 (Core)
[root@node1 ~]# unzip vdbench50407.zip -d vdbench50407

[root@node1 ~]# cd vdbench50407

[root@node1 vdbench50407]# ls -alh 
total 2.6M
drwxr-xr-x  11 root root 4.0K Apr  9 10:11 .
dr-xr-x---. 17 root root 4.0K Apr  9 10:09 ..
drwxrwxrwx   2 root root   24 Jun  5  2018 aix
-rwxrwxrwx   1 root root 1.2K Aug  7  2012 build_sds.txt
drwxrwxrwx   6 root root   75 Jun  5  2018 classes
-rwxrwxrwx   1 root root  429 Jun 14  2013 example1
-rwxrwxrwx   1 root root  500 Jun 14  2013 example2
-rwxrwxrwx   1 root root  671 Jun 14  2013 example3
-rwxrwxrwx   1 root root  698 Jun 14  2013 example4
-rwxrwxrwx   1 root root  574 Jun 14  2013 example5
-rwxrwxrwx   1 root root  498 Jun 14  2013 example6
-rwxrwxrwx   1 root root 1.5K Jun 14  2013 example7
drwxrwxrwx   4 root root   71 Aug  7  2012 examples
drwxrwxrwx   2 root root   24 Jun  5  2018 hp
drwxrwxrwx   2 root root   77 Jun  5  2018 linux
drwxrwxrwx   2 root root   24 Jun  5  2018 mac
-rwxrwxrwx   1 root root 1.6K Jun  5  2018 readme.txt
drwxrwxrwx   2 root root   59 Jun  5  2018 solaris
drwxrwxrwx   2 root root   63 Jun  5  2018 solx86
-rwxrwxrwx   1 root root  473 Oct 19  2013 swatcharts.txt
-rwxrwxrwx   1 root root 1.3K Jul 15  2016 vdbench
-rwxrwxrwx   1 root root 1.2K Jun  5  2018 vdbench.bat
-rwxrwxrwx   1 root root 982K Jun  5  2018 vdbench.jar
-rwxrwxrwx   1 root root 1.6M Jun  5  2018 vdbench.pdf
drwxrwxrwx   2 root root   48 Jun  5  2018 windows
```

由于它使用`Java`语言开发，所以咱还需要准备`Java`环境（<font color="#FF00000">建议`1.8`及以上版本</font>）

```shell
[root@node1 vdbench50407]# java -version 
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
```

执行测试命令验证`Vdbench`所需环境是否装好：

```shell
# linux
# 测试裸I/O工作负载
[root@node1 vdbench50407]# ./vdbench -t
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Vdbench distribution: vdbench50407 Tue June 05  9:49:29 MDT 2018
For documentation, see 'vdbench.pdf'.
...
10:14:35.871 Vdbench execution completed successfully. Output directory: /root/vdbench50407/output 

# 测试文件系统工作负载
[root@node1 vdbench50407]# ./vdbench -tf
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Vdbench distribution: vdbench50407 Tue June 05  9:49:29 MDT 2018
For documentation, see 'vdbench.pdf'.
...
10:15:43.257 Vdbench execution completed successfully. Output directory: /root/vdbench50407/output 


# windows
C:\Users\sine\Desktop\vdbench50407>vdbench.bat -t
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Vdbench distribution: vdbench50407 Tue June 05  9:49:29 MDT 2018
For documentation, see 'vdbench.pdf'.
...
09:57:21.315 Vdbench execution completed successfully. Output directory: C:\Users\sine\Desktop\vdbench50407\output

# 或者执行
C:\Users\sine\Desktop\vdbench50407>vdbench.bat -tf
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Vdbench distribution: vdbench50407 Tue June 05  9:49:29 MDT 2018
For documentation, see 'vdbench.pdf'.
...
09:58:48.564 Vdbench execution completed successfully. Output directory: C:\Users\sine\Desktop\vdbench50407\output
```

通过结果关键字`Vdbench execution completed successfully`，我们可以知道`Vdbench`的可执行环境没问题，可以开展后续测试工作了。

> 注：后续测试依赖环境的OS均为：CentOS7.9

## 开展测试

常用命令格式如下：

```shell
# Unix:
/home/vdbench/vdbench –f parmfile -o res+

# Windows:
c:\vdbench\vdbench.bat –f parmfile -o res+
```

> 命令里的`-f`和`-o`都是可执行参数，想知道可执行参数的更多细节，请参考[命令行参数详解](zh-cn/hero/execution-parameter-detail.md)
>
> 除了可执行参数，还有非常重要的事项 --- 编写测试脚本或者叫参数配置文件（`parmfile`），想知道参数配置文件有哪些参数，该怎么用，请参考[参数文件详解](zh-cn/hero/parameter-file-detail.md)

`Vdbench`主要依靠参数配置文件（`-f`参数后面接的文件）来执行任务，编写参数配置文件对于新手极度不友好。还好`Vdbench`提供了很多**样例**，经过样例学习和分析，我们可以快速开展我们自己的测试工作。样例位于`Vdbench`安装目录下：

```shell
[root@node1 vdbench50407]# ls -alhR example*
-rwxrwxrwx 1 root root  429 Jun 14  2013 example1
-rwxrwxrwx 1 root root  500 Jun 14  2013 example2
-rwxrwxrwx 1 root root  671 Jun 14  2013 example3
-rwxrwxrwx 1 root root  698 Jun 14  2013 example4
-rwxrwxrwx 1 root root  574 Jun 14  2013 example5
-rwxrwxrwx 1 root root  498 Jun 14  2013 example6
-rwxrwxrwx 1 root root 1.5K Jun 14  2013 example7

examples:
total 28K
drwxrwxrwx  4 root root   71 Aug  7  2012 .
drwxr-xr-x 13 root root 4.0K May  7 09:11 ..
-rwxr-xr-x  1 root root  15K Jun 14  2013 errorlog.html
drwxr-xr-x  2 root root  167 Aug  7  2012 filesys
drwxr-xr-x  2 root root 4.0K Aug  7  2012 raw
-rwxr-xr-x  1 root root 1.4K Jun 14  2013 readme.txt

examples/filesys:
total 36K
drwxr-xr-x 2 root root  167 Aug  7  2012 .
drwxrwxrwx 4 root root   71 Aug  7  2012 ..
-rwxr-xr-x 1 root root  449 Jun 14  2013 create_files
-rwxr-xr-x 1 root root 1.1K Jun 14  2013 delete_files
-rwxr-xr-x 1 root root  896 Jun 14  2013 multi_host
-rwxr-xr-x 1 root root  891 Jun 14  2013 random_read
-rwxr-xr-x 1 root root 1010 Jun 14  2013 random_rw
-rwxr-xr-x 1 root root  899 Jun 14  2013 random_write
-rwxr-xr-x 1 root root  832 Jun 14  2013 seq_read
-rwxr-xr-x 1 root root  951 Jun 14  2013 seq_rw
-rwxr-xr-x 1 root root  835 Jun 14  2013 seq_write

examples/raw:
total 84K
drwxr-xr-x 2 root root 4.0K Aug  7  2012 .
drwxrwxrwx 4 root root   71 Aug  7  2012 ..
-rwxr-xr-x 1 root root 1.7K Jun 14  2013 multi_host
-rwxr-xr-x 1 root root  270 Jun 14  2013 random_read
-rwxr-xr-x 1 root root  290 Jun 14  2013 random_read_curve
-rwxr-xr-x 1 root root  323 Jun 14  2013 random_read_xfersizes
-rwxr-xr-x 1 root root  275 Jun 14  2013 random_rw
-rwxr-xr-x 1 root root  295 Jun 14  2013 random_rw_curve
-rwxr-xr-x 1 root root  328 Jun 14  2013 random_rw_xfersizes
-rwxr-xr-x 1 root root  269 Jun 14  2013 random_write
-rwxr-xr-x 1 root root  289 Jun 14  2013 random_write_curve
-rwxr-xr-x 1 root root  322 Jun 14  2013 random_write_xfersizes
-rwxr-xr-x 1 root root  271 Jun 14  2013 seq_read
-rwxr-xr-x 1 root root  291 Jun 14  2013 seq_read_curve
-rwxr-xr-x 1 root root  324 Jun 14  2013 seq_read_xfersizes
-rwxr-xr-x 1 root root  540 Jun 14  2013 seq_rw
-rwxr-xr-x 1 root root  296 Jun 14  2013 seq_rw_curve
-rwxr-xr-x 1 root root  329 Jun 14  2013 seq_rw_xfersizes
-rwxr-xr-x 1 root root  270 Jun 14  2013 seq_write
-rwxr-xr-x 1 root root  290 Jun 14  2013 seq_write_curve
-rwxr-xr-x 1 root root  323 Jun 14  2013 seq_write_xfersizes
-rwxr-xr-x 1 root root  842 Jun 14  2013 tpcc
```

!> 建议把`example1~7`多看几遍，多执行几遍。注意：执行过程中不要把自己重要数据给删除了，因为会有写操作。

从前文知道`Vdbench`可以测试块存储和文件系统的性能，那咱分别找一个对应的样例来进行分析。

- 块存储

```shell
# example1
[root@node1 vdbench50407]# cat example1 

*
* Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.
*

*
* Author: Henk Vandenbergh.
*


*Example 1: Single run, one raw disk

*SD:	Storage Definition
*WD:	Workload Definition
*RD:	Run Definition
*
sd=sd1,lun=/dev/rdsk/c0t0d0sx
wd=wd1,sd=sd1,xfersize=4096,rdpct=100
rd=run1,wd=wd1,iorate=100,elapsed=10,interval=1

*Single raw disk, 100% random read of 4k records at i/o rate of 100 for 10 seconds
```

我们来分析一下这个文件：

| 分析结果：                                                   |                            |
| ------------------------------------------------------------ | -------------------------- |
| `*`开头的，是注释                                            | 其他符号可以当注释吗？     |
| 参数配置文件最少应该由`sd、wd、rd`组成                       | 还有别的吗？有顺序要求吗？ |
| 这个脚本是针对一个块设备进行的测试，100%的随机读，每次I/O的数据大小是4K，I/O的速率是100，总共执行10秒钟 |                            |
| `sd=、wd=、rd=`看赋值这么随意，应该是各自的名字              |                            |
| `lun=`是`/dev`开头的，看样子是块设备的名字，那么被测的块设备就是它了 |                            |

我们将`lun=`修改一下，执行这个脚本

```shell
[root@node1 vdbench50407]# cp example1 testraw01
# vim testraw01
# 修改lun为非系统盘的其他盘符
[root@node1 vdbench50407]# cat testraw01 

*
* Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.
*

*
* Author: Henk Vandenbergh.
*


*Example 1: Single run, one raw disk

*SD:	Storage Definition
*WD:	Workload Definition
*RD:	Run Definition
*
sd=sd1,lun=/dev/sda,openflags=o_direct
wd=wd1,sd=sd1,xfersize=4096,rdpct=100
rd=run1,wd=wd1,iorate=100,elapsed=10,interval=1

*Single raw disk, 100% random read of 4k records at i/o rate of 100 for 10 seconds

# 第21行，lun我修改成了/dev/sda，并添加了openflags参数
```

!> 注意不要用系统盘，用一个没用的盘开展测试

!> `Vdbench`程序里定义`lun`是`/dev`开头的时候，需要在配置文件中添加`openflags=o_direct`参数，所以我也添加上了

执行测试（结果我省略了一部分）：

```shell
[root@node1 vdbench50407]# ./vdbench -f testraw01  -o res+
...
18:03:08.003 Starting RD=run1; I/O rate: 100; elapsed=10; For loops: None
...
18:03:18.769 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res001
```

找到报告目录`res001`，即可查看报告。

```shell
[root@node1 res001]# ls -alh 
total 128K
drwxrwxrwx   2 root root 4.0K May  8 18:03 .
drwxr-xr-x. 44 root root 4.0K May  8 18:03 ..
-rw-r--r--   1 root root  178 May  8 18:03 config.html
-rwxrwxrwx   1 root root  212 May  8 18:03 errorlog.html
-rwxrwxrwx   1 root root 4.5K May  8 18:03 flatfile.html
-rwxrwxrwx   1 root root 2.7K May  8 18:03 histogram.html
-rwxrwxrwx   1 root root 2.1K May  8 18:03 localhost-0.html
-rwxrwxrwx   1 root root 3.4K May  8 18:03 localhost-0.stdout.html
-rwxrwxrwx   1 root root 2.2K May  8 18:03 localhost.html
-rwxrwxrwx   1 root root  40K May  8 18:03 localhost.var_adm_msgs.html
-rwxrwxrwx   1 root root 4.5K May  8 18:03 logfile.html
-rwxrwxrwx   1 root root  549 May  8 18:03 parmfile.html
-rwxrwxrwx   1 root root 1.2K May  8 18:03 parmscan.html
-rwxrwxrwx   1 root root 2.7K May  8 18:03 sd1.histogram.html
-rwxrwxrwx   1 root root 2.1K May  8 18:03 sd1.html
-rwxrwxrwx   1 root root 1.1K May  8 18:03 skew.html
-rwxrwxrwx   1 root root 1.1K May  8 18:03 status.html
-rwxrwxrwx   1 root root 3.0K May  8 18:03 summary.html
-rwxrwxrwx   1 root root  191 May  8 18:03 swat_mon_total.txt
-rwxrwxrwx   1 root root 1.4K May  8 18:03 swat_mon.txt
-rwxrwxrwx   1 root root  658 May  8 18:03 totals.html
```

!> 各个报告详解，请看[结果分析](#结果分析)

- 文件系统

```shell
# example7
[root@node1 vdbench50407]# cat example7 

*
* Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.
*

*
* Author: Henk Vandenbergh.
*



*Example 7: File system testing

fsd=fsd1,anchor=/dir1,depth=2,width=2,files=2,size=128k

fwd=fwd1,fsd=fsd1,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2

rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

*
* This parameter file will use a directory structure of 4 directories and 8 files
* The RD parameter 'format=yes' causes the directory structure to be completely
* created, including initialization of all files to the requested size of 128k.
* After the format completes the following will happen for 10 seconds at a rate
* of 100 reads per second:
*         Start two threads (threads=2; 1 thread is default).
*         Each thread:
*         Randomly selects a file (fileselect=random)
*         Opens this file for read (operation=read)
*         Sequentially reads 4k blocks (xfersize=4k) until end of file (size=128k)
*         Closes the file and randomly selects another file.
*
*
* Directory structure:
*
* find dir1 | grep file
* dir1/vdb_control.file
* dir1/vdb1_1.dir/vdb2_1.dir/vdb_f0001.file
* dir1/vdb1_1.dir/vdb2_1.dir/vdb_f0002.file
* dir1/vdb1_1.dir/vdb2_2.dir/vdb_f0001.file
* dir1/vdb1_1.dir/vdb2_2.dir/vdb_f0002.file
* dir1/vdb1_2.dir/vdb2_1.dir/vdb_f0001.file
* dir1/vdb1_2.dir/vdb2_1.dir/vdb_f0002.file
* dir1/vdb1_2.dir/vdb2_2.dir/vdb_f0001.file
* dir1/vdb1_2.dir/vdb2_2.dir/vdb_f0002.file
*
```

继续分析一下这个文件：

| 分析结果：                                                   |                                  |
| ------------------------------------------------------------ | -------------------------------- |
| `*`开头的是注释，跟块设备的脚本一样，这是通用的              |                                  |
| 参数配置文件最少应该由`fsd、fwd、rd`组成                     | 还有别的吗？有顺序要求吗？       |
| `fsd`参数：`anchor=/dir1`，锚点目录是`/dir1`，那这个应该就是被测目录了 | 如果被测目录不存在会自动创建吗？ |
| `fsd`参数：`depth=2,width=2,files=2,size=128k`，目录深度是`2`，宽度是`2`，每个目录里有`2`个文件，每个文件大小是`128KB` | 一共生成多少个文件呢？           |
| `fwd`参数：`operation=read,xfersize=4k`，读操作，每个`I/O`数据大小是`4K` |                                  |
| `fwd`参数：`fileio=sequential,fileselect=random,threads=2`，读文件是顺序读，文件选择是随机的，线程数是`2` |                                  |
| `rd`参数：`fwdrate=100,format=yes,elapsed=10,interval=1`，速率是`100`，需要格式化文件，执行时间是`10`秒，报告间隔是`1`秒 |                                  |
| 看注释中文件结构，会生成`4`个目录，每个目录里`2`个文件       |                                  |

我们将`anchor`修改成本地的路径，进行测试一下

```shell
[root@node1 vdbench50407]# cat testfile01 

*
* Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.
*

*
* Author: Henk Vandenbergh.
*



*Example 7: File system testing

fsd=fsd1,anchor=/mnt/nfs1/2024001,depth=2,width=2,files=2,size=128k

fwd=fwd1,fsd=fsd1,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2

rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

*
* This parameter file will use a directory structure of 4 directories and 8 files
* The RD parameter 'format=yes' causes the directory structure to be completely
* created, including initialization of all files to the requested size of 128k.
* After the format completes the following will happen for 10 seconds at a rate
* of 100 reads per second:
*         Start two threads (threads=2; 1 thread is default).
*         Each thread:
*         Randomly selects a file (fileselect=random)
*         Opens this file for read (operation=read)
*         Sequentially reads 4k blocks (xfersize=4k) until end of file (size=128k)
*         Closes the file and randomly selects another file.
*
*
* Directory structure:
*
* find dir1 | grep file
* dir1/vdb_control.file
* dir1/vdb1_1.dir/vdb2_1.dir/vdb_f0001.file
* dir1/vdb1_1.dir/vdb2_1.dir/vdb_f0002.file
* dir1/vdb1_1.dir/vdb2_2.dir/vdb_f0001.file
* dir1/vdb1_1.dir/vdb2_2.dir/vdb_f0002.file
* dir1/vdb1_2.dir/vdb2_1.dir/vdb_f0001.file
* dir1/vdb1_2.dir/vdb2_1.dir/vdb_f0002.file
* dir1/vdb1_2.dir/vdb2_2.dir/vdb_f0001.file
* dir1/vdb1_2.dir/vdb2_2.dir/vdb_f0002.file
*

# 第15行，我将anchor修改成了本地路径，/mnt/nfs1/存在，2024001不存在
```

```shell
# 部分结果被省略
[root@node1 vdbench50407]# ./vdbench -f testfile01 -o res+
...
10:27:15.593 localhost-0: Created anchor directory: /mnt/nfs1/2024001
10:27:17.003 Starting RD=format_for_rd1
10:27:17.084 localhost-0: anchor=/mnt/nfs1/2024001 mkdir complete.
10:27:17.117 localhost-0: anchor=/mnt/nfs1/2024001 create complete.
...
10:27:18.285 Miscellaneous statistics:
10:27:18.285 (These statistics do not include activity between the last reported interval and shutdown.)
10:27:18.286 FILE_CREATES        Files created:                                    8          8/sec
10:27:18.286 DIRECTORY_CREATES   Directories created:                              6          6/sec
10:27:18.286 WRITE_OPENS         Files opened for write activity:                  8          8/sec
10:27:18.287 DIR_BUSY_MKDIR      Directory busy (mkdir):                           4          4/sec
10:27:18.287 DIR_EXISTS          Directory may not exist (yet):                    4          4/sec
10:27:18.287 FILE_CLOSES         Close requests:                                   8          8/sec
10:27:18.287 
10:27:19.001 Starting RD=rd1; elapsed=10; fwdrate=100; For loops: None
...
10:27:29.256 Miscellaneous statistics:
10:27:29.256 (These statistics do not include activity between the last reported interval and shutdown.)
10:27:29.257 READ_OPENS          Files opened for read activity:                  30          3/sec
10:27:29.257 FILE_BUSY           File busy:                                        3          0/sec
10:27:29.257 FILE_CLOSES         Close requests:                                  28          2/sec
10:27:29.257 
10:27:29.836 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res002

# 第5行，先开始格式化，相当于预埋数据 --- 创建目录，生成文件
# 第18行，开始测试
# 第26行，执行成功，结果位于res002
```

!> 各个报告详解，请看[结果分析](#结果分析)

## 结果分析
