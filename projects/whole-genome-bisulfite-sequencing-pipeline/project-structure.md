# Project Structure

The source repository is organized by pipeline stage.

```text
Whole-Genome-Bisulfite-Sequencing-Pipeline/
  README.md
  config/
    config.sh
    config.R
  modules/
    00_QC/
      trimgalore.slurm
      multiqc.slurm
    01_reference/
      index_reference.slurm
    02_alignment/
      bismark_align.slurm
    03_deduplication/
      deduplicate.slurm
    04_methylation/
      extract_methylation.slurm
    05_reports/
      coverage_report.slurm
      extract_methylation_stats
    06_post_processing/
      step0_setup_differential_methylation.R
      step1_methylation_QC.R
      step2_methylation_filtering.R
      step3_annotate_cpgs.R
      step3b_alt_analysis_dmrs.R
  legacy/
    differential_methylation_methylkit.R
  rename_dirs.sh
```

## Root

| File | Purpose |
|---|---|
| `README.md` | Main project documentation, dependencies, setup, stages, usage, outputs, and troubleshooting |
| `rename_dirs.sh` | Utility script for renaming directories |

## Configuration

| File | Purpose |
|---|---|
| `config/config.sh` | Shell/SLURM configuration for paths, output directories, tool paths, resources, arrays, Bismark parameters, Trim Galore parameters, file-list names, and helper functions |
| `config/config.R` | R configuration for methylation directories, genome assembly, annotation inputs, metadata, methylKit parameters, filtering, tiling, differential methylation thresholds, annotation distances, package loading, output directories, and session info |

## Pipeline Modules

| Directory | Purpose |
|---|---|
| `modules/00_QC/` | Trim Galore paired-end trimming, FastQC, and MultiQC aggregation |
| `modules/01_reference/` | Bismark bisulfite reference preparation |
| `modules/02_alignment/` | Bismark alignment with configurable aligner backend |
| `modules/03_deduplication/` | Bismark duplicate removal for aligned BAMs |
| `modules/04_methylation/` | Bismark methylation extraction from deduplicated BAMs |
| `modules/05_reports/` | CpG coverage and methylation-summary reporting |
| `modules/06_post_processing/` | methylKit object setup, QC, filtering/normalization, tiled DMR-style preparation, and genomic annotation |

## Post-Processing Scripts

| Script | Purpose |
|---|---|
| `step0_setup_differential_methylation.R` | Find coverage files, validate metadata, create sample/treatment mappings, and save raw methylKit objects |
| `step1_methylation_QC.R` | Generate methylation and coverage plots, per-sample stats, and QC summaries |
| `step2_methylation_filtering.R` | Filter by coverage, normalize, unite CpGs, filter low-variability CpGs, optionally remove SNP-overlapping CpGs, and create tiled methylation objects |
| `step3_annotate_cpgs.R` | Annotate significant CpGs using genomic feature overlaps and nearest-gene context |
| `step3b_alt_analysis_dmrs.R` | Alternative DMR-style analysis path using tiled methylation objects |

## Legacy

The `legacy/` directory keeps an earlier methylKit analysis script. That is useful for preserving analysis history, but the main architecture is the stage-based modular workflow under `modules/`.

## Portfolio Interpretation

For interviews, the structure communicates that the project is not only a set of commands. It is a reproducible workflow decomposition:

- Configuration before execution.
- Read QC before alignment.
- Reference preparation before bisulfite alignment.
- Deduplication before methylation extraction.
- Coverage reporting before inference.
- methylKit object creation before filtering/modeling.
- Annotation after statistical results.
