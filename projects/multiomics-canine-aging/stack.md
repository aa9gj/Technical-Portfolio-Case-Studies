# Stack

## Languages And Authoring

| Tool | Role |
|---|---|
| R | Primary analysis language |
| Quarto | Executable notebooks and rendered analysis reports |
| Python | Custom UpSet-style figure generation |
| Bash/SLURM | Cluster helper script for upstream preprocessing |

## R Packages

| Area | Packages |
|---|---|
| Data handling | data.table, tidyverse, dplyr, tidyr, tibble, qs |
| Visualization | ggplot2, ggrepel, ggpubr, patchwork, cowplot, ComplexHeatmap, circlize, RColorBrewer |
| Single-cell | Seurat, sctransform, glmGamPoi, presto, Matrix, SparseArray |
| Differential expression | DESeq2, edgeR, limma-style modeling |
| Cell composition | speckle/propeller-style workflow |
| Genomic annotation | rtracklayer, AnnotationDbi, org.Cf.eg.db |
| Enrichment | clusterProfiler, fgsea, RaMP |
| Multi-omics | mixOmics, BiocParallel |
| Network analysis | WGCNA, matrixStats |

## Data Modalities

| Modality | Shape |
|---|---|
| Clinical markers | Complete blood count and serum chemistry-style tables |
| Single-cell RNA-seq | PBMC count matrix plus cell/sample metadata |
| Bulk RNA-seq | PBMC gene count matrix plus animal-level metadata |
| Metabolomics | Untargeted serum metabolite abundance matrix plus chemical and sample annotations |
| Support tables | Canine gene annotations, phenotype evidence, GWAS/orthology files, metabolite ID maps |

## Modeling And Statistics

| Method | Use |
|---|---|
| SCTransform | Single-cell normalization and technical-noise modeling |
| PCA/UMAP/clustering | Single-cell structure and visualization |
| Pseudobulk DESeq2 | Cell-type-aware differential expression |
| DESeq2 `~ sex + condition` | Bulk RNA-seq young-versus-senior comparison |
| GO/KEGG enrichment | Transcriptomic pathway interpretation |
| limma/edgeR-style modeling | Metabolomics differential abundance |
| RaMP pathway enrichment | Metabolite pathway interpretation |
| fgsea | Ranked metabolite-set enrichment |
| DIABLO | Supervised multi-block RNA/metabolomics integration |
| Jaccard stability | Selected-feature robustness across DIABLO design weights |
| WGCNA | RNA module construction and metabolite-trait association |

## Execution Notes

The repo expects a research-compute environment with large memory R sessions, Bioconductor packages, and access to derived data tables. Several stages are notebook-stateful, so reliable reproduction depends on running notebooks in dependency order or refactoring shared objects into a formal pipeline.
