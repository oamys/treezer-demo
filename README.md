# Treezer — demo

**Try it:** https://oamys.github.io/treezer-demo/

**One HTML file. No backend. No installation. Share your trees and annotations with ease.**

Treezer is a lightweight, responsive, and interactive phylogenetic tree explorer that runs entirely in your browser. Designed for exploring large phylogenies with over 100,000 even a million tips, Treezer lets you:

- **Explore large trees** – Collapse and expand clades by taxonomy for easier navigation.
- **Annotate and customise** – Annotate clades, search, group, and highlight taxa using taxonomy or metadata.
- **Compare taxonomies** – Visualise and explore differences between taxonomic classifications.
- **Switch between layouts** – View trees in rectangular, circular, or unrooted layouts keeping the same annotations.
- **Export publication-quality figures** – Create high-quality figures for publications and presentations.
- **Save and share your work** – Save your sessions as self-contained HTML files, preserving your tree, metadata, annotations, and visualisation settings for easy sharing and later use.

All you need to get started is a tree file and a metadata table containing tip names, taxonomy, and any additional fields of interest.

This repository hosts a demo build of Treezer and example datasets for testing and exploration.

- **Offline:** download [`treezer_demo.html`](treezer_demo.html) and double-click it.

Contents: `index.html` (landing page), `app/` (the viewer), `treezer_demo.html` (single-file viewer; `tree-viewer.html` is the same file under its old name), `data/` (demo datasets).

## Data

`data/` contains the Genome Taxonomy Database (GTDB) release 232 archaeal and bacterial trees and selected metadata columns
for species representatives (taxonomy, NCBI taxonomy and organism name, CheckM2 completeness/contamination, genome size,
GC %, isolation source, country, type species of genus). Source: https://gtdb.ecogenomic.org — licensed
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Treezer's source code is developed in a separate repository; this demo build is provided for evaluation.
