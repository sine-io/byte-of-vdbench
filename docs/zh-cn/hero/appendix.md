# 附录

## 参考包结构

如果你准备在本地对照官方资料阅读，建议将参考包解压到仓库根目录下的 `reference/`，主要分成两部分：

### `reference/vdbench50407/`

官方发布包，适合拿来跑样例和查参考文件：

- `example1 ~ example7`
- `examples/raw/`
- `examples/filesys/`
- `vdbench.pdf`
- `readme.txt`

### `reference/vdbench50407-src/`

参考源码，适合用来确认行为而不是替代教程正文。

!!! tip
    这两个目录默认不参与仓库版本管理。如果你是新 clone 仓库，本地可能并不存在它们；需要时再自行准备即可。

## 推荐的源码入口

如果你想知道“命令是怎么分发的”，优先看：

- `Vdb/Vdbmain.java`

这里能看到：

- `compare`
- `parse`
- `jstack`
- `sds`
- `showlba`
- `rsh`
- `edit`
- `csim`
- `dsim`
- `printjournal`

如果你想知道“参数文件怎么扫描的”，优先看：

- `Vdb/Vdb_scan.java`
- `Vdb/MiscParms.java`

这里尤其适合确认：

- 注释行规则
- `eof`
- `include=`
- `loop=`
- `showlba=yes`

如果你想知道“文件系统 `anchor`、`fsd` 这些规则怎么校验的”，优先看：

- `Vdb/FsdEntry.java`

如果你想知道“工具命令各自从哪里进”，优先看：

- `Vdb/ParseFlat.java`
- `Vdb/ShowLba.java`
- `Vdb/PrintJournal.java`
- `Vdb/PrintBlock.java`
- `VdbComp/WlComp.java`

## 阅读源码时的建议

- 先带着具体问题读，不要一上来就通读
- 先看命令入口和校验逻辑，再看执行细节
- 先用教程和官方样例建立基本认知，再去源码里确认边界行为

## 阅读官方样例时的建议

建议按下面顺序看：

1. `example1`
2. `example7`
3. `examples/raw/random_read`
4. `examples/filesys/random_read`
5. `examples/raw/*curve`
6. `examples/*/multi_host`

如果你想少走弯路，建议配合 [官方样例索引](example-map.md) 一起看。

## 本教程的定位

本教程更重视：

- 如何快速跑通
- 如何避免明显误用
- 如何把官方样例改成自己的场景
- 如何结合结果目录和少量源码确认行为

如果你需要“参数全集逐项考古”，应该把教程、参考包和源码一起看，而不是只依赖单一页面。
