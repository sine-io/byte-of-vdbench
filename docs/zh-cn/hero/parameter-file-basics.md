# 参数文件基础

## 基本结构

`Vdbench` 的参数文件不是任意堆叠的键值对，而是有明确顺序的。

- `Raw I/O`
  - `General` -> `HD` -> `SD` -> `WD` -> `RD`
- `File System`
  - `General` -> `HD` -> `FSD` -> `FWD` -> `RD`

`General` 和 `HD` 通常不是必选，但如果要写，就应放在前面。

## 注释与结束

按参考源码的参数扫描逻辑，下面这些行会被当作注释或空行跳过：

- 以 `/` 开头
- 以 `#` 开头
- 以 `*` 开头
- 空行

如果某一行以 `eof` 开头，后面的内容会被忽略。

## 缩写规则

大多数参数可以缩写为“最短唯一值”，但至少保留两个字符更稳妥。

例如：

```shell
xfersize=4k
xf=4k
```

建议：

- 写正式脚本时，优先用完整参数名
- 阅读官方样例时，再去识别缩写

## 常见值格式

### 大小

常见大小参数如 `size`、`xfersize`、`sizes` 可使用：

```shell
size=10g
xfersize=8k
sizes=(32k,50,64k,50)
```

### 时间

常见时间参数如 `elapsed`、`warmup` 可使用：

```shell
elapsed=120
elapsed=2m
elapsed=1h
```

### 多值与范围

```shell
iorate=(100,200,300)
forxfersize=(4k-512k,d)
count=(1,5)
```

看到括号时，通常意味着：

- 多组候选值
- 按比例分配
- 生成一系列对象
- 触发多轮循环执行

!!! tip
    旧版实测里有两个容易误解的写法：`(1-64,d)` 和 `(64,1,d)`。前者在部分场景下并没有按手册描述那样倍增，后者还可能直接报 `Mix of alpha and numeric values not allowed`。第一次写参数时，优先使用更直白的显式列表。

## `default` 的含义

很多定义都允许写成 `xxx=default`。

它的作用通常是：

- 先定义一组默认值
- 后续同类对象复用这些默认值

例如：

```shell
sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdb
sd=sd2,lun=/dev/sdc
```

## `include=` 的用途

`include=` 很适合把“经常变的部分”和“相对稳定的部分”分开：

- 设备或目录定义单独放
- 工作负载定义单独放
- 运行定义单独放

例如：

```shell
include=hd
include=fsd
include=fwd
include=rd
```

但要注意：`include=` 展开后仍然必须满足参数顺序规则。

一个常见拆分方式是：

```shell
include=hd
include=fsd
include=fwd
include=rd
```

这样便于频繁替换设备、目录或线程数，而不必每次改整份脚本。

## 变量替换

参数文件可以和命令行变量一起使用：

```shell
sd=sd1,lun=$lun
```

执行时：

```shell
./vdbench -f parmfile lun=/dev/sdb
```

适合经常变化的值：

- 被测 `lun`
- 线程数
- 输出目录
- 文件大小

## 先看 `parmfile.html` 和 `parmscan.html`

当你怀疑脚本没有按预期执行时，先看结果目录里的两个文件：

- `parmfile.html`
  - 记录本次实际使用的参数文件内容
- `parmscan.html`
  - 记录参数扫描和解析过程

这两个文件非常适合排查：

- `include=` 展开顺序
- 缩写识别
- 某个参数为什么没有生效
- 参数扫描在哪一行停止

## 先掌握的通用参数

虽然 `General` 参数很多，但一开始最值得记住的只有几项：

### `messagescan=`

控制是否扫描系统日志。

常见用法：

```shell
messagescan=no
messagescan=yes
messagescan=nodisplay
```

新手练习脚本时，最常见的是直接写：

```shell
messagescan=no
```

这样能减少与系统日志相关的干扰信息。

### `create_anchors=yes`

文件系统测试里，如果 `anchor` 的上级目录还不存在，通常要打开它。

```shell
create_anchors=yes
```

这在多级目录、临时目录和新建测试路径时很常见。

### `startcmd=` 与 `endcmd=`

它们可以在测试开始前或结束后执行额外命令。

常见形式：

```shell
startcmd="echo begin"
endcmd="echo end"
startcmd=("echo begin",cons)
endcmd=("echo end",sum)
```

适合：

- 开始前清理目录
- 打印环境信息
- 结束后做简单归档

### `fwd_thread_adjust=no`

文件系统测试里，`threads` 在多个 `FSD`、主机或操作之间分摊时，默认可能被自动向上调整。如果你不想接受这种自动调整，可以显式设置：

```shell
fwd_thread_adjust=no
```

这样一旦线程分配不合理，`Vdbench` 会终止，而不是静默帮你改成另一个线程数。

## `Raw I/O` 与 `File System` 的最小骨架

### Raw I/O

```shell
sd=sd1,lun=/dev/sdb,openflags=o_direct
wd=wd1,sd=sd1,xfersize=4k,rdpct=100
rd=rd1,wd=wd1,iorate=100,elapsed=10,interval=1
```

### File System

```shell
fsd=fsd1,anchor=/testdir,depth=1,width=1,files=10,size=1m
fwd=fwd1,fsd=fsd1,operation=read,xfersize=4k,fileio=random,fileselect=random,threads=4
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1
```

## 新手建议

!!! tip
    真正开始写复杂脚本前，先把官方 `example1` 和 `example7` 改成你自己的测试对象，先确认自己能稳定跑通最小闭环。

!!! warning
    写复杂 `forxxx`、`curve`、`format` 组合前，先用 `-s` 模拟，再决定是否真实执行。

## 下一步

- 想直接照着改脚本：看 [脚本模板](script-templates.md)
- 想继续看块设备参数：看 [Raw I/O 参数详解](raw-parameter-detail.md)
- 想继续看文件系统参数：看 [File System 参数详解](filesystem-parameter-detail.md)
