# MkDocs Migration Design

## Goal

将当前基于 `docsify` 的教程站点改造成基于 `MkDocs Material` 的静态站点，并补充可直接部署到 `GitHub Pages` 的 `GitHub Actions` 工作流。

## Current State

- 文档正文已经是 Markdown，主要位于 `docs/zh-cn/`
- 站点入口依赖 `docs/index.html`
- 导航依赖 `docs/_sidebar.md`
- 仓库中没有 `MkDocs` 配置、依赖声明或 Pages 工作流

## Design

### Documentation Structure

- 保留 `docs/` 作为 MkDocs 的 `docs_dir`
- 新增 `docs/index.md` 作为首页
- 保留 `docs/zh-cn/` 现有教程内容
- 删除 `docsify` 专属入口文件和侧边栏文件

### Site Configuration

- 新增 `mkdocs.yml`
- 使用 `mkdocs-material`
- 通过 `nav` 明确定义导航结构，复刻当前文档分组
- 启用搜索、代码复制、章节导航和中文界面

### Content Adaptation

- 将 `docsify` 的 `!>` 提示块迁移到 MkDocs admonition 语法
- 修正少量依赖 docsify 路径解析的相对链接
- 其余正文尽量不改，降低迁移风险

### Deployment

- 新增 `requirements.txt` 供本地和 CI 共用
- 新增 `.github/workflows/deploy.yml`
- `pull_request` 触发构建校验
- `main` 分支推送后自动构建并部署到 `GitHub Pages`

## Risks

- 旧文档里存在少量 docsify 专属语法，需要逐步消除
- 如果仓库的 Pages Source 未使用 `GitHub Actions`，首次发布前需要在仓库设置中确认

## Validation

- 本地执行 `mkdocs build`
- 检查首页、导航和章节页面是否正常生成
- 检查工作流 YAML 语法和依赖安装是否可执行
