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
- [ ] Author(s) and lab/affiliation in the footer (needs the user's details).

Everything else (version number, examples, hero picture, interface overview, viewer items) is now on the single task
list in the source repository: `docs/v2/STATUS.md`, section G (tasks 24–36) and task 23 (export as one HTML file).
