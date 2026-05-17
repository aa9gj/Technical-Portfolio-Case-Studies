# Pipeline And Workflow

This project is a stage-driven WGBS workflow. Each stage has a bounded responsibility and produces intermediate files that become the inputs for the next stage.

## Stage 0: Configuration

Files:

- `config/config.sh`
- `config/config.R`

Responsibilities:

- Define project and output directories.
- Point to the reference genome and annotation files.
- Configure tool paths where needed.
- Configure SLURM resources and array concurrency.
- Configure Bismark, Trim Galore, methylKit, filtering, tiling, and annotation parameters.
- Provide helper functions for validation, output-directory creation, and logging.

## Stage 1: FASTQ QC And Trimming

Files:

- `modules/00_QC/trimgalore.slurm`
- `modules/00_QC/multiqc.slurm`

Inputs:

- `R1_files.txt`
- `R2_files.txt`
- Raw paired FASTQ files.

Outputs:

- Trimmed paired FASTQs.
- FastQC reports.
- MultiQC aggregate report.

Key idea:

Trim Galore is run as a SLURM array, so each sample pair gets its own task. The script validates the R1/R2 file lists, checks each file path, creates output directories, and runs paired-end trimming with quality, length, stringency, and poly-A settings from config.

## Stage 2: Reference Preparation

File:

- `modules/01_reference/index_reference.slurm`

Inputs:

- Reference FASTA directory.

Outputs:

- Bismark `Bisulfite_Genome` index directory.

Key idea:

Bismark requires a bisulfite-aware reference index. Keeping this as its own stage prevents expensive alignment jobs from starting before the genome folder is prepared.

## Stage 3: Bisulfite Alignment

File:

- `modules/02_alignment/bismark_align.slurm`

Inputs:

- Trimmed R1/R2 file lists.
- Bismark-prepared genome folder.

Outputs:

- Bismark-aligned BAM files.
- Bismark alignment reports.

Key idea:

Alignment is a high-resource SLURM array stage. The script validates the reference index, selects HISAT2 or Bowtie2 behavior through `BISMARK_ALIGNER`, and emits Bismark outputs into the configured alignment directory.

## Stage 4: Deduplication

File:

- `modules/03_deduplication/deduplicate.slurm`

Inputs:

- `bismark_files_bam.txt`
- Bismark BAM files.

Outputs:

- Deduplicated BAM files.
- Bismark deduplication reports.

Key idea:

PCR duplicates can inflate methylation evidence. The stage detects paired-end versus single-end mode from BAM naming and runs `deduplicate_bismark` before methylation extraction.

## Stage 5: Methylation Extraction

File:

- `modules/04_methylation/extract_methylation.slurm`

Inputs:

- `dedup_bam_files.txt`
- Deduplicated BAM files.
- Reference genome folder.

Outputs:

- `*.bismark.cov.gz`
- bedGraph files.
- cytosine reports.
- methylation extraction reports.

Key idea:

This stage converts aligned reads into methylation calls. It uses Bismark's methylation extractor with gzip output, bedGraph output, cytosine reports, parallel workers, and configurable read-position ignore settings.

## Stage 6: Coverage And Methylation Reports

Files:

- `modules/05_reports/coverage_report.slurm`
- `modules/05_reports/extract_methylation_stats`

Outputs:

- `coverage_summary.tsv`
- `coverage_detailed.tsv`
- CpG/CHG/CHH methylation summary tables.

Key idea:

The reports summarize whether samples have enough CpG coverage for downstream analysis. Coverage thresholds such as 1x, 5x, 10x, 15x, and 30x provide a compact view of data quality.

## Stage 7: methylKit Setup

File:

- `modules/06_post_processing/step0_setup_differential_methylation.R`

Inputs:

- Bismark coverage files.
- Optional sample sheet.
- Optional metadata file.

Outputs:

- `myobj_raw.rds`
- `sample_metadata.rds`
- `file_list.rds`

Key idea:

The script loads coverage files, validates file availability, maps samples to IDs and treatment labels, and creates the initial methylKit object for downstream analysis.

## Stage 8: Methylation QC

File:

- `modules/06_post_processing/step1_methylation_QC.R`

Outputs:

- Per-sample methylation statistics.
- Methylation histograms.
- Coverage histograms.
- Combined coverage distribution plot.
- Methylation boxplot.
- QC summary text file.
- `myobj_qc.rds`

Key idea:

Before differential methylation, the pipeline makes sample-level methylation and coverage behavior visible. This catches low-coverage samples, extreme methylation patterns, and sample-level anomalies.

## Stage 9: Filtering, Normalization, And DMR Preparation

File:

- `modules/06_post_processing/step2_methylation_filtering.R`

Outputs:

- `meth_filtered.rds`
- `meth_tiles.rds`
- SD histogram.
- Correlation plot.
- Clustering plot.
- PCA plot.

Key idea:

The script filters high-coverage outliers, normalizes coverage, unites CpG loci across samples, filters low-variability CpGs, optionally removes SNP-overlapping CpGs, and creates tiled windows for DMR-style analysis.

## Stage 10: Annotation

Files:

- `modules/06_post_processing/step3_annotate_cpgs.R`
- `modules/06_post_processing/step3b_alt_analysis_dmrs.R`

Inputs:

- Differential CpG or tiled DMR-style results.
- GTF/GFF annotation.

Outputs:

- Annotated CpG or region tables with gene/feature context.

Key idea:

Raw methylation loci become more useful when connected to promoters, exons, introns, intergenic regions, nearby genes, and methylation direction.

## Expected Outputs

| Output | Meaning |
|---|---|
| FastQC/MultiQC reports | Read-level QC before downstream interpretation |
| Bismark alignment reports | Bisulfite alignment performance and mapping quality context |
| Deduplication reports | Duplicate burden per sample |
| `*.bismark.cov.gz` | Per-CpG methylation calls |
| bedGraph/cytosine reports | Browser- and cytosine-context-friendly methylation outputs |
| `coverage_summary.tsv` | Sample-level CpG coverage thresholds |
| `methylation_qc_stats.tsv` | Per-sample methylation/coverage summaries |
| `myobj_raw.rds` / `myobj_qc.rds` | methylKit handoff objects |
| `meth_filtered.rds` | Filtered CpG matrix |
| `meth_tiles.rds` | Tiled methylation regions for DMR-style analysis |
| Annotated result tables | Genomic interpretation layer for significant methylation changes |
