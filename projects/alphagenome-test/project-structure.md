# Project Structure

The source repo is a numbered-script pipeline.

```text
alphagenome-test/
  README.md
  .gitignore
  01_prepare_snps_for_alphagenome.py
  01B_liftover_hg19_hg38.py
  02_run_alphagenome_alltiss_allmodal.py
  02B_recollapse_from_long.py
  03_score_variant.py
  04_rank_prioritization.py
  05_collapse_eqtl_sqtl.py
```

## Files

| File | Purpose |
|---|---|
| `README.md` | Describes the locus, required support files, setup, run order, outputs, references, and terms |
| `.gitignore` | Excludes secrets, large genomics files, model/cache artifacts, generated outputs, and checkpoints |
| `01_prepare_snps_for_alphagenome.py` | Parses GWAS summary statistics, identifies significant SNPs, selects 1000 Genomes EUR samples, computes LD, clumps independent signals, and writes LD proxies |
| `01B_liftover_hg19_hg38.py` | Converts proxy variants from hg19 to GRCh38 and reverse-complements alleles for strand-flipped liftover hits |
| `02_run_alphagenome_alltiss_allmodal.py` | Runs AlphaGenome `predict_variant()` across GTEx tissues and output modalities, with retries and checkpoints |
| `02B_recollapse_from_long.py` | Rebuilds collapsed prediction summaries from long parquet output |
| `03_score_variant.py` | Runs AlphaGenome recommended variant scorers and writes tidy score rows |
| `04_rank_prioritization.py` | Aggregates scorer rows into per-scorer, consensus, and tissue-specific SNP rankings |
| `05_collapse_eqtl_sqtl.py` | Collapses tidy scores into per-SNP-per-gene eQTL-like and sQTL-like tables |

## Output Convention

The scripts communicate through named files in the working directory:

- Step 01 writes GWAS/LD proxy tables.
- Step 01B writes GRCh38-ready proxy variants.
- Step 02 writes long AlphaGenome predictions and collapsed summaries.
- Step 03 writes tidy scorer outputs.
- Step 04 writes ranked SNP prioritization tables.
- Step 05 writes gene-level RNA/splicing collapsed tables.

## Portfolio Interpretation

The structure shows a research pipeline evolving toward reproducibility:

- Numbered stages make run order obvious.
- File outputs are named and documented.
- Checkpoints make expensive model calls resumable.
- Recollapse logic avoids repeating API calls.
- The main improvement opportunity is moving constants and local paths into reusable configuration.
