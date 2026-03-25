# byte-of-vdbench

《简明 Vdbench 教程》的 `MkDocs Material` 文档站源码。

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

## GitHub Pages

推送到 `main` 分支后，`.github/workflows/deploy.yml` 会自动构建并发布到 `GitHub Pages`。

默认发布地址：
`https://sine-io.github.io/byte-of-vdbench/`
