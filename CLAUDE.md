# StyglasDK — CLAUDE.md

## Project

Personal knowledge base at **https://styglas.dk**, hosted on GitHub Pages.
Repo: https://github.com/styglas/styglasdk

- Jekyll + [just-the-docs](https://just-the-docs.com/) theme (light scheme)
- All content in **Danish**
- Deploy: push to `main` → GitHub Actions builds and deploys automatically (~30s)

## Structure

```
index.md                  # Home page (nav_exclude: true)
opskrifter/
  index.md                # "Opskrifter" section parent
  <recipe>.md             # Individual recipes
CNAME                     # styglas.dk
_config.yml               # Jekyll + just-the-docs config
Gemfile                   # gem "just-the-docs"
.github/workflows/
  pages-deploy.yml        # Build & deploy workflow
```

## Adding a recipe

Create `opskrifter/<slug>.md` with this front matter:

```yaml
---
layout: default
title: Opskriftens titel
parent: Opskrifter
nav_order: <number>
---
```

Then content sections (all in Danish):

```markdown
## Ingredienser

| Mængde | Ingrediens |
|--------|-----------|
| 500 ml | Piskefløde |

## Tid og temperatur

| Temperatur | Tid |
|------------|-----|
| 80 °C | 60 min |

## Fremgangsmåde

1. ...
```

## Content conventions

- **No BS** — ingredients, time, temperature, steps only. No intro text or backstory.
- **Measurements**: metric (g, ml, dl) or spoons (tsk = teaspoon, spsk = tablespoon)
- **Tables need a blank line** before them when following a heading (Kramdown requirement)
- `nav_order` controls sidebar order within a section — set sequentially

## Adding a new section (future)

Create `<section>/index.md` with `nav_order: <N>` (no `parent:`), then add child pages with `parent: <Section title>`.

## Local preview (optional)

```bash
bundle install
bundle exec jekyll serve
```

Requires Ruby 3.3+ and Bundler.
