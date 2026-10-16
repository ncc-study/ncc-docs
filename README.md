# NCC Study Documentation

Study manual and best-practice guides for the **Norwegian COVID-19 Cohort (NCC / Koronastudien)**, built with [Quarto](https://quarto.org) and published to GitHub Pages.

## Quick start

1. **Install Quarto** (>= 1.4): <https://quarto.org/docs/get-started/>.
2. Clone the repo and preview locally:
   ```bash
   git clone https://github.com/ncc-study/ncc-docs.git
   cd ncc-docs
   quarto preview
   ```
3. Edit the `.qmd` files. Add new chapters by creating the file and registering it under `book.chapters` in `_quarto.yml`.

## Publishing

Publishing is automatic: any push to `main` triggers `.github/workflows/publish.yml`, which renders the book and deploys it to the `gh-pages` branch.

**One-time setup:**

1. Push this repo to GitHub.
2. In **Settings → Pages**, set the source to the **`gh-pages` branch** (root folder). The workflow creates that branch on its first run.
3. `repo-url:` in `_quarto.yml` points to `ncc-study/ncc-docs`. If the repository is moved or renamed, update it there and in the clone command above.

The published site is **public**. Keep participant data, identifiers, credentials, and restricted material out of the repository — see `CLAUDE.md` for the full data-safety notes.

## Repository layout

See `CLAUDE.md` for a full description of the structure and conventions. In short:

- `manual/` — how the study is run.
- `best-practices/` — data and analysis conventions.
- `_quarto.yml` — book config and chapter order.

## Contributing

Work on a branch, open a pull request, and confirm `quarto render` succeeds locally before pushing.
