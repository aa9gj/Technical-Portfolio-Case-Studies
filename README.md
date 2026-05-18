# Technical Portfolio Case Studies

This repository is a lightweight companion to my public and private GitHub projects. It is meant to help hiring managers quickly understand the problems I chose to solve, how I decomposed them, what stacks I used, and what engineering tradeoffs shaped each implementation.

For private repositories, the case studies stay public while the source code can be shared separately with interviewers when appropriate.

## Project Index

### Applied AI

| Project | Problem Area | Stack | What It Shows | Status |
|---|---|---|---|---|
| [FitCast](projects/fitcast/) | Job search automation, fit scoring, resume tailoring | Python, Anthropic Claude, Pydantic, public ATS APIs, pytest, GitHub Actions | Product decomposition, LLM orchestration, deterministic scoring, cost-aware pipelines, CI/smoke testing | Draft v1 |
| [image-to-text-analysis](projects/image-to-text-analysis/) | Image-to-text extraction and product-change detection | Python, OpenAI GPT-5, OpenAI embeddings, Nemotron-Parse, RolmOCR, Nanonets OCR, PyTorch/CUDA, SQLite, RapidFuzz | Multimodal extraction, structured JSON generation, retry/error handling, embedding retrieval, deterministic similarity scoring, human-readable reports | Draft v1 |
| [Bio Claims App](projects/bio-claims-app/) | Biomedical RAG, claim verification, knowledge graph grounding | Python, Streamlit, FastAPI, LiteLLM, ChromaDB, BGE embeddings, biomedical KGs, SQLite, pytest | Hybrid RAG architecture, third-party KG integration, evidence provenance, claim scoring, human review, evaluation pipelines | Draft v1 |

### Computational Biology

| Project | Problem Area | Stack | What It Shows | Status |
|---|---|---|---|---|
| [bulk-rnaseq-nf](projects/bulk-rnaseq-nf/) | Bulk RNA-seq workflow automation | Nextflow DSL2, HISAT2, StringTie, RSeQC, featureCounts, HTSeq, MultiQC, Docker, Singularity, SLURM | Reproducible bioinformatics pipelines, modular workflow design, HPC/container profiles, QC diagnostics, count-matrix generation | Draft v1 |
| [low-pass-WGS-pipeline](projects/low-pass-wgs-pipeline/) | Low-pass whole-genome sequencing workflow automation | Bash, SLURM, BWA, SAMtools, GATK, BCFtools, FastQC, MultiQC, BEAGLE, R, bats | HPC genomics workflows, variant calling, joint genotyping, imputation, setup validation, modular pipeline documentation | Draft v1 |
| [Whole-Genome-Bisulfite-Sequencing-Pipeline](projects/whole-genome-bisulfite-sequencing-pipeline/) | WGBS methylation workflow automation | Bash, SLURM, Trim Galore, FastQC, MultiQC, Bismark, HISAT2/Bowtie2, SAMtools, methylKit, GenomicRanges, R | Bisulfite sequencing pipelines, CpG methylation extraction, differential methylation analysis, coverage/QC reporting, genomic annotation | Draft v1 |
| [GWASTargetChase](projects/gwas-target-chase/) | Cross-species GWAS target prioritization | R, Bioconductor, GenomicRanges, rtracklayer, OpenTargets GraphQL API, IMPC REST API, Zoonomia orthology, testthat | Translational genetics, gene prioritization, API/data integration, orthology mapping, R package design, offline/online workflows | Draft v1 |
| [alphagenome-test](projects/alphagenome-test/) | AI-assisted regulatory variant prioritization | Python, AlphaGenome API, pandas, NumPy, cyvcf2, bcftools, pyliftover, pyarrow, 1000 Genomes, GTEx metadata | GWAS locus follow-up, LD proxy extraction, hg19-to-hg38 liftover, checkpointed API prediction, cross-tissue scoring, variant ranking | Draft v1 |
| [Bone_proteogenomics_manuscript](projects/bone-proteogenomics-manuscript/) | Long-read proteogenomics and sQTL-to-isoform mapping | R, Python, SLURM, coloc, PacBio Iso-Seq, SQANTI3, cDNA_Cupcake, DESeq2, tappAS, GenomicRanges, GENCODE, GTEx, GEO, Zenodo | Manuscript-grade reproducible analysis, GWAS/sQTL colocalization, long-read transcriptome generation, isoform/protein impact mapping, source-data integration | Draft v1 |

### Clinical Data Analytics

No public case study yet. This section is reserved for projects focused on clinical data modeling, real-world evidence, cohort analytics, dashboards, or regulated health-data workflows.

## Reading Guide

Each project writeup is intentionally short and decision-focused:

- Problem statement: what pain point the project addresses.
- System design: how the work is decomposed into components and pipelines.
- Stack: languages, frameworks, APIs, data stores, testing, and deployment tools.
- Tradeoffs: what I optimized for, what I avoided, and why.
- Learning: what the project taught me technically.

The goal is not to duplicate every repo README. The goal is to make the engineering thinking easy to inspect.

## Folder Convention

Each project gets its own folder under `projects/`:

```text
projects/
  project-name/
    README.md
    architecture.md
    pipelines.md
    stack.md
    security-privacy.md
    project-structure.md
    screenshots/
```

Small projects can keep everything in the project `README.md`. Larger projects can use the supporting files when there is enough architecture, workflow, or security context to justify the extra detail.
