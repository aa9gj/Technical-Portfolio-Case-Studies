# Stack

## Core Stack

| Area | Tools |
|---|---|
| Analysis language | R |
| Report engine | Quarto |
| Input format | Excel workbook deliverables |
| Workbook loading | readxl |
| Data wrangling | dplyr, tidyr, tibble, purrr, readr, stringr |
| Tables | knitr |
| Visualization | ggplot2 |
| Statistical models | base R `aov`, formula interfaces |
| Timepoint contrasts | emmeans |
| Multiple testing | Benjamini-Hochberg q-values with `p.adjust` |
| Metabolite mapping | metaboliteIDmapping, KEGGREST |
| Enrichment | fgsea, `fora`, RaMP v3.0.7 |
| Output | CSV, self-contained HTML, PNG figures |
| Data hygiene | `.gitignore` for raw/private/generated artifacts |

## Testing And Validation Stack

The testing layer is analytical rather than package-style.

| Test Type | Implementation |
|---|---|
| File availability | `file.exists()` plus `stopifnot()` |
| Workbook schema checks | Required-column vectors plus explicit `stop()` messages |
| Identifier uniqueness | Duplicate metabolite slug aborts |
| Header mapping tests | Abort on abundance columns that fail CHEM_ID resolution |
| Metadata schema tests | Required sample/timepoint/subject column checks |
| Model reproduction test | Single-metabolite ANOVA spot-check against delivered statistics |
| Platform reproduction test | Pearson/Spearman agreement between local and delivered p-values |
| Numerical stability | P-value flooring before `-log10(p)` transforms |
| Model failure isolation | `tryCatch` around per-metabolite ANOVA/contrast fits |
| Multiple-testing checks | BH q-values by contrast family |
| Decision-rule tests | Threshold-sensitivity notebook across p-value/q-value/effect-size rules |
| External API hardening | RaMP v3.0.7 pinning, result-shape handling, fallback filtering |

This is a strong fit for a portfolio stack column because it shows the testing decisions that matter in scientific analysis: schema checks, reproduction gates, threshold sensitivity, and robustness to package/API drift.

## Data Inputs

| Input | Role |
|---|---|
| Chemical annotation table | Metabolite IDs, display names, pathway annotations, external IDs |
| Sample metadata table | Sample identifiers, timepoints, subject identifiers |
| Log-transformed abundance table | Main modeling matrix |
| Batch-normalized abundance table | Imputation and missingness diagnostics |
| Delivered ANOVA output | Reference statistics for reproduction tests |

## Reproducibility Notes

The source README references environment restoration with `renv`, but the inspected clone did not include a lockfile. The strongest next step would be committing or verifying an environment lockfile and adding a synthetic-data CI render.
