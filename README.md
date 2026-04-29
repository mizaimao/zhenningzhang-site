# zhenningzhang.net

Source for [zhenningzhang.net](https://zhenningzhang.net) — personal site built
with [Quarto](https://quarto.org), deployed via GitHub Pages.

## Layout

```
.
├── _quarto.yml             Site config: nav, theme, metadata
├── theme.scss              Custom SCSS layered on Bootswatch Litera
├── index.qmd               Landing page
├── projects.qmd            Listing of all projects
├── publications.qmd        Publications page
├── about.qmd               About / extended bio
├── references.bib          BibTeX entries (kept as data, page renders directly)
├── assets/
│   ├── zhenning_zhang_resume.pdf
│   └── img/
└── projects/               One .qmd per project
    ├── aacr-2026-contrast-classification.qmd
    ├── ic50-multimodal.qmd
    ├── segmentation-toolkit.qmd
    ├── data-ingestion-platform.qmd
    ├── multiplex-if-gnn.qmd
    ├── embedding-visualization.qmd
    ├── multi-agent-stock.qmd
    ├── music-gen.qmd
    ├── enigma.qmd
    └── raycasting.qmd
```

## Local development

Install Quarto once: <https://quarto.org/docs/get-started/>

```bash
quarto preview        # live-reloading local preview at localhost:4444
quarto render         # one-shot full build into _site/
```

## Adding a new project

Create a new `.qmd` in `projects/` with frontmatter:

```yaml
---
title: "Project name"
description: "One-sentence positioning, used as the listing card subtitle."
date: 2026-04-15
categories: [Professional, Medical Imaging]   # or [Personal, ...]
image: ../assets/img/placeholder-square.png   # optional thumbnail
---

Body content in Markdown.
```

Save → `git push` → site rebuilds via GitHub Action → live in ~60 seconds.

## Adding a new publication

Edit `publications.qmd` directly, following the existing entry format.
Optionally add to `references.bib` for record-keeping.

## Deployment

The `.github/workflows/publish.yml` action runs on every push to `main`. It
sets up Quarto, renders the site, and pushes the rendered output to the
`gh-pages` branch. GitHub Pages serves from that branch.

### One-time GitHub setup

1. Push this repo to GitHub (public, or private if you have GitHub Pro).
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, select **Deploy from a branch**.
4. Branch: `gh-pages`, folder: `/ (root)`.
5. Wait for the first push; the Action will create the `gh-pages` branch
   automatically.

### Custom domain (zhenningzhang.net)

The repo includes a `CNAME` file with `zhenningzhang.net`, so GitHub Pages
will serve the site from that domain once DNS is configured.

At your domain registrar, set DNS records to point at GitHub Pages. For an
apex domain (`zhenningzhang.net`) plus `www` subdomain:

| Type  | Name | Value                             |
|-------|------|-----------------------------------|
| A     | @    | 185.199.108.153                   |
| A     | @    | 185.199.109.153                   |
| A     | @    | 185.199.110.153                   |
| A     | @    | 185.199.111.153                   |
| AAAA  | @    | 2606:50c0:8000::153               |
| AAAA  | @    | 2606:50c0:8001::153               |
| AAAA  | @    | 2606:50c0:8002::153               |
| AAAA  | @    | 2606:50c0:8003::153               |
| CNAME | www  | mizaimao.github.io                |

(Replace `mizaimao` with whatever your GitHub username turns out to be.)

DNS propagation: ~15 min typical, up to a few hours.

### HTTPS

In **Settings → Pages**, after DNS resolves, check **Enforce HTTPS**.
GitHub provisions a Let's Encrypt cert automatically.

## Rendering locally vs in CI

`execute: freeze: auto` in `_quarto.yml` means computational outputs are
cached. For a content-only site (no Jupyter/R blocks), this is mostly moot.
If you start embedding executable blocks, the cache lives in `_freeze/` and
should be committed to git.

## Theme customization

`theme.scss` lays editorial-leaning typography (Fraunces display, Source
Serif body, JetBrains Mono code) and color (cream paper, deep ink, crimson
accent) on top of Bootswatch Litera. To dial the design louder or quieter:

- **Colors** → `$paper`, `$ink`, `$accent` block at top of `theme.scss`.
- **Fonts** → `$font-family-*` and `$headings-font-family` variables; web
  fonts are imported once via `@import url(...)` in the `scss:rules` block.
- **Hero typography** → `.hero-block .hero-name` rule.

## Demos (separate repos)

Live demos live in their own Hugging Face Spaces repos:

- Patient info dashboard — `huggingface.co/spaces/<username>/patient-dashboard`
- Multi-agent stock analysis — `huggingface.co/spaces/<username>/stock-multi-agent`

These are linked from the relevant project pages on this site.

## License

Content © Zhenning Zhang. Code released as you see fit (consider MIT or CC-BY).
