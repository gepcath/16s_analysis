# 16S Microbiome Analysis

Exploratory analysis of 16S rRNA metagenomics data to investigate associations between gut bacterial communities and phosphorylated Tau (pTau) — a biomarker relevant to neurodegenerative disease.

## Overview

This repository contains Jupyter notebooks for:

1. **Core microbiome profiling** (`new_big_font_no_kegg.ipynb`)
   - Genus- and phylum-level relative abundance visualisation (stacked bar charts)
   - Beta diversity analysis using Bray–Curtis dissimilarity with PCoA
   - Alpha diversity metrics (Observed Richness, Shannon, Simpson, Pielou's Evenness)
   - Gram stain classification of FDR-significant taxa

2. **KEGG data retrieval** (`kegg.ipynb`)
   - Queries the KEGG API to map FDR-significant species to KEGG organism codes
   - Fetches KEGG pathway presence for each species
   - Produces `species_to_kegg_org_candidates.csv`, `species_by_kegg_pathway_presence.csv`, and `kegg_pathway_id_to_name.csv` used downstream

3. **KEGG pathway functional analysis** (`kegg2.ipynb`)
   - Maps FDR-significant species to KEGG metabolic pathways
   - Assigns pathways to coarse functional categories (amino acid metabolism, energy metabolism, SCFA metabolism, lipid metabolism, carbohydrate metabolism, biotin metabolism)
   - Builds a bipartite species ↔ functional-category network
   - Exports summary tables (species → pathway and pathway → species)

4. **Alpha diversity summary** (`shannon_mean_std.ipynb`)
   - Computes per-sample Shannon diversity from the raw abundance table
   - Reports mean ± SD Shannon index and sample count
   - Performs Kruskal–Wallis test for group comparisons

## Repository structure

```
16s_analysis/
├── new_big_font_no_kegg.ipynb    # Core microbiome profiling
├── kegg.ipynb                    # KEGG API data retrieval
├── kegg2.ipynb                   # KEGG pathway network analysis
├── shannon_mean_std.ipynb        # Alpha diversity (Shannon) summary
├── requirements.txt              # Python dependencies
└── README.md

# Expected sibling directories (not committed):
../data/
│   ├── Metagenomics_NextGen_Feature_on_Rows.txt   # Raw abundance matrix (taxa × samples)
│   ├── pTau versus bacteria FDR 0.05.csv          # FDR-significant taxa (CSV)
│   └── pTau versus bacteria FDR 0.05.txt          # FDR-significant taxa (plain text)
../results/                                        # Generated outputs (PDFs, CSVs)
```

> **Note:** The `data/` and `results/` directories live one level above the repository root and are not tracked in version control.

## Data

| File | Description |
|------|-------------|
| `Metagenomics_NextGen_Feature_on_Rows.txt` | Tab-separated abundance matrix with taxonomic features as rows and sample IDs as columns |
| `pTau versus bacteria FDR 0.05.csv` | Regression results (coefficient, SE, p-value, FDR q-value) for taxa significantly associated with pTau at FDR < 0.05 |
| `pTau versus bacteria FDR 0.05.txt` | Plain-text list of taxon names from the FDR-significant set |

## Generated outputs

After running the notebooks the `../results/` directory will contain:

| File | Notebook | Description |
|------|----------|-------------|
| `sample_name_mapping.csv` | `new_big_font_no_kegg` | Original → cleaned sample ID mapping |
| `significant_genus_counts_top20.csv` | `new_big_font_no_kegg` | Genus-level counts of FDR-significant taxa |
| `top20_genus_relative_abundance_by_sample.csv` | `new_big_font_no_kegg` | Relative abundance of top 20 genera per sample |
| `big_font_top20_genera_stacked2.pdf` | `new_big_font_no_kegg` | Stacked bar chart — top 20 genera |
| `big_font_beta_diversity_pcoa2.pdf` | `new_big_font_no_kegg` | Bray–Curtis PCoA scatter plot |
| `beta_diversity_pcoa_coords.csv` | `new_big_font_no_kegg` | PCoA coordinates per sample |
| `big_font_phyla_level_stacked.pdf` | `new_big_font_no_kegg` | Phylum-level stacked bar chart |
| `phylum_relative_abundance_by_sample.csv` | `new_big_font_no_kegg` | Phylum relative abundances per sample |
| `phylum_average_abundances.csv` | `new_big_font_no_kegg` | Mean relative abundance per phylum |
| `big_font_gram_stain_classification_pie.pdf` | `new_big_font_no_kegg` | Pie chart of Gram stain classifications |
| `big_font_gram_stain_detailed_bar.pdf` | `new_big_font_no_kegg` | Horizontal bar — detailed Gram categories |
| `gram_stain_classification.csv` | `new_big_font_no_kegg` | Per-taxon Gram stain label |
| `species_to_kegg_org_candidates.csv` | `kegg` | Species → KEGG organism code mapping |
| `species_by_kegg_pathway_presence.csv` | `kegg` | Species × KEGG pathway binary matrix |
| `kegg_pathway_id_to_name.csv` | `kegg` | KEGG pathway ID → name lookup |
| `coarse_functional_network.pdf` | `kegg2` | Bipartite species ↔ category network |
| `corse_species_kegg_table.csv` | `kegg2` | Species → category → KEGG pathway table |
| `corse_kegg_species_table.csv` | `kegg2` | Category → KEGG pathway → species table |

## Setup

### Prerequisites

- Python ≥ 3.8
- [pip](https://pip.pypa.io/)

### Install dependencies

```bash
pip install -r requirements.txt
```

### Run the notebooks

```bash
jupyter notebook
```

Run the notebooks in order:
1. `new_big_font_no_kegg.ipynb` — core profiling
2. `kegg.ipynb` — fetches KEGG data (requires internet; generates intermediate CSVs in `../results/`)
3. `kegg2.ipynb` — pathway network analysis
4. `shannon_mean_std.ipynb` — Shannon diversity summary statistics

Make sure the `../data/` directory with the required input files is in place before running.

## Dependencies

See [`requirements.txt`](requirements.txt).
