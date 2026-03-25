# Repository Guidelines

## Project Structure & Module Organization
This repository contains the source for the MkDocs Material site for *A Byte of Vdbench*. Published content lives under `docs/`. Start pages are `docs/index.md` and `docs/zh-cn/README.md`. Topic pages are grouped under `docs/zh-cn/zero/` and `docs/zh-cn/hero/`. Planning notes in `docs/superpowers/` are excluded from the built site by `mkdocs.yml`. Site configuration lives in `mkdocs.yml`; Python dependencies are pinned in `requirements.txt`.

## Build, Test, and Development Commands
Create a local environment before editing:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

Use `mkdocs serve` to preview at `http://127.0.0.1:8000`. Use `mkdocs build` to generate the static site into `site/`; this is the same build step used in CI and GitHub Pages deployment. Do not commit `site/` or `.venv/`.

## Coding Style & Naming Conventions
Keep Markdown concise and instructional, matching the repository’s Chinese-first content. Use ATX headings (`#`, `##`), fenced code blocks with a language tag such as `shell`, and relative links between pages. New filenames should stay lowercase and hyphenated, for example `new-topic.md`. If you add, rename, or move a page, update the `nav:` section in `mkdocs.yml`. Use two-space indentation in YAML.

## Testing Guidelines
There is no separate automated test suite. Treat a clean `mkdocs build` as the required validation for every change. When editing navigation, links, or Markdown extensions, also run `mkdocs serve` and click through the affected pages to catch broken links, bad formatting, or missing assets before opening a PR.

## Commit & Pull Request Guidelines
Recent history favors short, focused subjects such as `docs: migrate site to mkdocs` and `docs: align site url with github pages domain`. Prefer concise imperative summaries, usually with a `docs:` prefix for content or site-config changes. Keep pull requests small, describe the pages or config touched, and link any related issue. Include screenshots when layout, theme, or navigation changes affect rendered pages.
