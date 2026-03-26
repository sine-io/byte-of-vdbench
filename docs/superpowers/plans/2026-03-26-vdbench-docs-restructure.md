# Vdbench Docs Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure the MkDocs site into a complete Vdbench tutorial with a clearer learning path, finished beginner flow, topic-based advanced sections, and a clean build.

**Architecture:** Keep the existing Markdown corpus as the source of truth, but split the current oversized parameter page into topic pages, replace placeholder pages with finished content, add a dedicated report-analysis page, and realign `mkdocs.yml` navigation to reflect the tutorial path. Clean obvious PDF-export residue and ensure `mkdocs build` completes without the current broken-anchor warnings.

**Tech Stack:** Markdown, MkDocs, MkDocs Material

---

### Task 1: Rework the site navigation and entry pages

**Files:**
- Modify: `mkdocs.yml`
- Modify: `docs/index.md`
- Modify: `docs/zh-cn/README.md`

- [ ] **Step 1: Rewrite the reader guide page**

Convert `docs/zh-cn/README.md` from a license-only page into a proper tutorial guide covering audience, risk warnings, environment prerequisites, reading order, and reference-material notes. Keep the license section, but move it to the end.

- [ ] **Step 2: Tighten the homepage**

Update `docs/index.md` so it links to the new reader guide, beginner path, and advanced path without duplicating long prose from inner pages.

- [ ] **Step 3: Replace the current navigation structure**

Update `mkdocs.yml` so the navigation reflects:
- 首页
- 阅读指南
- 新手村
- 进阶营
- 附录

Do not keep empty or placeholder pages in `nav`.

### Task 2: Complete the beginner learning loop

**Files:**
- Modify: `docs/zh-cn/zero/introduction.md`
- Modify: `docs/zh-cn/zero/quickstart.md`
- Create: `docs/zh-cn/zero/results-analysis.md`

- [ ] **Step 1: Fix the introduction page ending**

Remove the dangling `见xxx` ending from `introduction.md` and close the page with concrete guidance toward the quickstart and reference package.

- [ ] **Step 2: Refocus quickstart on installation and first runs**

Keep the existing installation, `-t/-tf`, `example1`, and `example7` walkthroughs, but move report interpretation content out of `quickstart.md` into a dedicated page. Update links accordingly.

- [ ] **Step 3: Add a dedicated results-analysis page**

Create `docs/zh-cn/zero/results-analysis.md` and explain the first reports a beginner should inspect:
- `summary.html`
- `totals.html`
- `flatfile.html`
- `histogram.html`
- `parmfile.html`
- `parmscan.html`

Use file names already shown in `quickstart.md` as anchors for the explanation.

### Task 3: Split the giant parameter page into topic-based advanced pages

**Files:**
- Create: `docs/zh-cn/hero/parameter-file-basics.md`
- Create: `docs/zh-cn/hero/raw-parameter-detail.md`
- Create: `docs/zh-cn/hero/filesystem-parameter-detail.md`
- Modify: `docs/zh-cn/hero/parameter-file-detail.md`

- [ ] **Step 1: Extract shared basics**

Create `parameter-file-basics.md` and move in the parameter-file rules that serve as prerequisites:
- abbreviations
- comments
- size/time formats
- multi-value syntax
- `include=`
- parameter ordering
- `parmfile.html` and `parmscan.html`

- [ ] **Step 2: Extract Raw I/O sections**

Create `raw-parameter-detail.md` and move the `SD/WD/RD` sections used for raw/block-device workloads into that page, preserving the existing examples and source-code-backed conclusions.

- [ ] **Step 3: Extract File System sections**

Create `filesystem-parameter-detail.md` and move the `FSD/FWD/RD` file-system sections into that page, keeping the current shared-anchor, formatting, totalsize, and `forxxx` coverage.

- [ ] **Step 4: Turn the old giant page into a redirect-style index**

Reduce `parameter-file-detail.md` to a short overview page that explains the split and links readers to the three topic pages instead of leaving a 5000-line monolith in the main flow.

### Task 4: Replace placeholder advanced pages with finished content

**Files:**
- Modify: `docs/zh-cn/hero/execution-parameter-detail.md`
- Create: `docs/zh-cn/hero/scenarios.md`
- Modify: `docs/zh-cn/hero/utility-functions-detail.md`
- Modify: `docs/zh-cn/hero/appendix.md`
- Delete or repurpose: `docs/zh-cn/hero/others.md`

- [ ] **Step 1: Tighten execution-parameter coverage**

Keep the useful, source-backed sections in `execution-parameter-detail.md`, but compress obvious placeholders and remove empty subheadings that are not supported by real guidance in this round.

- [ ] **Step 2: Add a scenarios page**

Create `scenarios.md` to gather cross-cutting topics that currently do not fit well as single parameters, such as:
- curve testing
- multi-host setup
- formatting strategies
- shared file systems
- safe dry-run habits

- [ ] **Step 3: Rewrite the utility-functions page**

Replace the empty headings in `utility-functions-detail.md` with usable summaries for tools such as:
- `compare`
- `parse`
- `printjournal`
- `showlba`
- `rsh`
- `jstack`
- `csim`
- `dsim`
- `sds`

Distinguish GUI-heavy tools from command-line tools and use the reference source usage text where available.

- [ ] **Step 4: Rebuild the appendix**

Rewrite `appendix.md` into a source-map style appendix:
- reference package contents
- useful source entry points
- where command dispatch and parameter scanning happen

- [ ] **Step 5: Remove the empty catch-all page**

Delete `others.md` or fold any remaining useful content into `scenarios.md` or `appendix.md`, then remove it from navigation.

### Task 5: Clean PDF residue and verify the site

**Files:**
- Modify: affected Markdown files under `docs/zh-cn/`
- Test: `mkdocs build`

- [ ] **Step 1: Remove broken `_bookmark` references**

Replace the current PDF-export anchor links with local section links or plain prose so `mkdocs build` no longer reports broken anchors.

- [ ] **Step 2: Normalize obvious HTML emphasis**

Replace high-visibility `<font>` emphasis with Markdown emphasis or admonitions where that improves readability, prioritizing the pages touched during this restructure.

- [ ] **Step 3: Build and inspect**

Run: `mkdocs build`

Expected:
- build succeeds
- the previous broken-anchor warnings are gone
- new pages are included in the site

- [ ] **Step 4: Review the final diff**

Run: `git status --short`

Expected:
- only intended documentation and configuration files changed
- no generated `site/` content is staged
