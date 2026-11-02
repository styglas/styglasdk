# DEVLOG

## 2026-11-02 — Opdatér GitHub Pages-actions (#5, #6, #7, #8)

- **Built:** In `.github/workflows/pages-deploy.yml`: `actions/checkout` v4 → v7, `actions/configure-pages` v4 → v6, `actions/upload-pages-artifact` v3 → v5, `actions/deploy-pages` v4 → v5. `permissions`, `concurrency` and the `github-pages` environment are unchanged.
- **Decisions:** All four bumps go in one PR (the user's decision on #5). I didn't set `include-hidden-files` on upload-pages-artifact. Since v4 it leaves dotfiles out by default, but the site has no dotfiles: `_config.yml` has no `include:`, the repo has no dot-paths besides `.github`/`.gitignore` (Jekyll skips both), and a Pages deploy through Actions doesn't need `.nojekyll`. `CNAME` is not a dotfile. Option considered: set `include-hidden-files: true` "just in case". I rejected it so that nothing unintended gets published.
- **Verified:** All four tags exist upstream (`gh api …/git/ref/tags/<v>`), and v5's `action.yml` has the `include-hidden-files` input (default false). There was no local build, because the factory host has neither Ruby nor Docker. The workflow only runs on push to main, so the first real build and deploy happens at merge and has to be watched (`gh run watch`).
- **Bugs:** None.
