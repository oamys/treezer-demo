# PhyloVista — demo

**Try it:** https://oamys.github.io/phylovista-demo/

PhyloVista is a fast, ARB-style phylogenetic tree explorer that runs entirely in the browser: fold large microbial trees by
taxonomy, annotate clades, search and highlight by metadata, compare taxonomies, re-root and export figures.
This repository hosts a demo build and demo datasets for testing.

- **Offline:** download [`phylovista.html`](phylovista.html) and double-click it.

Contents: `index.html` (landing page), `app/` (the viewer), `phylovista.html` (single-file viewer), `data/` (demo datasets).

## Data

`data/` contains the Genome Taxonomy Database (GTDB) release 232 archaeal and bacterial trees and selected metadata columns
for species representatives (taxonomy, NCBI taxonomy and organism name, CheckM2 completeness/contamination, genome size,
GC %, isolation source, country, type species of genus). Source: https://gtdb.ecogenomic.org — licensed
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

The viewer's source code is developed in a separate repository; this demo build is provided for evaluation.
