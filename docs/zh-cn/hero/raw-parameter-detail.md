# Raw I/O 参数详解

## 适用场景

当你要测试的是下面这些对象时，通常使用 `SD/WD/RD` 这一套：

- 块设备
- 裸盘
- 大文件
- 希望把文件当成“设备”来模拟 Raw I/O 的场景

## 参数顺序

```shell
# 推荐顺序
# General -> HD -> SD -> WD -> RD
```

## `SD`: Storage Definition

`SD` 用于描述“被测对象”。

最常见的参数有：

- `sd=name`
  - 定义唯一名字
- `lun=`
  - 指定设备或文件
- `size=`
  - 文件型测试对象通常需要显式指定大小
- `openflags=`
  - 对 `/dev/xxx` 场景尤其重要
- `count=(1,n)`
  - 批量生成一组 `SD`

### `sd=default`

这是最常见的写法之一，用来给后续所有 `SD` 提供默认值。

```shell
sd=default,openflags=o_direct
sd=sd1,lun=/dev/sdb
sd=sd2,lun=/dev/sdc
```

### `lun=` 的两个常见分支

#### 设备

```shell
sd=sd1,lun=/dev/sdb,openflags=o_direct
```

适合真实块设备。

#### 文件

```shell
sd=sd1,lun=/data/vdbench/testfile,size=10g
```

适合：

- 不方便直接打块设备
- 想先在普通文件上练习
- 在共享存储上做受控实验

!!! warning
    当 `lun` 是普通文件路径时，文件所在目录必须已经存在。`Vdbench` 会创建文件，但不会帮你创建路径中的目录。

### `openflags=`

如果 `lun` 是 `/dev/xxx` 这类设备路径，通常要显式指定 `openflags=o_direct` 或等价模式，避免把缓存行为误当成设备性能。

### `count=(1,n)`

如果你要一次生成一组测试对象，`count=` 很有用：

```shell
sd=sd,lun=/data/testfile%04d,count=(1,5),size=20m
```

这通常会生成：

- `sd1 ~ sd5`
- `testfile0001 ~ testfile0005`

非常适合：

- 多文件模拟多盘
- 多对象并行
- 曲线和线程实验

### `size=`

对文件型测试对象尤其重要。

经验上要注意三件事：

1. 如果是普通文件，通常要显式指定 `size=`
2. `size` 太小会导致工作负载根本无法成立
3. 当 `size` 小于 `xfersize` 的若干倍时，可能直接报 `Insufficient amount of lun size specified`

换句话说，第一次写脚本时，不要为了“快一点”把 `size` 压得过小。

### `formatsds=yes`

这是一个很有用但也很容易误解的通用参数。

它的核心作用是：

- 在正式负载前对 `SD` 做预格式化
- 让后续负载不至于打在一组“逻辑存在但实际上没被完整写过”的对象上

旧版实测里有一个很重要的观察：

- `formatsds=yes` 并不总等于“删掉旧文件重建”
- 在文件型 `SD` 场景里，它常常表现为覆盖式写入和补齐，而不是简单删除重建

因此：

- 把它当成“格式化准备阶段”
- 不要把它简单理解为“rm 后重新创建”

## `WD`: Workload Definition

`WD` 描述“对 `SD` 做什么 I/O”。

最常见参数：

- `sd=`
- `rdpct=`
- `xfersize=`
- `seekpct=`
- `threads=`
- `skew=`

### 最常见的组合

```shell
wd=wd1,sd=sd*,seekpct=random,rdpct=100,xfersize=8k
```

这表示：

- 随机
- 纯读
- 8K

### `rdpct=`

经验上最值得先记的只有三个值：

- `100`
  - 纯读
- `0`
  - 纯写
- `80`
  - 一个很直观的混合读写比例示例，接近 4:1

如果没写 `rdpct=`，默认通常按纯读理解。

### `seekpct=`

- `0` 或 `seq`
  - 顺序
- `100` 或 `random`
  - 随机

对大多数压测场景来说，先把它简化成两类就够了：

- 顺序：`0` / `seq`
- 随机：`100` / `random`

其余中间值、`eof`、`poisson` 之类更适合在明确需要时再研究。

### `xfersize=`

最常见的是单值：

```shell
xfersize=4k
xfersize=8k
xfersize=1m
```

