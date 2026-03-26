# 阅读指南

## 这本教程适合谁

- 没接触过 `Vdbench`，但需要尽快开展块设备或文件系统压测的人
- 已经能跑基础脚本，但对参数文件、报告、场景编排还不够熟悉的人
- 想结合官方参考包和源码理解 `Vdbench` 行为的人

## 阅读顺序

如果你是第一次使用 `Vdbench`，建议按下面的顺序阅读：

1. [介绍](zero/introduction.md)
2. [快速开始](zero/quickstart.md)
3. [结果分析](zero/results-analysis.md)
4. [参数文件详解](hero/parameter-file-detail.md)
5. [脚本模板](hero/script-templates.md)
6. [官方样例索引](hero/example-map.md)
7. 再按自己的场景选择 `Raw I/O`、`File System`、`命令行参数`、`常见场景`、`实用工具`

如果你已经跑通过 `example1` 或 `example7`，可以直接从进阶营开始查阅。

## 使用前提

- 本教程当前以 `Vdbench 5.04.07` 为基础
- 官方 `readme.txt` 标注的最低 `Java` 要求是 `1.7.0`
- 实际使用中建议至少准备 `Java 1.8`
- 新手应优先在测试环境、临时文件或无业务数据的块设备上练习

## 风险提示

!!! warning
    `Vdbench` 会真实执行读写、创建、删除等操作。请不要在系统盘、业务盘或存有重要数据的目录上直接试验样例。

!!! tip
    在正式执行前，优先使用 `-t`、`-tf` 验证环境，再结合 `-s` 模拟参数文件是否符合预期。

## 参考资料说明

仓库内置了两类高价值参考资料：

- `reference/vdbench50407/`
  - 官方发布包
  - 包含 `example1~7`、`examples/raw/`、`examples/filesys/`、`vdbench.pdf`
- `reference/vdbench50407-src/`
  - 参考源码
  - 可用于确认参数解析、命令分发、工具入口和部分行为细节

## 本教程的使用方式

本教程不是逐字翻译官方手册，而是结合三类信息整理而成：

- 官方示例
- 实际执行观察
- 关键源码入口

因此，某些页面更适合“先理解思路再回头查细节”，而不是从头到尾顺读。

## License

本书遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 协议。

- 您可以分享（复制、分发和传播）本书
- 您可以对本书进行更改（尤其是翻译）
- 您可以将其用于商业目的

!!! warning
    请勿出售本书的电子版或印刷版，除非您明确指出相关内容并非来自本书。

    如有对书中内容的引用，请注明出处，非常感谢。
