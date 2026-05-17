# low-pass-WGS-pipeline Stack

## Summary

The stack is a traditional HPC genomics stack: Bash and SLURM for orchestration, mature command-line bioinformatics tools for each stage, R for QC visualization, and bats for lightweight shell testing.

## Workflow And Runtime

| Layer | Tools | Role |
|---|---|---|
| Orchestration | Bash, SLURM, SLURM arrays | Stage-level job submission and parallelism |
| Shared utilities | `scripts/common.sh` | Logging, validation, module loading, cleanup |
| Config | `config/pipeline.config` | Environment-specific references, paths, resources |
| Validation | `scripts/validate_setup.sh` | Preflight checks before running large jobs |
| Testing | bats | Shell-script behavior testing |

## Bioinformatics Tools

| Stage | Tools |
|---|---|
| Raw QC | FastQC, MultiQC |
| Reference indexing | BWA, SAMtools, Picard/GATK |
| Alignment | BWA-MEM |
| BAM processing | SAMtools, GATK/Picard |
| Coverage | SAMtools depth and related summaries |
| Variant calling | GATK HaplotypeCaller, GenomicsDBImport, GenotypeGVCFs |
| VCF operations | BCFtools, htslib/tabix |
| Filtering/QC | VQSR-style GATK scripts, R plotting |
| Imputation | BEAGLE |

## Data Artifacts

| Artifact | Purpose |
|---|---|
| FASTQ | Raw paired sequencing reads |
| SAM/BAM/BAI | Alignment intermediates and indexed alignments |
| flagstat output | Alignment quality summaries |
| coverage summaries | Low-pass depth assessment |
| GVCF | Per-sample variant evidence |
| GenomicsDB workspace | Cohort import by chromosome |
| joint VCF | Cohort-level genotypes |
| imputed VCF | BEAGLE-imputed genotypes |
| QC plots | Variant and imputation quality diagnostics |

## Why This Stack Fits

- SLURM arrays are natural for per-sample and per-chromosome work.
- GATK GVCF workflows are familiar for cohort joint genotyping.
- BEAGLE is a standard choice for genotype imputation.
- Bash modules are transparent and easy to adapt on clusters.
- Setup validation protects against expensive avoidable failures.

## Where The Stack Could Evolve

- Nextflow or Snakemake could provide stronger DAG-level dependency tracking.
- Containers could reduce dependency drift across clusters.
- Parameter schemas could catch invalid configs before submission.
- Automated CI could run shellcheck and bats on every change.
