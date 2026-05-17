# Project Structure

The source repo follows a standard R package layout.

```text
GWASTargetChase/
  DESCRIPTION
  NAMESPACE
  README.md
  LICENSE
  GWASTargetChase.Rproj
  R/
    gwasFollowup.R
    gwasFollowupMan.R
    api_fetch.R
    zoonomia.R
    downloadData.R
    geneticAssocPrep.R
    l2gPrep.R
    gene2disease.R
    locus2gene.R
    IMPCprep.R
  data/
    impc.rda
    disease_target_genetic_association.rda
    l2g_annotated_full.rda
  data-raw/
    download_data.sh
    prepare_data.R
    prepare_real_data.R
  inst/
    extdata/
      example_gwas_sumstats.tsv
      example_dog_gwas_sumstats.tsv
      example_cat_gtf.gtf
      example_dog_gtf.gtf
      zoo_human_cat.txt
      zoo_human_dog.txt
      zoo_mouse_cat.txt
      zoo_mouse_dog.txt
  tests/
    testthat.R
    testthat/
      test-api-functions.R
      test-TargetChase.R
      test-TargetChaseManual.R
      test-zoonomia-extended.R
      test-data-loading.R
      test-input-validation.R
      test-helper-functions.R
  vignettes/
    GWASTargetChase.Rmd
  man/
    *.Rd
  run_dog_analysis.R
```

## Root

| File | Purpose |
|---|---|
| `DESCRIPTION` | Package metadata, dependencies, version, author, license, and imports |
| `NAMESPACE` | Exported functions and imported packages |
| `README.md` | Public install and quick-start documentation |
| `LICENSE` | MIT license file |
| `GWASTargetChase.Rproj` | RStudio project file |
| `run_dog_analysis.R` | Example dog body-weight GWAS analysis script |

## R Source

| File | Purpose |
|---|---|
| `gwasFollowup.R` | `TargetChase()` online API-based workflow |
| `gwasFollowupMan.R` | `TargetChaseManual()` local/pre-downloaded data workflow |
| `api_fetch.R` | OpenTargets and IMPC API fetchers plus caching helper |
| `zoonomia.R` | Orthology loading, gene translation, and orthology metadata joins |
| `downloadData.R` | Download and processing helper for IMPC, OpenTargets, L2G, and GENCODE data |
| `geneticAssocPrep.R` | OpenTargets gene-disease association preprocessing |
| `l2gPrep.R` | OpenTargets locus-to-gene preprocessing |
| `gene2disease.R` | Exact gene match helper for association tables |
| `locus2gene.R` | Exact gene match helper for L2G tables |
| `IMPCprep.R` | IMPC phenotype table preprocessing |

## Data

| Directory | Purpose |
|---|---|
| `data/` | Lazy-loaded package data objects |
| `data-raw/` | Scripts used to create or prepare package data |
| `inst/extdata/` | Example GWAS/GTF files and Zoonomia orthology files shipped with the package |

## Tests

The test suite covers:

- API fetchers and cache behavior.
- Main `TargetChase()` logic.
- Manual/local `TargetChaseManual()` logic using mock evidence files.
- Zoonomia orthology loading and translation.
- Data loading.
- Input validation.
- Helper function edge cases.

## Documentation

| Directory | Purpose |
|---|---|
| `vignettes/` | Longer user-facing package walkthrough |
| `man/` | Generated Rd documentation for exported functions and data |

## Portfolio Interpretation

The structure shows a progression from analysis script to reusable scientific package:

- Clear package metadata.
- Exported workflows.
- Helper functions with bounded responsibilities.
- Bundled example data.
- Generated documentation.
- Vignette-based user guide.
- Tests for deterministic and network-dependent behavior.
