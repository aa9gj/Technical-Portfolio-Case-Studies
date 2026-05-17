# bulk-rnaseq-nf Stack

## Summary

The stack is a conventional but carefully assembled RNA-seq workflow stack: Nextflow for orchestration, mature command-line bioinformatics tools for analysis, Conda/Docker/Singularity for reproducibility, and cluster/cloud profiles for portability.

## Workflow And Runtime

| Layer | Tools | Role |
|---|---|---|
| Workflow engine | Nextflow DSL2 | Resumable, modular orchestration |
| Runtime language | Groovy/Nextflow scripts, Bash process scripts | Process definitions and tool execution |
| Local environment | Conda/Bioconda | Developer and non-container installs |
| Container | Docker, micromamba | Bundled dependency image |
| HPC container | Singularity-compatible Docker image | Cluster execution where Docker is unavailable |
| Cluster/cloud execution | SLURM, PBS/Torque, AWS Batch | Scalable execution profiles |

## Bioinformatics Tools

| Step | Tool |
|---|---|
| Read trimming | Trim Galore / Cutadapt |
| Read QC | FastQC |
| Alignment | HISAT2 |
| BAM conversion/sorting/indexing | SAMtools |
| RNA-seq QC | RSeQC |
| Annotation conversion | UCSC `gtfToGenePred`, `genePredToBed` |
| Transcript assembly | StringTie |
| Count matrix generation | prepDE.py |
| Gene-level counting | featureCounts/Subread |
| Alternative counting | HTSeq |
| Report aggregation | MultiQC |

## Configuration And Outputs

| File/Artifact | Purpose |
|---|---|
| `nextflow.config` | Defaults, profiles, resources, reports, trace config |
| `params.yaml` | Run-specific samples, references, options |
| `environment.yml` | Conda/Bioconda dependencies |
| `Dockerfile` | Container build |
| `CITATIONS.md` | Tool citations |
| `results/` | Trimmed reads, BAMs, assemblies, counts, QC reports |
| `pipeline_info/` | Nextflow execution metadata |

## Why This Stack Fits

- Nextflow is well suited for resumable, file-based scientific workflows.
- HISAT2 and StringTie are transparent and widely used for splice-aware alignment and transcript assembly.
- RSeQC provides diagnostic depth beyond basic read QC.
- MultiQC gives a single review surface for many samples and tools.
- Docker/Singularity/Conda profiles make dependency management practical across laptops and clusters.

## Production Hardening Opportunities

- Add a parameter schema.
- Add `nf-test` coverage.
- Add a tiny test fixture.
- Pin container digests in production runs.
- Separate public example params from run-specific params.
- Add cloud object-storage examples for AWS Batch.
