# Pipeline And Workflow

`GWASTargetChase` has two main workflows: live API mode and manual local-data mode.

## Workflow 1: `TargetChase()`

This is the main exploratory workflow.

### Inputs

- GWAS summary statistics file.
- Species GTF file.
- Species label.
- P-value threshold.
- Output directory.
- Optional custom Zoonomia directory.

Required GWAS columns:

- `chr`
- `ps`
- `p_wald`
- `gene`

### Steps

1. Validate file paths.
2. Create the output directory if needed.
3. Read summary statistics with `data.table::fread()`.
4. Stop if the closest-gene column is missing.
5. Filter SNPs by `p_wald <= pval`.
6. Assign locus IDs to significant hits.
7. Import the GTF with `rtracklayer`.
8. Filter the annotation to protein-coding genes.
9. Find genes within 500 kb of each closest-gene TSS.
10. Translate non-human candidate genes to human orthologs for OpenTargets.
11. Translate non-human candidate genes to mouse orthologs for IMPC.
12. Query OpenTargets Platform GraphQL for disease associations.
13. Query IMPC REST API for phenotype evidence.
14. Join IMPC evidence onto gene-disease results where available.
15. Add orthology metadata to result rows.
16. Write `g2d_results.txt`.
17. Write `impc_results.txt` when IMPC results exist.
18. Create locus plots from GWAS points and nearby gene spans.

### Outputs

- `g2d_results.txt`
- `impc_results.txt`
- `plots.pdf`

## Workflow 2: `TargetChaseManual()`

This workflow is for local pre-downloaded OpenTargets and IMPC data.

### Inputs

- GWAS summary statistics file.
- Species GTF file.
- Species label.
- P-value threshold.
- Output directory.
- Prepared IMPC phenotype table.
- Prepared OpenTargets disease association table.
- Prepared OpenTargets L2G table.
- Optional custom Zoonomia directory.

### Steps

1. Validate file paths.
2. Read prepared OpenTargets genetic association data.
3. Read prepared OpenTargets L2G data.
4. Read prepared IMPC phenotype data.
5. Import the species GTF.
6. Filter to protein-coding genes.
7. Read and filter GWAS summary statistics.
8. Build 500 kb significant-locus windows.
9. Use `GenomicRanges` overlaps to collect candidate genes.
10. Translate non-human genes to human/mouse orthologs.
11. Match candidate genes to gene-disease associations.
12. Match candidate genes to locus-to-gene records.
13. Join IMPC phenotype evidence.
14. Add orthology metadata.
15. Write `g2d_results.txt`.
16. Write `l2g_results.txt`.

### Outputs

- `g2d_results.txt`
- `l2g_results.txt`

## Data Preparation Workflow

Function:

- `downloadData()`

Purpose:

- Download IMPC phenotype hits.
- Download OpenTargets Platform association and disease files.
- Download OpenTargets Genetics L2G and study-index files.
- Download GENCODE human GTF.
- Process those raw files into local tables for `TargetChaseManual()`.

Supporting functions:

- `IMPCprep()`
- `geneticAssocPrep()`
- `l2gPrep()`

## Orthology Workflow

Functions:

- `load_zoonomia_orthologs()`
- `translate_genes()`
- `add_orthology_info()`

Steps:

1. Select the Zoonomia file based on target species and query species.
2. Load the orthology table.
3. Extract gene symbols from transcript-style identifiers.
4. Match query genes case-insensitively.
5. Return target gene names plus orthology class and grade.
6. Attach orthology metadata to final evidence tables.

## Testing Workflow

The tests are structured to cover both offline deterministic logic and online API behavior.

| Test Area | Examples |
|---|---|
| Input validation | Missing files, empty paths, missing columns, too-strict p-value |
| Genomic logic | GTF parsing, TSS direction, 500 kb windows, chromosome conversion |
| Orthology | Supported species combinations, mixed case, unknown genes, deduplication, metadata joins |
| Offline workflow | Mock OpenTargets, L2G, and IMPC data for `TargetChaseManual()` |
| API workflow | OpenTargets and IMPC calls when network access is available |
| Caching | Cached OpenTargets and IMPC responses |

## Expected Output Interpretation

| Output | Interpretation |
|---|---|
| `g2d_results.txt` | Candidate genes with disease-association evidence from OpenTargets plus optional IMPC/orthology context |
| `impc_results.txt` | Mouse phenotype summaries from IMPC for translated genes |
| `l2g_results.txt` | OpenTargets locus-to-gene records for candidate genes |
| `plots.pdf` | Per-locus visual summaries of GWAS signal and local genes |

## Workflow Boundaries

The package does not:

- Run GWAS.
- Fine-map credible sets from raw genotype data.
- Prove causal genes.
- Replace biological review of candidate genes.

It does:

- Standardize the follow-up step after GWAS.
- Pull multiple evidence sources into one candidate-gene view.
- Make cross-species interpretation more inspectable.
