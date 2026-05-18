# Pipelines

## Main SSID1 Analysis

Purpose: validate acute storage-stability statistics and classify metabolite trajectories over time.

Main steps:

1. Load Quarto parameters.
2. Load required R packages.
3. Assert required workbook and delivered-statistics files exist.
4. Load chemical annotation.
5. Normalize column names to upper snake case.
6. Check required chemical annotation columns.
7. Create metabolite slugs from display names.
8. Abort on duplicate metabolite slugs.
9. Load sample metadata.
10. Check required metadata columns.
11. Load log-transformed abundance data.
12. Resolve numeric abundance headers to metabolite slugs.
13. Abort on unresolved abundance columns.
14. Load delivered ANOVA statistics.
15. Check required delivered-statistic columns.
16. Join delivered statistics to metabolite slugs.
17. Build a shared modeling table with abundance, timepoint, and subject factors.
18. Run EDA on database coverage, unknown metabolites, and imputation.
19. Run a single-metabolite ANOVA spot-check.
20. Run whole-platform ANOVA reproduction.
21. Compare local and delivered p-values with correlation diagnostics.
22. Run pairwise and polynomial `emmeans` contrasts for every metabolite.
23. Add BH-adjusted q-values across metabolites by contrast family.
24. Define first-break stability horizons.
25. Classify trajectories.
26. Summarize platform, super-pathway, and sub-pathway stability.
27. Generate exemplar trajectory plots.
28. Build storage-horizon decision tables.
29. Run PCA diagnostics.
30. Save derived CSVs and figures.

## Identifier Rescue And Enrichment

Purpose: improve pathway coverage and interpret unstable metabolites at pathway level.

Main steps:

1. Load cross-ID mapping from Bioconductor metabolite resources.
2. Normalize HMDB IDs.
3. Extract single KEGG compound IDs from multi-ID cells.
4. Resolve KEGG IDs directly, then through HMDB, PubChem, and CAS fallback paths.
5. Report KEGG coverage by direct and rescued source.
6. Cache KEGG pathway-compound mappings locally.
7. Build unsigned MSEA ranks from ANOVA p-values.
8. Run KEGG MSEA with `fgsea`.
9. Build hard hit lists from unstable metabolites.
10. Run KEGG ORA overall, by day, and by direction.
11. Build RaMP-compatible metabolite ID lists from all available identifier columns.
12. Run RaMP mapping diagnostics.
13. Run RaMP pathway enrichment with database background.
14. Filter noisy pathway sources.
15. Flatten list-columns for CSV export.

## Threshold Sensitivity

Purpose: test whether stability conclusions depend on the chosen decision rule.

Main steps:

1. Rebuild the core metabolite, metadata, and log-transformed modeling table.
2. Fit pairwise and polynomial contrasts.
3. Add BH q-values.
4. Check exact zero estimates.
5. Define shared helper functions for horizon calls and classification.
6. Run rule 1: `q < 0.05` and `|estimate| > 0.2`.
7. Run rule 2: `p < 0.05` and `|estimate| > 0`.
8. Run rule 3: `q < 0.05` and `|estimate| > 0`.
9. Compare platform-level stability across rules.
10. Compare trajectory category counts across rules.
11. Write rule-specific CSV outputs.

## Privacy And Output Flow

Raw workbooks, processed tables, generated CSVs, R objects, and generated figures are ignored by Git. The code and rendered report structure remain reviewable while private data stays local.
