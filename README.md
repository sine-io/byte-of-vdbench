# byte-of-vdbench

《简明 Vdbench 教程》的 `MkDocs Material` 文档站源码。

## 文档结构

站点当前按“导读 -> 新手村 -> 进阶营 -> 附录”组织：

- `docs/index.md`
  - 首页
- `docs/zh-cn/README.md`
  - 阅读指南
- `docs/zh-cn/zero/`
  - 新手村页面，如介绍、快速开始、结果分析
- `docs/zh-cn/hero/`
  - 进阶营页面，如参数基础、Raw I/O、File System、脚本模板、样例索引、工具说明
- `docs/superpowers/`
  - 设计和计划文档，不参与站点发布

如果新增、重命名或删除页面，请同步更新 `mkdocs.yml` 的 `nav`。

## 本地预览

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
mkdocs serve
```

默认访问地址：`http://127.0.0.1:8000`

## 本地构建

```bash
source .venv/bin/activate
mkdocs build
```

建议在文档结构、导航或链接调整后都执行一次 `mkdocs build`。

## GitHub Pages

推送到 `main` 分支后，`.github/workflows/deploy.yml` 会自动构建并发布到 `GitHub Pages`。

默认发布地址：
`https://www.sineio.top/byte-of-vdbench/`
