<!-- Treezer user manual (help pages). Matches Treezer 0.8.2. Source: docs/manual/USER_MANUAL.md in the Treezer repository. -->

# Treezer — User Manual

Manual version 1.0 · 8 October 2026 · describes the full edition on branch `v2-client`
Intended as Supplementary File S1 of the manuscript.

---

## Contents

1. [What Treezer is](#1-what-treezer-is)
2. [Getting started](#2-getting-started)
3. [Input files](#3-input-files)
4. [The screen](#4-the-screen)
5. [Navigating the tree](#5-navigating-the-tree)
6. [Folding and mixed resolution](#6-folding-and-mixed-resolution)
7. [Menu reference](#7-menu-reference)
8. [Layouts and branch order](#8-layouts-and-branch-order)
9. [Search, queries and groups](#9-search-queries-and-groups)
10. [Taxonomy: monophyly, comparison, rooting and editing](#10-taxonomy-monophyly-comparison-rooting-and-editing)
11. [Relative evolutionary divergence (RED)](#11-relative-evolutionary-divergence-red)
12. [Annotating the tree](#12-annotating-the-tree)
13. [The Table tab](#13-the-table-tab)
14. [Subtrees, re-rooting and undo](#14-subtrees-re-rooting-and-undo)
15. [Exporting figures and data](#15-exporting-figures-and-data)
16. [Saving work: sessions, shareable files and autosave](#16-saving-work-sessions-shareable-files-and-autosave)
17. [Link parameters](#17-link-parameters)
18. [Performance and limits](#18-performance-and-limits)
19. [Troubleshooting and FAQ](#19-troubleshooting-and-faq)
20. [Glossary](#20-glossary)
- [Appendix A. Example input files](#appendix-a-example-input-files)
- [Appendix B. Worked examples](#appendix-b-worked-examples)

---

## 1. What Treezer is

Treezer is a phylogenetic tree viewer for large microbial trees that runs entirely in a web browser. It is
designed for the everyday work of placing genomes in context and curating their taxonomy:

- **open** trees of up to hundreds of thousands of tips (the full GTDB bacterial tree, 189,801 species, stays smooth);
- **fold** the tree by taxonomic rank, or clade by clade, at mixed resolution;
- **annotate** clades with named, coloured groups, shading and trait strips;
- **check taxonomy against the tree**: which taxa are split, into how many pieces, by which tips, with what support;
- **compare** two taxonomies (e.g. GTDB vs NCBI, or two GTDB releases) on the same tree;
- **edit** taxonomy and name new taxa, guided by relative evolutionary divergence (RED);
- **export** publication figures (SVG, PDF, PNG) and data (Newick, TSV, sessions).

### Design principles

| Principle | What it means for you |
|---|---|
| No installation, no server | Open a web page or a single HTML file. Nothing to install, no account. |
| Your data stays on your computer | Files are read by your browser and never uploaded. Works offline. |
| The tree is never changed behind your back | Folding, colouring, ordering and highlighting are *views* of the tree. The tree itself changes only when you re-root it or open a subset, and both can be undone. |
| Annotations survive folding | Folded clades summarise what is inside them (counts, shares, means) instead of hiding it. |
| Taxonomy is checked, not assumed | A taxon is shown as one clade only if its tips really form one clade; otherwise it is shown as numbered pieces. |

### Editions

The public demo and the default build are the **full** edition (rectangular, circular and unrooted layouts, clade
shading, all annotation features). A slimmer **lite** build (rectangular only, no clade shading) can be made with
`VITE_EDITION=lite`. This manual describes the full edition.

---

## 2. Getting started

### 2.1 Requirements

- A current desktop browser. Tested: Google Chrome and Firefox (Linux). Safari's engine (WebKit) passed functional
  tests; Chromium-based browsers such as Microsoft Edge use the same engine as Chrome.
- An ordinary laptop. The 190k-tip GTDB bacterial tree needs about 50 MB of program memory; large metadata tables add
  more (see [§18](#18-performance-and-limits)).
- Phones and tablets work (touch, pinch-zoom), with a compact layout on narrow screens.

### 2.2 Three ways to run it

| Option | How | Best for |
|---|---|---|
| Demo site | Open **https://oamys.github.io/treezer-demo/** and pick a dataset card or the viewer | Trying it out; sharing links |
| Single file | Download `treezer_demo.html` from the demo site and double-click it | Offline use, private data, sending to collaborators |
| From source | `cd frontend && npm ci && npm run dev`, then open the printed address (Node 20+) | Development |

The single file (about 2.4 MB) contains the whole program and the built-in example. It works from any folder, a USB
stick or an e-mail attachment, without an internet connection.

### 2.3 The start screen

When no tree is loaded, the start screen offers:

- **Open ▾** — *Tree file…* for a Newick tree, or *Saved session…* for a `.treezer` session or a shareable Treezer HTML file;
- **Try the example** — the GTDB r232 archaeal tree (10,122 species) with its taxonomy;
- drag and drop — drop a tree, a session or a data table anywhere on the window.

### 2.4 A first session in five minutes

1. Click **Try the example**. The archaeal tree opens with GTDB taxonomy.
2. Click **phylum** in the toolbar: the tree folds into one triangle per phylum.
3. Double-click a phylum to unfold it; Shift+click a folded clade to unfold it to the next rank.
4. Press `/` and type a genus name (e.g. `Methanobrevibacter`), then Enter: the tree opens just enough to show it.
5. Open **monophyly**: every GTDB taxon is a single clade in its own tree. Choose another taxonomy under *compare with*
   (if one is loaded) to see which taxa it splits.
6. **Export ▾ → Save session** keeps everything in one file.

---

## 3. Input files

### Supported formats at a glance

| Kind | Formats | Made by, for example |
|---|---|---|
| **Trees** | Newick (`.nwk`, `.newick`, `.tree`, `.tre`, `.treefile`, `.txt`); **NEXUS** (`.nex`, `.nexus`, `.nxs`, `.trees`, `.t`, `.con.tre`) — the first tree of the file, tip names from its `translate` table | IQ-TREE, RAxML, FastTree, PhyML, GTDB-Tk, GTDB; MrBayes, BEAST, PAUP*, FigTree |
| **Support values** | internal labels (`95`, `0.98`, `85.3/97`, `'100.0:p__X'`) or annotations (`[&prob=0.95]`, `[&posterior=0.99]`, `[&&NHX:B=87]`) | IQ-TREE (SH-aLRT / UFBoot), RAxML, FastTree; MrBayes, BEAST, NHX |
| **Taxonomies** | GTDB (`d__…;p__…`), Greengenes 13_8 / 2 (`k__…; p__…`), SILVA (`Bacteria;Firmicutes;…;`, taxmap `path`), **PR2 5 and 4** (18S), QIIME 2 `taxonomy.tsv`, QIIME 1 / BIOM, mothur `.tax`, DADA2 `assignTaxonomy` (one column per rank), MetaPhlAn (`\|`); GTDB-Tk summaries | GTDB, QIIME 2, DADA2, mothur, SILVA, PR2, GTDB-Tk |
| **Ranks** | GTDB's 7 (domain … species; also Greengenes, SILVA prokaryotes), PR2 5's 9 (domain, supergroup, division, subdivision, class, order, family, genus, species), PR2 4's 8 — recognised from the lineages ([§10](#10-taxonomy-monophyly-comparison-rooting-and-editing)) | |
| **Data about the tips** | any TSV / CSV table (plain or `.gz`) with a column of tip names (`genome`, `accession`, `Feature ID`, `#OTU ID`, `ASV`, …); GTDB metadata; feature tables (KEGG KOs, DRAM) | your own tables, GTDB, DRAM |
| **Annotations** | any table (no special format: choose columns in Annotate); iTOL annotation files: colour strips, clade colours and ranges, text, collapse, labels, simple bars, heatmaps, binary, symbols, connections | your own tables; iTOL |
| **Treezer's own** | sessions (`.treezer`), shareable HTML files | Treezer |

Not read yet: PhyloXML, phylogenetic placements (`.jplace` from EPA-ng / pplacer), SILVA's eukaryote ranks (uneven
depth; needs SILVA's rank file), PR2's UTAX format. A NEXUS file with many trees (e.g. a MrBayes `.t` sample) opens its
first tree; BEAST dates and other annotations are not shown.

### 3.1 Trees

| Format | Notes |
|---|---|
| Newick (`.nwk`, `.newick`, `.tree`, `.tre`, `.treefile`, `.txt`) | Branch lengths optional. Internal labels may be support values (`90`, `100.0`, IQ-TREE's `85.3/97`), names (`node1`) or both. |
| NEXUS (`.nex`, `.nexus`, `.nxs`, `.trees`, `.t`; MrBayes `.con.tre`) | The first tree of the TREES block; numbered tips are named from its `translate` table. Support written as an annotation — MrBayes `[&prob=…]`, BEAST `[&posterior=…]`, NHX `B=…` — is read for nodes without a support label. |
| GTDB-decorated Newick | Quoted internal labels such as `'100.0:p__Pseudomonadota; c__Gammaproteobacteria'` are split into support and taxon names; these clades are named and can be folded by rank immediately. |
| Treezer session (`.treezer`) | Restores a complete earlier session ([§16](#16-saving-work-sessions-shareable-files-and-autosave)). |
| Shareable Treezer file (`.html`) | Opens the tree, data and view saved inside it ([§16.2](#162-shareable-html-files)). |

Trees of any depth are read without problems (very deep "caterpillar" trees included). A tree whose root has more than
two children is treated as unrooted for RED and monophyly, and Treezer says so.

### 3.2 Data tables (Load data… or drop)

**Load data…** reads tab- or comma-separated tables, plain or gzip-compressed (`.gz`). Large files are streamed in the
background with a progress bar; only rows that match a tip in the tree are kept. Treezer recognises:

| Table | Recognised by | What happens |
|---|---|---|
| **Taxonomy file** (e.g. GTDB `*_taxonomy_r232.tsv[.gz]`, or your own) | two columns, no header: tip ID, lineage — GTDB style `d__…;p__…;…;s__…`, Greengenes `k__…; p__…`, or without prefixes (`Bacteria;Pseudomonadota;…`, e.g. SILVA / 16S, ranks by position) | Becomes the tree's taxonomy: clades are named and can be folded by rank; the monophyly report is computed. Loaded onto a tree that already has a taxonomy, it is also kept as a second taxonomy to compare. |
| **Amplicon taxonomy tables** (16S / 18S) | QIIME 2 `taxonomy.tsv` (`Feature ID`, `Taxon`, `Confidence`); QIIME 1 / BIOM (`#OTU ID`, `taxonomy`); SILVA taxmap (`path`); mothur `.tax` (confidences like `Bacilli(99)` are dropped); DADA2 `assignTaxonomy` output (one column per rank: `Kingdom … Species`, with PR2 `Domain … Species`) | The column picker offers the lineage column — or **rank columns**, which joins DADA2's columns into one lineage per tip (a species epithet gets its genus). Ranks are recognised as for any taxonomy ([§10](#10-taxonomy-monophyly-comparison-rooting-and-editing)): GTDB / Greengenes / SILVA prokaryotes, PR2 5, PR2 4. MetaPhlAn-style lineages separated by `|` are read too. |
| **GTDB metadata** (`*_metadata_r232.tsv.gz`) | GTDB column names | A column picker opens with sensible GTDB defaults (completeness, contamination, genome size, GC, isolation source…). Any `*taxonomy` column (e.g. `ncbi_taxonomy`) can be used as the taxonomy. |
| **GTDB-Tk summary** (`gtdbtk.*.summary.tsv`) | `user_genome` + `classification` | Classifications fill the taxonomy for your genomes; useful columns (method, closest reference and ANI, RED, notes, warnings) go to the Table tab; your genomes become a highlighted group. |
| **Any metadata table** (TSV/CSV) | a column of tip names (`leaf_id`, `genome`, `accession`, `user_genome`, …) | Column picker; columns become available for search, the table, strips and summaries. |
| **Feature tables** (e.g. KEGG KOs, DRAM summaries) | long form `genome, feature[, description]`, or wide form features × genomes | Stored as presence/absence; features can be shown as strips ([§12.3](#123-annotate-tab-tip-labels-categories-numbers)). |
| **iTOL annotation files** | iTOL dataset headers | `DATASET_COLORSTRIP` → strip with its colours; `TREE_COLORS` (clade/range), `DATASET_RANGE`, `DATASET_TEXT` → coloured or named groups; `COLLAPSE` → folds; `LABELS` → the tips' own labels; `DATASET_SIMPLEBAR` → bars; `DATASET_HEATMAP` → a heatmap; `DATASET_BINARY` → one presence strip per field, in its colour; `DATASET_SYMBOL` → a strip by colour or label; `DATASET_CONNECTION` → links. Nodes given as `tipA|tipB` mean their common ancestor; a value given for a clade goes to all its tips. **Forgiving:** a SEPARATOR that does not match the data (e.g. TAB declared, commas used after editing in Excel), a missing DATA line, header keys in lower case, Windows line ends — all read, and said so. A **report** lists every line not used, with its number and the reason (ID not in the tree, not a number, too few fields). Not read yet: pie charts, multi-bars, domains, alignments, line charts, box plots, styles, pop-ups — load the same data as a table instead. |

The **column picker** lets you choose which columns to load (with a memory warning for very wide selections), which
column is the tip ID, which column to use as **taxonomy** ("Taxonomy from"), an **alternative ID** column ("Also
match tips by"), and whether to load the table as your own **editable fields**.

### 3.3 How tips are matched

Tip IDs are matched leniently, so tables from different sources line up with the tree:

- `RS_GCF_000001.1`, `GCF_000001.1` and `GCF_000001` are the same genome (GTDB prefixes and version numbers are ignored);
- GenBank and RefSeq accessions (`GCA_…` / `GCF_…`) match through GTDB's `ncbi_genbank_assembly_accession`;
- assembly file names such as `GCA_000001.1_ASM1v1_genomic` match their accession;
- any loaded column can be used as an alternative ID (e.g. strain names → accessions).

The load report says how many tips matched and gives examples of those that did not.

---

## 4. The screen

```
┌ toolbar ─────────────────────────────────────────────────────────────────────────────┐
│ ✿Treezer Open▾ Load data… Export▾ │ Collapse to [domain … genus | expand all] │ ⛶ ↶ ↷  │
│ [layout▾] RED                                            search box   ▣ side panel   │
├──────────────────────────────────────────────────────────────┬───────────────────────┤
│                                                              │ sidebar:              │
│                     tree canvas                              │  selection details    │
│                                                              │  groups               │
│                                                              │  highlights           │
│                                                              │  overview             │
├──────────────────────────────────────────────────────────────┴───────────────────────┤
│ panel tabs: Table · Monophyly · Annotate · Shading · Style (click to open/close)      │
├──────────────────────────────────────────────────────────────────────────────────────┤
│ status line: hints · tree name · tips · groups · draw time · items · labels          │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

### 4.1 Toolbar

One row: rank buttons that do not fit move under **more ▾** (narrow windows, or taxonomies with many ranks). Only **Open ▾** is filled; the rank buttons sit in one group; toggles
(RED, side panel) are shaded while on. The panels (table, monophyly, …) open from the tabs at the bottom.

| Control | What it does |
|---|---|
| **Treezer** (logo and name) | A menu: the Treezer home page and guide, *About this file* (shareable files) and *About Treezer* (version). From the downloaded file these open in a new tab; on the website Treezer asks first if a tree is open, since leaving loses anything not saved. |
| **Open ▾** | *Tree file…* opens a Newick tree; *Saved session…* opens a `.treezer` session or a shareable Treezer HTML file. Or drop any of them on the window. |
| **Load data…** | Load a taxonomy, metadata, GTDB-Tk, feature or iTOL table ([§3.2](#32-data-tables-load-data-or-drop)). |
| **Export ▾** | Sessions, figures and data downloads ([§7.4](#74-export-menu)). |
| **domain … genus** | Fold the whole tree to that rank. Shown only when the tree has named clades (GTDB labels or a loaded taxonomy), in that taxonomy's ranks (PR2: domain, supergroup, division, subdivision, class, order, family, genus). Ranks that do not fit on one row go under **more ▾**. |
| **expand all** | Unfold everything. |
| **⛶ (fit)** | Fit the tree to the window (key `f`). |
| **↶ ↷** | Undo / redo ([§14.3](#143-undo-and-redo)). The tooltip names the step. |
| **RED** | Lay the tree out by relative evolutionary divergence ([§11](#11-relative-evolutionary-divergence-red)). |
| **layout** | rectangular · circular · unrooted ([§8](#8-layouts-and-branch-order)). |
| **search box** | Search names, taxa and metadata ([§9](#9-search-queries-and-groups)). |
| **▣ (side panel)** | Show/hide the sidebar: selection, groups, highlights, overview (on phones: open it). |

### 4.2 Sidebar

- **Selection details** — for a tip: taxonomy and metadata; for a clade: tips, children, support, RED (with the
  closest rank), branch length, distance from the root, distance to tips (max / mean). *go to* zooms to it; *unselect*
  (or Esc, or a click on empty space) clears it. Alt/Option+click a second node to measure the patristic distance and
  find the common ancestor.
The side panel's width is adjustable: drag its left edge (remembered in this browser).

- **Groups** — your named sets of tips: highlight or label each on the tree, rename, focus, step through, open in
  the Table tab, merge, delete, paste or import IDs ([§9.3](#93-groups-your-sets-of-tips)).
- **Overview** — the whole current layout with coloured taxon bands at a chosen rank (phylum by default), the visible
  region as a box, and highlight ticks. Click or drag to move around.

### 4.3 Drawer

The buttons **Panels: Table · Monophyly · Annotate · Shading · Style** stay in a bar under the tree once a tree is
open: click one to open that panel below the tree, click it again (or ⌄) to close it. Resize it by dragging its top edge (the height is
remembered).

| Tab | Purpose | Section |
|---|---|---|
| tips / table | Table of the tips: taxonomy, metadata and your own fields | [§13](#13-the-table-tab) |
| monophyly | Which taxa are split in this tree, their pieces, strays and intruders | [§10](#10-taxonomy-monophyly-comparison-rooting-and-editing) |
| annotate | Tip labels (text at the tips), categories (symbols, colours), numbers (bars, scales, heatmaps), links, genes | [§12.3](#123-annotate-tab-tip-labels-categories-numbers) |
| shading | Clade shading, clade names, layout options | [§12.2](#122-clade-shading-and-clade-names) |
| style | Line width, branch support, branch lengths, scale bar | [§12.6](#126-branch-and-node-styles) |

### 4.4 Status line

Shows hints for the mouse, the current selection, and diagnostics: tree name, tips, number of named groups, draw time,
items and labels drawn, and the current zoom (pixels per row).

---

## 5. Navigating the tree

### 5.1 Mouse, keyboard and touch

| Action | Mouse / key |
|---|---|
| Select a tip or clade | click (details in the sidebar) |
| Unselect | click empty space, or Esc |
| Fold / unfold a clade | double-click |
| Unfold to the next rank | Shift+click a folded clade |
| Context menu | right-click a clade, a tip or empty space |
| Distance and common ancestor | select one node, Alt/Option+click another |
| Pick several branches anywhere (to highlight or group them) | Ctrl+click (⌘+click on a Mac); again to unpick; Esc clears |
| Pan | drag |
| Zoom | mouse wheel |
| Zoom width only (rectangular) | Shift+wheel |
| Fit to window | `f` |
| Search | `/` or Ctrl+F / ⌘F |
| Create group on the selected clade | `g` |
| Undo / redo | ⌘Z / Ctrl+Z · ⇧⌘Z / Ctrl+Y |
| Move a clade name | drag the name (circular, unrooted, rectangular with shading) |
| Touch | drag to pan, pinch to zoom, tap to select |

### 5.2 Scrollbar and overview

In the rectangular layout a vertical scrollbar shows where you are in the tree; drag the thumb or click the track to
page. The overview panel shows the whole tree with taxon bands; click or drag in it to jump.

### 5.3 What is drawn

To stay smooth on very large trees, Treezer draws only what can be told apart on screen: subtrees thinner than a
pixel are drawn as single lines, labels are drawn only where they fit (largest clades first), and the number of labels
per frame is capped. A folded clade's name that would overlap another is moved up to four rows up or down, with
a thin leader line back to its clade, rather than dropped. Zoom in to see more names. Exported figures are not capped ([§15](#15-exporting-figures-and-data)).

---

## 6. Folding and mixed resolution

### 6.1 Folding by rank

The rank buttons (**domain … genus**) fold the whole tree so that each taxon at that rank is one folded clade. A folded
clade is drawn as a triangle (a wedge in circular and unrooted layouts) labelled with its name and tip count, e.g.
`p__Thermoproteota (1,234)`.

Rank folding needs named clades, which come from GTDB-decorated trees or a loaded taxonomy. Split taxa are folded piece
by piece ([§10.1](#101-how-taxa-are-placed-on-the-tree)).

**Tips on their own.** After folding to a rank, a tip that is a whole piece of its taxon by itself is named like a
folded clade: `p__KSB1 (1)` when it is the only tip of its phylum, `p__Chloroflexota [4/7] (1)` when it is a
one-tip piece of a split phylum. A tip without a taxon at that rank keeps its name and says so:
`SE0170_vamb_bins.tsv.052 · no phylum`. These names are shown even where rows are too thin for ordinary tip names.
To make them stand out, right-click empty space → *At phylum level* → **Highlight the N tips alone in their phylum**
and **Highlight the N tips without a phylum** (two highlighted groups, two colours; at the rank you folded to).

### 6.2 Folding clade by clade

- double-click a clade to fold or unfold it;
- Shift+click a folded clade to unfold it to the next rank;
- right-click → **Expand to class / order / family / genus (n)** opens a clade to any lower rank (the count is the number of
  clades it will show), so different parts of the tree can be shown at different ranks (mixed resolution);
- right-click → **Fold each child** / **Unfold everything inside**.

Folding remembers nested state: if you fold a phylum that contains folded genera, unfolding the phylum shows the genera
still folded, as you left them.

### 6.3 Folded-clade summaries

A folded clade can carry a summary of one data column next to its name (Annotate tab → *Names of folded clades show*): the
most common value and its share for categories (e.g. `· freshwater 36%`) or the mean for numbers
(e.g. `· genome_size 6.46M`). Trait strips also summarise folded clades ([§12.3](#123-annotate-tab-tip-labels-categories-numbers)), and highlighted
tips inside a folded clade are counted on its outline (`· 12 highlighted`).

### 6.4 Guarded expansion (circular and unrooted)

Circular and unrooted pictures become unreadable beyond a few thousand items. When an action would open more than
about 4,000 rows (circular) or 5,000 items (unrooted), Treezer holds it back and offers:

- **Expand step by step** — open the largest clades first, within a readable budget;
- **Open as its own tree** — show the clade alone ([§14.1](#141-subtrees));
- **Expand anyway**.

---

## 7. Menu reference

### 7.1 Right-click a clade

The menu header shows the clade's name and number of tips.

| Item | What it does |
|---|---|
| **Fold / Unfold** | Toggle this clade. |
| **Flip branches here (swap children)** | Reverse the order of this clade's children ([§8.4](#84-branch-order)). |
| **Name as taxon…** | Give the clade a taxon name at a rank, guided by RED ([§10.6](#106-naming-a-clade-as-a-taxon)). |
| **Create group… (g)** / **Edit group "…"…** | Make or edit a named, coloured group ([§9.3](#93-groups-your-sets-of-tips)). |
| **Expand to class / order / family / genus (n)** | Open the clade down to that rank; n = clades shown. |
| **Fold each child** | Unfold this clade and fold each of its children. |
| **Unfold everything inside** | Open the whole subtree. |
| **Zoom to clade** | Fit this clade to the window. |
| **Highlight these n tips** | Make a highlighted group of the clade's tips. |
| **Show these tips in the table** | Open the Table tab scoped to this clade. |
| **Copy n tip IDs** | Copy the IDs to the clipboard. |
| **Download table (n)** | Download the clade's tips with the visible table columns. |
| **Open this clade as a new tree** | Prune the tree to this clade (← back returns). |
| **Pick one tip per taxon…** | Choose representatives inside this clade ([§10.7](#107-representatives-one-tip-per-taxon)). |
| **Root here (branch above this clade)** | Re-root the tree on the branch above this clade. |

### 7.2 Right-click a tip

| Item | What it does |
|---|---|
| **Zoom here** | Centre and zoom on the tip. |
| **Highlight this tip** | Make a highlighted group of one tip. |
| **Copy tip ID** | Copy the tip's ID. |
| **Edit label…** | Type the name shown at this one tip (e.g. a strain name for a figure). It is kept in your own field *my_label* (autosaved, saved in sessions, editable in the Table tab) and wins over tip labels from a column. Empty = the usual name again. |
| **Root here (branch above this tip)** | Re-root on the branch leading to this tip. |

### 7.3 Right-click empty space

| Item | What it does |
|---|---|
| **Fit to window** | Same as `f`. |
| **Expand all** | Unfold everything. |
| **Midpoint root** | Root at the midpoint of the longest tip-to-tip path (needs branch lengths). |
| **Pick one tip per taxon (whole tree)…** | Representatives over the whole tree (needs a taxonomy). |
| **Download tree as Newick (as displayed)** | Current tree (re-rooted / pruned) in the displayed branch order. |
| **Download tree with my group names (Newick)** | Group names written as internal node labels. |
| **New tree from “group” (n)** | One entry per group: prune the tree to those tips. |
| **Download my groups (TSV)** | Every group and its tips (columns `group`, `genome`). |
| **✓ Outgroup at the bottom** | On by default: the root's smallest clade (the outgroup) is drawn last — at the bottom of a rectangular tree — whatever the rest of the order; flipping the root still swaps it to the top. Remembered in this browser. |
| **Ladder right (default) / Ladder left** | Branch order ([§8.4](#84-branch-order)): right = larger clades on top, smaller below at every node (the outgroup ends at the bottom); left = the reverse. |
| **Match the order of another tree…** | Order clades like another tree (Newick or a list of names). |
| **As in the file** | Back to the original order. |

### 7.4 Export menu

| Item | Output |
|---|---|
| **Save session (.treezer)** | Everything in one file; reopen with *Open ▾ → Saved session…* |
| **Shareable HTML file…** | Treezer, the tree, its data and the current view in one HTML file that opens by double-click ([§16.2](#162-shareable-html-files)) |
| **Publication figure…** | Whole tree at print size: SVG, PDF or PNG ([§15.2](#152-publication-figures)). |
| **SVG (vector, editable in Illustrator/Inkscape)** | Current view, as on screen. |
| **PNG, 2× resolution** / **PNG, 4× resolution (posters)** | Current view as an image. |
| **PDF (print dialog → Save as PDF)** | Current view through the browser's print dialog. |
| **Tree as Newick** | The current tree. |
| **Taxonomy, GTDB format (with your edits)** | `genome<TAB>d__…;…;s__…` for every tip. |
| **Taxa you named (log, n)** | Every taxon you named, with rank, members and RED. |
| **Tree with my group names (Newick)** | Group names as internal labels. |
| **RED values for every clade (TSV)** | RED of each internal node with its name. |
| **Table (TSV)** | While the Table tab is open: the tips it lists, with its visible columns; otherwise every tip with every column. |
| **Copy the IDs** | The same tips' IDs to the clipboard. |
| **Group “…” with its data (TSV)** | One item per group: its tips with their data. |
| **Groups (TSV)** | Every group and its tips (columns `group`, `genome`); Groups → *import* reads it back. |
| **My fields (annotations TSV)** | Your own editable columns. |

---

## 8. Layouts and branch order

Choose the layout in the toolbar or in the Shading tab. Folding, selection, highlights, groups, shading and clade names
work in all three layouts.

### 8.1 Rectangular

The default. Horizontal position is the distance from the root (or RED, [§11](#11-relative-evolutionary-divergence-red));
each tip or folded clade has its own row. Folded clades are triangles whose height grows with the logarithm of their
size. The scrollbar and width-only zoom (Shift+wheel) are available here.

### 8.2 Circular

Rows become angles around a circle (with a 10° opening at the top) and distance from the root becomes the radius.
Folded clades are wedges, RED guides are circles, clade names sit outside the circle and are spread so they never
overlap (with leader lines when moved). Tip names are aligned on an outer ring with dotted guides. Trait strips
become rings between the tips and their names ([§12.3](#123-annotate-tab-tip-labels-categories-numbers)). Readable up to
about 4,000 rows; larger expansions are guarded ([§6.4](#64-guarded-expansion-circular-and-unrooted)).

### 8.3 Unrooted

An equal-angle unrooted drawing: each clade gets a cone proportional to its size, so branches never cross. Folded
clades are wedges reaching their deepest tip; clade shading is drawn as cones. **Branch lengths:** *as is*,
*square root* (shortens long branches so one long outgroup does not squash the rest) or *equal*. Clade names try
nearby positions with leader lines. Trait strips become dots beyond each tip and pies beyond folded clades
([§12.3](#123-annotate-tab-tip-labels-categories-numbers)). RED is not available in this layout. Readable up to about 5,000 items.

### 8.4 Branch order

New trees open **laddered right**: at every node the larger clade is drawn above the smaller one, so small clades and the outgroup end up at the bottom. Right-click empty space → *Ladder left* reverses it; *As in the file* restores the file's order. Saved sessions keep the order they were saved with.

The **outgroup** — the root's smallest clade — is drawn at the bottom by default (right-click empty space → *Outgroup at the bottom* to turn it off). It applies after ladderizing or matching another tree; flipping the root puts it on top again.

Branch order is a display setting; the tree itself is not reordered.

- **Flip branches here** (right-click a clade) swaps its children.
- **Ladderize** (right-click empty space) puts the largest clades at the bottom or top.
- **Match the order of another tree…** places each clade by the average position of its tips in another tree
  (Newick file or a list of names) — useful for comparing two trees side by side.
- **As in the file** restores the original order.

Order changes are undoable, saved in sessions, and used by *Download tree as Newick (as displayed)*.

---

## 9. Search, queries and groups

### 9.1 The search box

Press `/` (or Ctrl+F / ⌘F) and type. Results list tips and taxa as you type; ↑ ↓ move, **Enter** jumps to the
selected result (the tree opens only the path to it; everything else stays folded), **Shift+Enter** turns all matches
into a highlighted group, Esc closes.

### 9.2 Query syntax

The same syntax works in the search box and in the Table tab filter.

| Example | Meaning |
|---|---|
| `Escherichia` | name or taxon containing the text |
| `GCA_0188*`, `*metabat*`, `4018??_*` | wildcards: `*` any text, `?` one character |
| `habitat=hot_spring` | exact value (case-insensitive) |
| `isolation_source:spring` | value contains |
| `checkm2_completeness>=90 genome_size<3000000` | numeric comparisons; all terms must match |
| `genus=Bacillus`, `phylum:Omni` | taxonomy ranks as fields |
| `-source_type=context*`, `habitat!=soil` | exclude |
| `habitat="spring aquatic"` | quotes for values with spaces |

Unknown field names and non-numeric comparisons are reported instead of silently matching nothing.

### 9.3 Groups: your sets of tips

A **group** is a named, coloured set of any tips — your own MAGs or isolates, the sequences from one study, near-complete
genomes, a search result. Groups are listed under **Groups** in the side panel and appear in the Table tab (the
*groups* column, the *Group: …* scope, and the filter `group="My group"`). A tip can be in several groups.

A new group shows nothing on the tree until you switch it on (the dialog for a new group offers both):

- **☐ highlight** — the branches to its tips are coloured and their names bold in the group's colour (no extra
  symbol at the tips), folded clades say how many of its tips they hold, the overview shows ticks. Names of clades
  holding highlighted tips are always kept: where it is crowded the name moves over the clade's stem, below the
  branch, or is shortened to its first rank (*f__X* of *f__X; g__X*);
- **Aa label on the tree** — its largest clades are named on the tree, fold as triangles, appear in the legend and
  figures and in *Tree with my group names*. Tips in separate clades give one label each: *name [1/3]*,
  *[2/3]*… (the dialog warns above 20).

**Make a group** from:

- **IDs** — Groups → *+ paste IDs* (one ID per line, matched leniently, with a report of IDs not in the tree) or
  *import*: a list of IDs (one group, named after the file), a table with a `group` column and a tip column (one
  group per value), or a groups table saved by Export ▾ → *Groups*;
- **the tree** — Ctrl+click (⌘+click on a Mac) branches anywhere in the tree; they turn orange and a bar offers
  **Highlight**, **Make group…**, **Add to group…** and **Clear** (or Esc). Ctrl/⌘+click a picked branch again to
  unpick it; picking a clade replaces the picks inside it. Right-click a clade → *Create group…* (or `g`) or *Pick
  this branch*;
- **the Table tab** — filter it (e.g. `checkm2_completeness>=95 habitat=soil`) and press **Group…**, or tick
  tips and press **Group…** in the ticked bar;
- **search, the monophyly tab, a GTDB-Tk summary** — their *Highlight* actions make a group that is already
  highlighted.

**Each group** (first line: ☐ highlight, colour, name — double-click to rename —, number of tips; second line):
**Aa** label on the tree, ⊙ **focus** (fold every clade that contains none of its tips), ‹ › step through its
tips, ☰ **open in the Table tab** (see its tips, tick and remove some, add others:
[§13](#13-the-table-tab)), ▤ show as a marker track, × delete. Hover any of them for a short explanation.

Groups → **merge…** combines ticked groups into one (each tip once; keep or remove the originals); **clear**
deletes them all. Everything can be undone (⌘Z / Ctrl+Z).

**Fold everything except…** (Groups → *fold the rest…*, right-click empty space, or *Fold the rest…* in the Table
tab's ticked bar) keeps tips in view and folds the rest of the tree:

- **keep in view:** all highlighted groups, any one group, the tips of a marker track (older *Mark genomes* sessions), or the
  tips ticked in the Table tab;
- **fold the rest:** into the largest clades without them, or **to a rank** (e.g. phylum): everything else becomes
  named triangles and only the branches leading to your tips stay open.

Right-click empty space → *New tree from "group"* prunes the tree to a group. Export ▾ → *Table* downloads one
group with its data; Export ▾ → *Groups* writes every group with its tips (columns `group`, `genome`), which *import* reads
back. Groups are saved in sessions and autosaved in this browser.

---

## 10. Taxonomy: monophyly, comparison, rooting and editing

### Ranks: GTDB, PR2 5 and PR2 4 (18S)

Treezer reads two kinds of taxonomy ranks:

| Ranks | Lineages look like | Recognised |
|---|---|---|
| **GTDB** — domain, phylum, class, order, family, genus, species (also Greengenes, SILVA prokaryotes) | `d__Bacteria;p__Bacillota;…` or `Bacteria;Bacillota;…` (by position) | prefixes, or six–seven names |
| **PR2** (18S) — domain, supergroup, division, subdivision, class, order, family, genus, species | `Eukaryota;TSAR;Alveolata;Dinoflagellata;Dinophyceae;…` | nine names without prefixes |
| **PR2 4.x** (18S, older studies) — kingdom, supergroup, division, class, order, family, genus, species | `Eukaryota;Alveolata;Dinophyta;Dinophyceae;…` | eight names without prefixes |

The ranks are chosen automatically from the lineages; Load data → **Ranks** chooses them by hand (a link: `ranks=pr2`).
The rank buttons, menus, the Monophyly tab and shading follow them. PR2 names every level — a level without its own
name is the name above plus `_X` (`Chlorophyta_X`) — and Treezer keeps those names, so folding at any rank gives named
clades. PR2 names are shown and exported without prefixes (taxonomy, conflict, RED and named-taxa tables); the taxonomy export writes PR2's own format. A session remembers the
ranks.

### 10.1 How taxa are placed on the tree

When a taxonomy is loaded, Treezer finds for every taxon the **largest clades whose classified tips (genomes, sequences…) all belong
to that taxon** ("pieces"). A taxon whose tips form one clade gets one piece and is shown as a single named clade.
A taxon split by other tips gets several pieces, shown as `g__X [1/3]`, `g__X [2/3]`, `g__X [3/3]`; each piece is
folded and coloured on its own, and unrelated pieces are never merged.

**Unassigned tips** (no taxon at that rank, such as new MAGs) do not break a clade: a genus clade that also contains
an unclassified bin is still one piece. They are counted in the clade's size.

The common ancestor of a taxon's tips is never taken as proof that the taxon is a clade.

### 10.2 The monophyly tab

| Element | Meaning |
|---|---|
| **Checking the taxa of …** (top row) | Which taxonomy's taxa are checked against the tree: e.g. *GTDB taxonomy*, *NCBI taxonomy*, *GTDB r226 taxonomy* (any loaded lineage column or taxonomy file; its column or file name is shown in brackets). Switching is instant; with one taxonomy the row says how to load another. A GTDB tree checked against its own GTDB taxonomy has no split taxa by design — the interesting checks are other taxonomies, or your own tree. |
| **split taxa per rank** | Clickable counts for phylum … species; ranks with split taxa in red. The tab opens at the highest rank with splits. |
| **pieces** | Tips in each piece, largest first (e.g. `536+10`). |
| **main** | Share of the taxon's tips in its largest piece. |
| **outside** | Tips outside the largest piece ("strays"). |
| **support** | Highest support among the branches that separate the pieces. |
| **verdict** | strong / weak / no support for the split, against an adjustable threshold (default 95). |
| per-taxon actions | highlight the taxon, highlight its strays, highlight **intruders** (tips of other taxa inside the taxon's common-ancestor clade), jump to each piece. |
| **highlight all N strays** · **export TSV** | All tips outside their taxon's main piece; the export has one row per stray (the list on screen stops at 500 taxa). |

The table can be sorted by any column.

### 10.3 Comparing two taxonomies

Every taxonomy column of a loaded table is picked up for comparison automatically, ticked or not — in GTDB metadata
`gtdb_taxonomy`, `ncbi_taxonomy`, `ssu_silva_taxonomy` (16S), `ssu_gg_taxonomy` (Greengenes 16S) and
`lsu_silva_23s_taxonomy` (23S). Lineages without rank prefixes (SILVA, 16S, a hand-made file) are read by position:
domain, phylum, class, order, family, genus, species. Placeholder names (*uncultured*, *unidentified*, *Incertae Sedis*,
*none* …) count as no taxon at that rank, so they never join unrelated tips into one fake taxon. Example on the GTDB
r232 archaeal tree: NCBI splits 42 phyla and 87 genera, SILVA 16S 24 and 105, GTDB itself none.

Load a table that holds two lineage columns (e.g. GTDB metadata with `gtdb_taxonomy` and `ncbi_taxonomy`), or a second
taxonomy file, and switch between them in the top row of the monophyly tab (*Does each taxon form one clade in this tree? Checking the taxa of …*).
A taxonomy file loaded onto a tree that already has a taxonomy fills in tips without one *and* is kept on its own
under its file name, so you can compare e.g. GTDB r232 with `ar53_taxonomy_r226.tsv` (genomes new in r232 simply have no
r226 taxonomy and do not break any clade). Example: on the GTDB bacterial tree, 159 of 181 NCBI
phyla and 1,801 of 4,515 NCBI genera are not single clades, while every GTDB taxon is. The same works for two GTDB
releases (load the r226 and r232 taxonomy files).

### 10.4 Rooting and monophyly

Whether a taxon is a single clade depends on the root. If the root has more than two children the tab warns that the
tree looks unrooted. Fix it with:

- right-click a clade → **Root here** (branch above the clade);
- right-click empty space → **Midpoint root**;
- monophyly tab → **Root so that [taxon] is on one side** (an outgroup, e.g. a sister class, or the ingroup at any rank;
  the rank starts at the one the tree is folded to, phylum by default);
- right-click a clade or tip → **Root on the whole p__X (N tips)** — the whole taxon, even when the folded clade you
  clicked is only one piece of it (*Root here* roots above the clicked piece only).

Re-rooting keeps metadata, fields, taxonomy, highlights and groups, **and your folds**, and can be undone. A tree folded
to a rank is folded to that rank again (a taxon split before may now be one clade); otherwise every folded clade that is
still a clade after re-rooting stays folded (a clade the new root cuts through opens).

### Taxonomy names written in the tree file

Some trees carry taxon names on their internal nodes (GTDB's trees do: `100.0:p__X; c__Y`). These become a taxonomy of
their own, **names in the tree file**, used until you load another one. A loaded taxonomy (a GTDB table, or NCBI from
the metadata) then replaces them everywhere — folding, clade names, the monophyly report, RED — so names from two
taxonomies never mix; switch back under *Checking the taxa of …* in the Monophyly tab. Tips alone in their taxon have
no name in a tree file (names sit on internal nodes), so they show as *no phylum* etc. until a taxonomy table is loaded.

### 10.5 Editing taxonomy

Editing changes the taxonomy the viewer uses (clade names, folding by rank, the monophyly report, shading); the file
you loaded is never changed. There are two ways to edit, both in the Table tab:

**Set one rank for several tips** — *✎ Edit taxonomy: set [rank] to [name] [set for …]*, and *remove…* next to it

- **Which tips:** the **ticked** tips if any are ticked; otherwise **every listed tip** (the rows the table
  shows: selected clade, whole tree or a group, after the filter). The button always says which, e.g.
  *set for 3 ticked tips*, *set for 120 listed tips*, *set for all 997 tips*.
- **What changes:** that rank — and, only if you tick the fixes below, the ranks they name. Nothing else: moving
  tips to another order does not by itself change their class, family, genus or species.
- **Name:** with or without the prefix (`MyOrder` = `o__MyOrder`); *set for …* stays off until a name is typed. To
  **remove** that rank's name from the same tips, use *remove…* (you are asked to confirm). Names cannot contain `;`, tabs or line breaks, and placeholder words (`uncultured`, `unknown`,
  `none`, …) are refused because they mean "no name".
- **Preview:** as soon as you type a name, a line under the form says what will happen — how many tips change and
  from what — and warns where the edit would leave the hierarchy inconsistent:
  - the name **already exists under a different higher taxon** (e.g. `o__X` is in `c__A`, your tips are in `c__B`);
  - a **lower taxon would be split**: some of its tips are not in the edit (e.g. you move half of `f__Y` to a new
    order, so `f__Y` would sit in two orders).
- **Fixes** (tick boxes in the preview, shown only when they apply):
  - **Also set class = c__A, … — where o__X already is** (on by default): tips moved into an existing taxon take
    its higher ranks, so `o__X` stays in one class. Offered only when the existing tips agree on those ranks.
  - **Remove f__Y, … from the tips that change** (off by default): the split lower names are removed from the
    tips you move, so `f__Y` stays in one order. Only **family and above** are removed: genus and species names
    come from published descriptions (and a species name carries its genus), so they are never removed — a split
    genus or species stays as a warning for you to decide.

  With a fix off, its warning stays and the edit is still allowed; fix the other ranks with further edits if you
  prefer.
- **Species and genus:** a species name carries its genus (`s__Raphidiopsis brookii`). Setting a genus or species so
  that they no longer match (e.g. genus `g__NewGenus` with species `s__Raphidiopsis brookii`) is warned about.
- **Suggestions while typing:** names already in the taxonomy at that rank (matching anywhere, prefix optional); for a
  species, when the genomes share one genus, a GTDB-style name for a new species from the first genome's accession
  (`s__Raphidiopsis sp001784615` for GCA_001784615.1).

**Replace one tip's whole lineage** — double-click its **✎ Taxonomy (in use)** cell (or the column the taxonomy was read
from, e.g. `gtdb_taxonomy (from the file)`), type the lineage and press Enter (Esc cancels):

- prefixed ranks (`d__…;p__…;…;s__…`, `k__` is read as `d__`); ranks you leave out, or leave empty like `o__`, are
  removed; a rank given twice is refused;
- while you type, a line under the toolbar warns when a taxon in the new lineage would sit under a different higher
  taxon than its other tips (e.g. you change the order but keep `f__Y`, whose other tips are in `o__A`), or when
  the species name carries another genus than the one given. The warnings do not block saving.

Every edit is one undo step (⌘Z / Ctrl+Z). The edited taxonomy is marked "(edited)" so the original stays selectable
under *compare with*. Export it with **Export ▾ → Taxonomy, GTDB format**. Edits are kept
in saved sessions but not yet autosaved in the browser.

### 10.6 Naming a clade as a taxon

Right-click a clade → **Name as taxon…**:

- **rank** starts at the first rank the clade has no name for;
- **name** with or without the `g__`-style prefix; existing names are suggested while you type, and for a species
  a GTDB-style name for a new species is offered (`s__<genus> sp<accession digits>`, from the clade's first genome);
- a **RED bar** shows the clade's RED against this tree's rank bands, with a *use <band>* button;
- **warnings** appear live: the name already exists at that rank, the clade spans several taxa at the rank above,
  higher ranks are empty, the name is used elsewhere, the clade is already named, or a species name does not match the
  genus;
- **fill only empty** (default) or **replace** existing names.

**A single tip** can be named too (right-click the tip → *Name as taxon…*), e.g. an unplaced MAG as a provisional
new phylum. A tip's own RED is always 1, so the dialog shows the RED of the node it **branches off** from instead,
against the rank bands — e.g. "Branches off at RED 0.637 — in the phylum band".

Named taxa are logged (Export ▾ → *Taxa you named*) and can be undone. Example: a clade of new MAGs named as a
provisional genus, its RED shown against the genus band.

### 10.7 Representatives: one tip per taxon

Right-click a clade (or empty space for the whole tree) → **Pick one tip per taxon…**: choose a rank and a score —
CheckM2 quality (completeness − 5 × contamination), any numeric column, or first in tree order — and whether to keep
tips without a taxon at that rank. The chosen tips become a highlighted group, ready to export or to open as a new
tree.

---

### 10.8 Fold by any column (habitat, host, country, …)

The monophyly question works for any category, not only taxa: *does each habitat (host, country, sample, …) form one
clade in this tree?* Open **Monophyly** and switch **Check** to *any column*. Pick a column; columns of repeating text
values are offered (and number columns with at most 50 values, e.g. 0/1). Whole-lineage columns are checked under
*a taxonomy* instead.

**Fold the tree by <column>** folds the tree into the largest clades whose tips share one value, by the same rules as
taxa ([§10.1](#101-how-taxa-are-placed-on-the-tree)):

- tips **without a value** (empty, NA, none, unknown, -, ?) do not break a piece, but a piece is the smallest clade
  holding its tips, so blank tips at its edge stay out;
- a value found in several places is split into **pieces**, never merged: `soil [2/5]` is piece 2 of 5;
- a piece that also holds tips without a value says so: `soil [2/5] (3 of 40)` — 3 of its 40 tips are from soil;
- clades with no value at all are folded too, named `no <column>` in grey (labelled only where there is room);
- folded clades are coloured by value (the 12 most common; the rest grey), and the legend lists the colours.

The list gives, per value, its tips, the number of pieces and the share in the largest piece (*main*); sort by most
pieces, most tips or name. **highlight** marks every tip with the value; the numbered buttons jump to its pieces.
**unfold** (or *expand all*, or a rank button) ends the fold. The fold can be undone, survives re-rooting (the pieces are
worked out again on the new tree) and is kept in saved sessions.

With sparse data, a piece may be a large clade in which only a few tips have a value; read the "(k of n)" count.

## 11. Relative evolutionary divergence (RED)

RED places every clade between the root (0) and the tips (1), correcting for unequal rates of evolution. GTDB uses it
to make ranks comparable across the tree. Treezer computes RED on the loaded tree with its current root, the same way
as PhyloRank with a single rooting.

| Feature | Where |
|---|---|
| **RED layout** | Toolbar **RED**: horizontal position = RED from the current root, with a dashed line at each rank's median RED computed **as GTDB does** (PhyloRank): only taxa with at least two named taxa below them (e.g. phyla with ≥ 2 classes) count, and each taxon's RED is its median over trees rooted on each phylum with ≥ 2 classes.  A tree with fewer than two such phyla uses its own root. GTDB r232 archaea: phylum 0.29, class 0.43, order 0.57, family 0.76, genus 0.93. |
| **Rank bands** | Bands (rectangular) or rings (circular) for each rank, halfway between neighbouring medians. |
| **Node colouring** | RED chip: *plain*, *coloured by rank band*, or coloured with RED values written on nodes. |
| **RED of a clade** | Selection details: RED and the closest rank (tooltip: between which ranks). |
| **Naming guidance** | Name as taxon shows the RED bar against the bands ([§10.6](#106-naming-a-clade-as-a-taxon)). |
| **Export** | Export ▾ → RED values for every clade (TSV). |

**Cautions.** RED depends on the root: a chip shows "RED from root: …", and a warning appears when the tree looks
unrooted. The rank lines follow GTDB's method (above), but the tree's positions use its current root, so a clade's own RED can differ from the value GTDB lists for it. RED from
GTDB-Tk (`red_value`) refers to GTDB-Tk's own reference trees and is not directly comparable. RED is not available in
the unrooted layout.

---

## 12. Annotating the tree

### 12.1 Groups on the tree

Groups are your sets of tips ([§9.3](#93-groups-your-sets-of-tips)). Switch on **Aa** to label a group on the
tree: its largest clades are named in its colour and fold like any clade; a group spread over several clades gets one
label per clade, *name [1/3]*, *[2/3]*… — unrelated clades are never merged. Labels follow the group: renaming,
recolouring or adding tips updates them, and they survive re-rooting, pruning, sessions and reloading. Export ▾ →
*Tree with my group names* writes them as internal node labels in Newick.

Groups saved by earlier versions of Treezer (named clades) open as labelled groups.

### 12.2 Clade shading and clade names

Shading tab → **Shade clades at [rank]** shades every taxon at that rank as one filled area from its node to its leaves
(split taxa per piece).

| Option | Choices |
|---|---|
| Style | **fade** (clear at the node, full colour at the leaves; default), **solid**, **band** (a strip at the leaves only) |
| Depth | How far in from the leaves the shading reaches (default 50%; 100% = from the clade's node) |
| Palette | pastel, muted, colour-blind safe, earth, grey |
| Opacity | slider |
| Names | prefix on/off, tip counts, font size, minimum tips to name a clade |
| One-tip clades | shade and name · shade only · leave out. With *shade and name*, the clade name is the tip's only name, like any other named clade (no second "p__X (1)"); tick **their tip names too** to show the tip name as well. *Shade only* and *leave out* also drop the "p__X (1)" name that a tip alone in its taxon otherwise gets when the tree is folded to a rank |
| Tip names | on/off |
| Per taxon (Taxa list) | own colour, own label text, hide name, hide shading; colours stay stable when others are hidden |

**Moving names.** Drag any clade name on the tree to move it; a leader line connects it to its clade. Moved names are
remembered per taxon and layout; ↺ puts one back, *put N moved names back* resets all.

Folded clades inside a shaded clade take its colour; branches stay neutral under shading, while user groups still
colour their branches.

### 12.3 Annotate tab: tip labels, categories, numbers

Annotation tracks (strips) are drawn with the tree, in every layout. Open the **Annotate** tab. The left side lists the tracks on
the tree — each with its full name and kind (double-click the name to **rename** it; the new name is used in the track
header, the legend and folded-clade names), ⚙ settings in three parts (*Where and how it looks*, *Which values*, *Folded
clades*), ‹ › order and × remove; a heatmap is one row and moves as one. The right side asks **what do you want to
add?** Each card has a small picture of what it does to the tree, in three groups: *Tip labels*, *Data from your table*
(Categories, Numbers) and *More* (Links, Genes and pathways). The list on the left uses the same headings. Under it,
**Sizes** sets one size for **all symbols** and one text size for **all tip labels** (tip names; clade names keep
their size). In each form, the filled dark button (*Show as tip names*, *Add symbols*, *Add bars*…) is the action;
the outlined chips are choices. The action stays in view at the bottom of the panel while you scroll.

| Choice | Reads the column as | Example |
|---|---|---|
| **Categories** | a column with a limited set of values (also numeric codes such as 1, 2, 3) — **where**: **symbols at the tips** or **symbols in a column** (each value its own shape ● ■ ▲ ◆ ★ ✱ ✚ and colour), a **colour strip** beside the tree, **coloured names**, **coloured branches**, or names and branches | habitat · country · region · isolate or MAG |
| **Numbers** | numbers (also text such as `3.2 Mb`, `45%`, `1,234`; `NA` / `none` = no value). Tick **one column**: a colour scale, bars, **bubbles at each tip** or **values as text**; tick **several**: a **heatmap** (side by side, one block) or **stacked bars** (one bar per tip split by each column's share; click a column's colour in the list to change it). **Colours** in the form (and ⚙): a preset scale, **your own low and high colours** (optionally a middle one), and the **bar colour** | tip size · GC % · completeness · abundance per sample |
| **Tip labels** (own section at the top of the Annotate tab: they change the text at the tips, they are not tracks) | text from **a column or the taxonomy** — the *whole lineage* or one rank (species, genus, … phylum; prefix such as s__ dropped unless you tick *keep the prefix* in ⚙) — shown **instead of** each tip's name (one source at a time) or **after** it — add several *after* fields (e.g. habitat, then NCBI taxonomy): each follows the name in list order, in its **own colour** (change it in the annotation's settings) and is cut at *at most N characters* (default 40) with …; **hover a tip** to read its name and every field in full. Optionally *italic*. Single tips: right-click → **Edit label…**. Tips without a value keep their name; the table, search and exports keep the IDs; figures list the fields in the legend | species names instead of accessions · strain · habitat and NCBI taxonomy after the name |
| **Links between genomes** | a table of pairs (genome, genome[, weight][, category]; tab or comma, header optional; IDs matched leniently) — curves beside the tree (rectangular), chords through the circle, curves in the unrooted layout; links into a folded clade attach to it and links between the same two items merge into one thicker line; colour by category, *mixed* when merged links differ | horizontal gene transfer · shared plasmids |
| **Genes and pathways** | present / absent per gene, KO or pathway from a feature table — several genes added together form one compact **gene grid** (*as one gene grid*, on by default), listed and moved as one | KEGG KOs, DRAM results |

As in chiPlot, **the kind of annotation decides how a column is read**, not the type the file seemed to have. After
choosing a kind, the columns are listed with what they hold — for categories the number of values and the most common
ones, for numbers the range — and columns that cannot be read that way are shown greyed with an example of why.
Implausible choices are flagged (e.g. genome sizes as categories: "many different numbers … Numbers fits better").
⚙ → *read as categories / numbers* changes it afterwards. Taxonomy columns are not offered here (they belong to the
monophyly tab), except for Tip labels, which can show a whole lineage.

A choice that needs data you have not loaded is greyed out and says what is missing. The details of each:

- **Add a metadata column** — categories (one colour per value) or numbers (colour ramp);
- **Add a heatmap** — tick two or more number columns: they are added as one block, side by side without gaps, sharing
  one colour scale (the range over all of them) and one legend entry. Changing the colours, scale, range or width of one
  column changes the whole block. ⚙ → *heatmap scale*: **automatic** (the default: per column when the columns' ranges
  differ more than tenfold, otherwise shared; the chip says which it chose), **shared** (one range for all columns; for
  values in the same units, e.g. several abundances) or **per column** (each column from its own minimum to maximum; for columns in different
  units, e.g. GC % and genome size — the legend then lists each column's range);
- **Add features (presence / absence)** — from a feature table, e.g. KOs; filter (`K00001`, `hydrogenase…`), group
  (e.g. by DRAM module), add the selected or all shown (up to 60).

**Number strips as bars.** ⚙ → *show as: bar* draws a bar as long as the value instead of a coloured cell. A folded
clade shows its **mean as the bar and its range (minimum–maximum) as a thin line** across it, so a clade with a wide
spread is not mistaken for a uniform one. When the range spans zero, bars start at zero and negative values take the
other colour of the diverging ramp. In the circular layout bars point outward within the ring; in the unrooted layout
bar strips are shown as coloured dots.

**How strips look in each layout:**

| Layout | Strip | Tip | Folded clade |
|---|---|---|---|
| Rectangular | a column right of the tree, names angled above | one cell per row | stacked category shares across the cell, mean colour, or % with a feature |
| Circular | a ring just outside the tips (aligned, with dotted guides from the tips); names in the opening at the top, which widens to fit them | one cell per tip | categories stacked outward within the ring |
| Unrooted | a row of dots beyond each tip along its branch, one per strip; tip names start after them | a dot | a small **pie** of the categories, or a dot for numbers / presence, beyond the folded wedge |

**On folded clades and sub-pixel subtrees**, strips show a summary: stacked category shares (pies in the unrooted
layout), the mean colour for numbers, or the share of tips with a feature (any presence is drawn at least at a
visible shade). Hover a cell or dot for its value; click to
select the tip or clade. Strips are included in exported figures and in the figure legend.

**Colouring tip names and branches.** *Colour by category* → where: *tip names*, *branches* or *names and
branches* colours the tree itself instead of adding a track (⚙ → where switches later). A tip's name takes the colour
of its value; **a branch takes a colour when every tip below it has the same value** (tips without a value do not
break it), so whole clades light up and branches leading to a mix stay neutral; a folded clade takes the colour when its
tips agree. It works the same in every layout and in figures, and the legend names the column. If several colourings
are set, the last one in the list wins for names and for branches.

**Symbols per value.** *Categories* → *symbols at the tips* puts a symbol right at each tip (before its name), its
shape and colour given by the tip's value; *symbols in a column* puts the same symbols in a column beside the tree (a
ring in the circular layout). **Change a value's shape and colour** right in the list on the left: each value has a shape
menu (● ■ ▲ ◆ ★ ✱ ✚) and a colour box. A **folded clade shows a small pie** of the values inside it (one value: its
symbol). With *show only these values* (⚙), only the ticked values get a symbol. Different shapes also survive
black-and-white printing. Symbol size: Annotate → *Sizes* → *symbols* (one setting for all). Highlights, the selection and your coloured groups
are drawn on top. Values such as *none*, *NA*, *unknown* count as no value (⚙ to change), so they never colour half the
tree.

**Yes / no columns** are categories like any other: show them as symbols and leave out the "no" value (⚙ → *show only
these values*), so only the "yes" tips get a symbol.

**Show only these values.** ⚙ on a category strip lists its values (most common first): tick the ones that matter. Only
those keep their colour; every other value becomes one light grey *(other values)*. In folded clades a ticked value is
never drawn thinner than a visible sliver, so a clade with a single hot-spring tip still shows it (hover for the
exact counts). *show all* goes back.

**Strip options (⚙):** width (narrow / normal / wide); for numbers *show as* colour or bar; unknown values (*unknown*, *unreviewed*, *NA*, … drawn as no data
and left out of summaries); for numbers: linear or log scale, colour ramp (blue, green, orange, purple, grey,
diverging), fixed min–max (values outside are clamped). Reorder with ‹ ›, remove with ×.

### 12.3a Marked tips (older sessions)

*Mark genomes* (the old name) was removed in 0.6 (2026-10): **groups** show tips you pick (highlighted on the tree), and **symbols
come from columns** (*Categories* → symbols). Marks saved in older sessions still open, draw and can be removed; their
⚙ settings still work. To mark tips matching a search (e.g. `completeness>=90`), make a group from the search.

### 12.4 iTOL annotation files

Existing iTOL annotation files can be dropped onto the viewer ([§3.2](#32-data-tables-load-data-or-drop)). Colour strips
become strips, clade colours and ranges become coloured groups, text labels become named groups, and collapse lists
become folds. All are undoable.

### 12.5 Legends

Figure legends are added in the export dialog ([§15.2](#152-publication-figures)): *none*, *beside*, *below*, or *a
separate SVG*. They list what the figure shows: shaded taxa, strip categories and number ramps, highlighted groups,
coloured groups and RED bands (up to 80 entries per block).

### 12.6 Branch and node styles

Open the **Style** tab at the bottom.

| Setting | Choices |
|---|---|
| Line width | 0.5, 1, 1.5, 2, 3 px (highlighted, selected and group branches stay a little thicker) |
| Branch support | **off**, **node dots** (dark = strong, grey = moderate, none = weak), **branch colour** (strong branches dark and thick, moderate grey, weak light and thin), **values** (the number beside each node) |
| Value | for nodes with several values, e.g. IQ-TREE's `80.5/97` (SH-aLRT / UFBoot): which one to use |
| Thresholds | strong ≥ and moderate ≥ (defaults 95 / 70 for percentages, 0.95 / 0.7 for probabilities; *defaults* resets) |
| Branch lengths | the length above each branch, where it fits |
| Scale bar | a bar for a round branch length (e.g. 0.1), bottom left; on by default |
| Draw very long branches short | *Root and outgroup* → **draw very long branches short (marked //)**: a branch longer than the rest of the tree is wide (e.g. one distant outgroup genome), and root branches longer than the tallest clade below the root, are drawn as a short stub marked **//**, so one outlier does not squeeze the tree. Everything else keeps its scale; the scale bar stays true. Greyed out when no branch is that long. The same place folds the outgroup into a small triangle or swaps the root's sides. Rectangular and circular layouts and figures; not in the RED layout |

Support values are read from internal node labels: `90`, `100.0:p__…` (GTDB), `0.98` (posterior probabilities are
recognised and get their own thresholds), and IQ-TREE's `80.5/97`. A node's support belongs to the branch above it; the
root's label is not support. Typical thresholds: UFBoot 95, standard bootstrap 70, SH-aLRT 80, posterior 0.95.

Dots, values and branch lengths are drawn where nodes are far enough apart, so they appear on small trees and as you zoom
in on large ones; branch colouring works at every size. Values and lengths never cover tip names. All styles work in
the three layouts and in exported figures; the figure legend explains the support dots or colours. The scale bar is not
drawn in the RED layout (the axis is RED, not branch length) or in the unrooted layout with *square root* / *equal*
branch lengths. Styles are saved in sessions.

---

## 13. The Table tab

Open the **Table** tab at the bottom (or right-click a clade → *Show these tips in the table*). It lists the tree's
tips — genomes, 16S sequences, viral samples, whatever your tree holds — one row each. (Before 0.6.2 this tab was
called *Genomes*; links with `panel=genomes` still open it, as does `panel=table`.)

| Feature | How |
|---|---|
| Scope | **Selected clade**, **Whole tree**, or a group (☰ beside a group in the Groups list opens it here) |
| Filter | Query syntax of [§9.2](#92-query-syntax), e.g. `checkm2_completeness>=90 genome_size<3000000` — or **several tip names / accessions** (spaces, commas or new lines between them): the table lists exactly those, like R's `%in%`, and says which were not found (IDs are matched leniently: `GCA_0001` = `GB_GCA_0001.1`) |
| Row numbers | `#` at the start of every row, in the table's current order |
| Tick tips | the box at the start of a row; **Shift+click** ticks (or unticks) a range; the box in the header ticks all listed tips. Ticked tips are pink in the table **and on the tree**. With tips ticked: *Highlight as new set*, *Add to set…*, *Remove from "set"* (when the table shows a set), *show only ticked*, *Copy IDs* (Shift: download), *Untick all* |
| Column width | drag the right edge of a column header; **double-click** it to fit the column to its text (widths are remembered). Number, tick box and tip name stay in place when you scroll sideways |
| Sort | Click a column header (numbers sort numerically; missing values last) |
| Columns | *Columns* menu: show/hide any column, including taxonomy ranks (remembered) |
| Jump | Click a row to jump to the tip on the tree |
| Your own fields | **+ field** adds an editable column; double-click a cell to edit; **set for N** fills it for every listed tip |
| Taxonomy editing | [§10.5](#105-editing-taxonomy) |
| Highlight | **Highlight N** (listed tips) or **Highlight each value of…** |
| Export | In Export ▾ → *Table*: the table as listed (with the visible columns) while the table is open, its tip IDs, each group; the taxonomy and your fields are in Export ▾ too |

Columns loaded from files are read-only (double-clicking explains why); your own fields are autosaved in the browser
([§16.3](#163-autosave-in-the-browser)). Numbers are shown without floating-point noise; the tooltip shows the stored value.

---

## 14. Subtrees, re-rooting and undo

### 14.1 Subtrees

**Open this clade as a new tree** (right-click a clade) or **New tree from "set"** (right-click empty space) prunes the
tree to those tips. Distances between the kept tips are preserved, nodes left with a single child are merged, and
data, groups and highlights follow by tip name. **← back** returns to the previous tree. Download the subset with
*Download tree as Newick*.

### 14.2 Re-rooting

**Root here** (on a clade or tip), **Midpoint root**, or **Root so that [taxon] is on one side** (monophyly tab). The
new root is placed on the middle of the chosen branch; support values move with their branches. Folds are kept (see
above). The status remembers how
the tree was rooted ("as loaded", "midpoint", "branch above X", "taxon on one side"); sessions keep it.

### 14.3 Undo and redo

↶ ↷ (⌘Z / Ctrl+Z, ⇧⌘Z / Ctrl+Y) undo up to 50 steps of: folding, re-rooting, groups, branch order,
taxonomy edits and named taxa, field edits and new fields, clade shading settings, iTOL imports. The tooltip and status
line name the step. Not yet undoable: switching the compared taxonomy and strip settings.

---

## 15. Exporting figures and data

### 15.1 Figures of the current view

Export ▾ → **SVG**, **PNG 2×**, **PNG 4×**, or **PDF**: exactly what is on screen. SVG is vector and editable in
Illustrator or Inkscape, with real arcs and gradients.

### 15.2 Publication figures

Export ▾ → **Publication figure…** opens a full-window view that draws the **whole tree** at a print size, independent
of how far you are zoomed in. The figure is in the middle, its settings are on the right (below it on a phone), and the
bar at the top has the format (SVG, PDF, PNG) and **Export**. **← Back to the tree** (or Esc) returns to the tree,
which is unchanged. You can export several formats one after another without leaving the view.

The figure in the middle **is** the exported figure, drawn live by the same code as the export, so it changes as soon
as you change a setting. Drag to move it, use the mouse wheel (or + and −) to zoom, **Whole page** (key 0) to see all of
it, and **Print size** (key 1) to see it at about the size it will have on paper. The percentage is relative to that
print size.

**Changing names on the figure.** Changes made here belong to the figure only: the tree on screen and the taxonomy
are not changed. They are kept while Treezer is open and saved in session files (Export ▾ → Save session).

| To | Do this |
|---|---|
| Move the legend | Each block of the legend (e.g. *Order (shaded clades)*, a heatmap's scale) moves on its own: click and drag it. **Shift+drag** moves the whole legend. Legend → **on the figure** adds no room beside or below the tree, so you can put the legend in a free spot on it |
| Change the legend's words | Double-click a legend title or line (or select it and press Enter) to reword it; colour, italic and bold as for names; Delete hides a line. A shaded clade's line follows when you reword that clade's name on the figure (unless you gave the line its own words). Dragging a legend text moves its block |
| Move a name or the scale bar | Click it (a dashed frame shows it is selected) and drag it. Arrow keys move it by 1 px, Shift+arrow by 10. A name moved far from its clade gets a thin line back to it |
| Change the wording of a name | Double-click it, or select it and press Enter or **Edit text**. Enter keeps the new text, Esc cancels; an empty box brings back the automatic name |
| Colour, italic, bold | Select a name, a track name or your own text: a ready-made colour (one click) or the colour box picks its colour (**auto** goes back), ***I*** and **B** (or Ctrl/⌘+I, Ctrl/⌘+B) switch italic and bold — e.g. a species name in italics. **Reset** brings back the automatic look |
| Leave a name out | Select it and press Delete or **Hide**. Hidden names are listed under *Names and your own text*, each with **show** |
| Show a name that did not fit | Click the tip or folded clade (the branch or triangle, not empty space) and choose **Show its name**: it is drawn even where it overlaps others; drag it to a free spot |
| Add your own text (e.g. a panel letter) | **+ Add text**, type, Enter (Shift+Enter for a new line). Select it to colour it, make it italic, smaller (A−), larger (A+), bold (B), or **Delete** it |
| Undo | Ctrl/⌘+Z (redo: Ctrl/⌘+Shift+Z), or the Undo / Redo buttons. **Reset** puts one name back; **Reset all** clears every change |

All of this works in every layout (rectangular, circular, unrooted); names keep their direction (along the
branch or the radius) when moved. **Track names** (annotation tracks: the
slanted names above the columns, or the names in the opening at the top of a circular tree) can be moved, reworded
and hidden in every layout: a long name is shortened with … where it has to fit, and written in full once you drag
it out (with a thin line back to its column or ring). Positions are kept per layout.

| Setting | Choices |
|---|---|
| Size | Single column (85 mm), double column (180 mm), A4 page (180 × 250 mm), custom (mm) |
| Text size | in points (12 px on screen = the chosen pt) |
| Fit names | *Grow the figure until every name fits — tip names and clade names* (rectangular: height; circular: grows until every clade name and ≥ 98% of tip names fit, only the names the settings ask for, and stops when growing no longer adds names; unrooted: up to 1.5 m). The result line says how many of each fit, e.g. "9 of 9 tip names, 62 of 62 clade names" |
| Format | SVG with physical size, PDF (print dialog with page size), PNG at 300 or 600 dpi |
| Legend | none, beside, below, on the figure (no room added; drag it into place), separate SVG. Then: **legend size** (small, normal = the tree's text, large, extra large), **columns** (auto or 1–6), and the list of **blocks**: untick one to leave it out, ↑ ↓ to reorder. Long names are shortened with … to their column; the legend always sits outside the tree. The arrangement is kept for the next figure in the same session |
| Taxonomy names | Taxonomy names written next to folded clades (`p__X; c__Y (12)`) and next to lone tips (a tip alone in its taxon, `p__X (1)`): *all, as on screen*; *folded clades + my groups* (rectangular: no taxonomy on unfolded clades); *only lone tips*; *none*. Default: *only lone tips* when the Shading tab names the shaded clades. The bold Shading-tab names are not affected (turn them off in the Shading tab). Space for names is reserved only for the names actually drawn. Tip names (the tips' own names) are separate: printed wherever there is room. When the Shading tab names clades: **tip names on one-tip clades** (off: only the clade name, as in the Shading tab) |

The settings also say how many tip and clade names will fit. If nothing in the view needs a legend (no shading, tracks, highlights or coloured
groups), it says so and none is added. Folded clades stay folded; colours, shading, strips and
highlights are kept; your view on screen is not changed. Very large PNGs are capped at the browser's canvas limit
(16,000 px), with a note.

### 15.3 Data exports

Newick (as displayed, or with group names as labels), taxonomy in GTDB format, the log of named taxa, RED values,
groups, fields, tables of a clade, a group or the filtered table, conflict tables (monophyly tab). All
tables are tab-separated text.

---

## 16. Saving work: sessions, shareable files and autosave

### 16.1 Session files

Export ▾ → **Save session** writes one `.treezer` file (compressed JSON) holding: the current tree (including
re-rooting and pruning), metadata and your fields, feature tables, the taxonomy and its edits, aliases, groups,
folds, branch order, strips with their options, clade shading and moved names, RED settings, zoom and
selection. Open it with **Open ▾ → Saved session…** or by dropping it on the window. Sessions refer to tips by name, so they are
independent of tree internals, and older session files still open.

Sessions are the way to keep work, move it to another computer or send it to a collaborator who uses Treezer.

### 16.2 Shareable HTML files

Export ▾ → **Shareable HTML file…** saves one `.html` file that contains Treezer itself, the tree, its data and the
current view — everything a session holds. Whoever receives it double-clicks it and explores the tree in their
browser: no installation, no account, no internet connection and no separate tree or data files. Folding, search,
highlighting, the Table tab, layouts and figure export all work; their changes stay in their copy (Export ▾ saves
their own session or file).

Use it to send a tree to collaborators, as **supplementary material** for a journal, as a file linked from a paper,
or in an archive such as **Zenodo**.

Before saving, the dialog lists what goes into the file (the tree, every data column, the taxonomy, groups,
feature tables, the view) — check that you may share all of it. To leave data out, open the tree
again and load only the columns you want to share (the column picker in *Load data…*). It also asks for:

| Field | Shown as |
|---|---|
| Title (required) | The page title, the file name, and the heading of the details card |
| Description | What the tree shows and where the data come from |
| Authors | Who made it |
| How to cite | A reference or DOI |
| Data licence | e.g. CC BY 4.0 |

When the file is opened, a card with these details appears at the top left; the title stays in the top bar and
reopens the card. The file also records which Treezer version made it, so an archived file keeps working as it was.

Sizes: Treezer itself is about 2.5 MB; the data add little for small trees (GTDB archaea, 10,122 genomes with six
columns: 3.7 MB in all) and about 27 MB for GTDB bacteria (189,801 genomes, ten columns: 29.7 MB in all; made in about
4 s and opened in about 4 s on a laptop).

In the downloaded Treezer file this works offline. On the demo website, Treezer downloads a copy of itself for the
file, which needs an internet connection at that moment.

*Open ▾ → Saved session…* also opens a shareable file, so its tree and view can be taken back into your own Treezer.

### 16.3 Autosave in the browser

Your groups (with their folded state) and your own fields are also saved automatically in the browser and restored
when the same tree is opened again on the same computer and browser. Taxonomy edits, strips and shading are not
autosaved — save a session.

### 16.4 Privacy

All files are read locally; nothing is uploaded. The demo site serves only the program and the demo data. Autosaved
data stays in your browser's storage on your computer. A shareable HTML file contains all loaded data: share it
only where that data may go.

---

## 17. Link parameters

Links can open the viewer with data and settings, which is useful for demos, tutorials and supplementary material.

| Parameter | Example | Effect |
|---|---|---|
| `example` | `?example` | Open the built-in example |
| `session` | `?session=data/my.treezer` | Open a saved session from a URL |
| `tree` | `?tree=data/bac120.tree` | Load a tree from a URL |
| `taxonomy` | `&taxonomy=data/bac120_taxonomy.tsv.gz` | Load a taxonomy |
| `metadata` | `&metadata=data/meta.tsv.gz` (may repeat) | Load metadata tables |
| `columns` | `&columns=checkm2_completeness,genome_size` | Columns to keep from metadata |
| `taxonomyColumn` | `&taxonomyColumn=ncbi_taxonomy` | Use this column as the taxonomy |
| `aliasColumn` | `&aliasColumn=accession` | Alternative tip IDs |
| `features` | `&features=data/kos.tsv` | Load a feature table |
| `strips` | `&strips=habitat,genome_size` | Show these columns as strips |
| `featureStrips` | `&featureStrips=K00370,K02586` | Show these features as strips |
| `collapse` | `&collapse=phylum` | Fold to a rank |
| `layout` | `&layout=circular` / `unrooted` | Start in another layout |
| `shade` | `&shade=order` | Shade clades at a rank |
| `support` | `&support=symbols` (`branches`, `values`) | Show branch support |
| `branchLengths` | `&branchLengths` | Show branch lengths |
| `panel` | `&panel=monophyly` (table, monophyly, tracks, shading, style; the older names annotate and clades still work) | Open a drawer tab |

URLs are resolved against the page, so relative links work on a web server. Opened from disk (`file://`), the browser
does not allow loading other files by URL; use *Open…* and *Load data…* instead, or build a single file with the
data embedded (see the repository's `build-single.mjs --embed`).

---

## 18. Performance and limits

Measured on an ordinary laptop-class Linux machine (4 cores, 15 GB) in headless Chrome with software rendering; a
desktop browser with a graphics card is at least as fast.

| Tree | Tips | Load (file picked → tree on screen) | JavaScript memory | Pan / zoom | Fold to phylum (incl. repaint) | Fold one clade |
|---|---|---|---|---|---|---|
| GTDB r232 archaea | 10,122 | < 0.1 s | small | 60 fps | < 5 ms | < 5 ms |
| GTDB r232 bacteria | 189,801 | 0.8–0.9 s | ~50 MB | 60 fps, fully unfolded | ~40 ms | ~10 ms |
| 5 × GTDB bacteria (test) | 949,005 | 3.5 s | ~225 MB | 58–60 fps, fully unfolded | 30–45 ms | ~60 ms (all else unfolded) |

Load times are for the single-file version opened from disk. Memory is the page's JavaScript heap; the tree's compact
number arrays add roughly 20 bytes per node.

Other measurements: GTDB bacterial taxonomy loads in about 3 s; the full GTDB bacterial metadata (0.95 GB uncompressed,
715k rows) streams in about 9–10 s, keeping only the tree's tips (10 columns ≈ 145–205 MB, 40 columns ≈ 440 MB);
circular and unrooted redraws ≤ 11 ms at 190k tips fully unfolded.

**Limits.** Circular and unrooted layouts are meant for up to a few thousand visible items (larger expansions are
guarded). Very wide metadata selections cost memory. Trees beyond a few million tips are untested; the limits there are
reading the file and memory rather than drawing.

---

## 19. Troubleshooting and FAQ

**Rank buttons do nothing.** The tree has no named clades. Load a taxonomy (*Load data…*) or use a GTDB-decorated tree.

**Few tips matched my table.** Check the ID column chosen in the column picker; use *Also match tips by* for an
alternative ID column. The load report lists unmatched examples.

**A taxon I know is a clade shows as `[1/2]`, `[2/2]`.** The tree may be rooted inside it. Root on an outgroup
(monophyly tab → *Root so that … is on one side*) or use midpoint rooting.

**The monophyly tab says the tree looks unrooted.** The root has more than two children; RED and monophyly are not
meaningful until you root it ([§10.4](#104-rooting-and-monophyly)).

**My RED values differ from GTDB's.** GTDB averages several rootings and uses its own reference trees; Treezer uses
one rooting of your tree ([§11](#11-relative-evolutionary-divergence-red)).

**Names are missing on the tree.** Only names that fit are drawn; zoom in, or use *Publication figure…*, which draws all
names that fit at print size.

**Circular view says the expansion is held back.** See [§6.4](#64-guarded-expansion-circular-and-unrooted).

**I lost my work after closing the browser.** Groups and fields are autosaved; everything else needs *Save session*.

**Links with `?tree=` do not work from the downloaded file.** Browsers block URL loading from `file://`; open files with
the buttons instead.

**I want to cite or share a figure.** Use *Publication figure…* with a legend, and keep the session file as the
editable source.

---

## 20. Glossary

| Term | Meaning |
|---|---|
| Clade | A node and everything below it. |
| Fold / unfold | Draw a clade as one triangle or wedge / draw it in full. |
| Mixed resolution | Different parts of the tree folded at different ranks. |
| Group | A named, coloured clade you created. |
| Group | A named set of any tips; can be highlighted (coloured branches) and labelled (named clades) on the tree. |
| Piece (fragment) | A largest clade whose classified tips all belong to one taxon. |
| Monophyletic | A taxon whose tips form exactly one piece. |
| Stray | A tip of a taxon outside the taxon's main piece. |
| Intruder | A tip of another taxon inside a taxon's common-ancestor clade. |
| RED | Relative evolutionary divergence: 0 at the root, 1 at the tips. |
| Rank band | The RED interval associated with a rank in this tree. |
| Session | A `.treezer` file holding the complete state of your work. |
| Strip | A data column drawn beside the tree. |
| Clade shading | A filled area from a taxon's node to its leaves. |

---

## Appendix A. Example input files

**Newick with support values and GTDB labels**

```
((A:0.1,B:0.2)90:0.05,(C:0.1,D:0.1)'100.0:g__Example':0.02);
```

**GTDB-style taxonomy (TSV, no header)**

```
RS_GCF_000005845.2	d__Bacteria;p__Pseudomonadota;c__Gammaproteobacteria;o__Enterobacterales;f__Enterobacteriaceae;g__Escherichia;s__Escherichia coli
my_bin_001	d__Bacteria;p__Pseudomonadota;c__Gammaproteobacteria;o__;f__;g__;s__
```

**Metadata (TSV with header)**

```
genome	habitat	genome_size	checkm2_completeness
GCF_000005845	gut	4641652	99.9
my_bin_001	hot_spring	2310455	91.2
```

**Feature table, long form**

```
genome	feature	description
my_bin_001	K00370	narG; nitrate reductase alpha subunit
my_bin_001	K02586	nifD; nitrogenase molybdenum-iron protein alpha chain
```

**iTOL colour strip**

```
DATASET_COLORSTRIP
SEPARATOR TAB
DATASET_LABEL	habitat
DATA
my_bin_001	#d95f02	hot spring
```

---

## Appendix B. Worked examples

### B.1 Placing new MAGs in the GTDB context

1. Build a tree with your MAGs and GTDB references (e.g. from GTDB-Tk) and open it.
2. *Load data…* → the GTDB-Tk `summary.tsv`: your MAGs are classified and highlighted.
3. *Load data…* → GTDB taxonomy for the references (fills genomes without a taxonomy).
4. Root on an outgroup (monophyly tab → *Root so that … is on one side*).
5. Highlights → ⊙ focus on your MAGs: everything without them folds away.
6. Select a clade of MAGs: the sidebar shows its RED and closest rank. Right-click → *Name as taxon…* if it is a new
   genus or family.
7. Export a publication figure with a legend and save the session.

### B.2 Comparing GTDB and NCBI taxonomy on the GTDB tree

1. Open the GTDB bacterial tree; *Load data…* → GTDB metadata, keep `gtdb_taxonomy` and `ncbi_taxonomy`.
2. Monophyly tab → *compare with* `ncbi_taxonomy`. The per-rank row shows how many NCBI taxa are split.
3. Sort by *outside* to find the most scattered taxa; *highlight strays* to see where they sit.
4. Switch back to `gtdb_taxonomy` to compare.

### B.3 Mapping a metabolic trait across a clade

1. Open the tree and load a KO table (e.g. a DRAM summary) with *Load data…*.
2. Annotate tab → *Genes and pathways*: filter `nif` and add the nitrogenase KOs.
3. Fold to genus: folded genera show the percentage of genomes with each gene.
4. Highlight each value of a habitat column to see whether the trait follows habitat.
