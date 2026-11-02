# DEVLOG

## 2026-11-02 — Opdatér GitHub Pages-actions (#5, #6, #7, #8)

- **Built:** In `.github/workflows/pages-deploy.yml`: `actions/checkout` v4 → v7, `actions/configure-pages` v4 → v6, `actions/upload-pages-artifact` v3 → v5, `actions/deploy-pages` v4 → v5. `permissions`, `concurrency` and the `github-pages` environment are unchanged.
- **Decisions:** All four bumps go in one PR (the user's decision on #5). I didn't set `include-hidden-files` on upload-pages-artifact. Since v4 it leaves dotfiles out by default, but the site has no dotfiles: `_config.yml` has no `include:`, the repo has no dot-paths besides `.github`/`.gitignore` (Jekyll skips both), and a Pages deploy through Actions doesn't need `.nojekyll`. `CNAME` is not a dotfile. Option considered: set `include-hidden-files: true` "just in case". I rejected it so that nothing unintended gets published.
- **Verified:** All four tags exist upstream (`gh api …/git/ref/tags/<v>`), and v5's `action.yml` has the `include-hidden-files` input (default false). There was no local build, because the factory host has neither Ruby nor Docker. The workflow only runs on push to main, so the first real build and deploy happens at merge and has to be watched (`gh run watch`).
- **Bugs:** None.

## 2026-11-02 — #9: Vægt på æggeblommer og béarnaise essens

- **Built:** `opskrifter/bearnaise-sous-vide.md` ingredient table now shows weights: "2 (ca. 40 g)" Æggeblommer and "2 spsk (ca. 10 g)" Béarnaise essens.
- **Decisions:** 40 g is read as the total for the 2 yolks (≈20 g each), per the PM's note. Weights go in the Mængde column next to the count/spoon measure, so the table keeps 2 columns and the link text "Béarnaise essens" stays equal to the target page's title. `bearnaise-essens.md` also lists 2 yolks and 200 g butter, but those belong to the finished sauce, not the essence itself, so it was left unchanged.
- **Verified:** I checked the diff by hand: same column count on every row, the blank line before the table is kept, and the link targets haven't changed. After review, I added `.github/workflows/pr-build.yml` (build only, runs `bundle exec jekyll build` on pull requests, no Pages deploy and no `pages: write`), because the factory host has no Ruby or Docker. PR build on 5c3ff9d: pass (35s, run 41437643327).
- **Bugs found:** none.
- **Review fix (after merging main):** `pr-build.yml` now pins `actions/checkout@v7` to match `pages-deploy.yml` after #11.

## 2026-11-02 — #3: Rutinemæssige afhængighedsopdateringer (bundle update)

- **Built:** Ran `bundle update` in the Docker container (`docker compose run --rm jekyll bundle update`, ruby:3.3, Bundler 2.5.22). Only `Gemfile.lock` changes: addressable 2.9.0→2.9.2, bigdecimal 4.1.2→4.1.3, concurrent-ruby 1.3.7→1.3.8, csv 3.3.5→3.3.7, jekyll-seo-tag 2.9.0→2.9.1, listen 3.10.0→3.10.1, google-protobuf 4.35.1→4.36.2, sass-embedded 1.101.0→1.107.1, jekyll-include-cache 0.2.1→0.3.1, and json 2.19.9→2.21.2 (not in the issue table, but still within jekyll's `~> 2.6`). Jekyll 4.4.1, just-the-docs 0.12.0, Ruby, Bundler and the platform list are unchanged.
- **Decisions:** Kept the json bump: it's a minor bump inside jekyll's constraint and came with a plain `bundle update`. The option of `bundle update --conservative` limited to the listed gems was rejected, because the issue asks for a plain `bundle update`.
- **Verified:** Baseline `jekyll build` with the old lockfile passed. After the update I rebuilt the image (`docker compose build`; `run --rm` doesn't keep the updated gems), and `jekyll build` passed. `docker compose up` served `/`, `/opskrifter/`, `/opskrifter/kartoffelbroed-i.html`, `/opskrifter/pulled-pork-sous-vide.html` and the theme CSS/JS with HTTP 200. Diffed the rendered HTML against production (old lockfile): apart from livereload and the local URL, the only difference is the SEO tag version comment, so jekyll-include-cache 0.3 renders the same. The compiled CSS differs only in colour notation (newer Dart Sass writes `rgb(92.08%, …)` where it used to write `rgb(234.8, …)`, the same colour).
- **Bugs found:** None. Note: build output written by the container (`_site`, `.jekyll-cache`) is owned by root on the host, so remove it with `docker run --rm -v "$PWD":/w -w /w ruby:3.3 rm -rf _site .jekyll-cache`.

## 2026-11-02 — #4: Ruby 3.3 → 3.4 (Bundler 2.5.22 → 2.6.9)

- **Built:** `Dockerfile` now uses `FROM ruby:3.4`, `ruby-version: "3.4"` is set in both `pages-deploy.yml` and `pr-build.yml`, and `BUNDLED WITH` in `Gemfile.lock` is 2.6.9. CLAUDE.md now says ruby:3.4. No gem versions changed.
- **Decisions:** The user chose Ruby 3.4 (option 1), and 4.0 waits until Jekyll states support for it. A plain `bundle update --bundler` wrote `4.1.0.beta3`, the newest Bundler on rubygems and a prerelease. I pinned `--bundler=2.6.9` instead, the version that ships with ruby:3.4 (3.4.12), which is what the acceptance criterion asks for. setup-ruby's `bundler-cache` then installs the same Bundler in CI.
- **Verified:** Baseline `jekyll build` on ruby:3.3 passed. On ruby:3.4.12 with Bundler 2.6.9, `docker compose build` installs every gem, including the native gems eventmachine 1.2.7, ffi 1.17.4, google-protobuf 4.36.2 and sass-embedded 1.107.1. `jekyll build` passed. `docker compose up` served `/`, `/opskrifter/`, `/opskrifter/kartoffelbroed-i.html`, `/opskrifter/pulled-pork-sous-vide.html` and the theme CSS/JS with HTTP 200.
- **Bugs found:** None.
