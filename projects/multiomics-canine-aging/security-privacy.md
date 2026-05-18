# Security And Privacy

## Public-Safe Scope

This portfolio summary focuses on technical decomposition and does not include:

- Non-technical attribution language.
- Local workstation or cluster paths.
- Raw sample-level clinical tables.
- Vendor-specific raw metabolomics filenames.
- Private analysis directories or user names.
- Unpublished derived datasets.

## Data Sensitivity

The repo works with animal cohort data and omics assays. Even when the study is not human clinical data, sample-level tables should still be handled carefully because they may include animal identifiers, clinical measurements, cohort labels, and derived molecular phenotypes.

## Reproducibility Risks

| Risk | Mitigation |
|---|---|
| Local absolute paths in notebooks | Move paths into a config file excluded from private machine details |
| Sample ID mismatch across modalities | Keep hard alignment checks before integration |
| Large raw and intermediate files | Store outside Git and document accessions/manifests |
| Notebook execution order | Add a workflow wrapper or explicit dependency graph |
| Package drift | Add `renv.lock`, container image, or environment manifest |
| External annotation drift | Pin database versions and expected row counts |
| Private raw tables | Publish only approved derived tables or public supplementary materials |

## Credential Handling

The inspected workflow does not require secrets for the core analysis. If future data downloads or private storage systems are added, credentials should be passed through environment variables or local config files excluded from Git.

## Portfolio Framing

The safest public framing is:

- Cohort-level modality descriptions.
- Methods and statistical workflows.
- Model and integration strategy.
- Reproducibility tradeoffs.
- Figure-generation approach.

Avoid including raw sample records, local paths, or private operational details.
