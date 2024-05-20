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
