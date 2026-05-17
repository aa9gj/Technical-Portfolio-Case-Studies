# Stack

This project combines HPC shell orchestration with R/Bioconductor methylation analysis.

## Core Stack

| Area | Tools |
|---|---|
| Cluster execution | SLURM, Bash |
| Read trimming | Trim Galore, Cutadapt |
| Read QC | FastQC |
| QC aggregation | MultiQC |
| Reference prep | Bismark genome preparation |
| Alignment | Bismark with HISAT2 or Bowtie2 support |
| BAM handling | Bismark deduplication, SAMtools |
| Methylation extraction | Bismark methylation extractor |
| Coverage summaries | Bash, awk, shell utilities |
| Statistical methylation analysis | R, methylKit |
| Genomic intervals | GenomicRanges, genomation |
| Annotation import | rtracklayer |
| Data wrangling | data.table, tidyverse, dplyr, tidyr |
| Visualization | ggplot2, methylKit plots |
| Matrix/stat helpers | matrixStats |

## Why These Tools

| Tool | Reason |
|---|---|
| Bismark | End-to-end bisulfite sequencing support for reference preparation, alignment, deduplication, and methylation extraction |
| Trim Galore | Convenient adapter/quality trimming wrapper commonly paired with bisulfite sequencing workflows |
| FastQC/MultiQC | Standard way to inspect and aggregate read-level quality reports |
| SLURM arrays | Natural fit for sample-parallel FASTQ, BAM, and methylation-extraction jobs |
| methylKit | Practical R framework for CpG methylation objects, filtering, normalization, unification, differential methylation, and QC plots |
| GenomicRanges | Standard Bioconductor abstraction for interval overlap and feature annotation |
| rtracklayer | Imports GTF/GFF annotations into R for downstream genomic context |

## Configuration Surfaces

Shell configuration:

- Project directories.
- Reference genome directory.
- Output directory layout.
- Tool paths.
- SLURM sample count and concurrency.
- Memory, CPU, and time defaults.
- Bismark parallelism and aligner choice.
- Trim Galore quality, length, stringency, and poly-A behavior.
- Input file list names.

R configuration:

- Project and result directories.
- Coverage-file pattern.
- Genome assembly.
- GTF annotation path.
- Optional SNP file.
- Sample sheet and metadata paths.
- methylKit context and minimum coverage.
- Coverage filtering thresholds.
- CpG variability thresholds.
- Tiling window and step size.
- Differential methylation thresholds.
- Annotation distance thresholds.

## Environment Notes

The repo is set up for HPC environments where bioinformatics tools are typically available through modules, conda environments, or configured paths. The public case study does not assume that large genomic references or private FASTQs are present.

The most useful future packaging improvement would be adding environment files for both layers:

- A Conda or container environment for Trim Galore, FastQC, MultiQC, Bismark, aligner backend, SAMtools, and related command-line tools.
- An R environment lockfile for methylKit, GenomicRanges, genomation, rtracklayer, data.table, tidyverse, ggplot2, and matrixStats.

## Engineering Signal

The stack shows comfort moving between:

- Cluster scheduling and shell scripting.
- Genomics command-line tools.
- R/Bioconductor statistical workflows.
- File-oriented workflow design.
- QC-first scientific data processing.
