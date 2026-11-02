# Implementation log

## 2026-11-02 — #9: Vægt på æggeblommer og béarnaise essens

- **Built:** `opskrifter/bearnaise-sous-vide.md` ingredient table now shows weights: "2 (ca. 40 g)" Æggeblommer and "2 spsk (ca. 10 g)" Béarnaise essens.
- **Decisions:** 40 g is read as the total for the 2 yolks (≈20 g each), per the PM's note. Weights go in the Mængde column next to the count/spoon measure, so the table keeps 2 columns and the link text "Béarnaise essens" stays equal to the target page's title. `bearnaise-essens.md` also lists 2 yolks and 200 g butter, but those belong to the finished sauce, not the essence itself, so it was left unchanged.
- **Verified:** I checked the diff by hand: same column count on every row, the blank line before the table is kept, and the link targets haven't changed. After review, I added `.github/workflows/pr-build.yml` (build only, runs `bundle exec jekyll build` on pull requests, no Pages deploy and no `pages: write`), because the factory host has no Ruby or Docker. PR build on 5c3ff9d: pass (35s, run 41437643327).
- **Bugs found:** none.
