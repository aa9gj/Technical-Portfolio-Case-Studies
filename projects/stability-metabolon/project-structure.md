# Project Structure

The inspected source repo is compact and notebook-oriented.

```text
stability-metabolon/
  README.md
  .gitignore
  reports/
    ssid1_analysis.qmd
    ssid1_analysis.html
    ssid1_threshold_sensitivity.qmd
    trajectory_exemplars.png
  figures/
    .gitkeep
  results/
    .gitkeep
```

## Reading Order

Recommended review path:

1. `README.md` for study-arm framing and high-level pipeline notes.
2. `reports/ssid1_analysis.qmd` for the main ingestion, validation, modeling, classification, enrichment, and output workflow.
3. `reports/ssid1_threshold_sensitivity.qmd` for threshold robustness tests.
4. `.gitignore` for privacy boundaries around raw data and generated outputs.
5. `reports/ssid1_analysis.html` for rendered report review, when available and approved for sharing.

## Organization Pattern

The repo uses a Quarto-first research analysis pattern:

- Each notebook owns its own data ingestion and modeling path.
- Inputs are supplied through notebook parameters.
- Raw and generated data are excluded from Git.
- Rendered reports support reviewer inspection.
- Validation checks live next to the analysis code they protect.

The next engineering step would be extracting common helpers into reusable R scripts and adding a synthetic fixture workflow for automated checks.
