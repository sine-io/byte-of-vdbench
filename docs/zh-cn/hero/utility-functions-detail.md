# 实用命令详解

`Vdbench` 不只有压测主流程，还自带一批辅助工具。

```shell
./vdbench [compare] [csim] [dsim] [edit] [jstack] [parse] [print] [rsh] [sds] [showlba]
```

可以把它们分成两类：

- 偏图形或交互工具
- 偏命令行分析工具

## 偏图形或交互的工具

### `./vdbench sds`

这是 `SD` 参数生成工具。参考源码里的窗口标题就是 `Vdbench SD parameter generation tool`。

适合：

- 需要批量整理设备定义
- 想通过图形方式生成 `sd=` 段落

注意：

- 正常入口更偏 GUI
- 如果你当前环境没有图形界面，不适合作为首选工具

### `./vdbench compare`

用于比较两次或多次测试输出目录。

常见用法：

```shell
./vdbench compare dir_old dir_new
./vdbench compare dir_old dir_new -o diff.tsv
./vdbench compare old output001 output002 new output003 output004 -o diff.tsv
```

适合：

- 对比两轮不同参数结果
- 做版本或设备对比

### `./vdbench edit`

内置编辑器入口，适合在带图形环境时快速打开参数文件。

如果你已经习惯自己的编辑器，这个工具并不是必须项。

### `./vdbench showlba`

用于查看 `showlba=yes` 生成的轨迹文件。

常见用法：

```shell
./vdbench showlba [-w WDname] [-s SDname] [-o output.png] localhost-0.showlba.txt
```

参数含义：

- `-s SDname`
  - 只看指定 `sd`
- `-w WDname`
  - 只看指定 `wd`
- `-o output.png`
  - 不弹图形窗口，直接输出图片

适合：

- 看访问分布
- 过滤到指定 `WD` 或 `SD`
- 直接输出 PNG

## 偏命令行分析的工具

### `./vdbench parse`

源码中实际实现名是 `parseflat`，但主入口对 `parse` 和 `parseflat` 都能识别。

常见用法：

```shell
./vdbench parse -i flatfile.html -o output.csv
./vdbench parse -i output/flatfile.html -o output.tsv -a
```

适合：

- 从 `flatfile.html` 提取表格数据
- 导出成 `csv/tsv`
- 做自动筛选和批量对比

常见参数：

- `-i`
  - 输入 `flatfile.html`
- `-o`
  - 输出文件名
- `-c`
  - 只导出指定列
- `-f`
  - 按列值过滤
- `-a`
  - 只保留 `avg` 行

### `./vdbench print`

用于打印某个设备或文件上的数据块内容。

常见用法：

```shell
./vdbench print device lba xfersize [-f openflag]
./vdbench print /dev/sdb 4k 4k -f directio
```

适合：

- 辅助查看块内容
- 排查某个逻辑地址的数据
- 配合验证场景做底层定位

### `./vdbench printjournal`

用于查看 `.jnl` 文件内容。

常见用法：

```shell
./vdbench printjournal xxx.jnl [-l nnn]
./vdbench printjournal journal.jnl -l 4k
```

适合：

- journal 校验场景
- 恢复或验证相关排障

### `./vdbench rsh`

用于启动 `Vdbench` 自带的远程执行守护进程。

它的使用方式不是“执行一次命令就结束”，而是先在目标主机上启动守护进程，再由主节点连接。

适合：

- 多主机压测
- 不想直接依赖系统 `rsh/ssh` 的场景

最小使用顺序通常是：

1. 在目标主机执行 `./vdbench rsh`
2. 在主控主机执行多主机测试脚本
3. 由主控主机自动连接远端守护进程

### `./vdbench jstack`

调用 `Java` 的 `jstack` 工具抓线程栈。

适合：

- `Vdbench` 卡住时排查线程状态
- 辅助定位 `JVM` 侧问题

前提：

- 运行环境中能找到 `jstack`

### `./vdbench csim`

压缩率模拟工具。

常见用法：

```shell
./vdbench csim [-l nnn] [-p nnn] [-x nnn] [-s nnn] disk1 file1 ...
./vdbench csim -l 1 -p 0.1 -s 10 /data/file1 /data/file2
```

适合：

- 估算一组磁盘或文件数据的大致可压缩性

常见参数：

- `-l`
  - gzip 压缩级别
- `-p`
  - 抽样读取比例
- `-s`
  - 子集比例
- `-u`
  - 读取并压缩时使用的块大小

### `./vdbench dsim`

重删率模拟工具。

常见用法：

```shell
./vdbench dsim [-n sss] [-w nn] [-f nn] [-u nnnk] disk1 file1 ...
./vdbench dsim -n 60 -w 4 -f 2 -u 128k /data/file1 /data/file2
```

适合：

- 估算一组数据的大致 dedup 特征

常见参数：

- `-n`
  - 进度通知间隔
- `-w`
  - 哈希工作线程数
- `-f`
  - 读盘/读文件线程数
- `-u`
  - dedup 单元大小

## 新手最值得先学的几个工具

如果你还在入门阶段，优先掌握：

1. `parse`
2. `compare`
3. `showlba`
4. `rsh`

其余工具更适合在你已经有明确排障或分析目标时再使用。

## 一条实用路线

如果你已经完成了一轮测试，想把结果整理成“能比较、能复盘”的材料，最常见的顺序是：

1. 先看 [结果分析](../zero/results-analysis.md)
2. 用 `parse` 从 `flatfile.html` 抽出数据
3. 用 `compare` 对比多轮结果目录
4. 有访问分布疑问时，再用 `showlba`

## 下一步

- 想把结果目录整理成可对比数据：先看 [结果分析](../zero/results-analysis.md)
- 想直接找一份起步脚本：看 [脚本模板](script-templates.md)
