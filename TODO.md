# TODO

Shared backlog for the NCC documentation site. Tracked in git, but **not part
of the rendered book** — it is not listed in `book.chapters`, so it does not
appear on the site. Edit it like any other file, via a branch and a pull
request.

Note this repository is **public**, so this file is publicly visible on GitHub
even though it is not on the site. Keep it to work items: no participant data,
credentials, internal server paths, or restricted procedures. Link to the
secure internal location instead.

Items below were identified while getting the site onto GitHub Pages
(2026-08-04). None is a build blocker; the site renders clean and is live at
<https://ncc-study.github.io/ncc-docs/>.

## Publicly visible placeholder content

The site is public and indexable. Everything below is live to anyone with the
URL right now.

- [ ] **`manual/study-overview.qmd`** — drafted 2026-08-05. Study design, data
      sources, and governance-in-practice are now written from what the repo
      documents; the remaining gaps are facts only the study team has, and are
      marked with `TODO` callouts:
  - [ ] background and aims — why the study exists, primary/secondary aims,
        funder, responsible institution, current status
  - [ ] recruitment and follow-up schedule — recruitment period, follow-up
        cadence, which questionnaires are closed vs. open (duplicated with the
        questionnaire-schedule item below; write once, cross-reference)
  - [ ] population and eligibility — inclusion/exclusion, catchment, route into
        the cohort, cohort size
  - [ ] sub-studies — CLOC is listed; describe its aim/inclusion/approvals and
        add any sub-study not yet listed
  - [ ] approvals and legal basis — REK number, legal basis, data controller,
        DPIA/DPAs, retention and access rules (duplicated with the governance
        item below; write once, cross-reference)
  - [ ] the "At a glance" table carries four *TODO — study team* cells that
        resolve as the items above are filled in
- [ ] **`manual/data-collection.qmd`** — five `TODO` callouts for facts that
      can't be derived from the processing code and need the study team:
  - [ ] questionnaire schedule — when each was fielded, follow-up cadence,
        which are closed vs. open
  - [ ] consent details — what participants consent to, legal basis
        (REK / data protection), how withdrawal is requested
  - [ ] registry details — per registry: delivery period, governing approval,
        handling conditions
  - [ ] biobank details — collection timing, assays run, how results are
        reported back
  - [ ] governance
- [ ] **`best-practices/reproducible-analysis.qmd`** — "Placeholder chapter"
      callout; the *Project structure* and *Reporting* sections are prompts,
      not conventions.
- [ ] **`references.bib`** — contains only `ncc-placeholder`, explicitly marked
      "Placeholder reference. Add study citations here."

## New chapters to write

- [ ] **`best-practices/ai-tools.qmd`** — new chapter on how AI coding
      assistants (Claude Code, Copilot, ChatGPT and similar) may and may not be
      used in NCC work. Needs a decision from the study team before drafting,
      since it is guidance the group has to agree on rather than something that
      can be derived from the repo. At minimum it should cover:
  - [ ] what may be pasted into an assistant — the hard line is that NCC data
        lives on TSD and person-level data, identifiers and raw output must
        never leave it; state explicitly whether variable names, codebooks and
        aggregate results are also off limits
  - [ ] how assistants are used from inside TSD versus outside it, given TSD
        has no internet access (the working model is code out by hand, results
        back in by hand)
  - [ ] verification expectations — AI-written analysis code is a draft until a
        human has read it line by line; no generated result goes into a
        manuscript unchecked
  - [ ] disclosure — when AI assistance must be declared, in publications and
        in internal analysis documentation
  - [ ] approved tools and where they may run, plus who to ask before adopting
        a new one
  - [ ] register under `book.chapters` in `_quarto.yml` only when the content
        is agreed; keep it unlisted while it is a draft

## Repository / process

- [ ] **Enable branch protection on `main`.** Every push to `main` publishes
      straight to the public site with no review gate. During the setup session
      `main` was pushed to directly three times, each of which published. The
      `CLAUDE.md` workflow (feature branch → PR → merge) is currently a
      convention with nothing enforcing it.
- [ ] **`index.qmd:12`** says the "Edit this page" link appears *"once the
      repository URL is configured"* — `repo-url` is now configured and
      matches `origin`, so that hedge is stale. Reword.

## Consistency (low priority)

- [ ] **`best-practices/data-management.qmd`** sets its title via YAML
      `title:` instead of an H1. `CLAUDE.md` specifies exactly one H1 per file
      as the chapter title. `data-collection.qmd` was converted during setup
      (it needed the `{#sec-data-collection}` anchor); this one is the last
      holdout. Purely cosmetic — it renders correctly either way.
- [ ] **`_quarto.yml:43-52`** — optional PDF output is scaffolded but commented
      out. Decide whether a downloadable PDF is wanted; if so, uncomment and add
      `downloads: [pdf]`.

## Notes for future work

- **Don't add executable `` ```{r} `` chunks casually.** Quarto starts the
  knitr engine for any executable chunk *even with `eval: false`*, which makes
  the build require R + knitr. The workflow currently installs neither (the R
  setup steps in `publish.yml` are commented out). For illustrative code use a
  plain `` ```r `` block. If a chapter genuinely needs to execute R, uncomment
  those steps — and note `setup-renv` needs an `renv.lock`, which the repo
  doesn't have.
- **Never push a local `gh-pages`.** That branch is generated; `quarto publish`
  overwrites it every build. A stale local copy would clobber the live site.
- `main` carries one empty commit (`b4e3c36`) used to trigger the first
  successful publish. Harmless, just noise in the log.
