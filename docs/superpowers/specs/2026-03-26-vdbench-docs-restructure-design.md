# Vdbench Tutorial Restructure Design

## Goal

将当前 `MkDocs` 站点从“可浏览的资料集合”重构为“有明确学习路径的教程站点”，同时提升站点完整度、导航一致性、构建质量和后续维护成本。

## Current State

- 首页只有少量入口链接，无法承担课程导读职责
- `docs/zh-cn/README.md` 目前只包含许可证信息，不是教程导读页
- 新手村的 `quickstart.md` 有实际操作示例，但“结果分析”章节未完成
- 进阶营内容成熟度不均：
  - `parameter-file-detail.md` 信息量大且包含大量实测和源码对照
  - `execution-parameter-detail.md` 部分章节仍是占位
  - `utility-functions-detail.md`、`others.md`、`appendix.md` 基本未完成
- `parameter-file-detail.md` 中保留了 PDF 导出的 `_bookmark` 链接，`mkdocs build` 会产生失效锚点 warning
- 正文中混用了较多 HTML `font` 标签，不利于 Markdown 维护

## Design

### Information Architecture

- 保持 `docs/index.md` 为简短首页，只负责介绍教程定位和主要入口
- 将 `docs/zh-cn/README.md` 改写为真正的“阅读指南”页面，覆盖：
  - 适用人群
  - 使用风险
  - 环境前提
  - 学习顺序
  - 参考资料说明
- 将新手村组织为完整闭环：
  - `介绍`
  - `快速开始`
  - `结果分析`
- 将进阶营重构为按主题组织的页面，而不是让一个超长页面承担全部角色：
  - `参数文件基础`
  - `Raw I/O 参数详解`
  - `File System 参数详解`
  - `命令行参数详解`
  - `常见场景`
  - `实用工具`
- 将 `附录` 收敛为辅助信息：
  - 源码入口
  - 参考资料
  - 阅读建议

### Content Strategy

- 保留现有高价值内容，特别是 `parameter-file-detail.md` 中基于实测和源码的分析
- 不保留“空章节作为正式导航项”的做法
- 不能在本轮完成验证的内容不再占据主流程章节：
  - 能压缩成简短提示的，保留为“待补充”说明块
  - 无有效正文支撑的，移出主导航
- 优先将现有内容重组成“从入门到进阶”的可读路径，而不是追求一次补齐所有参数

### Page-Level Changes

- 新增 `docs/zh-cn/zero/results-analysis.md`
  - 承接 `quickstart.md` 中已出现的报告文件名
  - 解释 `summary.html`、`totals.html`、`flatfile.html`、`histogram.html`、`parmfile.html`、`parmscan.html`
- 新增 `docs/zh-cn/hero/parameter-file-basics.md`
  - 从原 `parameter-file-detail.md` 中抽取通用规则、注释、缩写、变量、`include=`、顺序规则等基础内容
- 新增 `docs/zh-cn/hero/raw-parameter-detail.md`
  - 从原 `parameter-file-detail.md` 中抽取 `SD/WD/RD` 的 Raw I/O 部分
- 新增 `docs/zh-cn/hero/filesystem-parameter-detail.md`
  - 从原 `parameter-file-detail.md` 中抽取 `FSD/FWD/RD` 的 File System 部分
- 新增 `docs/zh-cn/hero/scenarios.md`
  - 将可形成独立认知单元的内容整理为场景页，如曲线压测、多主机、格式化行为、共享目录、常见误用
- 重写 `docs/zh-cn/hero/utility-functions-detail.md`
  - 依据参考源码中的命令入口和 usage，区分 GUI 工具和 CLI 工具
- 重写 `docs/zh-cn/hero/appendix.md`
  - 不再放半成品源码解析，改成“源码入口地图”和参考包说明
- 删除或合并 `docs/zh-cn/hero/others.md`
  - 该页面不再作为独立导航项存在

### Content Hygiene

- 替换失效的 `_bookmark` 链接，改为：
  - 站内真实标题链接
  - 或普通文本说明
- 将高频 `<font>` 强调替换为：
  - 正常强调语法
  - `warning/info/tip` admonition
  - 更清晰的小节标题
- 保留必要的源码片段，但减少“只给类名、不解释结论”的片段
- 将“见xxx”“待研究”一类占位文本改成明确状态：
  - 删除
  - 简要说明
  - 迁移到附录或待办区

### Navigation

- 更新 `mkdocs.yml` 的 `nav`，使结构反映课程路径而不是文件历史
- 不在导航中暴露空页或明显未完成页

## Risks

- `parameter-file-detail.md` 体量大，拆分时容易破坏已有相对链接或造成内容遗漏
- 原页面中部分结论依赖上下文，拆页时需要补充必要前置说明
- 大量内容来自实测记录，拆分后必须保留原有示例的因果链，不然会降低说服力

## Validation

- 运行 `mkdocs build`
- 确认导航与新页面均可正常渲染
- 确认站内链接不再指向失效 `_bookmark`
- 检查新手路径是否形成闭环：
  - 导读 -> 快速开始 -> 结果分析 -> 进阶专题
