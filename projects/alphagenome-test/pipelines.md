# Pipeline And Workflow

The workflow is organized as numbered scripts. Each stage reads the previous stage's files from the working directory and writes named outputs.

## Step 01: GWAS Window And LD Proxies

Script:

- `01_prepare_snps_for_alphagenome.py`

Inputs:

- GWAS summary statistics.
- Lead SNP.
- 1000 Genomes sample panel.
- 1000 Genomes regional VCF via `bcftools`.

Key operations:

1. Normalize GWAS summary-statistic column names.
2. Locate the lead SNP.
3. Define a +/- window around the lead.
4. Find genome-wide significant SNPs inside the window.
5. Download or load the 1000 Genomes panel.
6. Select European samples.
7. Stream a regional VCF slice with `bcftools`.
8. Match seed variants by rsID or allele set.
9. Compute genotype dosage arrays.
10. Estimate pairwise LD as r^2.
11. Clump seed variants into independent signals.
12. Collect high-LD proxy SNPs above threshold.

Outputs:

- `region_info.txt`
- `region_significant_snps.csv`
- `index_snps_independent_signals.csv`
- `ld_proxies_union_all_signals.csv`

## Step 01B: Liftover

Script:

- `01B_liftover_hg19_hg38.py`

Inputs:

- `ld_proxies_union_all_signals.csv`
- UCSC hg19-to-hg38 chain file.

Key operations:

1. Validate required columns.
2. Convert 1-based hg19 coordinates to chain-file coordinates.
3. Lift to GRCh38.
4. Track whether the chain hit is on the forward or reverse strand.
5. Reverse-complement alleles for strand-flipped hits.
6. Drop variants that cannot be lifted.

Output:

- `ld_proxies_union_all_signals_GRCh38.csv`

## Step 02: AlphaGenome Predictions

Script:

- `02_run_alphagenome_alltiss_allmodal.py`

Inputs:

- GRCh38 proxy variants.
- `ALPHA_GENOME_API_KEY`.
- AlphaGenome output metadata.

Key operations:

1. Normalize variant input columns.
2. Create AlphaGenome client.
3. Enumerate available output types.
4. Enumerate GTEx tissues and ontology terms from AlphaGenome metadata.
5. For each variant/tissue pair, create a centered sequence interval.
6. Call `predict_variant()` for all requested outputs.
7. Extract track outputs where available.
8. Summarize local absolute deltas near the variant.
9. Save checkpoints after batches of calls.
10. Write long track-level output.
11. Collapse to per-variant/tissue/modality summaries.

Outputs:

- `alphagenome_allGTEx_allOutputs_LONG.parquet`
- `alphagenome_allGTEx_allOutputs_COLLAPSED.csv`
- `progress_checkpoint.csv`

## Step 02B: Recollapse Predictions

Script:

- `02B_recollapse_from_long.py`

Purpose:

- Rebuild collapsed summaries from long parquet output without re-calling AlphaGenome.

Output:

- `alphagenome_allGTEx_allOutputs_COLLAPSED.csv`

## Step 03: Recommended Variant Scorers

Script:

- `03_score_variant.py`

Inputs:

- GRCh38 proxy variants.
- `ALPHA_GENOME_API_KEY`.

Key operations:

1. Load AlphaGenome recommended variant scorers.
2. Normalize variant inputs.
3. Create 1 Mb AlphaGenome scoring intervals.
4. Call `score_variant()` with retry/backoff.
5. Convert scorer outputs to tidy rows.
6. Save checkpoint progress.

Outputs:

- `variant_scores_tidy.csv`
- `variant_scores_checkpoint.csv`

## Step 04: SNP Prioritization

Script:

- `04_rank_prioritization.py`

Input:

- `variant_scores_tidy.csv`

Key operations:

1. Prefer calibrated `quantile_score` when available.
2. Compute absolute effect scores.
3. Aggregate maximum and mean absolute scores per SNP/scorer.
4. Rank SNPs within each scorer.
5. Aggregate ranks across scorers.
6. Count how often a SNP appears in top scorer ranks.
7. Write overall consensus ranking.
8. Write tissue-specific rankings if tissue columns are present.

Outputs:

- `snp_prioritization_by_scorer.csv`
- `snp_prioritization_overall.csv`
- `snp_prioritization_by_tissue.csv`

## Step 05: eQTL/sQTL-Style Collapse

Script:

- `05_collapse_eqtl_sqtl.py`

Input:

- `variant_scores_tidy.csv`

Key operations:

1. Keep gene-annotated score rows.
2. Filter RNA-seq gene-mask scorer rows into eQTL-like outputs.
3. Filter splicing scorer/output rows into sQTL-like outputs.
4. Collapse by SNP and gene.
5. Keep best scorer, track, tissue, and interval metadata.
6. Rank genes within each SNP.

Outputs:

- `variant_scores_per_snp_gene_eqtl.csv`
- `variant_scores_per_snp_gene_sqtl.csv`

## Workflow Outputs

| Output | Meaning |
|---|---|
| `region_info.txt` | Locus parameters and lead-SNP metadata |
| `region_significant_snps.csv` | Significant regional GWAS hits |
| `index_snps_independent_signals.csv` | LD-clumped independent index SNPs |
| `ld_proxies_union_all_signals.csv` | hg19 proxy variants |
| `ld_proxies_union_all_signals_GRCh38.csv` | AlphaGenome-ready variants |
| `alphagenome_allGTEx_allOutputs_LONG.parquet` | Track-level predictions |
| `alphagenome_allGTEx_allOutputs_COLLAPSED.csv` | Variant/tissue/modality summary |
| `variant_scores_tidy.csv` | Tidy recommended-scorer outputs |
| `snp_prioritization_overall.csv` | Consensus SNP ranking |
| `variant_scores_per_snp_gene_eqtl.csv` | RNA-seq/gene-level collapsed signals |
| `variant_scores_per_snp_gene_sqtl.csv` | Splicing collapsed signals |
