# CLAUDE.md

Guidance for Claude Code (and other AI assistants) working in this repository. Written for whoever is using the assistant — this repo is maintained by several members of the NCC study group, so this file refers to "the user" and "contributors," not any specific person.

## What this repository is

This repo holds the **study documentation for the Norwegian COVID-19 Cohort (NCC / Koronastudien)**: the study manual and best-practice guides used by study personnel and research collaborators. It is built as a **Quarto book** and published as a website via **GitHub Pages**.

The audience is people who run, manage, and analyse the study. Documentation must be accurate, unambiguous, and safe to follow as an operational reference. When in doubt about a study-specific fact (a procedure, an approval, a variable definition), ask the user rather than inventing it — placeholders in the chapters mark where real content belongs.

## Repository structure

```
_quarto.yml                     Book config: metadata, chapter order, output formats
index.qmd                       Preface / landing page
manual/                         Part 1 — how the study is run
  study-overview.qmd
  data-collection.qmd
best-practices/                 Part 2 — shared data & analysis conventions
  data-management.qmd
  reproducible-analysis.qmd
references.qmd                  Bibliography container
references.bib                  Citation entries
styles.css                      Minimal, team-editable style overrides
_variables.yml                  Site-wide values used via {{< var >}} (e.g. Scholar profile URL)
_scholar-metrics.md             Scholar bibliometrics on the landing page (regenerated in CI; committed copy is the fallback)
scripts/fetch_scholar_metrics.py  Fetches Scholar metrics with `scholarly`; run by publish.yml
.github/workflows/publish.yml   CI: renders and deploys on every push to main
.gitignore
```

## Build and preview locally

Requires Quarto (>= 1.4). From the repo root:

- `quarto preview` — live-reloading local preview while editing.
- `quarto render` — one-off full build into `_book/` (git-ignored).

Always confirm `quarto render` succeeds before pushing — a render error will fail the publish workflow.

## How publishing works

Push to `main` → the `publish.yml` GitHub Action renders the book and deploys it to the `gh-pages` branch → GitHub Pages serves the site.

- The workflow also runs weekly (Mondays) so the Google Scholar metrics on the landing page stay current. Before rendering it runs `scripts/fetch_scholar_metrics.py`, which overwrites `_scholar-metrics.md` for that build only. Google Scholar often blocks CI runners; when the fetch fails the committed `_scholar-metrics.md` is used instead, so refresh that fallback occasionally by running the script locally and committing the result through a PR.
- Do **not** edit the `gh-pages` branch by hand; it is overwritten on every build.
- First-time setup: in the repo's **Settings → Pages**, set the source to the `gh-pages` branch (root). The workflow creates that branch on its first successful run.
- The site is public. See "Data safety" below.

## Drafting and review

Publishing is welded to the `main` branch: **only pushes to `main` publish.** Nothing on any other branch is ever deployed. This is the review gate — use it as follows.

**Normal draft-and-review flow (recommended):**

1. Create a feature branch off `main` (e.g. `draft/new-sop`).
2. Write the chapter and register it normally under `book.chapters` in `_quarto.yml`.
3. Run `quarto preview` — the whole book, including the new chapter, renders locally so it can be reviewed exactly as it will look, without anything going public.
4. Open a pull request for review. The published site does not change while the branch is under review.
5. Merge to `main` → the Action publishes it.

**Parking a draft inside `main` without publishing it.** If a work-in-progress chapter needs to be merged to `main` but kept off the live site, keep the `.qmd` file in the repo but **leave it out of (or comment it out in) `book.chapters`**:

```yaml
    - part: "Study Manual"
      chapters:
        - manual/study-overview.qmd
        # - manual/biosampling.qmd   # draft — not published yet
```

The file stays versioned and editable; it is simply not part of the rendered book. To preview an unlisted chapter, render it on its own (`quarto render manual/biosampling.qmd`) or temporarily re-list it on a branch.

**Important — do not use Quarto's `draft:` feature here.** Quarto's `draft: true` front-matter option and the `drafts:` / `draft-mode:` keys are a **website** feature and are **not honored in book projects** (a chapter marked `draft: true` still renders with full content in a book). For this book, control what publishes with the branch and the `chapters:` list described above — not with `draft:`.

## Adding or editing content

- Chapters are `.qmd` files (Quarto markdown). Plain `.md` prose works too, but `.qmd` is needed for callouts, cross-references, and executable code.
- To **add a chapter**: create the `.qmd` file, then register it under `book.chapters` in `_quarto.yml`. Order in that list is the order in the sidebar. A chapter file that is not listed will not appear.
- Use exactly **one H1 (`#`)** per file — it is the chapter title.
- Use cross-reference IDs for sections that other pages link to, e.g. `# Data Collection {#sec-data-collection}`, referenced with `@sec-data-collection`.
- Use callouts for operational emphasis: `::: {.callout-warning}` / `.callout-note` / `.callout-important`.
- Add citations to `references.bib` and cite with `[@key]`.

## Writing conventions

- **Language:** keep each document consistent in the language it is written in. The site's primary language is set by `lang:` in `_quarto.yml`; change it there if the default language changes.
- **Tone:** clear and instructional. Write procedures as numbered steps in the present tense, so they can be followed operationally.
- **Precision:** prefer specific, checkable statements over vague ones. Do not soften or embellish study procedures.

## R code in documentation

Some chapters include R examples. Follow these project conventions so code reads consistently across contributors:

- Tidyverse-first; fall back to base R only where there is no tidyverse equivalent.
- Native pipe `|>`.
- `snake_case` for variables, columns, datasets, and functions.
- Use `here::here()` for paths — never `setwd()` or absolute paths.
- Where a namespace conflict is possible (e.g. `MASS` loaded), write `dplyr::select()` / `dplyr::filter()` explicitly.
- Preserve variable/value labels on labelled data (`haven` / `labelled`); never strip them silently.
- Set a single agreed random seed once per analysis for reproducibility.
- In Quarto chunks, use the `#|` option style, name non-trivial chunks, and set `eval:`/`echo:` deliberately. Example chunks that are illustrative rather than run should use `eval: false`.

## Data safety — important

- **Never commit participant-level data, personal identifiers, or restricted registry extracts.** This repository is documentation only; `.gitignore` blocks common data formats, but the responsibility is on every commit.
- **Assume the published site is publicly visible.** Do not include credentials, access tokens, internal server names or paths, or confidential procedures.
- If a document needs to reference restricted material, **link to the secure internal location** instead of reproducing it here.
- If a requested change would place any of the above into the repo, stop and flag it to the user rather than proceeding.

## Contribution workflow

- All changes go through a feature branch and a pull request (see "Drafting and review" above for how this gates publishing).
- Verify `quarto render` passes locally before pushing — a render error will fail the publish workflow.
- Keep pull requests focused so changes are easy to review.
- Recommended for maintainers: enable branch protection on `main` (require an approved PR before merge) so nothing reaches the public site without review.
