# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Éloi Zablocki's academic personal website (https://eloiz.github.io), built on the
[al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. The vast majority of
day-to-day work here is **content editing** (publications, news, projects, CV data), not
theme development. The theme machinery (`_layouts/`, `_includes/`, `_sass/`, `_plugins/`)
is upstream al-folio and rarely needs to change.

## Common tasks (where content lives)

- **Add/edit a publication** → `_bibliography/papers.bib`. BibTeX entries grouped by year
  on the publications page (newest first).
- **Add a news item** → create a file in `_news/` (e.g. `_news/myitem.md`). Inline news
  use `inline: true` front matter; the news list is shown on the about page and `_pages/news.md`.
- **Add/edit a project** → `_projects/N_project.md`.
- **CV / students / coauthors / venues** → YAML files in `_data/` (`cv.yml`, `students.yml`,
  `alumni.yml`, `coauthors.yml`, `venues.yml`, `repositories.yml`).
- **About page bio, profile photo, toggles** → `_pages/about.md` front matter.
- **Site-wide settings** → `_config.yml`.

### papers.bib conventions

Custom (non-standard BibTeX) fields are used by the theme and stripped from the rendered
citation (see `filtered_bibtex_keywords` in `_config.yml`):
- `preview={img.png|gif}` — thumbnail in `assets/img/publication_preview/`.
- `selected={true}` — marks a paper as "selected" (only shown if `selected_papers: true`).
- `abstract`, `arxiv`, `code`, `website`, `pdf`, `html`, `video`, `slides`, `poster`,
  `award`, `bibtex_show={true}`.
- News items deep-link to papers via the citation key anchor, e.g.
  `[NAF](/publications#chambon2026naf)` → entry `@inproceedings{chambon2026naf, ...}`.
  Keep citation keys stable.

### Author linking

`jekyll-scholar` auto-links co-author names to their homepages using `_data/coauthors.yml`.
The key is the lowercased last name; list all first-name spellings/initials under
`firstname:` so variants match. The site owner's own name renders unlinked (configured via
the `scholar.last_name`/`first_name` block in `_config.yml`).

## Build & preview

```bash
bundle exec jekyll serve   # local dev server with --watch livereload
bundle exec jekyll build   # one-off build into _site/ (also bin/cibuild)
```

Docker alternative (matches CI Ruby version): `docker compose up` → serves on
http://localhost:8080.

This is a Ruby/Jekyll site with Python (`nbconvert`) for Jupyter notebook support.
`package.json` deps are only Prettier + the Liquid plugin (formatting), not a JS build.

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds with Jekyll,
runs PurgeCSS, and publishes `_site/` to the `gh-pages` branch (live site). **Do not
edit `gh-pages` by hand** — it is generated. The `bin/deploy` script does the same thing
locally but CI is the normal path. Note the `paths:` filter in the workflow: doc-only
changes (README/FAQ/INSTALL/CUSTOMIZE) and lighthouse results do not trigger a deploy.

## Formatting

Prettier (with `@shopify/prettier-plugin-liquid`) formats `.md`, `.html`, `.liquid` etc.
— `printWidth: 150`. Run `npx prettier --write .` (respects `.prettierignore`).
`.pre-commit-config.yaml` adds trailing-whitespace / EOF / YAML checks.

## Reference docs (upstream al-folio)

`INSTALL.md`, `CUSTOMIZE.md`, `FAQ.md` are the upstream theme docs — consult them for theme
features (collections, redirects, image config, MathJax, etc.) rather than reverse-engineering
the `_includes`/`_layouts` Liquid.
