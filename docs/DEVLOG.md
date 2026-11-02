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