也可以写成分布：

```shell
xfersize=(4k,10,8k,10,16k,80)
```

还可以写成随机范围：

```shell
xfersize=(4k,8k,1k)
```

第一次做性能对比时，优先使用固定值，更容易解释结果。

## `RD`: Run Definition

`RD` 描述“怎么执行这组工作负载”。

最常见参数：

- `wd=`
- `iorate=`
- `elapsed=`
- `interval=`
- `warmup=`
- `threads=`

### `iorate=`

你可以把它理解为“把负载压到多快”。

常见用法：

```shell
rd=rd1,wd=wd1,iorate=100,elapsed=30,interval=5
rd=rd1,wd=wd1,iorate=max,elapsed=30,interval=5
rd=rd1,wd=wd1,iorate=curve,elapsed=30,interval=5
```

- `iorate=100`
  - 限速运行
- `iorate=max`
  - 不控速，尽可能打满
- `iorate=curve`
  - 先找峰值，再按比例跑曲线

### `iorate` 的三个常用层次

#### 固定速率

```shell
iorate=100
iorate=(10,50,100)
iorate=(100-1000,100)
```

适合回答“在受控速率下表现如何”。

#### 峰值

```shell
iorate=max
```

适合先粗看上限，但不要把它直接等同于“稳定工作点”。

#### 曲线

```shell
iorate=curve
curve=(5-100,50)
stopcurve=0.1
```

适合回答：

- 峰值在哪里
- 哪个速率区间最值得工作
- 时延从什么时候开始明显变差

一个很有价值的经验结论是：

- `curve` 的基准峰值和 `threads` 强相关
- `stopcurve=` 可以把“超过某个平均时延就停止后续压测”这件事自动化

## 几个高价值的实测结论

### 1. 先顺序，再复杂

最容易解释的起点组合仍然是：

```shell
seekpct=seq
rdpct=0
xfersize=8k
iorate=100
```

### 2. `size` 不要压得过小

`size` 太小时，连 workload 上下文都可能算不出来，错误并不是“性能差”，而是“配置不成立”。

### 3. `curve` 比单次 `max` 更适合做决策

如果你是为了给设备选工作点、给业务做限流建议、或者评估时延拐点，优先跑 `curve`，而不是只跑一次 `iorate=max`。

## 推荐的第一份 Raw 脚本

```shell
messagescan=no

sd=sd1,lun=/dev/sdb,openflags=o_direct
wd=wd1,sd=sd1,xfersize=4k,rdpct=100,seekpct=random
rd=rd1,wd=wd1,iorate=100,elapsed=10,interval=1
```

如果你不方便直接使用设备，可以先改成文件：

```shell
messagescan=no

sd=sd1,lun=/tmp/vdbench.raw,size=10g
wd=wd1,sd=sd1,xfersize=4k,rdpct=100,seekpct=random
rd=rd1,wd=wd1,iorate=100,elapsed=10,interval=1
```

## 官方样例怎么用

优先从这些样例开始：

- `example1`
  - 单设备最小闭环
- `example2`
  - 多设备、多工作负载
- `example3`
  - 多个 `RD`
- `examples/raw/random_read`
  - 随机读
- `examples/raw/random_rw`
  - 混合随机读写
- `examples/raw/seq_read`
  - 顺序读
- `examples/raw/*curve`
  - 曲线压测

如果你已经跑通过 `example1`，建议下一步直接跳到：

- `examples/raw/random_read`
- `examples/raw/random_rw`
- `examples/raw/seq_read_curve`
- `examples/raw/random_rw_curve`

## 常见误区

### 误区 1：把系统盘当试验盘

不要这么做。

### 误区 2：只改 `threads`，不理解 `iorate`

`threads` 决定了并发度上限，`iorate` 决定了控速目标。两者一起决定最终压力形态。

### 误区 3：只看平均值，不看曲线

如果你要给设备定“可接受工作点”，请至少跑一次 `iorate=curve`。

### 误区 4：不核对结果目录中的参数文件

跑完后至少回头看：

- `summary.html`
- `totals.html`
- `parmfile.html`
- `parmscan.html`

## 继续阅读

- [脚本模板](script-templates.md)
- [命令行参数详解](execution-parameter-detail.md)
- [常见场景](scenarios.md)
- [结果分析](../zero/results-analysis.md)
