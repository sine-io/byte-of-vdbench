# 1. 前言

Vdbench是一款I/O负载生成工具，作者是Oracle的Henk Vandenbergh，它用于测试和评估裸设备、文件系统的性能。使用Java语言开发，适配过`Solaris Sparc`和`x86`、`各版本Windows`、`HP/UX`、`AIX`、`Linux`、`Mac OS X`、`zLinux`和`RaspBerry Pi`等操作系统。

> 注：新版本的Vdbench，可能没有适配全部的OS，此时请阅读对应目录中的`readme.txt`文件，按需进行Java JNI C编译即可。



# 2. 安装

从[Oracle网站](https://www.oracle.com/downloads/server-storage/vdbench-downloads.html)下载`Vdbench (5.04.07)`安装包（压缩包，zip格式），解压后长这样：

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

由于它使用Java语言开发，所以咱还需要准备Java环境（<font color="#FF00000">建议1.8及以上版本</font>）。

```shell
[root@node1 vdbench50407]# java -version 
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
```

执行测试命令验证Vdbench所需环境是否装好：

```shell
# linux
# 测试裸I/O工作负载
[root@node1 vdbench50407]# ./vdbench -t
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Vdbench distribution: vdbench50407 Tue June 05  9:49:29 MDT 2018
For documentation, see 'vdbench.pdf'.
...
10:14:35.871 Vdbench execution completed successfully. Output directory: /root/vdbench50407/output 

# 或者执行
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

完成测试后，通过浏览器访问`./output/summary.html`，查看Vdbench生成的报告。

> 注：后续测试环境的OS均为：CentOS7.9

# 3. 测试

验证完`Vdbench`的可执行环境没问题之后，我们开始测试工作。

命令格式如下：

```shell
# Unix:
/home/vdbench/vdbench –f parmfile

# Windows:
c:\vdbench\vdbench.bat –f parmfile
```

可以看到，这里面最重要的除了可执行参数，还有编写测试脚本或者叫参数配置文件（parmfile）。由于Vdbench可以测试裸设备和文件系统，这两者的参数配置文件又有所不同，咱们分开来说。









# 参数文件详解

## 通用规范

大多数参数都可以缩写为其最短的唯一值（最少2个字符）

```java
// Vdb_scan.java
/* Minimum 2 characters required: */
if (prm.keyword.length() < 2)
    parm_error("Keyword must contain a minimum of 2 characters: " + input);
```

```shell
xfersize=512
xf=512
```

参数的键可以大小写混合

```java
// Vdb_scan.java
stringxxx.toLowerCase()
```

```shell
# 示例
Xfersize=4k
```

参数值为一组时，必须用小括号括起来

```shell
files=(5,10)
```

当使用嵌入空格或其他特殊字符（`,`、`-`或`=`）时，必须用双引号将参数括起来

```shell
# 个人认为，空格和特殊字符可能在 startcmd 和 endcmd 常用
```

代表大小的参数（如`xfersize、size`等）可以用`KB/MB/GB/TB`、`k/m/g/t`表示 --- 大小写不敏感，随意组合（`KB、Kb、kB、K、k`等等），<font color="#FF00000">1k=1024bytes</font>

```shell
# 示例
xfersize=512kb
xfersize=512k
```

代表时间的参数（如`elapsed`）可以用分钟或小时表示

```shell
# 示例
elapsed=7200
elapsed=120m
elapsed=2h
```

参数值为多个数字

```shell
# 独立的值自由组合
keyword=(1,2,3,4,5,6,7,8,9,10,..)

# 自增组合
keyword=(1-10,1)
```

```shell
# 注意，手册中的这两种，经测试，跟手册描述不一致
# 第一种：
keyword=(1-64,d) # Doubles: from 1 to 64, each successive value doubled (1,2,4,8,16,32,64)
# 这种只取第一个值，就是1
# 测试脚本
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=fsd,depth=1,width=1,files=(1-64,d),sizes=1m,openflags=o_sync,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd*,format=yes


# 第二种：
keyword=(64,1,d) # Reverse double (divide by two)
# 报错，Mix of alpha and numeric values not allowed
# 测试脚本
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=fsd,depth=1,width=1,files=(64,1,d),sizes=1m,openflags=o_sync,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd*,format=yes
```

以`/`、`#`或`*`开头的行，行中第一个空格之后的任何内容（除非在双引号内）被视为注释

```java
// Vdb_scan.java
if (line.startsWith("/")   ||
    line.startsWith("#")   ||
    line.startsWith("*")   ||
    line.length() == 0)
    continue;
```

```shell
/ 注释1
# 注释2
* 注释3

create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10s,interval=1,totalsize=1m
rd=rd1,fwd=fwd*,format=yes
```

以`eof`开头的行被视为文件结束，之后的内容将被忽略

```java
// Vdb_scan.java
if (line.startsWith("eof"))
    break;
```

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,totalsize=1m
rd=rd1,fwd=fwd*,format=yes
eof
rd=rd2,fwd=fwd*,format=yes

# 第13行，有eof，后面的rd2不会执行
```

 在行尾加上逗号和空格，下一行以一个新的关键字开始，可以<font color="#FF00000">换行续写</font>

<font color="#FF00000">实测：不加逗号和空格也可以...保险起见还是添加吧</font>

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=fsd,depth=1,width=1,files=10,sizes=$sizes
openflags=o_sync
anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,totalsize=1m
rd=rd1,fwd=fwd*,format=yes

# 第6、7行，均会解析到fsd里
# 第5行末尾和第6行末尾均没加逗号和空格
```

详细的参数扫描报告将写入`output/parmscan.html`。当参数解析出现问题时，可以查看此文件，它显示了最后一个被读取和解析的参数。

所有输入参数将写入`output/parmfile.html`。为了在查看结果目录时，知道工作负载详情。

## General 参数

对于裸设备和文件系统是通用参数，<font color="#FF00000">非必选，必须放在参数文件的**首位**，位于所有HD、SD或FSD之前。</font>

| 参数                                                     | 说明                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| General parameters                                       |                                                              |
|                                                          |                                                              |
| abort_failed_skew=nnn                                    | Abort if requested workload skew is off by more than `nnn%`  |
|                                                          |                                                              |
| compratio=nn                                             | Specify the [compression ratio ](#_bookmark57)of the data pattern used for writes. |
| concatenatesds=yes                                       | See [SD Concatenation](#_bookmark134)                        |
|                                                          |                                                              |
| data_errors=nn                                           | [Terminate ](#_bookmark54)after ‘nn’ read/write/data validation errors (default 50) |
| data_errors=cmd                                          | Run command or script ‘cmd’ after first read/write/data validationerror, then [terminate.](#_bookmark54) |
|                                                          |                                                              |
| See also [Data Deduplication parameters:](#_bookmark137) |                                                              |
| dedupratio=                                              | Expected ratio. Default 1 (all blocks are unique).           |
| dedupunit=                                               | What size of data does Dedup compare?                        |
| dedupsets=                                               | How many sets of duplicates.                                 |
| deduphotsets=                                            | <font color="#FF00000">Dedicated</font>[^ 2 ] small sets of duplicates |
| dedupflipflop=                                           | Activate the Dedup <font color="#FF00000">flip-flop</font>[^ 3 ] logic. |
|                                                          |                                                              |
| formatsds=                                               | Force a one-time (pre)format of all SDs                      |
| formatxfersize=                                          | Specify xfersize used when creating, expanding, or (pre)formatting anSD. |
| fwd_thread_adjust=no                                     | Override the default of ‘yes’ to NOT allow FWD thread counts to be <font color="#FF00000">adjusted</font>[^ 4 ] because of integer <font color="#FF00000">rounding/truncation</font>[^ 5 ]. |
|                                                          |                                                              |
| histogram=(default,….)                                   | Override defaults for [response time histogram.](#_bookmark61) |
|                                                          |                                                              |
| ios_per_jvm=nnnnnn                                       | Override the 100,000 default warning for ‘i/o or operations per second per slave. This means that YOU will be responsible if you areoverloading your slaves. |
|                                                          |                                                              |
| journaling parameters:                                   |                                                              |
| journal=yes                                              | Activate [Data Validation and Journaling](#_bookmark148):    |
| journal=recover                                          | Recover existing [journal](#_bookmark31), validate data and run workload |
| journal=only                                             | Recover existing journal, validate data but do not run requestedworkload. |
| journal=noflush                                          | Use asynchronous I/O on journal files                        |
| journal=maponly                                          | Do NOT write before/after journal records                    |
| journal=skip_read_all                                    | After journal recovery, do NO read and validate every data block. |
| journal=(max=nnn)                                        | Prevent the journal file from getting larger than nnn bytes  |
| journal=ignore_pending                                   | Ignore pending writes during journal recovery.               |
|                                                          |                                                              |
| loop=                                                    | Repeat all Run Definitions: <br />`loop=nn`	repeat `nn` times <br />`loop=nn[s\|m\|h]`	repeat until `nn` seconds/minutes/hours See also [‘-l nn’ execution parameter](#_bookmark12) |
|                                                          |                                                              |
| monitor=/file/name                                       | See [External control of Vdbench termination](#_bookmark64)  |
|                                                          |                                                              |
| pattern=                                                 | Override the default [data pattern generation](#_bookmark56). |
|                                                          |                                                              |
| report=host_detail<br />report=slave_detail              | Specifies which SD detail reports to generate. Default is SD totalonly. |
| report=no_sd_detail<br />report=no_fsd_detail            | Will <font color="#FF00000">suppress</font>[^ 6 ] the creation of SD/FSD specific reports. |
|                                                          |                                                              |
| report_run_totals=yes                                    | [Reports run totals](#_bookmark205).                         |
|                                                          |                                                              |
|                                                          |                                                              |
| showlba=yes                                              | Create a ‘trace’ file so <font color="#FF00000">serve as</font>[^ 7 ] input to ./vdbench showlba |
|                                                          |                                                              |
| timeout=(nn,script)                                      | xxxxxxxxxxx                                                  |
|                                                          |                                                              |
| Data Validation parameters:                              |                                                              |
| validate=yes                                             | (-vt) [Activate Data Validation](#_bookmark148). Options can be combined:validate=(x,y,z) |
| validate=read_after_write                                | (-vr) Re-reads a data block immediately after it was written. |
| validate=no_preread                                      | (-vw) Do not read before rewrite, though <font color="blue">this defeats the purpose of data validation!</font> |
| validate=time                                            | (-vt) keep track of each write timestamp (memory intensive)  |
| validate=reportdedupsets                                 | Reports ‘last time used’ for all duplicate blocks if a duplicate block is found to be corrupted. Also activates validate=time. Note: large SDs with few dedup sets can generate loads of output! |

### include=parmfile

这个参数比较特殊，放哪里都可以 --- 不像其他General参数（必须放在开头）

可以根据需要，使用多个`include=parmfile`，但是太多的include会让脚本变的很复杂

使用场景一，将SD或FSD参数与其他参数分开 --- SD和FSD通常会频繁更改，而其他参数保持不变

使用场景二，想在已经创建的脚本中加入新的内容，如`include=payroll`或`include=email`

场景N，待发现

```shell
# 示例

[root@node1 testinclude]# cat fsd
fsd=fsd1,anchor=./dir,depth=1,width=1,files=10000,size=8k

[root@node1 testinclude]# cat fwd
fwd=fwd1,fsd=fsd1,operation=read,threads=16

[root@node1 testinclude]# cat rd
rd=rd1,fwd=fwd*,fwdrate=100,format=yes,elapsed=5,interval=1

# 虽然include可以有多个，但是参数位置不能乱，比如fsd必须要在fwd前面
[root@node1 testinclude]# cat create_files 
include=fsd
include=fwd
include=rd

# 执行
[root@node1 testinclude]# ~/vdbench50407/vdbench -f create_files
```

### data_errors=xxx

待研究

```shell
# 一种可以报错的组合

[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct,size=1024
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=0,xfersize=256,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,iorate=max,threads=$threads

# 第7行的 size=1024 和 第10行的 xfersize=256
# 会报错
```



### startcmd= and endcmd=

也可以写成`start_cmd`和`end_cmd`

如果命令包含空格，必须用双引号引起来

可以指定一个/多个命令，格式为`startcmd=(cmd1,cmd2,….)`，用括号括起来

`startcmd=cmd`会在本次运行前执行命令；`endcmd=cmd`会在全部运行结束或失败后立即执行。运行失败后 `endcmd` 是否会执行取决于错误的类型。当Vdbench运行过程中按CTRL-C后，不会执行`endcmd` 的命令。

| 参数                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| startcmd=command1         | Run `command1` on the first slave on a host                  |
| startcmd=("xxx yyy",cons) | Run `xxx yyy` sending output to the console.                 |
| startcmd=(cmd,master)     | Run `cmd` on the master; default `slave`. Note: <font color="blue">When used as a General Parameter, startcmd will always run on the master.</font> |
| startcmd=(cmd,sum)        | Send output to `summary.html`                                |
| startcmd=(cmd,log)        | Send output to `logfile.html` (default)                      |

#### startcmd=command1

```shell
[root@node1 testinclude]# cat create_files 
startcmd="echo start111"
endcmd="echo end222"
include=fsd
include=fwd
include=rd

[root@node1 testinclude]# cat output/logfile.html 
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
...
15:30:11.545 Start/end command: executing 'echo start111'
15:30:11.546 execute(): echo start111
15:30:11.551 Cmd: start111
...
15:30:21.307 Start/end command: executing 'echo end222'
15:30:21.308 execute(): echo end222
15:30:21.310 Cmd: end222
```

#### startcmd=("xxx yyy",cons)

```shell
[root@node1 testinclude]# cat create_files 
startcmd=("echo start111",cons)
endcmd="echo end222"
include=fsd
include=fwd
include=rd

# startcmd这种会在console里显示结果，也会在logfile.html里显示
# endcmd不会在console里显示结果，会在logfile.html里显示
```

#### startcmd=(cmd,sum)

```shell
[root@node1 testinclude]# cat create_files 
startcmd=("echo start111",cons)
endcmd=("echo end222",sum)
include=fsd
include=fwd
include=rd

# start111 在logfile.html里有结果
[root@node1 testinclude]# grep start111 -r output/
output/logfile.html:15:41:12.938 Start/end command: executing 'echo start111'
output/logfile.html:15:41:12.939 execute(): echo start111
output/logfile.html:15:41:12.943 Cmd: start111

# end222 结果从logfile.html移到了summary.html里
[root@node1 testinclude]# grep end222 -r output/
output/logfile.html:15:41:22.301 Start/end command: executing 'echo end222'
output/logfile.html:15:41:22.302 execute(): echo end222
output/summary.html:15:41:22.304 Cmd: end222 
```

#### startcmd=(cmd,log)

默认值，不演示了

#### startcmd=(cmd,master)

待研究

#### startcmd=(cmd1,cmd2)

```shell
[root@node1 testinclude]# cat create_files 
startcmd=("echo start111","echo start222",cons)
endcmd=("echo end222",sum)
include=fsd
include=fwd
include=rd


[root@node1 testinclude]# grep start* -r output/
output/logfile.html:15:48:50.911 Start/end command: executing 'echo start111'
output/logfile.html:15:48:50.912 execute(): echo start111
output/logfile.html:15:48:50.917 Cmd: start111
output/logfile.html:15:48:50.919 Start/end command: executing 'echo start222'
output/logfile.html:15:48:50.919 execute(): echo start222
output/logfile.html:15:48:50.924 Cmd: start222
```

### pattern=

待研究

### compratio=nn

待研究

### port=nnnn

```shell
[root@node1 testinclude]# cat create_files 
port=5555

include=fsd
include=fwd
include=rd

[root@node1 testinclude]# grep 5555 -r output/
output/logfile.html:15:54:09.645 Starting slave: /root/vdbench50407/vdbench SlaveJvm -m localhost -n localhost-10-240409-15.54.09.364 -l localhost-0 -p 5555   
output/parmfile.html:port=5555
output/parmscan.html:15:54:09.420 line: port=5555
output/parmscan.html:15:54:09.420 keyw: port=5555
output/parmscan.html:15:54:09.421 parm: 5555
output/localhost-0.stdout.html:15:54:09.804 15:54:09.803 SlaveJvm execution parameter:  '-p 5555'
output/localhost-0.stdout.html:15:54:09.936 15:54:09.935 Connection to localhost using port 5555 successful
output/config.html:root     1023366 1023326  0 15:54 pts/0    00:00:00 /bin/bash /root/vdbench50407/vdbench SlaveJvm -m localhost -n localhost-10-240409-15.54.09.364 -l localhost-0 -p 5555
output/config.html:root     1023370 1023366 65 15:54 pts/0    00:00:00 java -client -Xmx1024m -Xms128m -cp /root/vdbench50407/:/root/vdbench50407/classes:/root/vdbench50407/vdbench.jar Vdb.SlaveJvm SlaveJvm -m localhost -n localhost-10-240409-15.54.09.364 -l localhost-0 -p 5555
```

### ※ create_anchors=yes

<font color="#FF00000">因为上级目录不会被创建，所以需要多级目录（上级目录不存在时）需要指定为`yes`，这样会同步创建上级目录</font>

```shell
[root@node1 testinclude]# cat create_files 
create_anchors=yes  # 这里需要指定为yes，否则会报错

include=fsd
include=fwd
include=rd
[root@node1 testinclude]# cat fsd
fsd=fsd1,anchor=./dir/abc,depth=1,width=1,files=10000,size=8k  # 这里是多级目录，且上级目录没创建
```

### report=

从Vdbench 5.02版本开始，Vdbench不再为每个从节点或主机生成SD报告（200个SD在8个从节点和一个主机上会产生1800个报告！）。

如果需要这些报告，可以按需指定`report=host_detail`、`report=slave_detail`或缩写`report=(host,slave)`。

使用`report=no_sd_detail`可以完全禁止创建SD报告。



待研究



### histogram=

待研究

### formatxfersize=nnnn

待研究

### monitor=

待研究

### ※ messagescan=

默认情况下，Vdbench会每五秒钟扫描Solaris上的`/var/adm/messages`和Linux上的`/var/log/message`，以查找可能与您正在运行的工作负载相关的错误消息。

参数`messagescan=`可以让您对此进行一些控制。

| 参数                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| messagescan=no        | suppresses the scan                                          |
| messagescan=yes       | Keeps the default                                            |
| messagescan=nodisplay | Does the scan, but does not display the messages found on stdout, instead display on slave’s stdout. --- 后半句没测试，待测试完毕后验证 |
| messagescan=1000      | Stop scanning after 1000 messages. (default)                 |

```shell
[root@node1 testinclude]# cat create_files 
report=no_fsd_detail  # 不创建fsd的详情文件
create_anchors=yes
messagescan=no  # 不扫描message文件

include=fsd
include=fwd
include=rd
```

### timeout=(nn,script)

待研究

## 主机定义（HD）参数

对于裸设备和文件系统是通用的，<font color="#FF00000">多主机环境或单主机环境修改JVM数量时进行配置</font>

| 参数                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| clients=nn          | Very useful if you want to simulate <font color="#FF00000">numerous</font>[^ 1 ] clients for file servers without having all the hardware. <font color="#FF00000">Internally</font>[^ 2 ] is basically creates a new `hd=` parameter for each requested client. |
| mount="mount xxx …" | This mount command is <font color="#FF00000">issued</font>[^ 3 ] on the target host after the possibly needed mount directories have been created. |

### ※ hd=host_label

主机标签，唯一

值为`default`时，设定的参数，后面的HD共享

使用当前主机，用`localhost`即可

```shell
[root@node1 testinclude]# cat hd 
hd=default,user=root,shell=ssh,vdbench=/root/vdbench50407
hd=one,system=172.38.30.122  # 由于脚本都在122上，所以这个可以用 hd=localhost 替代
hd=two,system=172.38.30.127
```

### ※ system=

主机的IP地址或者DNS --- 个人认为是`hosts`里配置的映射关系

```shell
[root@node1 testinclude]# cat hd 
hd=default,user=root,shell=ssh,vdbench=/root/vdbench50407
hd=one,system=172.38.30.122  # 由于脚本都在122上，所以这个可以用 hd=localhost 替代
hd=two,system=172.38.30.127
```

### ※ vdbench=/vdbench/dir/name

指定在多主机上，vdbench的目录

默认与当前执行主机的目录相同

如果目录带空格，请使用双引号

```shell
[root@node1 testinclude]# cat hd 
hd=default,user=root,shell=ssh,vdbench=/root/vdbench50407
```

当`shell=vdbench`时，此参数不需要，将被忽略

### master=system_name/IP address

待研究，感觉很少用

### ※ user=

`shell=rsh`和`shell=ssh`时，需要使用这个参数

请保证配置的`user`是正确的，可以通过`rsh`和`ssh`正常访问的

```shell
[root@node1 testinclude]# cat hd 
hd=default,user=root,shell=ssh,vdbench=/root/vdbench50407
```

### ※ shell=

#### shell=ssh

```shell
# 2个节点
# 已做免密

[root@node1 testinclude]# cat create_files 
create_anchors=yes
messagescan=no

include=hd
include=fsd
include=fwd
include=rd

# ssh必须做免密，配置vdbench目录参数和user参数
[root@node1 testinclude]# cat hd 
hd=default,user=root,shell=ssh,vdbench=/root/vdbench50407
hd=one,system=172.38.30.122  # 由于脚本都在122上，所以这个可以用 hd=localhost 替代
hd=two,system=172.38.30.127

[root@node1 testinclude]# cat fsd
fsd=fsd1,anchor=./dir1,depth=1,width=1,files=10000,size=8k,shared=yes  # 由于只有一个fsd，所以这里要添加shared=yes

[root@node1 testinclude]# cat fwd
fwd=fwd1,fsd=fsd1,operation=read,threads=16
[root@node1 testinclude]# cat rd
rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=restart  # 由于添加了shared=yes，这里也要添加restart
```

#### shell=vdbench

```shell
# 所有node节点都执行
[root@node2 vdbench50407]# ./vdbench rsh
09:59:33.024 Vdbench rsh daemon: waiting for new users


[root@node1 testinclude]# cat hd
hd=default,shell=vdbench
hd=localhost
hd=two,system=172.38.30.127

# node1作为脚本执行机器，也作为压力节点之一，不执行./vdbench rsh也可以
```

#### shell=rsh

```shell
# Linux系统中，默认情况下，rsh服务是禁用的，出于安全考虑，它被认为是不安全的，因为它传输的数据不经过加密。
# 这里不测试了
```

### jvms=nn

jvms默认最大8个，vdbench会根据情况（SDs个数等）自动调整至最大值

```shell
(base) [root@node30133 vdbench50407]# cat testraw 
messagescan=no

hd=default,jvms=16  # 这里将最大值调到了16
hd=localhost

sd=default,openflags=o_direct
sd=sd,lun=./testdir001/filename%04d,count=(1,21),size=20m

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,thread=12
```

### clients=nn

暂时不知道有什么用

### mount=xxx

待研究

## 针对 - 裸设备（Raw I/O）

参数配置文件内容包含以下几部分：**General**, **HD, RG, SD, WD**, **RD**

```shell
# 必须按顺序
# General --- 非必须
# HD      --- 非必须
# RG      --- 非必须
# SD
# WD
# RD
```



### 回放组（RG）参数

待整理



### 存储定义（SD）参数

`lun`可以是块存储，也可以是文件系统的某个文件，当测试文件系统的某个文件时，由文件系统来负责所有I/O，此时读写操作会受缓存影响（`openflags=`）

```shell
[root@node30133 vdbench50407]# cat testraw 
sd=sd1,lun=/root/vdbench50407/filename-001,openflags=o_direct,size=1g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=1,iorate=max
```

#### ※ sd=name

存储定义唯一标识

将`default`作为SD名称时，输入的值将作为随后所有SD参数的默认值

`name`可以是任意自由格式的名称，但不能包含特殊字符（具体哪些字符不知道，直接使用字母数字组合进行测试最简单，别折腾）

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no

sd=default,openflags=o_direct  # openflags作为后续所有的sd的默认值
sd=sd1,lun=/root/vdbench50407/testdir001/filename-001,size=1g
```

#### ※ lun=lun_name

块设备或者文件名

<font color="#FF00000">不要使用含有重要数据的块设备</font>

块设备的第一个块永远不会被写入，以防止卷标被覆盖

<font color="#FF00000">当`lun`是文件名时，路径中的目录必须存在，它不会自动创建目录</font>

```shell
(base) [root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=yes

sd=default,openflags=o_direct
sd=sd,lun=/root/vdbench50407/testdir001/filename%04d,count=(1,100),size=20m  # testdir001及上级目录必须存在，它不会自动创建
```

保险起见，测试块设备时，请使用以下命令对块设备进行预格式化：

```shell
sd=sd1,lun=xx
wd=wd1,sd=*,xf=1m,seekpct=eof,rdpct=0
rd=rd1,wd=*,iorate=max,elapsed=100h,interval=60
```

> 原因：一些卷管理器在读取未写入过的块时会直接返回一个空块，磁盘实际上并未被读取，这并不是一次有效的工作负载。
>
> 格式化时间必须足够长，以完全格式化卷，Vdbench在格式化完毕后会自动停止。
>
> <font color="#FF00000">如果使用通用参数 ---  `formatsds=yes` 就不用执行上述命令了，但是，每次使用包含此参数的参数文件进行测试时，SDs都会进行格式化。</font>

#### host=name

多机执行测试时，每台机器的`lun`不相同，此时需要指定`host`，告诉Vdbench哪台主机上`lun`是什么

`host`和`lun`必须成对出现

```shell
# host和lun没有顺序，谁在前都可以
sd=sd1,host=systemA,lun=/dev/rdsk/1,host=systemB,lun=/dev/rdsk/2

sd=sd1,lun=/dev/rdsk/a,host=hosta,lun=/dev/rdsk/b,host=hostb
```

默认情况下，Vdbench 假定每台主机上的`lun`名称相同。

#### count=(nn,mm)

快速创建一系列的SD

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

hd=default,jvms=16
hd=localhost

sd=default,openflags=o_direct
sd=sd,lun=./testdir001/filename%04d,count=(1,21),size=20m

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,thread=12

# 第9行，count=(1,21) 会创建多个SDs，%04d会生成0001~0021这种格式的文件名
# sd=sd --- 生成的sd名字为sd1、sd2... --- 整体的组成就是sd的值 + 顺序数字
```

#### ※ size=nn

描述块设备或文件的大小，拒绝使用/dev开头的文件名

可以用`k/m/g/t`输入

未指定大小，将从块设备或文件中获取

Vdbench的寻址超过2GB --- 这个暂时不知道什么意思，size肯定是可以设置超过2G的

<font color="#FF00000">当`size`设置的值 ＜ `xfersize`的3倍时，会报错 --- 没设置`hitearea`和`range`</font>

```shell
# 复现代码
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct,size=23k
sd=sd1,lun=/dev/sdj
sd=sd2,lun=/dev/sda

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,iorate=max,threads=$threads

# 第6行的 size=23k 和 第10行的 xfersize=8k

# 执行结果：
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
14:50:03.275 wd=wd1
14:50:03.275 Byte addresses for hits: Low:                0; High:                0; Width:                0
14:50:03.276 Byte addresses for miss: Low:            8,192; High:           16,384; Width:            8,192
14:50:03.276 sd max xfersize=8192
14:50:03.276 hitarea_used: 0
14:50:03.276 hitarea: 1048576
14:50:03.276 
14:50:03.276 wd=wd1: Lun space specified for either cache hits or cache misses (8192 bytes) must be at least twice the xfersize (8192)
14:50:03.277 Remember that the first block in a lun is never accessed, this to protect sector zero.
14:50:03.277 Please check the 'sd=nnn,size=xx,hitarea=' and 'wd=nnn,range=' parameters.
14:50:03.780 
14:50:03.780 Insufficient amount of lun size specified
14:50:03.780 
java.lang.RuntimeException: Insufficient amount of lun size specified
	at Vdb.common.failure(common.java:350)
	at Vdb.WG_entry.size_error(WG_entry.java:1409)
	at Vdb.WG_entry.calculateContext(WG_entry.java:549)
	at Vdb.RD_entry.finalizeWgEntries(RD_entry.java:1754)
	at Vdb.Vdbmain.masterRun(Vdbmain.java:842)
	at Vdb.Vdbmain.main(Vdbmain.java:628)

```

##### 几个小测试

###### formatsds=yes

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=yes

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=10g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=yes
# 第6行 --- count=(1,5),size=10g
# 第10行 -- elapsed=10s

# 结果：先开启format --- 先用2个线程生成5个10g的文件，文件名为filename0001...0004；
# 然后执行rd测试 --- 写测试10s

[root@node30133 testdir001]# ls -alh 
total 50G
drwxrwxrwx 1 root   root     5 Apr 10 14:50 .
drwxrwxrwx 1 nobody nobody  16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   10G Apr 10 14:55 filename0001
-rw-rw-rw- 1 root   root   10G Apr 10 14:55 filename0002
-rw-rw-rw- 1 root   root   10G Apr 10 14:55 filename0003
-rw-rw-rw- 1 root   root   10G Apr 10 14:55 filename0004
-rw-rw-rw- 1 root   root   10G Apr 10 14:55 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=yes

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=15g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=yes
# 第6行 --- count=(1,5),size=15g
# 第10行 -- elapsed=10s

# 结果：先开启format --- 先用2个线程写这5个文件，但是从目录里看，这几个文件大小依旧显示10g；等append的大小超过10g后才会显示文件大小的变化
# 所以formatsds并不会删了文件重新写，而是覆盖写文件的一部分
# 然后执行rd测试 --- 写测试10s

[root@node30133 testdir001]# ls -alh 
total 75G
drwxrwxrwx 1 root   root     5 Apr 10 14:50 .
drwxrwxrwx 1 nobody nobody  16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0001
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0002
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0003
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0004
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=yes

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=5g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=yes
# 第6行 --- count=(1,5),size=5g
# 第10行 -- elapsed=10s

# 结果：先开启format --- 先用2个线程写这5个文件，写到5g后停止，但是从目录里看，这几个文件大小依旧显示10g；
# 所以formatsds并不会删了文件重新写，而是覆盖写文件的一部分
# 写到5g后，开始10s的rd测试工作

[root@node30133 testdir001]# ls -alh 
total 75G
drwxrwxrwx 1 root   root     5 Apr 10 14:50 .
drwxrwxrwx 1 nobody nobody  16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0001
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0002
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0003
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0004
-rw-rw-rw- 1 root   root   15G Apr 10 14:58 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=yes

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,10),size=5g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=yes
# 第6行 --- count=(1,10),size=5g
# 第10行 -- elapsed=10s

# 结果：先开启format --- 先用2个线程写这10个文件，写到5g后停止，但是从目录里看，已有的5个10g文件大小依旧显示10g，新加的5个文件是5g；
# 所以formatsds并不会删了文件重新写，而是覆盖写文件的一部分
# 写到5g后，开始10s的rd测试工作

[root@node30133 testdir001]# ls -alh 
total 100G
drwxrwxrwx 1 root   root     10 Apr 10 14:58 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root    15G Apr 10 15:00 filename0001
-rw-rw-rw- 1 root   root    15G Apr 10 15:00 filename0002
-rw-rw-rw- 1 root   root    15G Apr 10 15:00 filename0003
-rw-rw-rw- 1 root   root    15G Apr 10 15:00 filename0004
-rw-rw-rw- 1 root   root    15G Apr 10 15:00 filename0005
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:00 filename0006
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:00 filename0007
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:00 filename0008
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:00 filename0009
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:00 filename0010
```

综上，`formatsds=yes`时，如果没有文件会先创建`size`大小的文件，然后开始RD任务。

###### formatsds=no

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=5g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=10,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=no
# 第6行 --- count=(1,5),size=5g
# 第10行 -- elapsed=10s

# 结果：直接开始10s的rd测试工作，由于elapsed=10s，不够写完5g

[root@node30133 testdir001]# ls -alh 
total 1.6G
drwxrwxrwx 1 root   root      5 Apr 10 15:16 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   320M Apr 10 15:16 filename0001
-rw-rw-rw- 1 root   root   319M Apr 10 15:16 filename0002
-rw-rw-rw- 1 root   root   319M Apr 10 15:16 filename0003
-rw-rw-rw- 1 root   root   319M Apr 10 15:16 filename0004
-rw-rw-rw- 1 root   root   318M Apr 10 15:16 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=5g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=300,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=no
# 第6行 --- count=(1,5),size=5g
# 第10行 -- elapsed=300s

# 结果：直接开始300s的rd测试工作，150多秒的时候已经写到5g，但是依旧会将剩余的测试时间走完

[root@node30133 testdir001]# ls -alh 
total 25G
drwxrwxrwx 1 root   root      5 Apr 10 15:39 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:44 filename0001
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:44 filename0002
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:44 filename0003
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:44 filename0004
-rw-rw-rw- 1 root   root   5.0G Apr 10 15:44 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=7g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=300,interval=1,iorate=max,threads=12

# 第3行 --- formatsds=no
# 第6行 --- count=(1,5),size=7g
# 第10行 -- elapsed=300s

# 结果：直接开始300s的rd测试工作，所有文件还是会覆盖写，但是文件大小没变一直是5g，当超过5g时才会变，直到写完7g，写完后，依旧会将剩余的时间走完

[root@node30133 testdir001]# ls -alh 
total 35G
drwxrwxrwx 1 root   root      5 Apr 10 15:39 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   7.0G Apr 10 15:50 filename0001
-rw-rw-rw- 1 root   root   7.0G Apr 10 15:50 filename0002
-rw-rw-rw- 1 root   root   7.0G Apr 10 15:50 filename0003
-rw-rw-rw- 1 root   root   7.0G Apr 10 15:50 filename0004
-rw-rw-rw- 1 root   root   7.0G Apr 10 15:50 filename0005
```

综上，`formatsds=no`时，直接开始RD任务，size先到继续走elapsed剩下的时间，elapsed先到则任务停止。

#### range=(min,max)

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=7g,range=(0,50)

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=300,interval=1,iorate=max,threads=12

# 第6行，size=7g，range=(0,50)
# 结果，文件会从0字节开始写，写到3.5g结束

[root@node30133 testdir001]# ls -alh 
total 18G
drwxrwxrwx 1 root   root      5 Apr 10 15:58 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   3.5G Apr 10 16:00 filename0001
-rw-rw-rw- 1 root   root   3.5G Apr 10 16:00 filename0002
-rw-rw-rw- 1 root   root   3.5G Apr 10 16:00 filename0003
-rw-rw-rw- 1 root   root   3.5G Apr 10 16:00 filename0004
-rw-rw-rw- 1 root   root   3.5G Apr 10 16:00 filename0005
```

```shell
[root@node30133 vdbench50407]# cat testraw 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=7g,range=(50,60)

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=300,interval=1,iorate=max,threads=12

# 第6行，size=7g，range=(50,60)
# 结果，文件会从3.5G开始写，写到4.2g结束 --- 3.5+0.7

[root@node30133 testdir001]# ls -alh 
total 21G
drwxrwxrwx 1 root   root      5 Apr 10 16:20 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   4.2G Apr 10 16:06 filename0001
-rw-rw-rw- 1 root   root   4.2G Apr 10 16:06 filename0002
-rw-rw-rw- 1 root   root   4.2G Apr 10 16:06 filename0003
-rw-rw-rw- 1 root   root   4.2G Apr 10 16:06 filename0004
-rw-rw-rw- 1 root   root   4.2G Apr 10 16:06 filename0005

# 测试时发现的现象，filename000x瞬间就会到3.5g，怀疑使用了truncate命令，如下：
[root@node30133 testdir001]# truncate -s 2G bbb
[root@node30133 testdir001]# ls -alh bbb
-rw-rw-rw- 1 root   root   2.0G Apr 10 16:06 bbb
```

> 还有一个命令fallocate，但不是POSIX里必须要实现的，反之truncate是必须要有的，所以怀疑用了这个命令
>
> 可以看[这篇文章](https://zhuanlan.zhihu.com/p/622319148)的评论



默认情况下，将使用整个SD。要限制工作负载的寻址范围，可以指定SD的起始和结束范围：`range=(40,60)`将限制`I/O`活动从SD的40%开始，到SD的60%结束。

如果最大值大于100但小于200，Vdbench将认为这是在卷末端进行环绕。例如，使用range=(90,110)，Vdbench将生成一个I/O工作负载，使用卷最后的10%和卷开始的10%。

当值大于200时，将考虑这些值是以字节而不是百分比给出的；例如，'range=(1g,2g)'。

`range=`参数在Sd concatenation期间不可用。

如果需要`hitarea`参数，请将它放在`range`参数的开头。

#### ※ threads=nn

经测试，此参数是针对每个SD同时进行I/O的线程数量，感觉`threads`设置成 `≥2` 性能就上来了，基本达到顶点了

```shell
[root@node30133 vdbench50407]# cat testraw1 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj
sd=sd2,lun=/dev/sda
sd=sd3,lun=/dev/sdb
sd=sd4,lun=/dev/sdc
sd=sd5,lun=/dev/sdd
sd=sd6,lun=/dev/sde
sd=sd7,lun=/dev/sdf
sd=sd8,lun=/dev/sdg
sd=sd9,lun=/dev/sdh
sd=sd10,lun=/dev/sdi

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=2

# 第18行 threads=2，针对每个SD都会以2个线程进行压测，测试结果中有个queue，这个值 ~= threads*SDs个数
[root@node30133 vdbench50407]# ./vdbench -f testraw1 threads=2 -o res+
...
09:30:50.005 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=2

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1   272500.2  2128.91    8192   0.00    0.064    0.000    0.064     0.00   402.67    0.543   17.5  65.1  36.5
       2   317936.2  2483.88    8192   0.00    0.056    0.000    0.056     0.00    50.80    0.094   17.9  70.3  50.6
       3   321929.0  2515.07    8192   0.00    0.056    0.000    0.056     0.00    21.96    0.073   18.1  70.8  51.3
       4   314911.0  2460.24    8192   0.00    0.057    0.000    0.057     0.00    71.38    0.148   18.0  71.9  52.0
       5   315841.4  2467.51    8192   0.00    0.057    0.000    0.057     0.00    89.79    0.118   18.1  71.6  51.1
       6   320851.4  2506.65    8192   0.00    0.056    0.000    0.056     0.00    23.74    0.075   18.1  70.0  51.1
 avg_2-6   318293.8  2486.67    8192   0.00    0.057    0.000    0.057     0.00    89.79    0.105   18.1  70.9  51.2
```

`threads`决定了`lun`的最大并发性，而不是磁盘的并发性 --- 如果一个`lun`由16个物理磁盘构成，那么`threads=8`不能带来更多的并发

```shell
# 没环境试验，按照这句话的理解就是 --- threads要尽量≥磁盘的个数（单个SD由多个物理磁盘构成的话）
```

最大并发性仅在有足够需求时才会发生 --- 对一个可以处理`1000 iops`的设备请求10个iops，使用`thread=`参数不会起多大作用

```shell
[root@node30133 vdbench50407]# cat testraw1 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj
sd=sd2,lun=/dev/sda
sd=sd3,lun=/dev/sdb
sd=sd4,lun=/dev/sdc
sd=sd5,lun=/dev/sdd
sd=sd6,lun=/dev/sde
sd=sd7,lun=/dev/sdf
sd=sd8,lun=/dev/sdg
sd=sd9,lun=/dev/sdh
sd=sd10,lun=/dev/sdi

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=10,threads=16

# 第18行，iorate设置为10，threads设置的再高也没啥用
[root@node30133 vdbench50407]# ./vdbench -f testraw1 threads=16 -o res+
09:41:49.003 Starting RD=rd1; I/O rate: 10; elapsed=30; For loops: threads=16

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1       11.6     0.09    8192   0.00   23.891    0.000   23.891     0.00    54.59   18.581    0.3  11.6   3.9
       2        7.6     0.06    8192   0.00   14.168    0.000   14.168     0.00    49.87   15.284    0.1   1.3   0.5
       3        9.6     0.08    8192   0.00   10.339    0.000   10.339     0.00    43.48   10.484    0.1   1.5   0.5
       4        9.8     0.08    8192   0.00   10.224    0.000   10.224     0.00    45.52   11.204    0.1   2.4   0.9
       5        8.2     0.06    8192   0.00   15.532    0.000   15.532     0.00    51.86   14.006    0.1   3.7   1.2
       6       10.4     0.08    8192   0.00   24.588    0.000   24.588     0.00    54.03   16.512    0.3   5.4   2.2
 avg_2-6        9.1     0.07    8192   0.00   15.136    0.000   15.136     0.00    54.03   14.626    0.1   2.9   1.1
```

在使用SD concatenation时，此参数将被忽略 --- 这个没试验

可以使用`forthreads=`参数进行覆盖 --- 这个没试验

#### hitarea=nn

待测试

#### journal=name

待测试

#### offset=

开始偏移量，以字节为单位，必须是512的倍数

```shell
# 用块设备，不太好观测，这里用了文件进行测试

[root@node30133 vdbench50407]# cat testraw
messagescan=no
formatsds=no

sd=default,openflags=o_direct,offset=3g
sd=sd,lun=/mnt/nfs1/testdir001/filename%04d,count=(1,5),size=5g

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192

rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=2

# 第5行，offset=3g --- 生成的文件会直接显示3g，推测用了truncate命令
[root@node30133 testdir001]# ls -alh 
total 16G
drwxrwxrwx 1 root   root      5 Apr 12 09:55 .
drwxrwxrwx 1 nobody nobody   16 Apr 10 14:43 ..
-rw-rw-rw- 1 root   root   3.2G Apr 12 09:55 filename0001
-rw-rw-rw- 1 root   root   3.2G Apr 12 09:55 filename0002
-rw-rw-rw- 1 root   root   3.2G Apr 12 09:55 filename0003
-rw-rw-rw- 1 root   root   3.2G Apr 12 09:55 filename0004
-rw-rw-rw- 1 root   root   3.2G Apr 12 09:55 filename0005
```

#### align=

每当Vdbench生成一个随机的逻辑字节地址（LBA）时，默认情况下它总是在块边界上（`xfersize=`）。使用`align=`参数可以更改为始终在不同的对齐位置生成LBA。`align=`值以字节为单位，必须是512的倍数。

在指定`dedupratio=`时，不能使用此参数。

```shell
# 暂时不知道怎么去检查结果，用showlba参数，得到的结果好像也不行

```

#### ※ openflags=

跟操作系统的`open`和`close`参数有关。写操作根据文件系统的挂载方式或块设备默认处理方式来处理。

有可能数据到系统缓存后，写操作就显示完毕了。这样会有很好的性能，但是并不代表真实的结果。

Openflags可以针对SD、WD、FSD、FWD和RD参数进行设定，也可以创建任意的组合。



这里只列取Linux和Windows的 --- 因为这俩最常用

| 参数                                           | 说明                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Linux:                                         |                                                              |
| o_direct or directio                           | Gives you raw access to a full volume (no partition).  This parameter is *required* when using `/dev/xxx` volumes |
| o_dsync<br />o_rsync<br />o_sync               | These three all pass `0x01000` to the Linux `open()` function. *I highly suggest you check `/usr/include/bits/fcntl.h` since not all flavors of Linux use the same bits. Then if needed code `openflags=0x….` instead.* |
| fsync                                          | Call `fsync()` before the file is closed.                    |
| 0x……                                           | Any hex value, to be passed to the `open()` function.        |
|                                                |                                                              |
| Windows:                                       |                                                              |
| directio<br />o_dsync<br />o_rsync<br />o_sync | Opens a file using the `FILE_FLAG_NO_BUFFERING` flag         |

```java
// 源码 --- OpenFlags.java
// line 175 ~ 243
else if (common.onLinux()) {
    for (int i = 0; i < parm_list.length; i++) {
        String parm = parm_list[i];
        String tmp  = parm.toLowerCase();

        // o_driect/directio are 0x40000 on PPC

        /* This received 04/22/2016 from Forum: */
        if (arch.equals("aarch64")) {
            if (     tmp.equals("o_dsync"))   temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_rsync"))   temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_sync"))    temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_direct"))  temp_open_flags  |= 0x10000;
            else if (tmp.equals("directio"))  temp_open_flags  |= 0x10000;
            else if (tmp.startsWith("0x"))    temp_open_flags  |= hexFlags(tmp);
            else if (tmp.equals("fsync"))     temp_other_flags |= FSYNC_ON_CLOSE;
            else
            	common.failure("Invalid 'openflags=' parameter for Linux: " + parm);
        }

        /* This received 07/14/2016 from Pravin Kudav */
        else if (arch.equals("ppc64")) {
            if (     tmp.equals("o_dsync"))   temp_open_flags  |= 0x101000;
            else if (tmp.equals("o_rsync"))   temp_open_flags  |= 0x101000;
            else if (tmp.equals("o_sync"))    temp_open_flags  |= 0x101000;
            else if (tmp.equals("o_direct"))  temp_open_flags  |= 0x20000;
            else if (tmp.equals("directio"))  temp_open_flags  |= 0x20000;
            else if (tmp.startsWith("0x"))    temp_open_flags  |= hexFlags(tmp);
            else if (tmp.equals("fsync"))     temp_other_flags |= FSYNC_ON_CLOSE;
            else
            	common.failure("Invalid 'openflags=' parameter for Linux: " + parm);
        }

        else if (arch.equals("sparcv9")) {
            if (     tmp.equals("o_dsync"))   temp_open_flags  |=   0x2000;
            else if (tmp.equals("o_rsync"))   temp_open_flags  |= 0x802000;
            else if (tmp.equals("o_sync"))    temp_open_flags  |= 0x802000;
            else if (tmp.equals("o_direct"))  temp_open_flags  |= 0x100000;
            else if (tmp.equals("directio"))  temp_open_flags  |= 0x100000;
            else if (tmp.startsWith("0x"))    temp_open_flags  |= hexFlags(tmp);
            else if (tmp.equals("fsync"))     temp_other_flags |= FSYNC_ON_CLOSE;
            else
            	common.failure("Invalid 'openflags=' parameter for Linux: " + parm);
        }

        else {
            if (     tmp.equals("o_dsync"))   temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_rsync"))   temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_sync"))    temp_open_flags  |= 0x01000;
            else if (tmp.equals("o_direct"))  temp_open_flags  |= 0x004000;
            else if (tmp.equals("directio"))  temp_open_flags  |= 0x004000;
            else if (tmp.startsWith("0x"))    temp_open_flags  |= hexFlags(tmp);
            else if (tmp.equals("fsync"))     temp_other_flags |= FSYNC_ON_CLOSE;
            else
            	common.failure("Invalid 'openflags=' parameter for Linux: " + parm);
        }
    }
}
```

> 解释：
>
> ​		aarch64 --- ARM架构CPU，64位
>
> ​		ppc64 --- Linux和GCC开源软件社区内常用的，指向目标架构为64位PowerPC和Power Architecture处理器
>
> ​		SPARCv9 --- 基于RISC架构的处理器架构，通常用于Sun Microsystems的Solaris操作系统。

```java
// OpenFlags.java
// line 136 ~ 153
else if (common.onWindows()) {
    for (String parm : parm_list) {
        String tmp  = parm.toLowerCase();

        if (tmp.equals("o_dsync") || tmp.equals("o_rsync") || tmp.equals("o_sync")) {
            common.ptod("Windows: changing openflags=%s to openflags=directio", tmp);
            tmp = "directio";
        }

        if (tmp.equals("directio")) temp_open_flags |= WINDOWS_DIRECTIO;
        else
        	common.failure("Invalid 'openflags=' parameter for Windows: " + parm);
    }
}
```

```java
// OpenFlags.java
// line 27 ~ 36

/* These are flags that are directly passed to the Unix open() function: */
private        int openflags        = 0;
public  static int WINDOWS_DIRECTIO = 0x000001;  /* Hardcoded in vdbwin2k.c */

/* These are options to be used for other reasons: */
private int       otherflags       = 0;
public static int FSYNC_ON_CLOSE   = 0x000001;

// 第6行，WINDOWS_DIRECTIO = 0x000001，详见下面的vdbwin2k.c源码中第9行

// 第10行，FSYNC_ON_CLOSE   = 0x000001
```

```c
// 源码 --- vdbwin2k.c
// line 223 ~ 240

// 注意第9行，int WINDOWS_DIRECTIO = 1;
extern jlong file_open(JNIEnv *env, const char *filename, int openflag, int write)
{
    HANDLE fhandle;
    DWORD  access_type;
    int WINDOWS_DIRECTIO = 1;
    DWORD OVERLAP = FILE_FLAG_OVERLAPPED;
...
    if (openflag != 0 && openflag !=  WINDOWS_DIRECTIO)
    {
        PTOD1("Invalid open parameter for Windows: 0x%08x", openflag);
        abort();
    }
...
}
```

> 解释：
>
> ​		关于十六进制的值，根据OS不同，值也不一样，可以使用`grep`命令进行搜索，一般是fcntl.h文件
>
> ```shell
> [root@node30133 include]# grep O_DIRECT -r .
> ./asm-generic/fcntl.h:#ifndef O_DIRECT
> ./asm-generic/fcntl.h:#define O_DIRECT	00040000	/* direct disk access hint */
> ./asm-generic/fcntl.h:#ifndef O_DIRECTORY
> ./asm-generic/fcntl.h:#define O_DIRECTORY	00200000	/* must be a directory */
> ./asm-generic/fcntl.h:#define O_TMPFILE (__O_TMPFILE | O_DIRECTORY)
> ./asm-generic/fcntl.h:#define O_TMPFILE_MASK (__O_TMPFILE | O_DIRECTORY | O_CREAT)      
> ./scsi/sg.h:#define SG_INFO_DIRECT_IO_MASK	0x6
> ./scsi/sg.h:#define SG_INFO_DIRECT_IO 	0x2	/* direct IO requested and performed */
> ./bits/fcntl-linux.h:#ifndef __O_DIRECTORY
> ./bits/fcntl-linux.h:# define __O_DIRECTORY  0200000
> ./bits/fcntl-linux.h:#ifndef __O_DIRECT
> ./bits/fcntl-linux.h:# define __O_DIRECT	 040000
> ./bits/fcntl-linux.h:# define __O_TMPFILE   (020000000 | __O_DIRECTORY)
> ./bits/fcntl-linux.h:# define O_DIRECTORY	__O_DIRECTORY	/* Must be a directory.	 */
> ./bits/fcntl-linux.h:# define O_DIRECT	__O_DIRECT	/* Direct disk access.	*/
> (base) [root@node30133 include]# pwd
> /usr/include
> ```
>
> 关于几个SYNC，详情见[链接](https://zhuanlan.zhihu.com/p/645460951)
>
> ​		O_SYNC 等待写完成（数据和属性）
>
> ​		O_DSYNC 等待写完成（数据）
>
> ​		O_RSYNC 同步读和写
>
> ​		O_FSYNC 等待写完成（仅FreeBSD 和 Mac OS X)
>
> ​		O_ASYNC 异步I/O （仅FreddBSD 和 Mac OS X)
>
> 
>
> 关于O_DIRECT 和 O_SYNC的试验，见[链接](https://www.cnblogs.com/wyk930511/p/7414229.html)



### 工作负载定义（WD）参数

定义了 --- SD要执行的工作负载

许多参数都可以在运行定义（RD）中使用`forxxx=`参数进行覆盖

```shell
# Example: 
wd=wd1,sd=(sd1,sd2),rdpct=100,xfersize=4k
```

#### ※ wd=name

WD的唯一标识

如果将`default`指定为WD名称，那么输入的值将被用作后续所有WD参数的默认值

```shell
wd=default,xfersize=4k
wd=wd1,sd=(sd1,sd2),rdpct=100
wd=wd2,sd=(sd3,sd4),rdpct=100
```

#### host=host_label

多机操作时，必须指定哪台机器执行哪个负载

```shell
wd=wd1,host=hosta,…
wd=wd2,host=hostb,…
```

#### ※ sd=name

以下几种都可以：

```shell
# 单独一个
wd=wd1,sd=sd1

# 多个，可以连续或者不连续
wd=wd2,sd=(sd1,sd3)

# 多个，连续，可以用
wd=wd3,sd=(sd1-sd10)
# 注意这种方式不能前面加0
# 错误示范 -> wd=wd3,sd=(sd01-sd010)

# 多个，所有SD
wd=wd4,sd=sd*
```

`sd=` 还可以作为RD的子参数，RD章节再说

#### ※ rdpct=nn

读占比，默认为100 --- 纯读

可以使用`forrdpct=`覆盖 --- RD里的参数，到RD里再介绍

```shell
wd=wd4,sd=sd*,rdpct=100
# 100 代表纯读
wd=wd4,sd=sd*,rdpct=0
# 0   代表纯写
wd=wd4,sd=sd*,rdpct=80
# 80  代表混合读写比例4:1
```

```shell
[root@node30133 vdbench50407]# cat testraw3
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=seq,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 第8行，没添加rdpct参数，则其默认为100 --- 纯读
[root@node30133 vdbench50407]# ./vdbench -f testraw3 -o res+ threads=1
...
17:10:59.002 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    24752.6   193.38    8192 100.00    0.037    0.037    0.000    24.18     0.00    0.074    0.9  13.3   5.5
       2    27276.0   213.09    8192 100.00    0.036    0.036    0.000     0.86     0.00    0.004    1.0   4.2   3.0
       3    27570.6   215.40    8192 100.00    0.035    0.035    0.000     3.49     0.00    0.011    1.0   5.5   3.4
       4    27357.0   213.73    8192 100.00    0.036    0.036    0.000     0.47     0.00    0.004    1.0   4.7   3.3
       5    26886.8   210.05    8192 100.00    0.036    0.036    0.000     2.10     0.00    0.009    1.0   5.6   3.7
       6    26627.8   208.03    8192 100.00    0.037    0.037    0.000     1.50     0.00    0.008    1.0   4.4   2.9
 avg_2-6    27143.6   212.06    8192 100.00    0.036    0.036    0.000     3.49     0.00    0.008    1.0   4.9   3.2
```

```shell
[root@node30133 vdbench50407]# cat testraw3
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=seq,rdpct=0,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 第8行，rdpct=0，纯写
[root@node30133 vdbench50407]# ./vdbench -f testraw3 -o res+ threads=1
...
17:14:22.002 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    18910.2   147.74    8192   0.00    0.046    0.000    0.046     0.00    25.82    0.089    0.9  15.2   7.8
       2    19412.0   151.66    8192   0.00    0.046    0.000    0.046     0.00     4.83    0.017    0.9  16.3   9.2
       3    19561.8   152.83    8192   0.00    0.046    0.000    0.046     0.00     1.02    0.007    0.9  15.0   9.1
       4    19258.4   150.46    8192   0.00    0.046    0.000    0.046     0.00     0.92    0.008    0.9  23.7  11.9
       5    19782.0   154.55    8192   0.00    0.045    0.000    0.045     0.00     2.20    0.010    0.9  16.4   9.6
       6    19942.4   155.80    8192   0.00    0.045    0.000    0.045     0.00     1.39    0.008    0.9  15.1   8.8
 avg_2-6    19591.3   153.06    8192   0.00    0.046    0.000    0.046     0.00     4.83    0.011    0.9  17.3   9.7
```

```shell
[root@node30133 vdbench50407]# cat testraw3
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=seq,rdpct=80,xfersize=8192
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 第8行，rdpct=80，读写比例：4:1
[root@node30133 vdbench50407]# ./vdbench -f testraw3 -o res+ threads=1
...
17:15:58.003 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1     5994.4    46.83    8192  79.77    0.152    0.104    0.339    27.81    19.85    0.707    0.9   4.9   1.6
       2     6384.6    49.88    8192  79.73    0.152    0.102    0.348    16.61    20.04    0.712    1.0   5.3   2.6
       3     6626.6    51.77    8192  80.12    0.148    0.100    0.345    16.39    20.04    0.701    1.0  10.7   4.4
       4     6553.4    51.20    8192  79.78    0.150    0.102    0.342     9.96    20.05    0.697    1.0   5.3   2.9
       5     6597.2    51.54    8192  80.12    0.149    0.101    0.344    10.75    19.78    0.683    1.0   3.6   1.9
       6     6543.2    51.12    8192  79.76    0.151    0.102    0.343    10.08    20.07    0.673    1.0   3.8   2.3
 avg_2-6     6541.0    51.10    8192  79.91    0.150    0.101    0.344    16.61    20.07    0.693    1.0   5.7   2.8
```

#### rhpct=nn and whpct=nn

待测试

#### ※ xfersize=nn

每个I/O操作的数据传输量，单位允许使用`k`和`m`

<font color="#FF00000">默认值：4k，取值范围：≥512字节，经现阶段测试，一般4k、8k的取值，性能就能起来</font>

可以使用`forxfersize`替换 --- RD参数，到RD章节再说

```shell
# 直接定义
xfersize=8192
xfersize=4k
xfersize=1m

# 组合定义，占比加起来要等于100
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=seq,xfersize=(4k,10,8k,10,16k,80),rdpct=0
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 第14行 xfersize=(4k,10,8k,10,16k,80)
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
09:15:50.003 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    15802.6   216.26   14349   0.00    0.053    0.000    0.053     0.00     3.64    0.018    0.8  15.2   7.4
       2    16299.6   222.94   14342   0.00    0.054    0.000    0.054     0.00     1.65    0.010    0.9  13.6   8.0
       3    16643.8   227.60   14338   0.00    0.053    0.000    0.053     0.00     1.07    0.008    0.9  13.5   8.1
       4    16386.8   223.88   14325   0.00    0.054    0.000    0.054     0.00     1.12    0.008    0.9  12.9   7.9
       5    16089.0   220.24   14353   0.00    0.055    0.000    0.055     0.00     1.05    0.009    0.9  23.4  11.4
       6    16482.8   225.50   14345   0.00    0.054    0.000    0.054     0.00     0.45    0.007    0.9  13.8   8.5
 avg_2-6    16380.4   224.03   14341   0.00    0.054    0.000    0.054     0.00     1.65    0.008    0.9  15.4   8.8
```

```shell
# 随机值，需要搭配SD的align使用
# 指定 dedupratio= 时，不能使用这个参数
xfersize=(min,max,align)
# 示例
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=seq,xfersize=(4k,8k,1k),rdpct=0
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
09:24:00.003 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=30; For loops: threads=1

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    20317.4   119.19    6151   0.00    0.042    0.000    0.042     0.00    28.75    0.093    0.8  19.7   9.2
       2    22240.2   130.42    6149   0.00    0.041    0.000    0.041     0.00     0.86    0.007    0.9  15.9   9.6
       3    22521.0   131.85    6139   0.00    0.040    0.000    0.040     0.00     1.06    0.008    0.9  16.1   9.2
       4    22682.0   133.06    6151   0.00    0.040    0.000    0.040     0.00     1.45    0.009    0.9  16.6   9.9
       5    21673.4   127.06    6147   0.00    0.042    0.000    0.042     0.00    22.04    0.080    0.9  16.2   9.7
       6    21821.2   127.65    6134   0.00    0.042    0.000    0.042     0.00     1.24    0.008    0.9  16.1   9.6
 avg_2-6    22187.6   130.01    6144   0.00    0.041    0.000    0.041     0.00    22.04    0.036    0.9  16.2   9.6

# 按源代码逻辑，计算大致如下：
min=4k
max=8k
align=1k
blocks=(max - min) / align=(8-4)/1=4
random=ownmath.zero_to_one() * (blocks + 1)=0.398*5=1.99
xfersize=random * align + min=1.99*1024+4096=6134

# 详见源代码 --- Vdb.WG_entry.java
```

```java
// Vdb.WG_entry.java
/**
* xfersize= parameter. Either a single xfersize, a distribution list with
* xfersizes and percentages, or a three-part xfersize with min, max, align
* coded to allow for a random xfersize between min and max with an xfersize
* on an 'align' boundary.
*/
public static int wg_dist_xfersize(double xfersizes[], String wd_name)
{
    /* Single xfersize: */
    if (xfersizes.length == 1)
    	return(int) xfersizes[0];

    /* Distribution list: */
    if (xfersizes.length != 3)
    {
        double pct = (ownmath.zero_to_one() * 100);
        double cumpct = 0;
        int i;

        for (i = 0; i < xfersizes.length; i+=2)
        {
            cumpct += xfersizes[i+1];
            if (pct < cumpct)
            	return(int) xfersizes[i];
        }

        /* The parser in WD_entry allows a little rounding error to 100%, so do the */
        /* same here. If we get this far we're at the end of the list, so use it: */
        return(int) xfersizes[ xfersizes.length - 2];
    }

    /* xfersize=(min,max,align): */
    long   min      = (long) xfersizes[0];
    long   max      = (long) xfersizes[1];
    long   align    = (long) xfersizes[2];
    long   blocks   = (max - min) / align;
    long   random   = (long) (ownmath.zero_to_one() * (blocks + 1));
    long   xfersize = (long) (random * align + min);
    
    return(int) xfersize;
}
```

```java
// Vdb.ownmath.java
public static int  rand()
/*
*	return a pseudo random number [0 - (2**15) - 1] (0..32767)
*/
{
    next *= 1103515245;
    next += 12345;
    return((next >> 16) & 0x7FFF);
}

public static double  zero_to_one()
/*
*	return a *FAST* pseudo random number [0 - 1)
* only returns 32768 unique random numbers.  Do Not use as high precision!
*/
{
	return(rand() / 32768.0);
}
```

#### skew=nn

暂时不研究

#### ※ seekpct=nn

随机寻址（LBA）百分比

| 参数                                 | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| seekpct=100 or seekpct=random        | Every I/O will go to a different random seek address.        |
| seekpct=0 or seekpct=sequential      | There will be NO random seeks, and the run will therefore be purely sequential. When the end of the volume or file is reached, Vdbench will continue at the beginning, unless `seekpct=eof` (see below) is specified. <font color="#FF00000">Be aware that if the volume size is smaller than the cache size, continued processing will be all cache hits.</font> <br />Note: a 100% sequential workload is allowed to run on only ONE JVM, because without that limitation we could end up reading blocks 1,1,1,2,2,2,3,3,3,etc. Wonderful marketing performance numbers, but we're trying to stay honest here. |
| seekpct=seqnz seekpct=sequentialnz   | The ‘**nz**’ here tells Vdbench to NOT start on block zero each time, but instead start on a random block within the SD. This lowers the possibility that a second Vdbench run will just find all the data in cache. |
| seekpct=20                           | On average, 20% of the I/O operations will start at a new random seek address. This means that on average there will be one random seek, with 5 consecutive blocks of data transferred. See `stride=` below for skip sequential I/O. |
| seekpct=-1 or seekpct=eof            | A negative *one* value causes a sequential workload to terminate as soon as end-of-file is reached. Vdbench will continue until all workloads using `seekpct=eof` have reached EOF. |
| seekpct=poisson seekpct=(poisson,nn) | A Poisson seek distribution. See [fileselect=(poisson,nn)](#_bookmark180) |

一般就是顺序或随机 --- sequential（0） 或 random（100）

``` shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=max,threads=$threads

# 第8行：
#	seekpct=0  			 --- 顺序
#	seekpct=sequential   --- 顺序
#	seekpct=seq			 --- 顺序
#	seekpct=100			 --- 随机
#	seekpct=random	     --- 随机
```

其他值很少看见人用，暂时没研究 --- 这里有两点需要探究 ---> 源代码里的LBA和seek

可以被`forseekpct=`参数（不接受`random`或`seq`，只接受数值）覆盖

#### stride=(min,max)

暂未测试

#### range=nn

暂未测试

#### iorate=

暂未测试

#### priority=

暂未测试



### 运行定义（RD）参数

定义了 --- 需要执行哪些工作负载（WD），及IO速率、执行时间等等

```shell
rd=run1,wd=(wd1,wd2),iorate=1000,elapsed=60,interval=5 
```

#### ※ rd=name

rd的名字，唯一

值为`default`时，输入的值将作为所有随后的SD参数的默认值

```shell
rd=default,iorate=max
rd=rd1,wd=wd1,...
```

#### ※ wd=

需要执行的工作负载

```shell
# 指定单个wd
wd=wd1
# 逐个输入，指定多个wd
wd=(wd1,wd3)
# 范围输入，指定多个wd，这种不允许有0 --- wd01，这种不允许
wd=(wd1-wd99)
# 通配符，指定所有
wd=wd*
```

#### sd=xxx

一般都会在WD里指定`sd=`，但是在RD里设定`sd=`也是可以的，可以用`forxxx`参数

当WD和RD同时设定了`sd=`，WD里的相关参数会被覆盖

```shell
# 暂时没测试
```

#### ※ iorate=nn

I/O速率，测读写最大性能时，一般用`max`

| 参数                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| iorate=100             | Run a workload of 100 I/Os per second                        |
| iorate=(100,200,…)     | Run a workload of 100 I/Os per second, then 200, etc.        |
| iorate=(100-1000,100)  | Run workloads with I/O rates from 100 to 1000, incremented by100. |
| iorate=curve           | Run a performance curve. See below.                          |
| iorate=max             | Run the maximum **uncontrolled** I/O rate possible. See below. |
| iorate=(nn,ss,nn,ss,…) | nn,ss: pairs of I/O rates and seconds of duration for this I/O rate.See also ['distribution=variable'.](#_bookmark121) |

##### iorate=100

```shell
# 示例1
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=30,interval=5,iorate=100,threads=$threads

# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=2
16:17:20.003 Starting RD=rd1; I/O rate: 100; elapsed=30; For loops: threads=2

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1      100.0     0.78    8192   0.00    0.334    0.000    0.334     0.00    28.33    2.232    0.0   3.5   0.9
       2       92.4     0.72    8192   0.00    0.062    0.000    0.062     0.00     0.13    0.010    0.0   0.9   0.3
       3      104.0     0.81    8192   0.00    0.116    0.000    0.116     0.00     3.87    0.248    0.0   2.2   0.7
       4      105.2     0.82    8192   0.00    0.087    0.000    0.087     0.00     9.32    0.427    0.0   2.5   1.2
       5       96.0     0.75    8192   0.00    0.066    0.000    0.066     0.00     0.13    0.015    0.0  10.7   4.0
       6       98.8     0.77    8192   0.00    0.065    0.000    0.065     0.00     0.15    0.013    0.0   2.0   0.9
 avg_2-6       99.3     0.78    8192   0.00    0.080    0.000    0.080     0.00     9.32    0.228    0.0   3.7   1.4
```

##### iorate=(10,50,100)

```shell
# 示例2
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=(10,50,100),threads=$threads

# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
16:21:40.003 Starting RD=rd1; I/O rate: 10; elapsed=20; For loops: threads=1 iorate=10

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1        7.6     0.06    8192   0.00    6.992    0.000    6.992     0.00    28.72    9.213    0.1   4.1   1.3
       2       11.6     0.09    8192   0.00    3.012    0.000    3.012     0.00    50.85    9.515    0.0   2.4   1.0
       3       12.8     0.10    8192   0.00    4.400    0.000    4.400     0.00    46.72   10.439    0.1  11.5   4.1
       4        8.0     0.06    8192   0.00    5.463    0.000    5.463     0.00    48.61   10.010    0.0   0.6   0.3
 avg_2-4       10.8     0.08    8192   0.00    4.166    0.000    4.166     0.00    50.85    9.994    0.0   4.9   1.8

16:22:01.002 Starting RD=rd1; I/O rate: 50; elapsed=20; For loops: threads=1 iorate=50

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1       47.0     0.37    8192   0.00    0.265    0.000    0.265     0.00    18.62    1.517    0.0   2.6   0.6
       2       53.6     0.42    8192   0.00    0.088    0.000    0.088     0.00     3.04    0.241    0.0   5.3   2.1
       3       50.8     0.40    8192   0.00    0.140    0.000    0.140     0.00     3.39    0.337    0.0   1.5   0.5
       4       49.6     0.39    8192   0.00    0.070    0.000    0.070     0.00     0.56    0.050    0.0   1.7   0.7
 avg_2-4       51.3     0.40    8192   0.00    0.099    0.000    0.099     0.00     3.39    0.243    0.0   2.8   1.1

16:22:22.002 Starting RD=rd1; I/O rate: 100; elapsed=20; For loops: threads=1 iorate=100

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1       90.4     0.71    8192   0.00    0.159    0.000    0.159     0.00    24.44    1.327    0.0   9.9   3.3
       2      103.6     0.81    8192   0.00    0.060    0.000    0.060     0.00     0.12    0.009    0.0   0.9   0.3
       3      104.4     0.82    8192   0.00    0.077    0.000    0.077     0.00     2.45    0.142    0.0   1.6   0.7
       4       98.6     0.77    8192   0.00    0.092    0.000    0.092     0.00    12.23    0.550    0.0   3.4   1.1
 avg_2-4      102.2     0.80    8192   0.00    0.076    0.000    0.076     0.00    12.23    0.323    0.0   2.0   0.7

16:22:42.826 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res019 
```

##### iorate=(100-1000,500)

<font color="#FF00000">注意代码第10行 --- iorate=(100-1000,500) --- 取值100、600，由于第三个值超过了1000，所以直接结束了</font>

```shell
# 示例3
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=(100-1000,500),threads=$threads

# 注意第10行 --- iorate=(100-1000,500) --- 取值100、600，由于第三个值超过了1000，所以直接结束了
# 结果如下
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
16:27:06.003 Starting RD=rd1; I/O rate: 100; elapsed=20; For loops: threads=1 iorate=100

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1       87.2     0.68    8192   0.00    0.257    0.000    0.257     0.00    24.19    1.753    0.0  12.3   4.4
       2      103.8     0.81    8192   0.00    0.070    0.000    0.070     0.00     4.83    0.210    0.0   1.6   0.5
       3      110.6     0.86    8192   0.00    0.131    0.000    0.131     0.00    16.82    0.919    0.0   1.3   0.6
       4      102.4     0.80    8192   0.00    0.062    0.000    0.062     0.00     0.16    0.012    0.0   1.9   0.8
 avg_2-4      105.6     0.83    8192   0.00    0.089    0.000    0.089     0.00    16.82    0.557    0.0   1.6   0.6

16:27:27.002 Starting RD=rd1; I/O rate: 600; elapsed=20; For loops: threads=1 iorate=600

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1      576.6     4.50    8192   0.00    0.079    0.000    0.079     0.00    24.27    0.597    0.0   4.0   1.1
       2      604.2     4.72    8192   0.00    0.057    0.000    0.057     0.00     0.15    0.009    0.0  10.5   3.6
       3      602.4     4.71    8192   0.00    0.063    0.000    0.063     0.00    13.91    0.254    0.0   4.3   2.0
       4      592.0     4.63    8192   0.00    0.057    0.000    0.057     0.00     0.15    0.008    0.0   2.1   1.1
 avg_2-4      599.5     4.68    8192   0.00    0.059    0.000    0.059     0.00    13.91    0.147    0.0   5.7   2.2

16:27:47.796 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res021 
```

##### iorate=curve

步骤1：根据iorate=max算出最大值

步骤2：<font color="#FF00000">默认</font>依次按照最大值的10%、50%、70%、80%、90%、100%速率依次开展测试，每个都执行`elapsed=`时长。如果要改，请使用`curve=`参数，详见示例4-2

步骤3：如果想平均响应时间达到`n.n ms`后停止测试，可以设置 `stopcurve=n.n`参数，详见示例4-3

<font color="#FF00000">注意：`iorate=max`时的最大值跟`threads=`有关系</font>

与skew=参数结合时，表现一致，只是加上了偏移量，这里后续再测试

```shell
# 示例4-1
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=curve,threads=$threads

# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
16:33:03.002 Starting RD=rd1; I/O rate: Uncontrolled curve; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    18319.8   143.12    8192   0.00    0.044    0.000    0.044     0.00    28.26    0.106    0.8  12.3   6.4
       2    20085.8   156.92    8192   0.00    0.044    0.000    0.044     0.00    19.95    0.084    0.9  17.3   9.7
       3    21274.0   166.20    8192   0.00    0.042    0.000    0.042     0.00     0.81    0.007    0.9  16.3   9.6
       4    20303.8   158.62    8192   0.00    0.044    0.000    0.044     0.00     0.91    0.008    0.9  23.3  11.7
 avg_2-4    20554.5   160.58    8192   0.00    0.044    0.000    0.044     0.00    19.95    0.048    0.9  19.0  10.3

16:33:24.002 Starting RD=rd1_(10%); I/O rate: 2100; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
avg_2-4     2105.2    16.45    8192   0.00    0.053    0.000    0.053     0.00     3.72    0.021    0.1   3.8   1.9

16:33:45.002 Starting RD=rd1_(50%); I/O rate: 10300; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    10280.9    80.32    8192   0.00    0.052    0.000    0.052     0.00    19.45    0.087    0.5  13.0   7.0

16:34:06.001 Starting RD=rd1_(70%); I/O rate: 14400; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    14441.4   112.82    8192   0.00    0.050    0.000    0.050     0.00     2.96    0.010    0.7  16.4   8.8

16:34:27.001 Starting RD=rd1_(80%); I/O rate: 16500; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    16476.5   128.72    8192   0.00    0.048    0.000    0.048     0.00    21.00    0.056    0.8  14.2   8.3

16:34:48.001 Starting RD=rd1_(90%); I/O rate: 18500; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    18507.1   144.59    8192   0.00    0.046    0.000    0.046     0.00     0.91    0.007    0.9  17.7   9.9

16:35:09.001 Starting RD=rd1_(100%); I/O rate: 20600; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
              rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
  avg_2-4    20487.3   160.06    8192   0.00    0.044    0.000    0.044     0.00     2.03    0.009    0.9  17.5  10.0

16:35:29.832 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res022 
```

```shell
# 示例4-2
[root@node30133 vdbench50407]# cat testraw4
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=curve,threads=$threads,curve=(5-100,50)

# 注意第10行，iorate=curve 和 curve=(5-100,50)
# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
16:52:51.002 Starting RD=rd1; I/O rate: Uncontrolled curve; elapsed=20; For loops: threads=1

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
              rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    20762.3   162.21    8192   0.00    0.043    0.000    0.043     0.00     3.90    0.011    0.9  15.3   9.1

16:53:12.001 Starting RD=rd1_(5%); I/O rate: 1100; elapsed=20; For loops: threads=1
...
 avg_2-4     1091.3     8.53    8192   0.00    0.054    0.000    0.054     0.00     0.27    0.007    0.1   3.7   1.7

16:53:33.001 Starting RD=rd1_(55%); I/O rate: 11500; elapsed=20; For loops: threads=1
...
 avg_2-4    11492.3    89.78    8192   0.00    0.052    0.000    0.052     0.00    38.82    0.107    0.6  14.4   7.6

16:53:53.763 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res026 
```

```shell
# 示例4-3
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=curve,threads=$threads,curve=(5-100,50),stopcurve=0.1

# 第10行 --- iorate=curve,curve=(5-100,50),stopcurve=0.1
# 当resp time这一列的值，超过0.1 ms后，会停止下一轮的测试
# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=4
...
17:01:00.002 Starting RD=rd1; I/O rate: Uncontrolled curve; elapsed=20; For loops: threads=4
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    34896.4   272.63    8192   0.00    0.101    0.000    0.101     0.00    30.83    0.166    3.5  28.8  14.8
       2    35840.0   280.00    8192   0.00    0.103    0.000    0.103     0.00    23.94    0.194    3.7  21.4  13.6
       3    36450.8   284.77    8192   0.00    0.102    0.000    0.102     0.00    16.67    0.118    3.7  21.9  13.8
       4    36629.8   286.17    8192   0.00    0.102    0.000    0.102     0.00     3.59    0.044    3.7  21.4  14.1
 avg_2-4    36306.9   283.65    8192   0.00    0.102    0.000    0.102     0.00    23.94    0.133    3.7  21.5  13.8

17:01:21.001 Starting RD=rd1_(5%); I/O rate: 1900; elapsed=20; For loops: threads=4
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1     1790.0    13.98    8192   0.00    0.164    0.000    0.164     0.00    30.14    0.935    0.3   7.8   2.8
       2     1866.6    14.58    8192   0.00    0.113    0.000    0.113     0.00    15.49    0.476    0.2   2.5   1.3
       3     1886.6    14.74    8192   0.00    0.135    0.000    0.135     0.00    14.98    0.542    0.3  12.2   4.9
       4     1892.8    14.79    8192   0.00    0.075    0.000    0.075     0.00     1.71    0.095    0.1   2.2   1.3
 avg_2-4     1882.0    14.70    8192   0.00    0.108    0.000    0.108     0.00    15.49    0.420    0.2   5.6   2.5

17:01:41.030 Vdbench terminating because of requested 'stopcurve=0.100' with observed response time of 0.108 ms
17:01:41.535 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res028 

# 看第35行的提示，avg resp time ＞ 0.1ms，所以后续的测试停止了
```

##### iorate=max

以最大的I/O速率开展测试 --- 跟`threads=`参数（压力大小）有关系

跟WD或FWD的skew=参数共同使用时，待研究了skew参数后进行补充

FWD中fwdrate=参数共同使用时，待研究了fwdrate参数后进行补充

当在RG（回放）中使用时，待研究了RG后进行补充 --- 暂定补充到RG章节

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=5,iorate=max,threads=$threads

# 以不同的threads执行，结果如下：
# threads=1
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
17:19:40.002 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=20; For loops: threads=1
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    20634.3   161.21    8192   0.00    0.044    0.000    0.044     0.00     2.17    0.010    0.9  18.7  10.4
17:20:01.416 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res029 

# threads=4
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=4
...
17:20:06.002 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=20; For loops: threads=4
interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
...
 avg_2-4    36343.3   283.93    8192   0.00    0.102    0.000    0.102     0.00    23.14    0.104    3.7  24.8  14.9
17:20:27.412 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res030 
```

#### ※ elapsed=nn

每个WD或FWD的运行时间，单位是`秒`

取值最少是`interval=`参数的两倍

```java
// 看源代码逻辑
// RD_entry.java
/* Check some values: */
if (rd.getElapsed() / rd.getInterval() < 2)
	common.failure("'elapsed=' time value must be at least twice the 'interval=' value");

if (rd.getElapsed() != RD_entry.NO_ELAPSED && rd.getElapsed() % rd.getInterval() != 0)
    common.failure("'elapsed="  + rd.getElapsed() +
    "' must be multiple of 'interval=" + rd.getInterval() + "'.");
```

与`maxdata=`参数结合时，详见`maxdata=`章节

与`format=limited`参数结合时，

与`seek=eof`结合时，



#### ※ interval=nn

报告时间间隔，单位`秒`

如果运行时间太长，间隔又很短，报告会变的非常大，这时可以设置时间间隔长一点，比如：60s

<font color="#FF00000">时间间隔设置的越长，性能的短板越难暴露出来，所以需要设置一个合适的值 --- 具体多少得视情况而定</font>

#### ※ warmup=nn

预热时间，单位`秒`

Vdbench将排除前`warmup/interval`个间隔的数据

使用了`warmup`，则表示在`warmup`结束后再运行`elapsed=`秒

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj,align=1k

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=2,iorate=max,threads=$threads,warmup=4

# 看第9行 --- elapsed=20,interval=2,warmup=4
# warmup是interval的倍数
# 预期结果应该是 warmup/interval=2，所以前2个间隔的数据被排除；elapsed/interval=10，所以一共12组数据

[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
09:16:30.003 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=20 warmup=4; For loops: threads=1

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1    14790.0   115.55    8192   0.00    0.046    0.000    0.046     0.00    24.24    0.159    0.7  26.6   9.9
       2    18883.0   147.52    8192   0.00    0.046    0.000    0.046     0.00    18.46    0.123    0.9  18.0  10.1
       3    20623.0   161.12    8192   0.00    0.043    0.000    0.043     0.00     0.51    0.006    0.9  15.3   8.9
       4    19514.5   152.46    8192   0.00    0.046    0.000    0.046     0.00     1.59    0.013    0.9  17.0  10.1
       5    19953.5   155.89    8192   0.00    0.045    0.000    0.045     0.00     1.07    0.008    0.9  15.2   9.2
       6    19831.0   154.93    8192   0.00    0.045    0.000    0.045     0.00     0.27    0.006    0.9  14.9   8.7
       7    20355.5   159.03    8192   0.00    0.044    0.000    0.044     0.00     0.19    0.006    0.9  14.7   9.0
       8    20573.0   160.73    8192   0.00    0.044    0.000    0.044     0.00     0.41    0.006    0.9  14.5   8.8
       9    19668.5   153.66    8192   0.00    0.046    0.000    0.046     0.00     1.84    0.012    0.9  15.1   9.1
      10    20342.5   158.93    8192   0.00    0.044    0.000    0.044     0.00     0.39    0.006    0.9  15.1   9.2
      11    20451.5   159.78    8192   0.00    0.044    0.000    0.044     0.00     1.08    0.008    0.9  17.3   9.4
      12    20683.5   161.59    8192   0.00    0.044    0.000    0.044     0.00     0.29    0.006    0.9  16.3   9.8
avg_3-12    20199.7   157.81    8192   0.00    0.045    0.000    0.045     0.00     1.84    0.008    0.9  15.5   9.2

09:16:55.394 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res004 
```

`warmup`必须是`interval`的整数倍 --- 这个提示感觉反了，应该是`warmup must be multiple of interval`

```java
// 看源代码逻辑
// RD_entry.java
/* Check some values: */
if (rd.getWarmup() % rd.getInterval() != 0)
    common.failure("'interval="  + rd.getInterval() +
    "' must be multiple of 'warmup=" + rd.getWarmup() + "'.");
```

#### ※ maxdata=

读写数据大小，三种参数可用：`maxdata=`、`maxdatawritten=`、`maxdataread=`

##### 0≤ maxdata ≤100 时：为百分比

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct,size=1g
sd=sd1,lun=/dev/sdj
sd=sd2,lun=/dev/sda

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,iorate=max,threads=$threads,maxdata=50

# 第5行 --- size=1g，每个SD大小是1GB
# 第6&7行 --- 共2个SD
# 第10行 --- maxdata=50，值处于0~100之间
# 总限制大小计算：1G*2*50%=1GB
```

##### maxdata ＞100 时：为实际的字节数

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct,size=1g
sd=sd1,lun=/dev/sdj
sd=sd2,lun=/dev/sda

wd=wd1,sd=sd*,seekpct=0,xfersize=512,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,iorate=1,threads=$threads,maxdata=1000

# 第5行 --- size=1g
# 第9行 --- xfersize=512
# 第10行 --- iorate=1,maxdata=1000 --- maxdata ＞ 100，所以为实际字节数
# 设置iorate=1是为了更好的观察，由于Vdbench将第一个interval的report丢弃，所以在第3个interval的时候就超过了1000字节

# 结果如下：
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
15:19:25.002 Starting RD=rd1; I/O rate: 1; elapsed=20; For loops: threads=1

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1        1.0     0.00     512   0.00    0.300    0.000    0.300     0.00     0.30    0.000    0.0   6.1   1.1
       2        1.0     0.00     512   0.00    9.610    0.000    9.610     0.00     9.61    0.000    0.0   2.6   0.6
       3        1.0     0.00     512   0.00    0.466    0.000    0.466     0.00     0.47    0.000    0.0   1.2   0.3

15:19:28.024 Reached maxdata=1k. rd=rd1 shutting down after next interval. 

      4        0.0     0.00       0   0.00    0.000    0.000    0.000     0.00     0.00    0.000    0.0   2.7   0.8
avg_2-4        0.7     0.00     512   0.00    5.038    0.000    5.038     0.00     9.61    6.466    0.0   2.1   0.5

15:19:30.462 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res023 
```

##### 与`elapsed=`参数一起使用时

以哪个先到为准

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,iorate=max,threads=$threads,maxdata=100g

# 第9行 --- elapsed=20 和 maxdata=1g或100g
# 谁先到，就结束
```

##### 与warmup=参数一起使用时

`maxdata`的数据量计算，不计算`warmup=`参数的时间间隔

`maxdata`仅在报告间隔（`interval`）结束后进行数据量检查

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0
rd=rd1,wd=wd*,elapsed=20,interval=1,warmup=5,iorate=max,threads=$threads,maxdata=1g

# 第9行 --- elapsed=20,interval=1,warmup=5,maxdata=1g
# warmup/interval=5/1=5，所以warmup会有5个报告间隔
# 再5个报告间隔后，开始maxdata的计算

# 结果 --- 注意第34行
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
15:40:05.003 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=20 warmup=5; For loops: threads=1

interval        i/o   MB/sec   bytes   read     resp     read    write     read    write     resp  queue  cpu%  cpu%
               rate  1024**2     i/o    pct     time     resp     resp      max      max   stddev  depth sys+u   sys
       1     8465.0    66.13    8192   0.00    0.054    0.000    0.054     0.00    24.27    0.364    0.5  10.8   2.7
       2    20606.0   160.98    8192   0.00    0.041    0.000    0.041     0.00     0.86    0.008    0.9  15.7   8.5
       3    19235.0   150.27    8192   0.00    0.045    0.000    0.045     0.00     0.28    0.006    0.9  15.0   8.3
       4    19759.0   154.37    8192   0.00    0.045    0.000    0.045     0.00     1.43    0.017    0.9  22.5  11.9
       5    20091.0   156.96    8192   0.00    0.045    0.000    0.045     0.00     0.99    0.009    0.9  15.6   9.0
	   # 前5行不计算在内，从第6个时间间隔开始计算数据量
       6    20232.0   158.06    8192   0.00    0.044    0.000    0.044     0.00     0.51    0.006    0.9  14.6   8.2
       7    20692.0   161.66    8192   0.00    0.044    0.000    0.044     0.00     0.25    0.005    0.9  15.9   8.6
       8    19908.0   155.53    8192   0.00    0.045    0.000    0.045     0.00     0.18    0.006    0.9  15.8   9.3
       9    20638.0   161.23    8192   0.00    0.044    0.000    0.044     0.00     2.23    0.016    0.9  16.3   8.8
      10    20633.0   161.20    8192   0.00    0.044    0.000    0.044     0.00     1.18    0.011    0.9  14.5   8.8
      11    20359.0   159.05    8192   0.00    0.044    0.000    0.044     0.00     0.13    0.005    0.9  14.5   8.8
      12    19942.0   155.80    8192   0.00    0.045    0.000    0.045     0.00     0.78    0.007    0.9  14.7   8.7
15:40:17.016 Reached maxdata=1.086g. rd=rd1 shutting down after next interval. 

      13    21122.0   165.02    8192   0.00    0.043    0.000    0.043     0.00     0.28    0.005    0.9  15.7   8.7
avg_6-13    20440.8   159.69    8192   0.00    0.044    0.000    0.044     0.00     2.23    0.009    0.9  15.3   8.7
15:40:19.401 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res030 
```

##### 源代码逻辑（供参考）

```java
// 源代码
// CollectSlaveStats.java
// run() ---> doDetailedReporting() ---> checkMaxData()

/**
* Shut down this RD after maxdata= bytes.
* If maxdata < 100, then it is a multiplier of all active SDs.
*/
private static void checkMaxData(long bytes_read, long bytes_written)
{
	/* Calculate total active SD size: */
    double limit_t = getLimit(rd.max_data);
    double limit_r = getLimit(rd.max_data_r);
    double limit_w = getLimit(rd.max_data_w);
	...
    /* Shutdown if maxdata= requested: */
    if (rd.max_data != Double.MAX_VALUE && total_bytes > limit_t)
    {
        common.pboth("Reached maxdata=%s. rd=%s shutting down after next interval. ",
        FileAnchor.whatSize(total_bytes), rd.rd_name);
        Vdbmain.setWorkloadDone(true);
    }
    else if (rd.max_data_r != Double.MAX_VALUE && bytes_read > limit_r)
    {
        common.pboth("Reached maxdataread=%s. rd=%s shutting down after next interval. ",
        FileAnchor.whatSize(limit_r), rd.rd_name);
        Vdbmain.setWorkloadDone(true);
    }
    else if (rd.max_data_w != Double.MAX_VALUE && bytes_written > limit_w)
    {
        common.pboth("Reached maxdatawritten=%s. rd=%s shutting down after next interval. ",
        FileAnchor.whatSize(limit_w), rd.rd_name);
        Vdbmain.setWorkloadDone(true);
    }
}

// 其中 limit_t、limit_r、limit_w 由getLimit方法得来
private static double getLimit(double max)
{
    double limit = max;
    if (max <= 100)
    {
    	limit = 0;
        for (SD_entry sd : Vdbmain.sd_list)
        {
            if (sd.isActive())
                limit += sd.end_lba;
        }
        limit = limit * max / 100;
    }
    return limit;
}
```

#### distribution=xxx

待研究

#### pause=nn

在下一次`RD`执行前休眠`nn`秒，第一次不会休眠

使用 `pause=nn` 可以让处理一些事情，如清空缓存等 --- 这里我能想到的是用`startcmd=` & `endcmd=`这俩参数进行操作

```shell
[root@node30133 vdbench50407]# cat testraw4 
messagescan=no
formatsds=no

sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdj

wd=wd1,sd=sd*,seekpct=0,xfersize=8k,rdpct=0

rd=default,wd=wd*,elapsed=10,interval=2,iorate=max,threads=$threads,pause=5
rd=rd1,endcmd=("echo rd1 end.",cons)
rd=rd2,startcmd=("echo rd2 start.",cons)

# 第10行 --- pause=5
# 结果
[root@node30133 vdbench50407]# ./vdbench -f testraw4 -o res+ threads=1
...
16:11:50.002 Starting RD=rd1; I/O rate: Uncontrolled MAX; elapsed=10; For loops: threads=1
...
 avg_2-5    19619.6   153.28    8192   0.00    0.045    0.000    0.045     0.00     1.78    0.009    0.9  16.8   9.7
16:12:00.167 localhost-0: Start/end command: executing 'echo rd1 end.'
16:12:00.197 localhost-0: Cmd: rd1 end.

# 第2个RD执行前，pause了5秒
16:12:00.312 Waiting 5 seconds; requested by 'pause' parameter

16:12:05.333 localhost-0: Start/end command: executing 'echo rd2 start.'
16:12:05.340 localhost-0: Cmd: rd2 start.
16:12:06.002 Starting RD=rd2; I/O rate: Uncontrolled MAX; elapsed=10; For loops: threads=1
...
 avg_2-5    19791.4   154.62    8192   0.00    0.045    0.000    0.045     0.00     3.91    0.015    0.9  16.3   9.3

16:12:17.047 Vdbench execution completed successfully. Output directory: /root/vdbench50407/res035 
```

#### RD里直接编写WD参数 --- forxxx

这里先不研究，挺复杂的



## 针对 - 文件系统（File system）

参数配置文件内容包含以下几部分：**General**, **HD, FSD, FWD**, **RD**

```shell
# 必须按顺序
# General --- 非必须
# HD      --- 非必须
# FSD
# FWD
# RD
```

### 文件系统存储定义（FSD）参数

警告：指定目录和文件结构很容易，但也很容易使其过大。`Width=5,depth=5,files=5`会产生3905个目录和15625个文件！

<font color="blue">Vdbench允许每个FSD最多有3200万个文件，在64位Java上运行时为1.28亿个文件。每个文件大约需要64字节的Java堆空间，可能会导致内存问题。需要调整`./vdbench`脚本中的`Xmx`参数进行解决。</font>

#### ※ fsd=name

<font color="#FF00000">文件系统唯一标识，不能重复（多个`default`没事儿，其他`fsd`名字不能重复，必须得有一个不是`default`的`fsd`）</font>

```shell
[root@node30133 vdbench50407]# cat testfilesystem 
messagescan=no
cr=yes

fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd1,anchor=/mnt/nfs1
fsd=fsd2,anchor=/mnt/nfs1/testdir

fwd=fwd1,fsd=fsd*,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

# 第6、7行
# 可以设置为多个default的fsd；
# 必须得有一个不是default的fsd，否则报错 --- Could not find fsd=fsd* for FWD=fwd1
# 如果多个fsd都叫同一个名字（比如fsd1），会报错，详见下面重复校验源代码
```

```java
// 重复校验源代码
// FsdEntry.java
/* Don't allow duplicates: */
for (int i = 0; i < fsd_list.size(); i++)
{
    fsd = (FsdEntry) fsd_list.elementAt(i);
    if (fsd.name.equalsIgnoreCase(prm.alphas[0]) &&
    fsd.fsdcount == 0)
    	common.failure("Duplicate fsd name: " + fsd.name);
}
```

如果值为`default`，则设定的参数，后续的FSD通用

```shell
[root@node30133 vdbench50407]# cat testfilesystem
messagescan=no

fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd1,anchor=/mnt/nfs1/dir001
fsd=fsd2,anchor=/mnt/nfs1/dir002

fwd=fwd1,fsd=fsd*,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1
```

#### ※ anchor=

待测试目录

不能是另一个`fsd`里`anchor=`参数的父目录或子目录

```shell
[root@node30133 vdbench50407]# cat testfilesystem 
messagescan=no
cr=yes

fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd1,anchor=/mnt/nfs1
fsd=fsd2,anchor=/mnt/nfs1/testdir

fwd=fwd1,fsd=fsd*,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

# 第6、7行
# fsd1的anchor包含fsd2的anchor，会报错，详见下面的源代码
```

```java
private static void checkParameters()
{
    ...
    for (int j = i+1; j < fsd_list.size(); j++)
    {
        ...
        else
        {
            if ((fsd.dirname + File.separator).startsWith(fsd2.dirname  + File.separator) ||
            (fsd2.dirname + File.separator).startsWith(fsd.dirname + File.separator))
            {
                common.ptod("");
                common.ptod("fsd=" + fsd.name + ",dir=" + fsd.dirname);
                common.ptod("fsd=" + fsd2.name + ",dir=" + fsd2.dirname);
                common.failure("Directory (anchor) names used in fsd parameters may not be parents " +
                "or children of each other: ");
            }
        }
    }
}
```

<font color="#FF00000">父目录/子目录校验小Bug</font> --- 以文件分隔符结尾时，校验不出来 --- <font color="#FF00000">**建议：anchor的末尾不要添加文件分隔符**</font>

```shell
fsd=fsd1,anchor=/mnt/nfs1/
fsd=fsd2,anchor=/mnt/nfs1/testdir/
# 第1、2行的anchor的某个父目录以文件分隔符结尾，这个时候校验不出来父子目录

# 按照上面的代码逻辑，尝试推理如下：
# 1. fsd1的dirname+File.separateor = /mnt/nfs1//
# 2. fsd2的dirname+File.separateor = /mnt/nfs1/testdir//
# 3. 校验，这肯定不是startwith关系，因为多一个文件分隔符
```

如果不同`fsd`的`anchor`相同，那么目录结构（`depth=`和`width=`等参数）必须相同



如果父目录不存在，需要指定`create_anchors=yes`参数

```shell
[root@node30133 vdbench50407]# cat testfilesystem 
messagescan=no
create_anchors=yes

fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd1,anchor=/mnt/nfs1/testdir/dir001/
fsd=fsd2,anchor=/mnt/nfs1/testdir/dir002/

fwd=fwd1,fsd=fsd*,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

# 第3行，可以缩写为cr=yes，因为缩写最少需要2个字符，见下面源代码
```

```shell
# MiscParms.java
else if ("create_anchors".startsWith(prm.keyword))
	create_anchors = prm.alphas[0].toLowerCase().startsWith("y");

# 第2行，只要由prm.keyword开头即可，字少2个字符
```

#### count=(nn,mm)

快速创建一系列FSDs --- 序列

```shell
[root@node30133 vdbench50407]# cat testfilesystem 
messagescan=no
cr=yes

fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd,anchor=/mnt/nfs1/testdir,count=(1,5)

fwd=fwd1,fsd=fsd*,operation=read,xfersize=4k,fileio=sequential,fileselect=random,threads=2
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1

# 第6行
# fsd的名字依次是：fsd1、fsd2、fsd3、fsd4、fsd5 --- fsd的值+number
# anchor的值依次是：/mnt/nfs1/testdir1...testdir5
```

#### ※ shared=

默认值为`no`

多机运行时，共享同一个FSD，需要指定`shared=yes`，当设置为`yes`时，Vdbench不会维护控制文件（`vdb_control.file`）

一般用于下面这种情况：

```shell
[root@node1 vdbench50407]# cat testfile 
create_anchors=yes
messagescan=no

hd=default,shell=ssh,user=root
hd=one,system=node1
hd=two,system=node2

fsd=default,depth=2,width=2,files=1000,size=8k,shared=yes
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=fsd*,operation=read,threads=8
rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=restart

# 一个fsd，多个hd（2个）
# 2个hd会平均所有文件 --- 一共2^2*1000=4000，每台hd分2000个
```

<font color="#FF00000">经实测，当使用`shared=yes`时，不允许指定`format=yes`</font>

```shell
[root@node1 vdbench50407]# cat testfile 
create_anchors=yes
messagescan=no

hd=default,shell=ssh,user=root
hd=one,system=node1
hd=two,system=node2

fsd=default,depth=2,width=2,files=1000,size=8k,shared=yes
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=fsd*,operation=read,threads=8

rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=yes


# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile -o res+ 
...
11:43:35.024 *
11:43:35.024 *******************************************************************************
11:43:35.024 * fsd=fsd1 is requesting shared threads.                                      *
11:43:35.024 * This requires you to split your format into two steps:                      *
11:43:35.024 * rd=step1,.....,format=(clean,only)   This removes the old file structure    *
11:43:35.024 * rd=step2,.....,format=(restart,only) This recreates the new file structure  *
11:43:35.024 *                                                                             *
11:43:35.024 * This is necessary because otherwise hostA, still in the 'clean' phase of    *
11:43:35.024 * the format, could delete files or directories that hostB has just created.  *
11:43:35.024 *                                                                             *
11:43:35.024 *******************************************************************************
11:43:35.024 *
11:43:35.531 
11:43:35.531 'format=yes' not allowed for shared FSDs used in rd=rd1
11:43:35.531 
java.lang.RuntimeException: 'format=yes' not allowed for shared FSDs used in rd=rd1
	at Vdb.common.failure(common.java:350)
	at Vdb.common.failure(common.java:297)
	at Vdb.RD_entry.createFormatList(RD_entry.java:1501)
	at Vdb.RD_entry.insertFwdFormatIfNeeded(RD_entry.java:1405)
	at Vdb.RD_entry.buildNewRdListForFwd(RD_entry.java:1583)
	at Vdb.Vdbmain.masterRun(Vdbmain.java:771)
	at Vdb.Vdbmain.main(Vdbmain.java:628)
```

查看源代码如下

```java
// RD_entry.java
if (fwg.shared)
{
    if (format.format_requested && !format.format_clean && !format.format_restart)
    {
        BoxPrint box = new BoxPrint();
        box.add("fsd=%s is requesting shared threads.", fwg.fsd_name);
        box.add("This requires you to split your format into two steps:");
        box.add("rd=step1,.....,format=(clean,only)   This removes the old file structure");
        box.add("rd=step2,.....,format=(restart,only) This recreates the new file structure");
        box.add("");
        box.add("This is necessary because otherwise hostA, still in the 'clean' phase of");
        box.add("the format, could delete files or directories that hostB has just created. ");
        box.add("");
        box.print();
        common.failure("'format=yes' not allowed for shared FSDs used in rd=%s", rd_name);
    }
}
```

如果必须如此操作，根据错误提示进行修改即可

```shell
[root@node1 vdbench50407]# cat testfile 
create_anchors=yes
messagescan=no

hd=default,shell=ssh,user=root
hd=one,system=node1
hd=two,system=node2

fsd=default,depth=2,width=2,files=1000,size=8k,shared=yes
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=fsd*,operation=read,threads=8

rd=default,fwd=fwd*,fwdrate=100,elapsed=5,interval=1
rd=step1,format=(clean,only)
rd=step2,format=(restart,only)

# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile -o res+ 
...
11:47:53.793 Anchor size: anchor=/mnt/shareDir/standard-nfs001/30122dir1: dirs: 6; files: 4,000; bytes: 31.250m (32,768,000)
...
11:47:56.002 Starting RD=format_for_step1
...
11:47:57.216 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 1 directories; 1/sec
11:47:57.217 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 2 directories; 2/sec
11:47:57.773 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 3 directories; 2/sec
11:47:57.783 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 4 directories; 3/sec
11:47:57.791 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 5 directories; 3/sec
11:47:57.792 two-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 deleted 1 directories; 0/sec
...
11:47:58.187 Miscellaneous statistics:
11:47:58.187 (These statistics do not include activity between the last reported interval and shutdown.)
11:47:58.188 FILE_DELETES        Files deleted:                                2,000      1,000/sec
11:47:58.188 DIRECTORY_DELETES   Directories deleted:                              6          3/sec
11:47:58.188 FILE_DELETE_SHARED  Shared file delete failed:                    1,850        925/sec
11:47:58.188 DIR_DELETE_SHARED   Shared directory delete failed:                   6          3/sec
...
11:47:59.001 Starting RD=format_for_step2
11:47:59.336 two-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 mkdir complete.
11:47:59.424 one-0: anchor=/mnt/shareDir/standard-nfs001/30122dir1 mkdir complete.
...
11:48:20.146 Miscellaneous statistics:
11:48:20.147 (These statistics do not include activity between the last reported interval and shutdown.)
11:48:20.147 FILE_CREATES        Files created:                                4,000        190/sec
11:48:20.147 DIRECTORY_CREATES   Directories created:                              6          0/sec
11:48:20.147 WRITE_OPENS         Files opened for write activity:              4,000        190/sec
11:48:20.147 DIR_BUSY_MKDIR      Directory busy (mkdir):                           6          0/sec
11:48:20.147 DIR_EXISTS          Directory may not exist (yet):                    8          0/sec
11:48:20.147 FILE_CLOSES         Close requests:                               4,000        190/sec
11:48:20.147 DIR_CREATE_SHARED   Shared directory already exists:                  6          0/sec
```



注意：Vdbench官方手册中的这段话感觉不适用了

> 当指定`format=yes`时，会删除已存在的文件结构。每个从机都会尝试删除所有文件，如果删除失败（其他`hd`删除了此文件），不会生成错误消息。但这些失败的删除操作会有统计（`Miscellaneous statistics`下的`FILE_DELETE_SHARED`）。这也会导致`FILE_DELETES`计数器的值＞实际存在的文件数。



如果每次运行都要删除已有的文件，可以使用`startcmd="rm -rf /file/anchor"`来进行删除。<font color="#FF00000">但是，请注意Vdbench只会删除其生成的文件，而`rm -rf /root`会删除目录下的所有文件。</font>

#### ※ width=

子目录个数

#### ※ depth=

目录深度层级

#### ※ files=

##### 与`width=`和`depth=`参数结合

###### 每个anchor的files必须大于0

```java
// FsdEntry.java
if (fsd.files <= 0)
    common.failure("'files=' parameter must be greater than zero");
```

###### 每个anchor的files必须小于1000w+

```java
// FsdEntry.java

/* Too many problems, like 'ls' failing with 'Not enough space', */
/* but also problems in java LISTING those directories: */
if (fsd.files > 10000000 && fsd.files % 1000000 != 777)
{
    common.failure("'anchor=%s,files=%d' File count is limited to 10 million "+
                   "in a single directory.", fsd.dirname, fsd.files);
}
```

###### 每个anchor总文件数有限制

`total-files = width ^ depth * files`

必须小于MAX（32位和64位详见源代码）

- 32位：`32*1024*1024=33,554,432`
- 64位：`128*1024*1024=134,217,728`

```java
// FileAnchor.java

/* Determine how many files we'll allow per FSD: */
long MAX32 =  32l * 1024l * 1024l;
long MAX64 = 128l * 1024l * 1024l;
long MAX   = MAX32;
...
if (maximum_file_count > MAX)
{
	...
    txt.add(String.format("There will be %,12d directories and %,12d files under this anchor.",
                          total_directories, maximum_file_count));
    txt.add("Vdbench code currently supports only a maximum of " + MAX + " files.");
    txt.add("To work around this you can specify multiple smaller FSDs spread");
    txt.add("out over multiple JVMs.");

    if (!common.running64Bit())
        txt.add("Vdbench code for 64bit java supports " + MAX64 + " files.");
	...
    common.failure("Too many files");
}
```

##### 其他（待研究）

每个`fwd=xxx,threads=`参数至少需要一个文件，如果文件数量不够，会尝试最多10,000次查找可用文件，然后停止Vdbench运行。

```shell
# 这个不太好测出来
# 下面这个可以测出来，后面源代码逻辑理顺后，再分析
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=2g
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=*,operation=read,fileio=seq,fileselect=random,threads=32,xfersize=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=60,interval=1,format=yes

# 测试机器CPU为12核 3204铜牌，内存64G

# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile2 -o res+ threads=32 sizes=2g
...
14:12:31.001 Starting RD=rd1; elapsed=60; fwdrate=max. For loops: None
...
14:12:42.904 
14:12:42.904 Message from slave localhost-0: 
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0000.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0001.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0002.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0003.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0004.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0005.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0006.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0007.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0008.file,busy=true
14:12:42.904 file=/mnt/shareDir/standard-nfs001/30122dir1/vdb.1_1.dir/vdb_f0009.file,busy=true
14:12:42.904 Thread: FwgThread read /mnt/shareDir/standard-nfs001/30122dir1 rd=rd1 For loops: None
14:12:42.904 
14:12:42.904 last_ok_request: Thu Apr 18 14:12:30 CST 2024
14:12:42.905 Duration: 12.40 seconds
14:12:42.905 consecutive_blocks: 10001
14:12:42.905 last_block:         FILE_BUSY           File busy
14:12:42.905 operation:          read
14:12:42.905 
14:12:42.905 Do you maybe have more threads running than that you have 
14:12:42.905 files and therefore some threads ultimately give up after 10000 tries?
14:12:42.930 *
14:12:42.930 ******************************************************
14:12:42.930 * Slave localhost-0 aborting: Too many thread blocks *
14:12:42.930 ******************************************************
14:12:42.930 *
14:12:45.047 
14:12:45.047 Slave localhost-0 prematurely terminated. 
14:12:45.047 
14:12:45.047 Slave aborted. Abort message received: 
14:12:45.047 Too many thread blocks
14:12:45.047 
14:12:45.047 Look at file localhost-0.stdout.html for more information.
14:12:45.552 
14:12:45.552 Slave localhost-0 prematurely terminated. 
14:12:45.552 
java.lang.RuntimeException: Slave localhost-0 prematurely terminated. 
	at Vdb.common.failure(common.java:350)
	at Vdb.SlaveStarter.startSlave(SlaveStarter.java:188)
	at Vdb.SlaveStarter.run(SlaveStarter.java:47)
```

Vdbench会跟踪每个目录或文件的大量信息，这需要大约每个目录/文件100字节的Java堆空间。建议针对每个目录/文件，规划大约200字节的Java堆空间，以便快速选择和切换不同的文件，并避免可能的垃圾回收问题。请参阅本文档中有关“GcTracker”信息的部分。

```shell
# 这个源码中暂时没找到相关代码
```

#### ※ sizes=

文件大小

##### 单个值

文件都是统一大小

```shell
[root@node1 vdbench50407]# cat testfile1 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=64k
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=8,xfersize=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes
```

##### 2个值组合（第二个是0）

指定`sizes=(nnn,0)`时，将创建平均大小为`nnn`字节的文件。

文件大小规则：

- ＞10m，则大小将是1m的倍数。
- ＞1m，则大小将是100k的倍数。
- ＞100k，则大小将是10k的倍数。
- ＜100k，则大小将是1k的倍数。

```shell
[root@node1 vdbench50407]# cat testfile1 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=(101k,0)
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=8,xfersize=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes
```

```java
// FileAnchor.java -> #getFileSize() -> #calculateVarSize()

/**
   * Get a file size from our distribution table.
   */
private long getFileSize()
{
    if (filesizes.length == 1)
        return(long) filesizes[0];

    if (filesizes.length == 2 && filesizes[1] == 0)
        return calculateVarSize();

    int pct = file_size_randomizer.nextInt(100);

    int cumpct = 0;
    int i;

    for (i = 0; i < filesizes.length; i+=2)
    {
        cumpct += filesizes[i+1];
        if (pct < cumpct)
            break;
    }

    long size = (long) filesizes[i];

    return size;
}

// 这个计算不同大小的文件
private long calculateVarSize()
{
    int mb10  =  10 * 1024 * 1024;
    int mb1   =   1 * 1024 * 1024;
    int kb100 = 100 * 1024;
    int kb10  =  10 * 1024;
    int kb1   =   1 * 1024;

    int wanted = (int) filesizes[0];
    
    int half   = wanted / 2;
    int rand   = file_size_randomizer.nextInt(wanted);
    float result = rand + half;


    if (result >= mb10)
        wanted = Math.round(result / mb1) * mb1;
    else if (wanted >= mb1)
        wanted = Math.round(result / kb100) * kb100;
    else if (wanted >= kb100)
        wanted = Math.round(result / kb10) * kb10;
    else
        wanted = Math.round(result / kb1) * kb1;

    return wanted;
}
```

##### 多值组合

按占比走

```shell
[root@node1 vdbench50407]# cat testfile1 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=(32k,50,64k,50)
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=8,xfersize=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes

# 第5行，(32k,50,64k,50) --- 32k和64k各占50%，也就是都是5个
```

#### ※ openflags=

Vdbench手册里表示，目前支持的参数有：`O_DSYNC, O_RSYNC, O_SYNC`

但是使用其他的也可以，详见SD的`openflags`章节的代码逻辑

> 注：后续了解深入了，再补充

#### ※ totalsize=

参数`depth=、width=、files=、sizes=`共同控制生成<font color="#FF00000">文件的总大小 --- 总文件数 * 文件大小</font>

<font color="#FF00000">生成文件总大小到`totalsize`后停止创建新文件</font>

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=100m,totalsize=500m
fsd=fsd,anchor=/mnt/shareDir/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=16,xfersize=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes

# 第5行，sizes=100m，totalsize=500m
# 一共生成10个文件，总大小1000m
# 由于totalsize=500m的限制，会生成5个100m的文件

# 结果：
[root@node1 vdbench50407]# ./vdbench -f testfile2 -o res+
...
14:44:03.382 Anchor size: anchor=/mnt/shareDir/30122dir1: dirs: 1; files: 10; bytes: 1000.000m (1,048,576,000)
...
14:44:04.281 localhost-0: Created totalsize= 500.0m using 5 of 10 files for anchor=/mnt/shareDir/30122dir1
...
14:44:06.440 localhost-0: Created totalsize= 500.0m using 5 of 10 files for anchor=/mnt/shareDir/30122dir1
14:44:07.001 Starting RD=rd1; elapsed=20; fwdrate=max. For loops: None
...
```

##### 测试结论

###### totalsize > 总文件大小 && != Long.MAX_VALUE

<font color="#FF00000">报错</font>

`Long.MAX_VALUE` 值是 `9223372036854775807` ≈ **8ZiB**

```java
// FileAnchor.java
if (total_size != Long.MAX_VALUE && total_size > bytes_in_file_list)
{
    common.ptod("rd=" + RD_entry.next_rd.rd_name);
    common.failure("fwd=" + fwg.getName() + ",fsd=" + fwg.fsd_name +
                   ": The requested totalsize=" + whatSize(total_size) +
                   " is greater than the estimated total anchor size of " +
                   whatSize(bytes_in_file_list));
}
```

###### 1byte <= totalsize <= 总文件大小

按`totalsize`的大小正常限制

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=100m,totalsize=450m
fsd=fsd,anchor=/mnt/shareDir/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=16,xfersize=1m
rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes

# 第5行，sizes=100m，totalsize=450m
# 一共生成10个文件，总大小1000m
# 由于totalsize=450m的限制，生成5个100m的文件后，新文件不再生成
```

###### 0byte <= totalsize < 1byte

<font color="#FF00000">totalsize取值[0,1)，此参数不生效</font>

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=100m,totalsize=0
fsd=fsd,anchor=/mnt/shareDir/30122dir1

fwd=fwd1,fsd=*,operation=write,fileio=seq,fileselect=random,threads=16,xfersize=1m
rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=yes

# 第5行，totalsize取值[0,1)，不生效
# 还是会生成10个文件，总大小1000m
```

###### totalsize < 0byte

<font color="#FF00000">报错</font>

```java
// FsdEntry.java
else if ("total_size".startsWith(prm.keyword) || "totalsize".startsWith(prm.keyword))
{
    fsd.total_size = (long) prm.numerics[0];
    if (prm.num_count > 1)
        fsd.total_size /= (long) prm.numerics[1];
    if (fsd.total_size < 0)
        common.failure("A percentage value as a 'totalsize=' parameter is NOT " +
                       "allowed for an FSD; only for an RD.");
}
```

`format=limited`参数（RD）--- 详见针对文件系统的RD章节

`fortotal=`参数（RD）--- 详见针对文件系统的RD章节

#### distribution=

当`distribution=bottom`时（<font color="#FF00000">默认值</font>），指的是在<font color="#FF00000">最深层目录</font>创建的文件个数

```shell
[root@node1 vdbench50407]# cat testfile1 
create_anchors=yes
messagescan=no

fsd=default,depth=4,width=4,files=100,size=1,distribution=bottom
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=fsd*,operation=read,threads=16

rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=yes

# 第5行的 distribution=bottom 可以省略
```

当`distribution=all`时，指的是在<font color="#FF00000">所有目录</font>创建的文件总个数

```shell
[root@node1 vdbench50407]# cat testfile1 
create_anchors=yes
messagescan=no

fsd=default,depth=4,width=4,files=100,size=1,distribution=all
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir1

fwd=fwd1,fsd=fsd*,operation=read,threads=16

rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=yes
```

#### workingsetsize=nn or wss=nn

暂未测试

```java
// FileAnchor.java

if (working_set > bytes_in_file_list)
    common.failure("fwd=" + fwg.getName() + ",fsd=" + fwg.fsd_name +
                   ": The requested workingset=" + whatSize(working_set) +
                   " is greater than the estimated total anchor size of " +
                   whatSize(bytes_in_file_list));

if (fwg.total_size > 0 && fwg.working_set > fwg.total_size)
{
    common.ptod("fwg.total_size:  " + whatSize(fwg.total_size));
    common.ptod("fwg.working_set: " + whatSize(fwg.working_set));
    common.failure("fwd=" + fwg.getName() + ",fsd=" + fwg.fsd_name +
                   ": The requested workingset=" + whatSize(fwg.working_set) +
                   " is greater than the estimated totalsize=" +
                   whatSize(fwg.total_size));
}
```

#### journal=dir

暂未测试

Where to store your Data Validation journal files.

#### mask=(vdb_f%04d.file, vdb.%d_%d.dir)

暂未测试

The default `printf()` mask used to generate file and directory names. This allows you to create your own names, though they still need to start with `vdb` and end with `.file` or `.dir` .ALL files are numbered consecutively starting with `zero`. The first `%` mask is for directory depth, the second for directory width.

### 文件系统工作负载定义（FWD）参数

#### ※ fwd=name

唯一

值为`default`时，设定的参数值为后续的FWD通用

#### ※ fsd=

哪个或哪些FSDs用于此FWD

```shell
# 单个
fsd=fsd1
# 多个
fsd=(fsd1,fsd5)
# 多个，连续
fsd=(fsd1-fsd10)
```

#### ※ fileio=

文件I/O类型

| 参数                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| fileio=random          | Random I/O, one thread per file only.                        |
| fileio=(random,shared) | Random I/O, but allow multiple threads to share the same file. |
| fileio=sequential      | Sequential I/O (<font color="#FF00000">default</font>).      |
| fileio=sequentialnz    | Sequential i/o, but starting at a random block in the file.  |
| fileio=(seq,delete)    | Delete the file before writing.                              |

源代码逻辑如下：

```java
// FwdEntry.java
private void fileIoParameters(Vdb_scan prm)
{
    if ("random".startsWith(prm.alphas[0]))
    {
        sequential_io = false;
        if (prm.getAlphaCount() > 1)
        {
            if ("shared".startsWith(prm.alphas[1]))
            {
                if (Validate.isRealValidate())
                    common.failure("'fileio=(random,shared)' may not be used with Data Validation");
                file_sharing = true;
            }
            else
                common.failure("Invalid 'fileio' parameter contents: " + prm.alphas[1]);
        }
    }

    else if (prm.alphas[0].startsWith("seq"))
    {
        sequential_io = true;
        if (prm.alphas[0].contains("nz"))
            seq_io_start0 = false;
        if (prm.getAlphaCount() > 1)
        {
            if ("delete".startsWith(prm.alphas[1]))
                del_b4_write = true;
            else
                common.failure("Invalid 'fileio' parameter contents: " + prm.alphas[1]);
        }
    }
    else
        common.failure("Invalid 'fileio' parameter contents: " + prm.alphas[0]);
}
```

##### fileio=random

##### fileio=(random,shared)

##### fileio=sequential

源代码

```java
// OpWrite.java
protected boolean doOperation()
{
	...
    if (fwg.sequential_io)
    {
        boolean rc = doSequentialWrite(false);
        return rc;
    }
    else
    {
        boolean rc = doRandomWrite();
        return rc;
    }
}
```

##### fileio=sequentialnz

##### fileio=(seq,delete)



除了顺序和随机I/O，暂时不知道其他细节，待分析源码后，进行补充，可以出个专题



#### rdpct=

一般使用 `operation=read` 只允许读取，`operation=write` 只允许写入

如果指定了 `rdpct=`，可以在同一个选中文件中进行混合读写

<font color="#FF00000">一般用于随机I/O</font>，因为顺序 I/O，可能会得到 `读取块1、写入块2、读取块3......` 结果

```shell

```

#### stopafter=



#### ※ fileselect=



```java
// FwdEntry.java
/**
   * fileselect=
   *
   * - random
   * - sequential
   * - once
   * - xxxxx
   * - average=nn
   */
private void parseFileSelect(ArrayList <String> parms)
{
    for (String parm : parms)
    {
        if ("random".startsWith(parm))
            select_random = true;

        else if (parm.startsWith("seq"))
        {
            select_random = false;
            if (parm.contains("nz"))
                selseq_start0 = false;
        }

        else if (parm.equals("once"))
            select_once = true;

        else if (parm.equals("full"))
            select_full = true;

        else if (parm.equals("empty"))
            select_empty = true;

        else if (parm.equals("notfull"))
            select_nfull = true;

        else if (parm.equals("skewed"))  // renamed to 'poisson' after first tests
        {
            select_random = true;
            poisson_skew = 3;
        }

        else if (parm.equals("poisson"))
        {
            select_random = true;
            poisson_skew = 3;
        }

        else if (parm.startsWith("midpoint=")) // also for compatibility
        {
            String[] split = parm.split("=");
            if (split.length != 2)
                common.failure("Invalid 'fileselect' parameter contents: " + parm);
            if (!common.isNumeric(split[1]))
                common.failure("Invalid 'fileselect' parameter contents: " + parm);

            select_random = true;
            poisson_skew = Double.parseDouble(split[1]);
            if (poisson_skew <= 0)
                common.failure("Invalid 'fileselect' parameter contents: " + parm);
        }

        else if (common.isDouble(parm))
            poisson_skew = Double.parseDouble(parm);

        else
            common.failure("Invalid 'fileselect' parameter contents: " + parm);
    }

    if (select_once && !selseq_start0)
        common.failure("'fileselect=(seqnz,once)' may not be used together.");
}
```



#### ※ xfersizes=

默认值：4k

```java
// FwdEntry.java
public  double[]  xfersizes     = new double[] { 4096 };
```

可以单个值，也可以组合值

```shell
xfersizes=8192

xfersizes=(32k,50,64k,50) # 百分比总和必须要 = 100
```

```java
// FwdEntry.java
else if ("xfersizes".startsWith(prm.keyword))
{
    fwd.xfersizes = prm.numerics;

    double cumpct = 0;
    for (int i = 0; i < prm.numerics.length; i++)
    {
        if (i % 2 == 1)
            cumpct += prm.numerics[i];
    }
    if (prm.numerics.length > 1 && (int) cumpct != 100)
        common.failure("Xfersize distribution does not add up to 100");
}
```

<font color="#FF00000">注意：源代码没做过多的限制，大家使用过程中，尽量选用跟场景符合的`xfersizes`</font>



#### ※ operation=

可选项：`mkdir, rmdir, create, delete, open, close, read, write, access, getattr, setattr`

多个操作组合的话，参考RD的`operations=`参数

对同一文件执行混合读写操作，请指定`fwd=xxx,rdpct=nn`

注意`FWD`只接收`operation=`参数，且参数只接收一个参数，看源码

```java
else if ("operation".startsWith(prm.keyword))
{
    if (prm.getAlphaCount() > 1)
        common.failure("'fwd=" + fwd.fwd_name + ",operations=' accepts only ONE parameter.");
    fwd.operation = Operations.getOperationIdentifier(prm.alphas[0]);
    if (fwd.operation == -1)
        common.failure("Unknown operation: " + prm.alphas[0]);
}

// 第4行，感觉提示应该是 operation=，不应该加s，给人错觉。
```

#### skew=

该Fwd的总工作量（RD的`fwdrate=`参数）占比

默认值：所有FWDs均匀分配

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,10)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=8k
fwd=fwd1,fsd=(fsd1-fsd3),operation=write
fwd=fwd2,fsd=(fsd4-fsd6),operation=write
fwd=fwd3,fsd=(fsd7-fsd10),operation=read

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=10,interval=1,format=restart

# 基本均分，不是严格均分
```

多个`skew=`参数值加起来要等于`100`

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=8k
fwd=fwd1,fsd=fsd1,operation=write,skew=10
fwd=fwd2,fsd=fsd2,operation=open,skew=20
fwd=fwd3,fsd=fsd3,operation=read,skew=70

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=10,interval=1,format=restart

# 第9~11行
# 按占比分配 --- fwd1：10%、fwd2：20%、fwd3：70%
```

多机+指定`host=`参数时

<font color="#FF00000">注意同一个`host`的`skew`加起来要等于`100`，比如下面的`host=hd1`的`fwd1`和`fwd2`</font>

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

hd=hd1,shell=ssh,system=node1,user=root
hd=hd2,shell=ssh,system=node2,user=root

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,10)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=8k
fwd=fwd1,fsd=(fsd1-fsd3),operation=write,skew=40,host=hd1
fwd=fwd2,fsd=(fsd4-fsd6),operation=write,skew=60,host=hd1
fwd=fwd3,fsd=(fsd7-fsd10),operation=read,skew=100,host=hd2

rd=rd1,fwd=(fwd1,fwd2),fwdrate=max,elapsed=10,interval=1,format=restart
rd=rd2,fwd=fwd3,fwdrate=max,elapsed=10,interval=1,format=restart

# 第12、13行，fwd1和fwd2的工作量加到一起=100，指定这两个fwd的host时，需要在同一hd上
```

#### ※ threads=

该`FWD`的并发线程数，如果该`FWD`包含多个`FSDs`，那么这个线程数是所有`FSDs`共享的。

代表该`FWD`同时并发`nn`个`文件或目录`操作（按理解，是每个文件或目录一个线程），而不是每个文件或目录并发`nn`个线程。除非使用`fileio=(random,shared)`参数，否则对`目录或文件`的操作都是**单线程**的。

Vdbench将总线程数分配给所有FSDs时，每个FSD分到的线程数要`≥1`，如果不够1个，会自动向上进行调整至1个。举例：`threads=1`，FSDs总数是`2`，推断需要最少`2`个线程，Vdbench会自动调整`threads=2`并在结果输出中进行提示（`Note`）。

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,10)

fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=5,xfersizes=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=restart

# 第6行 --- 10个fsd，每个fsd里100个files
# 第8行 --- threads=5，总数为5，比总fsds小，咱没设置fwd_thread_adjust参数，所以会自动调整，详见结果中的Note（第27~33行）
# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile2 -o res+
...
10:20:42.223 input argument scanned: '-ftestfile2'
10:20:42.223 input argument scanned: '-ores+'
10:20:42.223 input argument scanned: 'sizes=5m'
10:20:42.231 input argument scanned: 'threads=5'
10:20:42.231 input argument scanned: 'xsize=1m'
10:20:42.293 
10:20:42.294 Note: fwd=fwd1,threads=5,...
10:20:42.294 Note: rd=rd1,fwd=fwd1,... 
10:20:42.294 Note: Mismatch between threads requested (5) and used (10)
10:20:42.294 
10:20:42.294 Note: Requesting threads across multiple fsds, operations, slaves or hosts 
10:20:42.295       may result in fractions of threads. 
10:20:42.295       There will be a minimum of one thread for each fsd, operation, 
10:20:42.295       slave or host, or integer truncation of the resulting thread count.
10:20:42.295       Contact the author of Vdbench if there are unexplained differences.
10:20:42.295 
10:20:42.295       Use general parameter 'fwd_thread_adjust=no' to cause Vdbench to abort instead. 
10:20:42.295 
...
```

不想自动调整，可以在General参数中添加`fwd_thread_adjust=no`，Vdbench将输出警告后终止本次测试。

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no
fwd_thread_adjust=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,10)

fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=5,xfersizes=1m

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=restart

# 第4行 --- 添加了参数为no
# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile2 -o res+ sizes=5m threads=5 xsize=1m
...
15:16:52.618 input argument scanned: '-ftestfile2'
15:16:52.618 input argument scanned: '-ores+'
15:16:52.618 input argument scanned: 'sizes=5m'
15:16:52.625 input argument scanned: 'threads=5'
15:16:52.626 input argument scanned: 'xsize=1m'
15:16:52.688 
15:16:52.689 Note: fwd=fwd1,threads=5,...
15:16:52.689 Note: rd=rd1,fwd=fwd1,... 
15:16:52.689 Note: Mismatch between threads requested (5) and used (10)
15:16:52.689 
15:16:52.689 Note: Requesting threads across multiple fsds, operations, slaves or hosts 
15:16:52.689       may result in fractions of threads. 
15:16:52.689       There will be a minimum of one thread for each fsd, operation, 
15:16:52.689       slave or host, or integer truncation of the resulting thread count.
15:16:52.689       Contact the author of Vdbench if there are unexplained differences.
15:16:52.689 
15:16:52.690       Use general parameter 'fwd_thread_adjust=no' to cause Vdbench to abort instead. 
15:16:52.690 
15:16:53.196 
15:16:53.196 User requested 'fwd_thread_adjust=no'
15:16:53.196 
java.lang.RuntimeException: User requested 'fwd_thread_adjust=no'
	at Vdb.common.failure(common.java:350)
	at Vdb.RD_entry.checkThreadCount(RD_entry.java:1298)
	at Vdb.RD_entry.createFwgListForOneRd(RD_entry.java:1245)
	at Vdb.RD_entry.buildNewRdListForFwd(RD_entry.java:1578)
	at Vdb.Vdbmain.masterRun(Vdbmain.java:771)
	at Vdb.Vdbmain.main(Vdbmain.java:628)
```

两点思考：

- 如果默认设置，那么计算`threads`时，就要结合告警综合考虑
- 如果设置为`no`，那么测试时，就要提前计算出`threads`

#### host=host_label

执行该FWD的主机，一般多机运行时，可以指定。

示例为了简单，用了一个localhost。

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,10)

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,host=localhost

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=20,interval=1,format=restart

# 第9行 --- host=localhost
```

### 运行定义（RD）参数

#### fwd=

FWD标识符，唯一

```shell
# 单一
fwd=fwd1
# 多个组合
fwd=(fwd1,fwd2,fwd3)
# 多个顺序组合
fwd=(fwd1-fwd3)
# 通配符
fwd=fwd*
```

#### fwdrate=

文件系统各种操作的执行速率

文件系统的读写操作可以视为`I/O`操作，其他的操作（`mkdir, rmdir, create, delete, open, close, access, getattr`,  `setattr`），Vdbench将它们元数据操作。

##### fwdrate=100

平均每秒100次Ops

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,rdpct=50

rd=rd1,fwd=fwd*,fwdrate=1000,elapsed=20,interval=1,format=restart

# 第12行，会将速度限制在1000左右 --- 结果列（ReqstdOps-rate）
# 如果是读写，那读写速率（read-rate，write-rate）加起来是1000左右
```

##### fwdrate=(100,200,…)

100、200...依次执行

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,rdpct=50

rd=rd1,fwd=fwd*,fwdrate=(100,200,300,400),elapsed=20,interval=1,format=restart

# 第12行，依次以100,200,300,400的速率分别执行，rd一共执行4次，相当于写了4个脚本，只是fwdrate不一样而已
```

##### fwdrate=(100-1000,100)

100、100+100、100+100+100...

每次自增100，如果最后自增超过最大值，则不执行

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,rdpct=50

rd=rd1,fwd=fwd*,fwdrate=(100-400,200),elapsed=10,interval=1,format=restart

# 第12行，最开始100，第二次300，因为第三次超过400了，所以不执行
```

##### fwdrate=curve

先以max找出最大均值，然后分别以最大值的10%、50%、70%、80%、90%、100%的速率去依次执行

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,rdpct=50

rd=rd1,fwd=fwd*,fwdrate=(100-400,200),elapsed=10,interval=1,format=restart

# 第12行，先找到max时的最大平均值，然后以10%，50%，70%，80%，90%，100% 依次分别执行
# 举例：比如 max时，平均值=1200
# 10% --- 120
# 50% --- 600
# 70% --- 840
# 80% --- 950
# 90% --- 1100
# 100% --- 1200
```

##### fwdrate=max

使用默认的`skew`保持工作负载同步 --- 这句话暂时不知道什么意思

```shell
[root@node1 vdbench50407]# cat testfile2 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=format,xfersize=10m,threads=32
fwd=fwd1,fsd=*,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=$xsize,rdpct=50

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=10,interval=1,format=restart

# 直接以最大速率执行
```

#### format=

预埋数据（目录和文件结构）

<font color="#FF00000">`rd=default`里不允许使用`format=`参数</font>

默认情况下，预埋数据工作负载定义为`threads=8,xfersize=128k`，如需更改默认预填数据工作负载定义，可以定义FWD：`fwd=format,threads=nn,xfersize=nn`，当`fwd=format`时，除了`threads`和`xfersize`，其他参数均会被忽略

##### format=no

默认值，不进行数据预埋

<font color="#FF00000">注意：任何格式化行为（yes、only、dir、clean等等）都会删掉预埋数据</font>

<font color="#FF00000">注意：这时必须有已存在的数据（文件结构）且必须和FSD中的定义一致才可以开展测试，否则测试会停止</font>

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=8k
fwd=fwd1,fsd=fsd1,operation=write,skew=10
fwd=fwd2,fsd=fsd2,operation=open,skew=20
fwd=fwd3,fsd=fsd3,operation=read,skew=70

rd=rd1,fwd=fwd*,fwdrate=max,elapsed=10,interval=1,format=no

# 第13行，format=no
# 没有数据预埋或者已存在的文件结构和fsd中不一致，会导致测试停止
```

##### format=yes

先删掉旧的由Vdbench生成的文件（默认命名格式为`vdb_f%04d.file, vdb.%d_%d.dir`，详见`fsd`里的`mask=`参数），然后按照FSDs的定义创建新的文件结构，创建完毕后开展RD测试。

<font color="#FF00000">警告：</font>如果文件结构想重用，就不要使用`format=yes`参数

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=8k
fwd=fwd1,fsd=fsd1,operation=write,skew=10
fwd=fwd2,fsd=fsd2,operation=open,skew=90
fwd=fwd3,fsd=fsd3,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=(fwd1,fwd2),format=yes
rd=rd2,fwd=fwd3,format=yes

# 第14、15行
# 每个rd都可以单独指定format参数，只针对当前rd生效
```

##### format=restart

不进行文件删除，重用旧文件，只对没创建的文件或者没达到预期大小的文件进行操作（没创建的创建，不够大小的继续补充至规定值）

<font color="#FF00000">注意：经测试，如果手动删过原目录的文件，同时`fwd`的`threads`又相对较少的话，Vdbench不会补充全部的文件。</font>

使用下面的Scripts，手动删掉30122dir1里的部分文件，脚本执行完毕后，不会补充到100个。

<font color="#FF00000">所以使用format=restart时，不要手动修改anchor里的文件结构。</font>

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=restart
rd=rd2,fwd=fwd2,format=restart

```

Vdbench will create only files that have not been created and will also expand files that have not reached their proper size. (This is where totalsize and workingsetsize can come into play).

与totalsize=参数共用

```shell
# 只使用totalsize大小的文件（使用的文件个数自动调整）
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync,totalsize=300m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=restart
rd=rd2,fwd=fwd2,format=restart

# 第6行
# sizes=5m，files=100，totalsize=300m --- 只使用60个文件即可
```

与workingsetsize=参数共用

```shell
# 待测试
```

##### format=only

<font color="#FF00000">除了不执行`RD`，其他跟`format=yes`一样，输出日志中也不会有`Starting RD=rd1; ...`</font>

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=only
rd=rd2,fwd=fwd2,format=restart

# 第13行，format=only
# rd1只重新创建文件结构，不执行RD1
```

##### format=dir(ectories)

<font color="#FF00000">除了只创建目录，其他跟format=yes一样，也会执行RD</font>

由于只创建目录，没有文件，所以执行是失败的，输出日志中有`Messages：Vdbench is trying to write to a file, but no files are available`

输出日志中含有`Starting RD=rd1; ...`

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=directories
rd=rd2,fwd=fwd2,format=restart

# 第13行，format=dir
# 30122dir1和30122dir3里只创建目录结构，不创建文件
# RD1执行时，由于没有文件，会失败，输出日志里有Messages的记录
```

##### format=clean

删掉已有文件结构（会保留`no_dismount.txt`和`vdb_control.file`文件），不执行RD

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=clean
rd=rd2,fwd=fwd2,format=restart

# 第13行，format=clean
# 会删掉30122dir1和30122dir3里的文件结构（vdb.x_x.dir），会保留no_dismount.txt和vdb_control.file文件
```

##### format=once

This overrides the default behavior that a format is done for each forxxx parameter loop done.

由于需要结合forxxx参数使用，暂未测试

##### format=(only,limited)

<font color="#FF00000">只能使用`format=(only,limited)`</font>，否则会报错（`java.lang.RuntimeException: The use of 'format=limited' requires the use of 'format=(only,limited)'`）

```java
// ForamtFlags.java
if (format_limited && !format_only_requested)
    common.failure("The use of 'format=limited' requires the use of 'format=(only,limited)'");
```

因为有`only`，只生成文件结构，不执行`RD`

因为有`limited`，所以会在到达规定时间（`elapsed=`）后立即停止，不管是否达到`fsd`里规划的文件结构（当`fsd`里包含`totalsize=`参数也是一样的，以`elapsed=`参数为准）

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=50m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=(only,limited)
rd=rd2,fwd=fwd2,format=restart

# 第5、6行，要生成约5g的文件
# 第13行，limited，当执行10s后，format就结束了，不管是否达到5g
```

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=50m,openflags=o_sync,totalsize=2g
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1
rd=rd1,fwd=fwd1,format=(only,limited)
rd=rd2,fwd=fwd2,format=restart

# 第5、6行，要生成约5g的文件，但是totalsize为2g
# 第13行，limited，当执行10s后，format就结束了，不管是否达到totalsize
```

##### format=complete

这个参数会告诉Vdbench，`format`工作做完了，可以开启`RD`阶段了

设置了这个参数，Vdbench不会检查测试机上文件结构和文件大小是否符合`fsd`的定义

所以除非你非常确定，否则测试过程中执行目录或文件的创建或删除是非常危险的

<font color="#FF00000">个人感觉，使用这个不如使用`format=restart`</font>

只能与`format=no`一起使用，请看源码解析

```java
// ForamtFlags.java
// 这俩参数互斥
if (format_requested && format_complete)
    common.failure("'format=yes' and 'format=complete' are mutually exclusive.");

// 这块的逻辑除了format=no，其他都会将format_requested置为true
if ("directories".startsWith(parms[i]))
    format_dirs_requested = format_requested =true;
else if ("restart".equals(parms[i]))
    format_restart = format_requested = true;
else if ("only".equals(parms[i]))
    format_only_requested = format_requested = true;
else if ("once".equals(parms[i]))
    format_once_requested = format_requested = true;
else if ("clean".equals(parms[i]))
    format_only_requested = format_clean = format_requested = true;
else if ("limited".equals(parms[i]))
    format_limited = format_requested = true;
else if ("complete".equals(parms[i]))
    format_complete = format_requested = true;
else if ("yes".startsWith(parms[i]))
    format_requested = true;
else
    common.failure("Invalid contents of 'format=' parameter: " + parms[i]);

// 看代码到现在的程度，感觉第19、20行的代码逻辑应该改为false，原因如下：
// 因为format默认为no，complete又告诉Vdbench程序format已经完成，所以format_requested置为true没什么意义
// 以上仅代表个人观点
```

#### operations=

针对文件系统的组合操作（`mkdir, rmdir, create, delete, open, close, read, write, access, getattr and setattr`）

设置这个参数的RD<font color="#FF00000">同时运行组合操作</font>

```java
// Operations.java
private static String[] operations =
{
    "read",         //  0
    "write",        //  1
    "mkdir",        //  2
    "rmdir",        //  3
    "copy",         //  4
    "move",         //  5
    "create",       //  6
    "delete",       //  7
    "getattr",      //  8
    "setattr",      //  9
    "access",       // 10
    "open",         // 11
    "close",        // 12
    "put",          // 13
    "get"           // 14

    // we should add a 'backward sequential' file selection?
};
```

会覆盖`FWDs`里的`operation=`参数

```shell
# 可以指定一个
# operations=write
# 可以指定多个
# operations=(write,read,access)

[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1,operations=(access,getattr,setattr)
rd=rd1,fwd=fwd1,format=restart
rd=rd2,fwd=fwd2,format=restart

# 第12行，operations= 会覆盖第9、10行的operation=
# rd1会同时执行access,getattr,setattr三种操作
# rd2会同时执行access,getattr,setattr三种操作
```

没有已存在的符合FWDs的文件结构，又没设置`format=yes`等自动补全，下面几种情况需要注意：

- `operations=read`，Vdbench将失败（`Vdbench is trying to read from a file, but no files are available, and no threads are currently active creating new files.`），没有可用的文件

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1,operations=read
rd=rd1,fwd=fwd1,format=no
rd=rd2,fwd=fwd2,format=no

# 测试时，我将测试目录（30122dir1~30122dir3）完全删掉了
# 第13、14行，format=no & 第12行 operations=read，操作将失败

# 结果 --- 每个测试目录里只创建了vdb_control.file
[root@node30133 nfs1]# ls -alh 30122dir* -R
30122dir1:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file

30122dir2:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file

30122dir3:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file
```

- `operations=(create,read)`，Vdbench将执行一次<font color="#FF00000">无效的测试（结果值都是0）</font>，没有可用目录

```shell
[root@node1 vdbench50407]# cat testfile3 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=(fsd1,fsd3),operation=write
fwd=fwd2,fsd=fsd2,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1,operations=(create,read)
rd=rd1,fwd=fwd1,format=no
rd=rd2,fwd=fwd2,format=no

# 测试时，我将测试目录（30122dir1~30122dir3）完全删掉了
# 第13、14行，format=no & 第12行 operations=(create,read)，操作将失败

# 执行结果
10:36:05.001 Starting RD=rd2; elapsed=10; fwdrate=max. For loops: None                                                       
10:36:15.238 Miscellaneous statistics:
...
10:36:15.238 MISSING_PARENT      Parent directory does not exist (yet):       74,514      7,451/sec
# 第23行，有个报错

# 测试目录结果 --- 每个测试目录里只创建了vdb_control.file
[root@node30133 nfs1]# ls -alh 30122dir* -R
30122dir1:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file

30122dir2:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file

30122dir3:
total 512
drwxrwxrwx 1 root   root     1 Apr 28 10:24 .
drwxrwxrwx 1 nobody nobody  25 Apr 28 10:24 ..
-rwxrwxrwx 1 root   root   214 Apr 28 10:24 vdb_control.file
```

- `operations=(mkdir,create,read)`，Vdbench将执行一次<font color="#FF00000">无效的测试（结果值都是0）</font>，生成了文件存在，但是文件大小是0

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=5,sizes=5m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1,operation=write

rd=default,fwdrate=max,elapsed=10,interval=1,operations=(mkdir,create,read)
rd=rd1,fwd=fwd1,format=no

# 测试时，我将测试目录（30122dir）完全删掉了
# 第12行的format=no & 第11行 operations=(mkdir,create,read)，操作将失败

# 执行结果 --- FILE_NOT_FULL
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
10:45:19.258 Miscellaneous statistics:
10:45:19.258 (These statistics do not include activity between the last reported interval and shutdown.)
10:45:19.259 FILE_CREATES        Files created:                                    5          0/sec
10:45:19.259 DIRECTORY_CREATES   Directories created:                              1          0/sec
10:45:19.259 WRITE_OPENS         Files opened for write activity:                  5          0/sec
10:45:19.259 DIR_BUSY_MKDIR      Directory busy (mkdir):                           2          0/sec
10:45:19.260 DIR_EXISTS          Directory may not exist (yet):                    5          0/sec
10:45:19.260 FILE_BUSY           File busy:                                      134         13/sec
10:45:19.260 FILE_MAY_NOT_EXIST  File may not exist (yet):                         5          0/sec
10:45:19.260 FILE_NOT_FULL       File has not been completely written:        45,016      4,501/sec
10:45:19.260 FILE_CLOSES         Close requests:                                   5          0/sec

# 测试目录结果 --- 文件都生成了，但是大小都是0
[root@node30133 nfs1]# ls -alh 30122dir* -R
30122dir:
total 512
drwxrwxrwx 1 root   root     2 Apr 28 10:45 .
drwxrwxrwx 1 nobody nobody  23 Apr 28 10:45 ..
drwxrwxrwx 1 root   root     5 Apr 28 10:45 vdb.1_1.dir
-rwxrwxrwx 1 root   root   208 Apr 28 10:45 vdb_control.file

30122dir/vdb.1_1.dir:
total 0
drwxrwxrwx 1 root root 5 Apr 28 10:45 .
drwxrwxrwx 1 root root 2 Apr 28 10:45 ..
-rw-rw-rw- 1 root root 0 Apr 28 10:45 vdb_f0000.file
-rw-rw-rw- 1 root root 0 Apr 28 10:45 vdb_f0001.file
-rw-rw-rw- 1 root root 0 Apr 28 10:45 vdb_f0002.file
-rw-rw-rw- 1 root root 0 Apr 28 10:45 vdb_f0003.file
-rw-rw-rw- 1 root root 0 Apr 28 10:45 vdb_f0004.file
```

- `operations=(mkdir,create,write,read)`，Vdbench执行正常

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=5,sizes=5m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1,operation=write

rd=default,fwdrate=max,elapsed=10,interval=1,operations=(mkdir,create,write,read)
rd=rd1,fwd=fwd1,format=no

# 第11行，包含了write
# 可以正常执行测试

# 测试目录结果 --- 文件结构创建了，且文件大小正常
[root@node30133 nfs1]# ls -alh 30122dir* -R
30122dir:
total 512
drwxrwxrwx 1 root   root     2 Apr 28 10:51 .
drwxrwxrwx 1 nobody nobody  23 Apr 28 10:51 ..
drwxrwxrwx 1 root   root     5 Apr 28 10:51 vdb.1_1.dir
-rwxrwxrwx 1 root   root   208 Apr 28 10:51 vdb_control.file

30122dir/vdb.1_1.dir:
total 25M
drwxrwxrwx 1 root root    5 Apr 28 10:51 .
drwxrwxrwx 1 root root    2 Apr 28 10:51 ..
-rw-rw-rw- 1 root root 5.0M Apr 28 10:51 vdb_f0000.file
-rw-rw-rw- 1 root root 5.0M Apr 28 10:51 vdb_f0001.file
-rw-rw-rw- 1 root root 5.0M Apr 28 10:51 vdb_f0002.file
-rw-rw-rw- 1 root root 5.0M Apr 28 10:51 vdb_f0003.file
-rw-rw-rw- 1 root root 5.0M Apr 28 10:51 vdb_f0004.file
```

问：多个操作组合时，`fwdrate=`怎么分配？比如：`fwdrate=100,operations=(mkdir,create,write,read)`

答：4个操作（`mkdir,create,write,read`）均分全部的`fwdrate` --- 每个操作分到`25`，当`mkdir`和`create`执行完毕后，这些线程会被终止（因为没事情可做）；剩下的`read`和`write`依旧按照之前的`fwdrate`执行（`fwdrate=25`），总共为`fwdrate=50`

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=5m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1,operation=write

rd=default,fwdrate=100,elapsed=20,interval=1,operations=(mkdir,create,write,read)
rd=rd1,fwd=fwd1,format=no

# 第11行，fwdrate=100，operations=(mkdir,create,write,read)

# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
10:59:39.197 Anchor size: anchor=/mnt/shareDir/standard-nfs001/30122dir: dirs:            1; files:          100; bytes:   500.000m (524,288,000)
...
10:59:41.003 Starting RD=rd1; elapsed=20; fwdrate=100; For loops: None
10:59:41.251 Message from slave localhost-0: 
10:59:41.251 Anchor: /mnt/shareDir/standard-nfs001/30122dir
10:59:41.251 Vdbench is trying to create a new directory, but all directories
10:59:41.251 already exist and no threads are currently active deleting directories.
10:59:41.251 FwgThread.canWeGetMoreDirectories(): Shutting down threads for operation=mkdir
10:59:41.251 
read ....read..... ....write.... ..mb/sec... mb/sec .xfer.. ...mkdir.... ...create... ....open.... ...close....
pct   rate   resp   rate   resp  read write  total    size  rate   resp  rate   resp  rate   resp  rate   resp
42.5   17.0  0.716   23.0 60.553 17.00 23.00  40.00 1048576   1.0  0.895  24.0  2.120  36.0  0.936  30.0  0.040
53.1   26.0  0.929   23.0 62.885 26.00 23.00  49.00 1048576   0.0  0.000  22.0  1.365  31.0  0.734  29.0  0.030
48.9   23.0  0.976   24.0 68.374 23.00 24.00  47.00 1048576   0.0  0.000  23.0  1.282  32.0  0.725  32.0  0.032
50.0   27.0  0.579   27.0 56.590 27.00 27.00  54.00 1048576   0.0  0.000  25.0  1.148  36.0  0.673  37.0  0.026
10:59:45.141 
10:59:45.141 Message from slave localhost-0: 
10:59:45.141 Anchor: /mnt/shareDir/standard-nfs001/30122dir
10:59:45.141 Vdbench is trying to create a new file, but all files already exist, 
10:59:45.141 and no threads are currently active deleting files
10:59:45.141 FwgThread.canWeGetMoreFiles(): Shutting down threads for operation=create
10:59:45.141 
...
49.9   25.5  0.979   25.5 56.313 25.47 25.53  51.00 1048576   0.0  0.000   4.0  1.241  14.2  0.465  14.1  0.040
...
11:00:01.267 Miscellaneous statistics:
11:00:01.267 (These statistics do not include activity between the last reported interval and shutdown.)
11:00:01.268 FILE_CREATES        Files created:                                  100          5/sec
11:00:01.268 DIRECTORY_CREATES   Directories created:                              1          0/sec
11:00:01.268 READ_OPENS          Files opened for read activity:                 102          5/sec
11:00:01.268 WRITE_OPENS         Files opened for write activity:                203         10/sec
11:00:01.269 DIR_EXISTS          Directory may not exist (yet):                    4          0/sec
11:00:01.269 FILE_BUSY           File busy:                                      174          8/sec
11:00:01.269 FILE_MAY_NOT_EXIST  File may not exist (yet):                       278         13/sec
11:00:01.269 FILE_MUST_EXIST     File does not exist (yet):                    2,772        138/sec
11:00:01.270 FILE_NOT_FULL       File has not been completely written:           777         38/sec
11:00:01.270 FILE_CLOSES         Close requests:                                 297         14/sec

# 第26、39行，提示“Shutting down threads for operation=mkdir”和“Shutting down threads for operation=create”
# 第42行，结果我省略了，只保留了一个，可以看到只有write和read还在执行，且每个操作的fwdrate约等于25
```

#### foroperation[s]=

覆盖所有FWDs里的`operation=`参数，并按顺序循环执行，参数最后的`s`可以不写

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=100,sizes=$sizes,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1,operation=read

rd=default,fwdrate=max,elapsed=10,interval=1,foroperations=(write,read)
rd=rd1,fwd=fwd*,format=yes

# 第11行的foroperations=(write,read) 会覆盖第9行的operation=read
# 会生成2个RD，一个RD的operation是write，一个是read，逐步执行

# 结果
15:43:30.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=write
...
15:43:48.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=read
```

与RD里的`operations=`参数互斥

```java
/**
   * Store the requested operations.
   */
private void storeOperations(Vdb_scan prm)
{
    if (foroperations_used)
        common.failure("rd=" + rd_name + ": 'operations' and 'foroperations' parameters are mutually exclusive");
	...
}

public void storeForOperations(Vdb_scan prm)
{
    foroperations_used = true;
    if (operations_used)
        common.failure("rd=" + rd_name + ": 'operations' and 'foroperations' parameters are mutually exclusive");
	...
}
```

使用`foroperations=(mkdir,create,write,read)`创建文件结构，由于是循环依次执行，当文件不存在时，会自动创建

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,foroperations=(mkdir,create,write,read)
rd=rd1,fwd=fwd*,format=no

# 第11行的 foroperations=(mkdir,create,write,read) 和 第12行的 format=no
# 当目标目录不存在文件时，且format=no时，测试也可以正常执行

# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
15:06:05.002 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=mkdir
...
15:06:05.412 FwgThread.canWeGetMoreDirectories(): Shutting down threads for operation=mkdir
...
15:06:07.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=create
...
15:06:07.466 FwgThread.canWeGetMoreFiles(): Shutting down threads for operation=create
...
15:06:09.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=write
...
15:06:20.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: operation=read
...

# 测试目录结果
[root@node30133 nfs1]# ls -alh 30122dir/ -R
30122dir/:
total 512
drwxrwxrwx 1 root   root     2 Apr 29 15:06 .
drwxrwxrwx 1 nobody nobody  23 Apr 29 15:06 ..
drwxrwxrwx 1 root   root    10 Apr 29 15:06 vdb.1_1.dir
-rwxrwxrwx 1 root   root   207 Apr 29 15:06 vdb_control.file

30122dir/vdb.1_1.dir:
total 10M
drwxrwxrwx 1 root root   10 Apr 29 15:06 .
drwxrwxrwx 1 root root    2 Apr 29 15:06 ..
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0000.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0001.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0002.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0003.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0004.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0005.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0006.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0007.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0008.file
-rw-rw-rw- 1 root root 1.0M Apr 29 15:06 vdb_f0009.file
```

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### fordepth=

覆盖所有FSDs里的`depth=`参数，并循环依次执行

单值 --- `fordepth=5` --- `depth=5`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1

rd=default,fwdrate=max,elapsed=10,interval=1,fordepth=5
rd=rd1,fwd=fwd*,format=yes

# 第5行，depth=1
# 第11行，fordepth=5，覆盖了第5行的值
# 执行结果如下：--- 注意第19行的depth=5
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
11:11:39.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: depth=5
...
```

自增组合 --- `fordepth=(5-10,3)` --- 按`3`自增，`depth=5`、`depth=8`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1

rd=default,fwdrate=max,elapsed=10,interval=1,fordepth=(5-10,3)
rd=rd1,fwd=fwd*,format=yes

# 第11行，fordepth=(5-10,3)
# 执行结果如下 --- 注意第20行和22行的 depth=5、depth=8
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
11:14:31.002 Starting RD=format_for_rd1
...
11:14:34.002 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: depth=5
...
11:14:48.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: depth=8
...
```

自由组合 --- `fordepth=(5,8)` --- `depth=5`、`depth=8`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1

rd=default,fwdrate=max,elapsed=10,interval=1,fordepth=(5,8)
rd=rd1,fwd=fwd*,format=yes

# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
11:14:31.002 Starting RD=format_for_rd1
...
11:14:34.002 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: depth=5
...
11:14:48.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: depth=8
...
```

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### forwidth=

覆盖所有FSDs里的`width=`参数，并循环依次执行

单值 --- `forwidth=5` --- `width=5`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1

rd=default,fwdrate=max,elapsed=10,interval=1,forwidth=5
rd=rd1,fwd=fwd*,format=yes

# 第11行的 forwidth=5 将第5行的 width=1 覆盖
# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
11:28:15.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: width=5
...
```

自增组合 --- `forwidth=(5-10,3)` --- 按`3`自增，`width=5`、`width=8`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd1,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd1

rd=default,fwdrate=max,elapsed=10,interval=1,forwidth=(5-10,3)
rd=rd1,fwd=fwd*,format=yes

# 第11行的 forwidth=(5-10,3)会生成width=5、width=8
# 执行结果 --- 注意第18行和20行
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
11:30:38.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: width=5
...
11:30:55.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: width=8
...
```

自由组合 --- 使用方法和执行逻辑与`fordepth=`参数类似，不再赘述

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### forfiles=

覆盖所有FSDs里的`files=`参数，并循环依次执行

单值 --- `forfiles=5` --- `files=5`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forfiles=5
rd=rd1,fwd=fwd*,format=yes

# 第11行的 forfiles=5 会覆盖所有fsd的files=10
# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
13:56:23.504 Estimated totals for all 3 anchors: dirs: 3; files: 15; bytes: 15.000m 
...
13:56:27.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: files=5
...
```

自增组合 --- `forfiles=(5-10,3)` --- 按`3`自增，`files=5`、`files=8`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir,count=(1,3)

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forfiles=(5-10,3)
rd=rd1,fwd=fwd*,format=yes

# 第11行，forfiles=(5-10,3)
# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ sizes=1m
...
14:17:55.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: files=5
...
14:18:08.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: files=8
...
```

自由组合 --- 使用方法和执行逻辑与`fordepth=`参数类似，不再赘述

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### forsizes=

覆盖所有FSDs里的`sizes=`参数，并循环依次执行

单值 --- `forsizes=5m` --- `sizes=5m`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forsizes=5m
rd=rd1,fwd=fwd*,format=yes

# 第11行的forsizes=5m 覆盖了 第5行的 sizes=1m
```

自增组合 --- `forsizes=(5m-10m,3m)` --- 按`3`自增，`sizes=5m`、`sizes=8m`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forsizes=(5m-10m,3m)
rd=rd1,fwd=fwd*,format=yes

# 结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ sizes=1m
...
15:13:48.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: sizes=5m
...
15:14:06.000 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: sizes=8m
```

自由组合 --- 使用方法和执行逻辑与`fordepth=`参数类似，不再赘述

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### fortotal[size]=

覆盖所有FSDs里的`totalsize=`参数，并循环依次执行，中括号里的size可以省略

单值 --- `forwidth=5m` --- `totalsize=5m`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync,totalsize=10m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,fortotalsize=5m
rd=rd1,fwd=fwd*,format=yes

# 第11行的 fortotalsize=5m 覆盖了 第5行的 totalsize=10m
# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
15:48:23.022 localhost-0: Created totalsize=   5.0m using 5 of 10 files for anchor=/mnt/shareDir/standard-nfs001/30122dir
...
15:48:25.414 localhost-0: Created totalsize=   5.0m using 5 of 10 files for anchor=/mnt/shareDir/standard-nfs001/30122dir
15:48:26.002 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: totalsize=5m
...
```

自增组合 --- `forwidth=(5m-10m,3m)` --- 按`3m`自增，`totalsize=5m`、`totalsize=8m`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync,totalsize=10m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,fortotalsize=(5m-10m,3m)
rd=rd1,fwd=fwd*,format=yes

# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
...
15:50:57.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: totalsize=5m
...
15:51:11.001 Starting RD=rd1; elapsed=10; fwdrate=max. For loops: totalsize=8m
...
```

自由组合 --- 使用方法和执行逻辑与`fordepth=`参数类似，不再赘述

如果只想首个`RD`执行是进行`format`，可以设置`format=(yes,restart)`参数

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=$sizes,openflags=o_sync,totalsize=10m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,fortotalsize=(5m,3m)
rd=rd1,fwd=fwd*,format=(yes,restart)

# 第12行，format=(yes,restart)
# RD首次执行时（totalsize=5m），会format
# 再次执行时（totalsize=3m），不会format了
```

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### forwss=

覆盖所有FSDs里的`workingsetsize=`或`wss=`参数，并循环依次执行

单值 --- `forwss=5m` --- `wss=5m`，执行一次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=$sizes,openflags=o_sync,totalsize=10m,wss=1m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forwss=5m
rd=rd1,fwd=fwd*,format=(yes,restart)

# 第11行forwss=5m 覆盖了 第5行的wss=1m
```

自增组合 --- `forwss=(5m-10m,3m)` --- 按`3m`自增，`wss=5m`、`wss=8m`，执行两次

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=$sizes,openflags=o_sync,totalsize=10m,wss=1m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forwss=(5m-10m,3m)
rd=rd1,fwd=fwd*,format=(yes,restart)

# 第11行的forwss=(5m-10m,3m) 覆盖了 第5行的wss=1m
```

自由组合 --- 使用方法和执行逻辑与`fordepth=`参数类似，不再赘述

与其他forxxx参数结合时，执行顺序见`forxxx的执行顺序`

#### forxxx的执行顺序

简单理解：在RD中谁最后定义谁的LOOP就最先执行完 --- <font color="#FF00000">注意，写多个`forxxx`参数后，可以用`-s`参数先模拟一下，看是否符合自己的预期</font>

> 声明：不排除有特殊的forxxx参数，发现后再补充规则

```shell
# 示例
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=default,depth=1,width=1,files=10,sizes=1m,openflags=o_sync,totalsize=10m,wss=1m
fsd=fsd,anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=max,elapsed=10,interval=1,forwss=(5m-10m,3m),fortotalsize=(10m-20m,6m),forfiles=(20-30,6)
rd=rd1,fwd=fwd*,format=yes

# 执行结果
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ -s
...
16:29:38.820 Simulating RD=format_for_rd1
16:29:38.820 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=5m totalsize=10m files=20
16:29:38.820 Simulating RD=format_for_rd1
16:29:38.820 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=5m totalsize=10m files=26
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=5m totalsize=16m files=20
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=5m totalsize=16m files=26
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=8m totalsize=10m files=20
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=8m totalsize=10m files=26
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=8m totalsize=16m files=20
16:29:38.821 Simulating RD=format_for_rd1
16:29:38.821 Simulating RD=rd1; elapsed=10; fwdrate=max. For loops: wss=8m totalsize=16m files=26

# 执行逻辑图示
wss=5m
|---> totalsize=10m
|		|---> files=20	--- 执行顺序1
|		|---> files=26	--- 执行顺序2
|---> totalsize=16m
		|---> files=20	--- 执行顺序3
		|---> files=26	--- 执行顺序4
wss=8m
|---> totalsize=10m
|		|---> files=20	--- 执行顺序5
|		|---> files=26	--- 执行顺序6
|---> totalsize=16m
		|---> files=20	--- 执行顺序7
		|---> files=26	--- 执行顺序8
```

# 命令行参数详解

## ※※※ -f xxx yyy zzz

`-f`后面接参数文件（单个或多个均可），多个参数文件的顺序要符合参数文件顺序规则

参数文件规则，详见`参数文件详解`

```shell
[root@node1 testinclude]# cat create_files 
create_anchors=yes
messagescan=no

[root@node1 testinclude]# cat fsd 
fsd=fsd1,anchor=./dir1,depth=1,width=1,files=10000,size=8k,shared=yes

[root@node1 testinclude]# cat fwd 
fwd=fwd1,fsd=fsd1,operation=read,threads=16

[root@node1 testinclude]# cat rd 
rd=rd1,fwd=fwd*,fwdrate=100,elapsed=5,interval=1,format=restart

# 按顺序放好，就可以执行
[root@node1 testinclude]# ../vdbench -f create_files fsd fwd rd

# 如果打乱顺序，就会出现错误 --- 会出现找不到fsd
# ../vdbench -f create_files fwd rd fsd
```

## ※ -o xxx

结果输出目录，默认为当前目录下的`output`文件夹，文件夹不存在时会自动创建，存在时会先删除所有旧的`html`文件

可以在目录名后添加一个`+`，例如 `-o dirname+`，每次执行测试，这个目录自动`+1`，最大自增到`999` --- `dirname001~dirname999`

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+
```

还可以添加时间戳：`-o output.tod` --- 生成名叫`output.yymmdd.hhmmss`的目录

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res.tod
...
11:00:34.210 Created output directory '/root/vdbench50407/res.240506.110034'
...
```

## ※ -t

快速执行一次内置的测试（用于块，5秒） --- 常用于测试Vdbench是否可用

```shell
[root@node1 vdbench50407]# ./vdbench -t
...
11:02:22.633 input argument scanned: '-f/tmp/parmfile'
11:02:22.704 Starting slave: /root/vdbench50407/vdbench SlaveJvm -m localhost -n localhost-10-240506-11.02.22.506 -l localhost-0 -p 5570   
11:02:23.045 All slaves are now connected
11:02:23.150 Vdbench will attempt to expand a disk file if the requested file size is a multiple of 1mb
11:02:23.151 lun=/tmp/quick_vdbench_test does not exist or is too small. host=localhost
11:02:24.002 Starting RD=SD_format; I/O rate: Uncontrolled MAX; elapsed=(none); For loops: threads=2 iorate=max
11:02:24.118 All sequential workloads on all slaves are done.
11:02:24.118 This triggers end of run inspite of possibly some non-sequential workloads that are still running.
...
11:02:26.002 Starting RD=rd1; I/O rate: 100; elapsed=5; For loops: None
...
11:02:32.520 Vdbench execution completed successfully. Output directory: /root/vdbench50407/output 
```

## ※ -tf

快速执行一次内置的测试（用于文件，5秒） --- 常用于测试Vdbench是否可用

```shell
[root@node1 vdbench50407]# ./vdbench -tf
...
11:02:38.507 input argument scanned: '-f/tmp/parmfile'
11:02:38.585 Anchor size: anchor=/tmp/fsd1: dirs:            1; files:          100; bytes:    12.500m (13,107,200)
11:02:38.629 Starting slave: /root/vdbench50407/vdbench SlaveJvm -m localhost -n localhost-10-240506-11.02.38.351 -l localhost-0 -p 5570   
11:02:38.981 All slaves are now connected
11:02:39.048 localhost-0: Created anchor directory: /tmp/fsd1
11:02:40.003 Starting RD=format_for_rd1
11:02:40.038 localhost-0: anchor=/tmp/fsd1 mkdir complete.
11:02:40.072 localhost-0: anchor=/tmp/fsd1 create complete.
...                                           
11:02:41.263 
11:02:41.263 Miscellaneous statistics:
11:02:41.263 (These statistics do not include activity between the last reported interval and shutdown.)
11:02:41.264 FILE_CREATES        Files created:                                  100        100/sec
11:02:41.264 DIRECTORY_CREATES   Directories created:                              1          1/sec
11:02:41.264 WRITE_OPENS         Files opened for write activity:                100        100/sec
11:02:41.264 DIR_BUSY_MKDIR      Directory busy (mkdir):                           1          1/sec
11:02:41.265 DIR_EXISTS          Directory may not exist (yet):                    3          3/sec
11:02:41.265 FILE_CLOSES         Close requests:                                 100        100/sec
11:02:41.265 
11:02:42.001 Starting RD=rd1; elapsed=5; fwdrate=100; For loops: None
...
11:02:47.257 Miscellaneous statistics:
11:02:47.257 (These statistics do not include activity between the last reported interval and shutdown.)
11:02:47.257 READ_OPENS          Files opened for read activity:                   8          1/sec
11:02:47.257 WRITE_OPENS         Files opened for write activity:                  8          1/sec
11:02:47.257 
11:02:48.421 Vdbench execution completed successfully. Output directory: /root/vdbench50407/output
```

## -e nn

临时改变`RD`里的`elapsed=`参数。

在快速发现问题时非常有用。例如，创建了一个包含多个运行定义（RD）的24小时测试。如果存在问题，肯定不希望在运行了数小时后才发现。只需将测试的运行时间设置为几秒或几分钟，就可以快速检查一切是否正常。

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ -e 5
```

## -i nn

临时改变`RD`里的`interval=`参数

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ -i 2
```

## -w nn

临时改变`RD`里的`warmup=`参数

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -o res+ -w 5
```

## -m nn

受CPU限制，单JVM能够处理的最大IOPS或线程数量有限。一个Java进程对应一个JVM，所以单进程完成任务有限，为了缓解这个情况，Vdbench会每10万个I/O请求（通过`ios_per_jvm=nnnn`修改）启动一个Slave进程。Slave进程的数量可以通过`-m nn`或`hd=default,jvms=nn`进行设置，或者Vdbench自动取`SDs`的个数和默认值（`8`）中的较小值。

如果测试机器内存很小，设置很多个`JVMs`可能会出现问题，一个原则是将每个`JVM`的`IOPS`保持在10万以下即可，不要设置太多的`JVMs`

除了顺序工作负载，每个工作负载均在每个`JVM`或`Slave`上执行。在每个Slave上执行顺序工作负载会导致每个Slave读取相同的顺序块，虽然性能值很高，但不是一个准确的顺序工作负载。因此，顺序工作负载会在每个可用的JVM/Slave上轮流执行。

设置`JVMs`数量 --- 首选`hd=default,jvms=nn`或`hd=hostX,jvms=nn`参数。

当每个`JVM`完成的`IOPS`超过`100,000`时，Vdbench会有警告输出，告诉你没有足够的活跃`JVM`，可以尝试增加这个值。

对于文件系统，`Vdbench`不会自动设置`JVM/Slave`数量

## -v

开启数据验证，每次写入数据块时都会记录（内存）。

对同一个数据块再次读取或写入时，验证该数据块的原内容。

```java
// Vdbmain.java
else if (thisarg.startsWith("-v"))
{
    Validate.setValidate(); // 将 Validate.validate 设为 true
    if (thisarg.contains("r")) Validate.setImmediateRead(); // 将 Validate.validate_immed 设为 true
    if (thisarg.contains("2")) Validate.setImmediateRead(); // 同上
    if (thisarg.contains("w")) Validate.setNoPreRead(); // 将 Validate.validate_nopreread 设为 true
    if (thisarg.contains("t")) Validate.setStoreTime(); // 将 Validate.validate_time 设为 true
    if (thisarg.contains("c"))
    {
        common.failure("'validate=continue' no longer supported");
        Validate.setContinueOldMap();
        return;
    }

}
```



### -vr

待后续研究

数据块写入后立即进行读取和验证

在对大容量LUN进行I/O操作时，通常需要很长时间才会再次使用某个数据块，因此，使用`-vr`可能有助于快速确认数据是否正确。

注意，数据库写入后立即读取验证，可能出现数据达到了文件系统或存储控制器缓存，没有真实落盘的情况。

### -vw

待后续研究

数据块再次被写入时，才进行读取验证

### -vt

待后续研究

在内存中保存最后一次成功读取或写入的时间戳（不支持日志记录）。当该数据块出现故障时，将报告此时间戳。知道数据块在哪个时间点是正常的有助于识别可能导致问题的错误注入。

注意：每个数据块额外需要8字节的Java堆内存（因此，对于512字节的数据块，内存需求可能会过高）。目前设置了一个内部限制，最多允许31位的数据块，大约16GB的Java堆内存。

## -j

创建日志文件，后续可用于数据验证

```java
// Vdbmain.java
else if (thisarg.startsWith("-j"))
{
    Validate.setValidate(); // 将 Validate.validate 设为 true
    Validate.setJournaling(); // 将 Validate.journal_active 设为 true

    if (thisarg.contains("r")) Validate.setJournalRecovery(); // 将 Validate.journal_recovery 设为 true
    if (thisarg.contains("n")) Validate.setNoJournalFlush(); // 将 Validate.journal_flush 设为 false
    if (thisarg.contains("m")) Validate.setMapOnly(); // 将 Validate.journal_maponly 设为 true
    if (thisarg.contains("o")) Validate.setRecoveryOnly(); // 将 Validate.journal_rec_only 设为 true
    if (thisarg.contains("2")) Validate.setImmediateRead(); // 将 Validate.validate_immed 设为 true
    if (thisarg.contains("t")) Validate.setStoreTime(); // 将 Validate.validate_time 设为 true
    if (thisarg.contains("i")) Validate.setIgnorePending(); // 将 Validate.ignore_pending 设为 true
    if (thisarg.contains("s")) Validate.setSkipRead(); // 将 Validate.skip_data_read 设为 true
}
```



### -jr



#### -jro



#### -jri



### -jm



### -jn



## -s

工作负载模拟，不真实执行

当不确定你定义的参数文件是否符合预期时，可以用这个参数测试一下

## -k

Solaris系统专属，暂未涉及，跳过。



## -c

执行RD之前，删除FSD的预埋数据

<font color="#FF00000">注意：虽然看了源码，但是执行不符合预期，再没有确切搞懂它真正使用方式之前，建议不要使用</font>

```java
// Vdbmain.java
else if (thisarg.startsWith("-c"))
{
    if (     thisarg.indexOf("o") != -1) force_format_only    = true;
    else if (thisarg.indexOf("y") != -1) force_format_yes     = true;
    else if (thisarg.indexOf("n") != -1) force_format_no      = true;
    else /*                         */   force_fsd_cleanup    = true;
}
```

```java
// FwgRun.java

/* Do a forced cleanup (-c execution parameter) the first time we get anchor: */
for (int i = 0; i < fwgs_for_slave.size(); i++)
{
    FwgEntry fwg = (FwgEntry) fwgs_for_slave.elementAt(i);
    if (first_time_map.get(fwg.anchor.getAnchorName()) == null)
    {
        if (work.force_fsd_cleanup)
        {
            common.ptod("Deleting old file structure because of forced cleanupOldFiles ('-c').");
            fwg.anchor.setDeletePending(true);
        }
    }
    first_time_map.put(fwg.anchor.getAnchorName(), fwg.anchor.getAnchorName());
}
```

### -co

同`format=only`，覆盖参数文件里的`foramt=`参数

### -cy

同`format=yes`，覆盖参数文件里的`foramt=`参数

### -cn

同`format=no`，覆盖参数文件里的`foramt=`参数

## -p nnn

设置Vdbench的socket端口（默认5570）

```shell
[root@node1 vdbench50407]# ./vdbench -f testfile4 -p 1111
```

## -l nnn --- 小写L

重复执行所有RDs，注意，是所有

`-l` --- 死循环，一直执行

`-l nnn` --- 执行nnn秒，也可以后面添加单位`s/m/h`，代表执行`nnn seconds/minutes/hours`，如：-l 24h

```java
// Vdbmain.java

/**
   * -l nnn parameter.
   * Without 'nn', loop is endless, with it ends after 'nn' seconds.
   */
private static void scanLoopParameter(String parm)
{
    loop_all_runs = true;
    if (parm.equals("-l"))
        return;

    String number = parm.substring(2);

    int multiplier = 1;
    if (number.endsWith("s"))
    {
        multiplier = 1;
        number = number.substring(0, number.length() -1);
    }
    else if (number.endsWith("m"))
    {
        multiplier = 60;
        number = number.substring(0, number.length() -1);
    }
    else if (number.endsWith("h"))
    {
        multiplier = 3600;
        number = number.substring(0, number.length() -1);
    }
    else if (!common.isNumeric(number))
    {
        common.failure("Expecting no parameter or a numeric paramater after '-l': " + parm);
    }

    loop_duration = Long.parseLong(number) * multiplier;
    loop_duration *= 1000;
}
```

```java
public static boolean shutDownAfterLoops(long first_start_tod)
{
	...
    if (System.currentTimeMillis() - first_start_tod > Vdbmain.loop_duration)
    {
        BoxPrint.printOne("Terminating loop run after %d seconds",
                          (System.currentTimeMillis() - first_start_tod) / 1000);
        return true;
    }

    return false;
}

// 第4行
// 当所有RDs运行总时间 × LOOP ＞ nnn时，会停止下一次LOOP
```

注意，`loop=`参数，当没单位时，是定义的`loop`次数，这个跟`-l`参数不一样，谨记！

注意，当定义了`loop=`参数时，再使用`-l`参数，以`loop=`参数为准。究其原因，是`-l`参数先被解析，参数文件后被解析，后者把前者覆盖了

```java
// Vdbmain.java

/* Get execution parameters:   */
pdm_output = PdmStart.lookForOutputDir(args);
check_args(args);
RshDeamon.readPortNumbers();

/* Parse everything we've got: */
parms_report = Report.createHmtlFile("parmscan.html");
//Vdb_scan.copy_file = Report.createHmtlFile("parmfile.html");
Report.getSummaryReport().printHtmlLink("Copy of input parameter files",
                                        "parmfile", "parmfile");
Report.getSummaryReport().printHtmlLink("Copy of parameter scan detail",
                                        "parmscan", "parmscan");
/* Just in case we die very early: */
Report.flushAllReports();

Vdb_scan.Vdb_scan_read(parm_files, true);
parseParameterLines();

// 第5行，解析可执行参数 --- 着重看里面的 scan_args(nargs) ---> scanLoopParameter(thisarg)
// 第19行，解析参数文件 --- 着重看 MiscParms.readParms()
```

## -r rdname

如果参数文件中定义了许多个`RDs`，那么`-r rdname`参数代表从`rdname`开始执行，将`rdname`之前的`RDs`都从执行队列中移除

```shell
[root@node1 vdbench50407]# cat testfile4 
create_anchors=yes
messagescan=no

fsd=fsd,depth=1,width=1,files=10,sizes=1m,openflags=o_sync,
anchor=/mnt/shareDir/standard-nfs001/30122dir

fwd=default,fileio=random,fileselect=random,threads=16,xfersizes=1m
fwd=fwd1,fsd=fsd*

rd=default,fwdrate=1,elapsed=10,interval=1,format=yes
rd=rd1,fwd=fwd*
rd=rd2,fwd=fwd*
rd=rd3,fwd=fwd*

# 第12、13、14行，总共包含3个RD
# 执行结果 --- 从RD2开始执行，RD1不执行
[root@node1 vdbench50407]# ./vdbench -f testfile4 -r rd2
# 执行结果 --- 从RD3开始执行，RD1和RD2不执行
[root@node1 vdbench50407]# ./vdbench -f testfile4 -r rd3
```

源码如下，喜欢研究源码的可以看看

```java
// RD_entry.java

/**
   * The '-r rdname' parameter:
   * Removes ALL RDs found before this RD.
   */
private static void keepRestart(Vector <RD_entry> new_list)
{
    for (int i = 0; i < new_list.size(); i++)
    {
        RD_entry rd = new_list.get(i);
        if (rd.rd_name.startsWith(SD_entry.SD_FORMAT_NAME))
            common.failure("Skipping rd=%s during restart is not allowed.", rd.rd_name);

        else if (rd.rd_name.startsWith(FSD_FORMAT_RUN))
            common.failure("Skipping rd=%s during restart is not allowed.", rd.rd_name);

        if (rd.rd_name.startsWith(restart_rd))
            break;
        new_list.set(i, null);
    }

    while (new_list.remove(null));
    if (new_list.size() == 0)
        common.failure("Restart rd=%s not found", restart_rd);
}
```

## xxx=yyy …..

变量替换，可以在参数文件中使用变量（如：`$lun`），然后可以通过命令行进行替换。

参数文件内只要设置了参数，就必须通过命令行进行赋值；相同的道理，命令行给变量赋值，参数文件中也必须得找得到，否则Vdbench都会停止运行

```shell
# 参数文件里设置变量
sd=sd1,lun=$lun

# 执行时进行变量赋值
./vdbench –f parmfile lun=/dev/x
```

如果参数文件嵌入`shell`脚本中，您还可以使用`!`来防止脚本语言意外替换变量，例如 `sd=sd1,lun=!lun`

```shell
# 这种情况没遇到过，遇到了再补充。
```

# 实用命令详解

```shell
./vdbench [compare] [csim] [dsim] [edit] [jstack] [parse] [print] [rsh] [sds] [showlba]
```

部分命令是带GUI界面的，由于我的Linux测试机没装GUI界面，所以相关命令用Windows来测试

## ./vdbench sds



## ./vdbench jstack



## ./vdbench rsh







## ./vdbench print



## ./vdbench edit





## ./vdbench compare



## ./vdbench parse



## ./vdbench csim



## ./vdbench dsim



## ./vdbench printjournal



## ./vdbench showlba







# 其他



# 附录



## 源码解析



### File system

`FwgThread`类继承自 `Java`原生`Thread`类

`OpWrite`、`OpRead`等操作类继承自`FwgThread`类

`Thread`类调用`start()`方法的结果是 --- 最终调用类中的`run()`方法 --- 详见[链接](https://blog.csdn.net/qq_21383435/article/details/123021752)

```java

```



### forxxx参数源码分析

```java
// For_loop.java
```

