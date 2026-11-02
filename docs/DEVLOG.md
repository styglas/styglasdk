# Implementation log

## 2026-11-02 — #9: Vægt på æggeblommer og béarnaise essens

- **Built:** `opskrifter/bearnaise-sous-vide.md` ingredient table now shows weights: "2 (ca. 40 g)" Æggeblommer and "2 spsk (ca. 10 g)" Béarnaise essens.
- **Decisions:** 40 g is read as the total for the 2 yolks (≈20 g each), per the PM's note. Weights go in the Mængde column next to the count/spoon measure, so the table keeps 2 columns and the link text "Béarnaise essens" stays equal to the target page's title. `bearnaise-essens.md` also lists 2 yolks and 200 g butter, but those belong to the finished sauce, not the essence itself, so it was left unchanged.
- **Verified:** No Ruby/Docker on the factory host, and the Pages workflow builds only on push to main, so there is no pre-merge build. I checked the diff by hand: same column count on every row, the blank line before the table is kept, and the link targets haven't changed. The Pages build runs on merge.
- **Bugs found:** none.
