# low-pass-WGS-pipeline Project Structure

## Top-Level Map

```text
low-pass-WGS-pipeline/
  README.md
  config/
  docs/
  scripts/
  Modules/
  tests/
```

## Config And Shared Scripts

| Path | Responsibility |
|---|---|
| `config/pipeline.config.example` | Template for references, paths, samples, chromosomes, resources, modules, and imputation settings |
| `scripts/common.sh` | Shared Bash logging, validation, command checks, array helpers, module loading, cleanup |
| `scripts/validate_setup.sh` | Preflight validation for tools, config, reference indexes, structure, SLURM, and disk space |

## Documentation

| File | Purpose |
|---|---|
| `README.md` | Project overview, quick start, module list, requirements, test instructions |
| `docs/WORKFLOW.md` | End-to-end staged workflow instructions |
| `docs/DEPENDENCIES.md` | Tool requirements, installation options, disk/memory guidance |

## Modules

| Path | Responsibility |
|---|---|
| `Modules/00_fastq_qc/fastqc.slurm` | FastQC per-sample array job |
| `Modules/00_fastq_qc/multiqc.slurm` | Aggregate QC reports |
| `Modules/01_reference/index_reference.slurm` | Build BWA, SAMtools, and GATK reference indexes |
| `Modules/02_alignment/bwa_align.slurm` | BWA-MEM paired-end alignment |
| `Modules/02_alignment/calculate_avg_depth.slurm` | SAM/BAM processing and average coverage |
| `Modules/02_alignment/calculate_alignment_stats.slurm` | Generate `samtools flagstat` outputs |
| `Modules/02_alignment/extract_relevant_stats` | Parse key alignment stats into TSV |
| `Modules/04_coverage/calculate_depth.slurm` | Detailed depth calculation |
| `Modules/05_processing/dedup.slurm` | Duplicate marking |
| `Modules/05_processing/bqsr.slurm` | Generate base recalibration tables |
| `Modules/05_processing/apply_bqsr.slurm` | Apply base recalibration |
| `Modules/05_processing/collect_metrics.slurm` | Alignment summary metrics |
| `Modules/05_processing/collect_insert_size.slurm` | Insert-size metrics |
| `Modules/06_variant_calling/haplotypecaller_gvcf.slurm` | Per-sample GVCF calling |
| `Modules/06_variant_calling/genomic_DBImport.slurm` | GenomicsDB import by chromosome |
| `Modules/06_variant_calling/genotype_gvcfs.slurm` | Joint genotyping by chromosome |
| `Modules/06_variant_calling/merge_index_jointvcf.slurm` | Merge and index chromosome VCFs |
| `Modules/06_variant_calling/snp_VQSR.slurm` | SNP recalibration model |
| `Modules/06_variant_calling/apply_VQSR.slurm` | Apply VQSR filtering |
| `Modules/06_variant_calling/compressnblock.slurm` | Compress and index final VCFs |
| `Modules/06_variant_calling/raw_variant_qc` | Extract raw variant QC metrics |
| `Modules/06_variant_calling/qc_plots.R` | Create variant QC plots |
| `Modules/09_imputation/beagle_impute.slurm` | BEAGLE genotype imputation |
| `Modules/09_imputation/imputation_qual_extract.slurm` | Extract imputation quality metrics |
| `Modules/09_imputation/imputation_quality.R` | Analyze imputation quality |

Most modules also have companion `*_README.md` files that explain purpose, inputs, outputs, usage, resources, and caveats.

## Tests

| Path | Responsibility |
|---|---|
| `tests/test_extract_relevant_stats.bats` | bats test for parsing `samtools flagstat` output |
| `tests/data/example.flagstat.txt` | Small fixture for alignment-stat parsing |

## Structure Strengths

- The numeric module prefixes communicate workflow order.
- Module-level READMEs make each step independently understandable.
- Shared utilities reduce copy/paste error handling.
- The validation script gives users a preflight checklist before expensive runs.

## Structure Improvements

- Add a public `examples/` directory with sanitized sample lists and configs.
- Add `tests/fixtures/` for more small parser and validation examples.
- Add `ci/` or GitHub Actions for shellcheck and bats.
- Add a workflow-engine version if the project graduates from staged SLURM scripts.
