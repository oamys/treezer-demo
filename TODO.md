# Landing page — parked items

## Decisions pending (in order)
1. [x] Confirm the name "Treezer" (confirmed 2026-10-10; rename done, tag `treezer-0.1.0`).
2. [ ] Once the name is confirmed: decide whether there will be a paper/preprint. Then fill in the "Cite Treezer"
       section (currently heading + "Coming soon." only).
3. [x] Move the site address away from "phylovista": the demo now lives in oamys/treezer-demo
       (https://oamys.github.io/treezer-demo/), and the old address forwards there.

4. [ ] Open-source decision (whether or not there is a paper):
       - Check ownership with UQ (UniQuest) if Treezer was built as part of a UQ role.
       - Choose a licence: GPL-3.0 (modified copies must stay open and keep the copyright notice) or MIT (most permissive).
       - Make the source repository public at release / submission, not before.
       - Archive the release on Zenodo (GitHub integration) to get a citable DOI, even without a paper.
       - Add CITATION.cff so GitHub shows "Cite this repository"; use the DOI in the "Cite Treezer" section.
       - Until then: copyright notice ("© 2026 [holder]. All rights reserved.") in footer/README and a LICENSE file
         stating evaluation-only use; holder to be confirmed.

## Content to add later
- [ ] Author(s) and lab/affiliation in the footer.
- [ ] Version number in the hero build line (currently "Demo build · October 2026 · tested with GTDB r232").
- [ ] Non-GTDB example datasets (e.g. a small species tree, a gene or viral tree).

## Viewer (outside the landing page)
- [x] Viewer renamed to Treezer (sessions save as `.treezer`); screenshots in `img/` retaken from the renamed build.
- [ ] Option to hide single-genome tip labels: singleton phyla still show "p__X (1)" with singleton names turned off.
      The circular screenshot has those two labels removed by hand.
