# Treezer — demo

**Try it:** https://oamys.github.io/treezer-demo/

Treezer is a fast, ARB-style phylogenetic tree explorer that runs entirely in the browser: fold large microbial trees by
taxonomy, annotate clades, search and highlight by metadata, compare taxonomies, re-root and export figures.
This repository hosts a demo build and demo datasets for testing.

- **Offline:** download [`treezer.html`](treezer.html) and double-click it.

Contents: `index.html` (landing page), `app/` (the viewer), `treezer.html` (single-file viewer; `tree-viewer.html` is the same file under its old name), `data/` (demo datasets).

## Data

`data/` contains the Genome Taxonomy Database (GTDB) release 232 archaeal and bacterial trees and selected metadata columns
for species representatives (taxonomy, NCBI taxonomy and organism name, CheckM2 completeness/contamination, genome size,
GC %, isolation source, country, type species of genus). Source: https://gtdb.ecogenomic.org — licensed
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Treezer's source code is developed in a separate repository; this demo build is provided for evaluation.
