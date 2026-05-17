# bulk-rnaseq-nf Project Structure

## Top-Level Map

```text
bulk-rnaseq-nf/
  README.md
  CITATIONS.md
  nextflow.config
  params.yaml
  environment.yml
  Dockerfile
  workflows/
  modules/
  .github/workflows/
```

## Workflow Entrypoint

| File | Responsibility |
|---|---|
| `workflows/main.nf` | Main DSL2 workflow orchestration, parameter validation, channel construction, module wiring, completion logging |

## Modules

| Path | Responsibility |
|---|---|
| `modules/preprocess/concat_fastq.nf` | Concatenate multi-lane FASTQ files |
| `modules/preprocess/trim_galore.nf` | Adapter trimming and FastQC |
| `modules/align/hisat2_index.nf` | Build HISAT2 index from genome FASTA |
| `modules/align/hisat2_align.nf` | Align paired reads with HISAT2 and write BAM |
| `modules/align/samtools_sort.nf` | Sort and index BAM files, emit stats |
| `modules/qc/gtf_to_bed.nf` | Convert GTF annotation to BED12 for RSeQC |
| `modules/qc/rseqc_bam_stat.nf` | Basic BAM statistics |
| `modules/qc/rseqc_infer_experiment.nf` | Library strandedness inference |
| `modules/qc/rseqc_read_distribution.nf` | Read distribution across genomic regions |
| `modules/qc/rseqc_inner_distance.nf` | Insert size diagnostics |
| `modules/qc/rseqc_gene_body_coverage.nf` | Gene body coverage bias |
| `modules/qc/rseqc_tin.nf` | Transcript Integrity Number |
| `modules/qc/multiqc.nf` | MultiQC report generation |
| `modules/transcript_assembly/stringtie_first.nf` | First-pass StringTie assembly |
| `modules/transcript_assembly/stringtie_merge.nf` | Merge sample assemblies |
| `modules/transcript_assembly/stringtie_second.nf` | Second-pass StringTie quantification |
| `modules/transcript_assembly/prepde.nf` | Count matrix generation from StringTie outputs |
| `modules/quantification/featurecounts.nf` | Optional featureCounts matrix |
| `modules/quantification/htseq.nf` | Optional HTSeq per-sample counts and merge |

## Config And Environment

| File | Responsibility |
|---|---|
| `nextflow.config` | Pipeline defaults, profiles, resources, reports, and trace settings |
| `params.yaml` | Run-specific samples, references, and options |
| `environment.yml` | Conda environment with Bioconda/Conda-forge tools |
| `Dockerfile` | Micromamba-based container image |
| `.github/workflows/docker-publish.yml` | Build and publish image to GitHub Container Registry |
| `.github/dependabot.yml` | Dependency update configuration |

## Documentation

| File | Purpose |
|---|---|
| `README.md` | User-facing installation, usage, parameters, outputs, troubleshooting |
| `CITATIONS.md` | Scientific/tool citations |

## Generated Outputs

Typical result directories include:

| Directory | Contents |
|---|---|
| `trimmed/` | Trimmed FASTQ files and trimming reports |
| `fastqc/` | FastQC HTML/ZIP reports |
| `hisat2/` | HISAT2 alignment logs |
| `aligned/` | Sorted BAM and BAI files |
| `stringtie/` | First-pass, merged, and quantification outputs |
| `ballgown/` | Ballgown tables |
| `counts/` | StringTie/prepDE gene and transcript count matrices |
| `featurecounts/` | Optional featureCounts output and summary |
| `htseq/` | Optional HTSeq count matrix |
| `rseqc/` | RSeQC module outputs |
| `multiqc/` | MultiQC report and data |
| `pipeline_info/` | Nextflow timeline, report, trace, DAG |

## Current Structure Strengths

- Process modules are grouped by workflow stage.
- The main workflow stays readable because tool commands live in modules.
- Profiles keep execution concerns separate from biological logic.
- Documentation includes both usage and QC interpretation.

## Structure Improvements

- Add `conf/` for reusable profile fragments.
- Add `assets/` or `examples/` for sanitized parameter files.
- Add `tests/` with tiny FASTQ/reference fixtures.
- Add `modules.json` or nf-core style metadata if moving toward nf-core conventions.
