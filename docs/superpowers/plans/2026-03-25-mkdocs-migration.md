# MkDocs Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current docsify site with an MkDocs Material site and add GitHub Actions based GitHub Pages deployment.

**Architecture:** Keep the existing Markdown source files under `docs/`, add a single `mkdocs.yml` for navigation and theme configuration, then build and deploy the generated `site/` directory through GitHub Pages. Limit content changes to syntax that is incompatible with MkDocs and to links that depend on docsify behavior.

**Tech Stack:** MkDocs, MkDocs Material, GitHub Actions, GitHub Pages

---

### Task 1: Add MkDocs project files

**Files:**
- Create: `mkdocs.yml`
- Create: `requirements.txt`
- Create: `.gitignore`
- Test: local `mkdocs build`

- [ ] **Step 1: Confirm the current repository does not already build with MkDocs**

Run: `python3 -m mkdocs build`
Expected: fail because `mkdocs` is not installed and the repository is not configured yet.

- [ ] **Step 2: Add the MkDocs configuration and dependencies**

Create `mkdocs.yml` with:
- `docs_dir: docs`
- `site_dir: site`
- `theme.name: material`
- `plugins: [search]`
- explicit `nav` entries matching the current docs structure

Create `requirements.txt` with:
- `mkdocs>=1.6,<2.0`
- `mkdocs-material>=9.6,<10.0`

Create `.gitignore` with:
- `site/`
- `.venv/`
- `__pycache__/`

- [ ] **Step 3: Install dependencies**

Run: `python3 -m pip install -r requirements.txt`
Expected: MkDocs and MkDocs Material install successfully.

### Task 2: Replace docsify entrypoints

**Files:**
- Delete: `docs/index.html`
- Delete: `docs/_sidebar.md`
- Delete: `docs/home.md`
- Create: `docs/index.md`

- [ ] **Step 1: Add the new MkDocs homepage**

Create `docs/index.md` using the existing `docs/home.md` content as the base page.

- [ ] **Step 2: Remove obsolete docsify files**

Delete:
- `docs/index.html`
- `docs/_sidebar.md`
- `docs/home.md`

- [ ] **Step 3: Keep navigation in `mkdocs.yml` only**

Verify that the new home page and navigation no longer depend on docsify runtime files.

### Task 3: Adapt Markdown content for MkDocs

**Files:**
- Modify: `docs/zh-cn/README.md`
- Modify: `docs/zh-cn/zero/quickstart.md`

- [ ] **Step 1: Replace docsify admonitions**

Convert each `!>` block into MkDocs admonition syntax such as:

```md
!!! warning
    message
```

- [ ] **Step 2: Fix relative links that relied on docsify routing**

Update `docs/zh-cn/zero/quickstart.md` links:
- `zh-cn/hero/execution-parameter-detail.md` -> `../hero/execution-parameter-detail.md`
- `zh-cn/hero/parameter-file-detail.md` -> `../hero/parameter-file-detail.md`

- [ ] **Step 3: Keep all other tutorial text unchanged**

Only touch content required for compatibility or navigation.

### Task 4: Add GitHub Pages deployment

**Files:**
- Create: `.github/workflows/deploy.yml`
- Modify: `README.md`

- [ ] **Step 1: Add the Pages workflow**

Create `.github/workflows/deploy.yml` with:
- trigger on `push` to `main`
- trigger on `pull_request` for build validation
- Python setup
- dependency installation from `requirements.txt`
- `mkdocs build`
- Pages artifact upload
- deploy job using `actions/deploy-pages`

- [ ] **Step 2: Document local preview and deployment**

Update `README.md` with:
- local install command
- local serve command
- local build command
- GitHub Pages publish behavior

### Task 5: Verify the migration

**Files:**
- Test: repository root build output

- [ ] **Step 1: Run the build after adding configuration**

Run: `mkdocs build`
Expected: site is generated under `site/` without fatal errors.

- [ ] **Step 2: Inspect generated output**

Confirm that:
- homepage exists
- navigation pages render
- no docsify files are required anymore

- [ ] **Step 3: Review the git diff**

Run: `git status --short`
Expected: only the intended migration files are changed.
