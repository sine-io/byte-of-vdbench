# 官方样例索引

这一页的目标是解决一个很实际的问题：

> 发布包里样例很多，我现在到底应该先看哪个？

建议把它当成“官方样例 -> 教程页面 -> 自己脚本”的跳板。

## 第一层：先从哪几个样例开始

如果你是第一次接触 `Vdbench`，优先顺序建议如下：

1. `example1`
2. `example7`
3. `examples/raw/random_read`
4. `examples/filesys/random_read`
5. `examples/raw/*curve`
6. `examples/*/multi_host`

原因很简单：

- `example1`
  - 最容易建立 `SD/WD/RD` 的基本认知
- `example7`
  - 最容易建立 `FSD/FWD/RD` 的基本认知
- `random_read`
  - 最容易看懂“随机 / 顺序 / 读 / 写 / 速率”的区别
- `curve`
  - 最适合从“会跑”进入“会分析”
- `multi_host`
  - 最适合在你已经掌握单机场景后再扩展

## 第二层：`example1 ~ example7` 怎么看

### `example1`

- 场景：单个 Raw I/O 最小闭环
- 重点：`sd`、`wd`、`rd`
- 建议后续：
  - [Raw I/O 参数详解](raw-parameter-detail.md)
  - [脚本模板](script-templates.md)

### `example2`

- 场景：两个 Raw 设备、两个工作负载
- 重点：多个 `sd`、多个 `wd`
- 适合什么时候看：
  - 当你已经不满足于单对象测试

### `example3`

- 场景：多个 `RD`
- 重点：同一组工作负载如何分多轮执行
- 建议后续：
  - [命令行参数详解](execution-parameter-detail.md)
  - 重点看 `-r`、`-l`

### `example4`

- 场景：曲线与不同 `xfersize`
- 重点：`iorate=curve`、`forx`
- 建议后续：
  - [Raw I/O 参数详解](raw-parameter-detail.md)
  - [常见场景](scenarios.md)

### `example5`

- 场景：多主机 Raw I/O
- 重点：主机与设备映射
- 建议后续：
  - [常见场景](scenarios.md)
  - [实用命令详解](utility-functions-detail.md)

### `example6`

- 场景：回放与跟踪相关
- 重点：不是新手第一优先
- 建议：
  - 先把基础压测和结果解读做熟，再回头看

### `example7`

- 场景：文件系统最小闭环
- 重点：`fsd`、`fwd`、`rd`
- 建议后续：
  - [File System 参数详解](filesystem-parameter-detail.md)
  - [脚本模板](script-templates.md)

## 第三层：`examples/raw/` 应该怎么分组看

### 先看这几类

- `random_read`
- `random_write`
- `random_rw`
- `seq_read`
- `seq_write`
- `seq_rw`

它们最适合帮助你建立下面这组最核心的认知：

- 随机 vs 顺序
- 读 vs 写 vs 混合
- `iorate=max` 时的基础行为

### 再看 `*curve`

- `random_read_curve`
- `random_write_curve`
- `random_rw_curve`
- `seq_read_curve`
- `seq_write_curve`
- `seq_rw_curve`

这些样例适合你已经能回答下面这些问题之后再看：

- 为什么不能只看一次 `max`
- 为什么还要看曲线和时延变化

### 再看 `*xfersizes`

- `random_read_xfersizes`
- `random_write_xfersizes`
- `random_rw_xfersizes`
- `seq_read_xfersizes`
- `seq_write_xfersizes`
- `seq_rw_xfersizes`

这些样例适合你已经开始关心：

- 块大小对吞吐和时延的影响
- 同一设备在不同 `xfersize` 下的工作点

### 最后看

- `multi_host`
- `tpcc`

它们更适合作为专题阅读，而不是入门第一批样例。

## 第四层：`examples/filesys/` 应该怎么分组看

### 先看这几类

- `create_files`
- `random_read`
- `random_write`
- `random_rw`
- `seq_read`
- `seq_write`
- `seq_rw`

这组样例最适合建立：

- 文件结构怎么生成
- 文件 I/O 怎么组织
- `format=yes` 的基本作用

### 再看

- `delete_files`

适合在你已经理解了：

- 文件结构
- 控制文件
- 删除类操作和正常读写不是同一种工作负载

### 最后看

- `multi_host`

它更适合你已经掌握：

- 单机文件系统压测
- `shared=yes`
- 共享目录和格式化策略

## 教程页和样例的映射

### 想学最小闭环

- 样例：
  - `example1`
  - `example7`
- 教程：
  - [快速开始](../zero/quickstart.md)
  - [结果分析](../zero/results-analysis.md)

### 想学参数规则

- 样例：
  - `example1`
  - `example7`
  - `random_read`
  - `random_rw`
- 教程：
  - [参数文件基础](parameter-file-basics.md)
  - [Raw I/O 参数详解](raw-parameter-detail.md)
  - [File System 参数详解](filesystem-parameter-detail.md)

### 想学曲线压测

- 样例：
  - `examples/raw/*curve`
- 教程：
  - [Raw I/O 参数详解](raw-parameter-detail.md)
  - [常见场景](scenarios.md)

### 想学多主机

- 样例：
  - `examples/raw/multi_host`
  - `examples/filesys/multi_host`
- 教程：
  - [常见场景](scenarios.md)
  - [实用命令详解](utility-functions-detail.md)

### 想直接开始写自己的脚本

- 样例：
  - `example1`
  - `example7`
- 教程：
  - [脚本模板](script-templates.md)

## 最实用的阅读方法

推荐这样读，而不是在参考包里顺着文件名逐个看：

1. 先选一个最接近自己场景的样例
2. 看懂它最小能跑的参数
3. 去教程对应页面补参数含义
4. 回到 [脚本模板](script-templates.md)，写自己的第一份版本
5. 跑完后看 [结果分析](../zero/results-analysis.md)

## 下一步

- 想直接复制改模板：看 [脚本模板](script-templates.md)
- 想继续学参数：看 [参数文件详解](parameter-file-detail.md)
- 想继续学结果：看 [结果分析](../zero/results-analysis.md)
