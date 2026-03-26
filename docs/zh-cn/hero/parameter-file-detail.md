# 参数文件详解

`Vdbench` 的核心是参数文件。为了避免把所有规则、所有参数、所有实验都堆在一个页面里，本教程将参数内容拆成三部分：

## 先看哪一页

- [参数文件基础](parameter-file-basics.md)
  - 先理解注释、缩写、顺序、`include=`、变量替换、结果目录里的 `parmfile.html` 与 `parmscan.html`
- [Raw I/O 参数详解](raw-parameter-detail.md)
  - 适合块设备、大文件、吞吐与时延压测
- [File System 参数详解](filesystem-parameter-detail.md)
  - 适合目录树、文件创建、读写、元数据操作等场景

## 推荐阅读顺序

如果你刚跑通 `example1` 或 `example7`，建议按下面的顺序看：

1. [参数文件基础](parameter-file-basics.md)
2. 根据场景选择：
   - [Raw I/O 参数详解](raw-parameter-detail.md)
   - [File System 参数详解](filesystem-parameter-detail.md)
3. [命令行参数详解](execution-parameter-detail.md)
4. [常见场景](scenarios.md)

## 官方示例如何映射到这三页

- `example1 ~ example6`
  - 更接近 `Raw I/O`
- `example7`
  - 更接近 `File System`
- `examples/raw/`
  - 更适合在理解 `SD/WD/RD` 后阅读
- `examples/filesys/`
  - 更适合在理解 `FSD/FWD/RD` 后阅读

## 使用参数文件时最容易踩的坑

- 只会抄样例，不确认参数顺序和适用对象
- 在 `/dev/xxx` 上直接试验，忽略写风险
- `format=yes`、`format=restart`、`shared=yes` 混着用，但不知道它们对文件结构的影响
- 看结果只看 `summary.html`，不回头核对 `parmfile.html` 和 `parmscan.html`

## 本页之外还应该看什么

- [命令行参数详解](execution-parameter-detail.md)
- [常见场景](scenarios.md)
- [实用命令详解](utility-functions-detail.md)
