# Security And Privacy

## Data Sensitivity

The source repo is private because it analyzes proprietary metabolomics workbook data and sample-level storage-stability measurements. This portfolio case study intentionally avoids:

- Raw workbook filenames.
- Local filesystem paths.
- Non-technical organization identifiers.
- Sample-level measurements.
- Subject identifiers.
- Generated result tables that may reveal private study data.

## Git Hygiene

The source `.gitignore` excludes:

- Raw data directories.
- Processed data directories.
- Excel workbooks.
- CSV and TSV outputs.
- R serialized objects.
- R session files.
- Quarto cache directories.
- Generated figures.

The repo keeps code, notebook structure, rendered report examples, and placeholder output directories reviewable without committing private data.

## Public Portfolio Boundary

Safe public framing:

- Storage-stability problem.
- Workbook ingestion architecture.
- Statistical validation gates.
- Threshold-sensitivity testing.
- Pathway enrichment approach.
- Privacy and reproducibility tradeoffs.

Avoid:

- Raw sample values.
- Study-specific workbook names.
- Local machine paths.
- Proprietary operational details.
- Private organization identifiers.

## Reproducibility Risk

| Risk | Mitigation |
|---|---|
| Private data cannot run in CI | Add synthetic workbook fixtures |
| Notebook paths can leak environment details | Move paths into local config or parameter files |
| Package/API drift | Pin package versions and database versions |
| Workbook schema drift | Keep required-column checks and clear abort messages |
| Threshold-dependent conclusions | Maintain sensitivity analyses as part of the report |
| Generated result leakage | Keep derived CSVs and figures ignored unless explicitly approved |
