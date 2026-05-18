# Project Structure

The source repo is intentionally compact.

```text
multiomics-canine-aging/
  README.md
  .gitignore
  analysis/
    complete_analysis_single_cell.qmd
    complete_analysis_bulk_RNAseq.qmd
    complete_analysis_bulk_RNAseq.md
    complete_analysis_metabolomics.qmd
    complete_analysis_integration.qmd
    run_combine.slurm
  figures/
    Figure2_December2025.R
    Figure3_December2025.R
    Figure4_December2025.R
    Figure5_December2025.R
    Figure6_December2025.R
    Figure7_December2025.R
    FigureS2_December2025.R
    UpSet_plot.py
```

## Reading Order

Recommended review path:

1. Start with `README.md` for cohort modalities and analysis links.
2. Read `analysis/complete_analysis_single_cell.qmd` for single-cell QC, annotation, pseudobulk DE, and composition modeling.
3. Read `analysis/complete_analysis_bulk_RNAseq.qmd` for DESeq2, annotation, enrichment, phenotype mining, and GWAS overlays.
4. Read `analysis/complete_analysis_metabolomics.qmd` for differential abundance and metabolite pathway enrichment.
5. Read `analysis/complete_analysis_integration.qmd` for DIABLO, feature stability, and WGCNA.
6. Review `figures/` to see how analysis outputs are converted into manuscript panels.

## Organization Pattern

The current structure is optimized for manuscript analysis:

- Quarto notebooks keep narrative, code, and intermediate interpretation together.
- Figure scripts are separated from primary statistical workflows.
- Large data assets are referenced externally rather than committed.
- Serialization through `qs` helps checkpoint expensive R objects.

The next engineering step would be extracting repeated utilities into R scripts, moving paths/thresholds into config, and wrapping notebook rendering in a dependency-aware workflow.
