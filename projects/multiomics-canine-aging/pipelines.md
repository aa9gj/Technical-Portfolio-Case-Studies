# Pipelines

## Single-Cell RNA-seq

Purpose: characterize immune-cell composition and cell-type-specific aging signatures.

Main steps:

1. Load single-cell count matrix and metadata.
2. Create a Seurat object.
3. Filter low-support genes and low-quality cells.
4. Remove high mitochondrial-content cells.
5. Normalize with SCTransform and `glmGamPoi`.
6. Run PCA and choose dimensions using diagnostic plots.
7. Build nearest-neighbor graph and cluster cells.
8. Run UMAP for visualization.
9. Identify marker genes and annotate major immune lineages.
10. Subcluster granulocytes, monocytes, B cells, CD4 T cells, CD8 T cells, dendritic cells, and miscellaneous populations.
11. Save checkpointed Seurat objects with `qs`.
12. Run all-cell pseudobulk DE.
13. Run per-cell-type pseudobulk DESeq2.
14. Test cell composition shifts with propeller.
15. Calculate focused immune ratios.

Outputs: annotated Seurat object, marker tables, pseudobulk DE results, per-cell-type DE tables, composition summaries, and immune-ratio outputs.

## Bulk RNA-seq

Purpose: identify cohort-level gene expression changes and connect them to pathway, phenotype, and cross-species evidence.

Main steps:

1. Load sample metadata and gene count matrix.
2. Clean sample names.
3. Impute missing counts as zero where required by the input matrix.
4. Verify count/metadata alignment and stop if mismatched.
5. Set condition and sex factors.
6. Run DESeq2 with `~ sex + condition`.
7. Reconcile transcript and gene identifiers using GTF-derived lookup tables.
8. Deduplicate gene-level results.
9. Split significant genes by direction.
10. Run GO and KEGG over-representation analysis.
11. Mine phenotype annotations for enriched biological terms.
12. Test clinical marker relationships.
13. Overlay external GWAS and orthology evidence.
14. Compare bulk signals with single-cell pseudobulk/per-cell-type results using GSEA.

Outputs: bulk DE table, annotated gene summaries, GO/KEGG enrichment outputs, phenotype overlays, clinical-marker summaries, and cross-modality comparison tables.

## Metabolomics

Purpose: identify serum metabolite abundance shifts and pathway-level metabolic signatures.

Main steps:

1. Load imputed, non-imputed, chemical annotation, and sample metadata tables.
2. Filter and normalize sample identifiers.
3. Harmonize metabolite identifiers across KEGG, HMDB, PubChem, ChEBI, names, and cleaned names.
4. Prepare metabolite-by-sample matrices.
5. Run differential abundance models with edgeR/limma-style functions.
6. Summarize significant up/down metabolite sets.
7. Build pathway-level counts by metabolite subpathway.
8. Map metabolite IDs into RaMP input format.
9. Run RaMP pathway enrichment for up and down metabolite sets.
10. Run fgsea metabolite-set enrichment over metabolite pathway groupings.

Outputs: differential abundance tables, significant metabolite sets, pathway summaries, RaMP enrichment outputs, and metabolite-set enrichment tables.

## Multi-Omics Integration

Purpose: identify coordinated RNA/metabolite signatures across matched animals.

Main steps:

1. Convert DESeq2 bulk RNA-seq object to a VST-normalized matrix.
2. Apply transcript/gene-name reconciliation.
3. Transpose RNA matrix to sample-by-feature shape.
4. Load and clean metabolomics matrix and annotation.
5. Match metabolomics rows to RNA-seq sample names.
6. Stop if RNA and metabolomics sample order is not identical.
7. Keep top variable RNA features.
8. Remove near-zero-variance RNA and metabolite features.
9. Define young/senior outcome labels.
10. Tune DIABLO design weights and `keepX` grids with repeated cross-validation.
11. Save fit objects and final stability results.
12. Compare selected features across design weights with Jaccard similarity.
13. Choose final DIABLO configuration.
14. Extract selected RNA and metabolite features.
15. Run WGCNA using filtered RNA expression and selected metabolite traits.
16. Test module-trait relationships and annotate modules with GO/KEGG enrichment.

Outputs: DIABLO fits, feature-selection stability summaries, selected RNA/metabolite signatures, WGCNA modules, module-trait heatmaps, and module enrichment tables.

## Figure Generation

Purpose: regenerate manuscript-style figures from analysis outputs.

Main steps:

1. Load derived analysis tables or objects.
2. Generate single-cell, bulk, metabolomics, and integration figures.
3. Compose multi-panel figures with ggplot2, patchwork, ComplexHeatmap, and Python matplotlib.
4. Save final panel outputs.

Outputs: manuscript-ready figure panels and custom UpSet-style visualizations.
