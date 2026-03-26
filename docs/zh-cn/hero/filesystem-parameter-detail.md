# File System 参数详解

## 适用场景

当你要测试的是目录、文件树和文件操作时，通常使用 `FSD/FWD/RD` 这一套。

典型场景包括：

- 文件读写
- 创建、删除、重命名
- 元数据操作
- 多目录、多文件规模模拟

## 参数顺序

```shell
# 推荐顺序
# General -> HD -> FSD -> FWD -> RD
```

## `FSD`: File System Definition

`FSD` 描述文件结构。

最常见参数：

- `fsd=name`
- `anchor=`
- `depth=`
- `width=`
- `files=`
- `sizes=`
- `shared=`

### `fsd=default`

和 `SD` 一样，`fsd=default` 很适合先定义一组目录结构默认值，再由多个具体 `FSD` 继承。

```shell
fsd=default,depth=2,width=2,files=2,size=128k
fsd=fsd1,anchor=/data/dir001
fsd=fsd2,anchor=/data/dir002
```

### `anchor=`

它表示测试根目录。

要点：

- 上级目录如果不存在，通常需要 `create_anchors=yes`
- 不同 `FSD` 的 `anchor` 不应互为父子目录
- 正式执行前，请确认该目录里没有重要数据

旧版源码对照和实测里有两个很关键的结论：

1. 不同 `FSD` 的 `anchor` 如果互为父子目录，会直接失败
2. `anchor` 末尾带文件分隔符时，某些父子目录校验可能更难看清，写脚本时最好不要主动在末尾补 `/`

### `depth` / `width` / `files`

这三个参数共同决定目录树规模。

简单理解：

- `depth`
  - 目录层级
- `width`
  - 每层分叉数
- `files`
  - 每个目录下的文件数

不要一上来就把它们设得很大，否则会很快产生大量目录和文件。

其中最容易失控的是 `files=`。第一次试验时，建议先从非常小的目录树开始，例如：

```shell
depth=1,width=1,files=10
```

### `sizes=`

定义文件大小，可以是单值，也可以是多值组合。

```shell
sizes=128k
sizes=(32k,50,64k,50)
```

### `shared=yes`

当多个主机共享同一个文件结构时才需要。

这种场景通常还要同时重新思考：

- `format=`
- 控制文件
- 文件结构是否需要预先准备

还有一个必须记住的限制：

- `shared=yes` 场景下，不能直接配 `format=yes`
- 通常需要拆成两步：
  - `format=(clean,only)`
  - `format=(restart,only)`

## `FWD`: File Workload Definition

`FWD` 描述对文件做什么操作。

常见参数：

- `fsd=`
- `operation=`
- `fileio=`
- `fileselect=`
- `xfersizes=`
- `threads=`
- `rdpct=`

### `operation=`

最常见：

- `read`
- `write`
- `create`
- `delete`
- `mkdir`
- `rmdir`
- `access`
- `getattr`
- `setattr`

如果你只是想先看最基础、最稳妥的文件系统压测，优先使用：

- `read`
- `write`

像 `mkdir`、`delete`、`access`、`getattr` 这类更偏元数据行为，适合单独成组观察。

### `fileio=`

常见模式：

- `random`
- `sequential`

对大多数人来说，先把它理解成下面两类就够了：

- `random`
  - 随机文件 I/O
- `sequential`
  - 顺序文件 I/O

`(random,shared)`、`sequentialnz`、`(seq,delete)` 这类变体，等你明确需要时再启用。

### `fileselect=`

常见模式：

- `random`
- `sequential`

文件选择方式和文件内部的 I/O 方式不是一回事，这一点要分清。

一个很常见的组合是：

- `fileio=sequential`
- `fileselect=random`

意思不是“随机块读”，而是“随机选文件，再顺序读该文件”。

## `RD`: Run Definition

文件系统测试里，`RD` 最重要的通常是：

- `fwd=`
- `fwdrate=`
- `format=`
- `elapsed=`
- `interval=`

### `fwdrate=`

它表示文件系统操作速率。

常见形式：

```shell
fwdrate=100
fwdrate=max
fwdrate=curve
fwdrate=(100,200,300)
```

可以把它理解成文件系统版本的 `iorate=`。

### `format=`

这是文件系统测试里非常关键、也非常容易误用的参数。

最常见形式：

- `format=yes`
  - 先创建或重建文件结构，再执行测试
- `format=no`
  - 假设文件结构已存在并可直接使用
- `format=restart`
  - 重用已有结构，但前提是结构与当前 `FSD` 定义一致
- `format=only`
  - 只做预埋，不执行正式 `RD`

再补几个容易踩坑的点：

- `format=no`
  - 前提最苛刻，要求文件结构已经存在且与当前 `FSD` 定义一致
- `format=yes`
  - 最适合从零开始，但不适合想重用旧结构的场景
- `format=restart`
  - 看起来最省事，但前提是你没有手动破坏过原有结构
- `format=only`
  - 很适合把“预埋数据”和“正式压测”分两步做

!!! warning
    如果你还没完全搞清楚现有文件结构和本次 `FSD` 定义是否一致，优先显式使用 `format=yes` 或 `format=only` 做一次受控准备。

## 几个高价值的实测结论

### 1. `anchor` 设计错误，比性能参数错误更致命

目录冲突、父子目录冲突、共享目录误用，都会让测试在开始前就失去意义。

### 2. `shared=yes` 不是“多机版 yes”

它会改变控制文件和格式化逻辑，必须把文件结构准备过程单独设计。

### 3. `threads` 可能被自动调整

当一个 `FWD` 覆盖多个 `FSD`、多个操作或多个主机时，线程分配可能出现“你写了 5，实际用了 10”的情况。需要结合结果里的 Note 一起看。

### 4. `format=restart` 不是万能修复键

它适合重用结构，但不适合修复你手动删改过一部分目录树之后的混乱状态。

## 推荐的第一份 File System 脚本

```shell
create_anchors=yes
messagescan=no

fsd=fsd1,anchor=/tmp/vdbench-fs,depth=1,width=1,files=10,size=1m
fwd=fwd1,fsd=fsd1,operation=read,xfersize=4k,fileio=random,fileselect=random,threads=4
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=10,interval=1
```

## 官方样例怎么用

优先从这些样例开始：

- `example7`
  - 最基础的文件系统读测试
- `examples/filesys/create_files`
  - 创建文件结构
- `examples/filesys/random_read`
  - 随机读
- `examples/filesys/random_rw`
  - 随机混合读写
- `examples/filesys/delete_files`
  - 删除与清理
- `examples/filesys/multi_host`
  - 多主机

如果你已经跑通过 `example7`，建议下一步直接去看：

- `examples/filesys/random_read`
- `examples/filesys/random_rw`
- `examples/filesys/delete_files`
- `examples/filesys/multi_host`

## 常见误区

### 误区 1：把 `anchor` 当成普通临时目录

`Vdbench` 会在其中创建控制文件、目录和数据文件。不要直接指到已有业务目录。

### 误区 2：没理解 `format` 就直接重跑

文件系统测试里的很多“结果不对”其实不是性能问题，而是文件结构状态不对。

### 误区 3：共享场景下还沿用单机场景的思路

涉及 `shared=yes`、多主机、共享目录时，要先把文件结构管理方式想清楚，再决定是否 `restart`。

## 继续阅读

- [脚本模板](script-templates.md)
- [命令行参数详解](execution-parameter-detail.md)
- [常见场景](scenarios.md)
- [结果分析](../zero/results-analysis.md)
