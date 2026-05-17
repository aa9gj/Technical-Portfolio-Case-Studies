# Stack

`GWASTargetChase` is an R/Bioconductor package with external biomedical data integrations.

## Core Stack

| Area | Tools |
|---|---|
| Package framework | R, DESCRIPTION, NAMESPACE, roxygen2-generated docs |
| Genomic coordinates | GenomicRanges, S4Vectors |
| GTF parsing | rtracklayer |
| Tabular data | data.table, dplyr |
| Large data | arrow |
| API/JSON | jsonlite, curl |
| Visualization | ggplot2, ggpubr, patchwork, grDevices |
| Testing | testthat |
| Documentation | README, vignette, Rd man pages |
| Bundled data | LazyData, `data/`, `inst/extdata/` |

## External Data Sources

| Source | Usage |
|---|---|
| OpenTargets Platform | Gene-disease association evidence through GraphQL and bulk downloads |
| OpenTargets Genetics | Locus-to-gene scores and study annotations for manual workflows |
| IMPC | Mouse knockout phenotype evidence |
| Zoonomia | Cross-species orthology translation for cat/dog genes to human and mouse |
| GENCODE/Ensembl-style GTFs | Protein-coding gene annotations and genomic windows |

## Package Design

The package exposes:

- `TargetChase()`
- `TargetChaseManual()`
- `downloadData()`
- `fetch_opentargets()`
- `fetch_impc()`
- `geneticAssocPrep()`
- `l2gPrep()`
- `IMPCprep()`
- `load_zoonomia_orthologs()`
- `translate_genes()`

The package also includes:

- Example GWAS summary statistics.
- Example cat and dog GTF files.
- Zoonomia orthology files.
- Prepared package data objects.
- testthat tests.
- A vignette.

## Why This Stack

| Choice | Reason |
|---|---|
| R package | Fits genomics users, supports Bioconductor tooling, and makes the workflow installable |
| GenomicRanges | Reliable interval logic for SNP windows, gene ranges, and overlaps |
| rtracklayer | Standard way to import GTF/GFF annotations into R |
| data.table | Fast GWAS and evidence-table reading |
| OpenTargets API | Keeps exploratory use lightweight without requiring huge local downloads |
| Manual local mode | Gives reproducibility when users need pinned data releases |
| Zoonomia | Provides the cross-species bridge needed for cat/dog GWAS interpretation |
| testthat | Keeps scientific logic testable and guards against regressions |

## Engineering Signal

The stack demonstrates:

- Scientific package engineering.
- Translational genomics data integration.
- API and bulk-data workflow design.
- Genomic interval reasoning.
- Testable R/Bioconductor code.
- Public examples that avoid requiring private GWAS data.
