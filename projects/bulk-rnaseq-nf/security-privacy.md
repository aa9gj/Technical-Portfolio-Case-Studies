# bulk-rnaseq-nf Security And Privacy Notes

## Sensitive Data

Bulk RNA-seq workflows can expose sensitive information even when the pipeline code is public:

- Sample IDs.
- Study names.
- Organism and tissue context.
- Local filesystem paths.
- FASTQ/BAM files.
- Reference files and annotations.
- Count matrices and QC reports.
- Cluster account names and queue settings.

The public case study focuses on workflow architecture and avoids sample-specific details.

## Gitignore Policy

The source repo excludes common large and sensitive artifacts:

| Pattern | Reason |
|---|---|
| `work/` | Nextflow intermediate files |
| `results/` | Generated outputs and QC reports |
| `.nextflow/`, `.nextflow.log*` | Runtime cache and logs |
| FASTQ/FQ files | Raw sequencing data |
| BAM/SAM files | Aligned sequencing data |
| FASTA/FA files | Large reference genomes |
| GTF/GFF files | Large annotation files |
| Pipeline reports | Run-specific execution outputs |
| Conda/cache/editor files | Local environment noise |

## Parameter File Risk

Run-specific parameter files can contain local paths and sample identifiers. For public use, a sanitized `params.example.yaml` is preferable to committing real run parameters.

Good practice:

- Keep real sample manifests private.
- Use placeholder sample IDs in examples.
- Use environment-agnostic reference paths in docs.
- Avoid committing cluster account names if they identify a private environment.

## Container Supply Chain

The Docker image is built from `environment.yml` and published through GitHub Actions. For production reproducibility, future releases could pin:

- Exact package versions.
- Container digests.
- Reference genome and annotation versions.
- Tool checksums where practical.

## Data Governance

Because RNA-seq data can be sensitive, access controls should be handled outside the workflow:

- Store raw data in controlled storage.
- Run on approved compute environments.
- Keep work directories out of shared public locations.
- Retain or delete intermediate files according to project policy.
- Archive parameter files and reports with the analysis record.

## Public Case Study Strategy

This portfolio writeup describes:

- Workflow design.
- Tool choices.
- QC and quantification logic.
- Execution profiles.
- Reproducibility tradeoffs.

It does not include real sample IDs, file paths, count matrices, QC reports, or generated biological results.
