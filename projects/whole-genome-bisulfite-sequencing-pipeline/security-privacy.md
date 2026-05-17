# Security And Privacy

WGBS workflows handle sensitive biological data and should be documented carefully when summarized in a public portfolio.

## Data Sensitivity

Potentially sensitive artifacts include:

- Raw FASTQ files.
- Trimmed FASTQs.
- Bismark BAM files.
- Deduplicated BAM files.
- Per-CpG methylation coverage files.
- cytosine reports and bedGraph files.
- Sample sheets and metadata.
- Treatment/group labels.
- Reference paths and internal directory structures.
- Differential methylation results.
- Annotated genomic result tables.
- SLURM logs containing file paths, sample IDs, or error messages.

## Public Case Study Boundary

The portfolio writeup describes:

- The problem being solved.
- The workflow architecture.
- The tool stack.
- The pipeline stages.
- The engineering tradeoffs.
- The validation opportunities.

It intentionally does not include:

- Raw sample IDs.
- Private metadata values.
- Internal filesystem paths.
- Result tables.
- Plots from private data.
- Methylation findings.
- Dataset-specific interpretation.

## Repo Hygiene Recommendations

The source repository should continue to keep large or sensitive data out of git:

- `*.fastq`
- `*.fastq.gz`
- `*.fq`
- `*.fq.gz`
- `*.bam`
- `*.sam`
- `*.cram`
- `*.bai`
- `*.cov`
- `*.cov.gz`
- `*.bedGraph`
- cytosine reports
- reference genomes and indexes
- sample sheets with identifiers
- metadata files
- output directories
- SLURM log files
- RDS objects generated from private data
- differential methylation result tables

## Operational Controls

For real WGBS projects, the safest operating pattern is:

1. Store raw and derived genomic files in approved research storage.
2. Keep sample metadata in access-controlled locations.
3. Use deidentified sample IDs in pipeline file lists.
4. Avoid committing absolute internal paths.
5. Keep config examples generic.
6. Review SLURM logs before sharing them.
7. Publish only synthetic or explicitly approved example outputs.
8. Treat methylation findings as study results, not generic pipeline artifacts.

## Interview Positioning

When discussing the project publicly, the best framing is architecture-first:

- "I built a modular WGBS pipeline around Bismark and methylKit."
- "The repo shows how I decomposed bisulfite sequencing into cluster stages and R post-processing."
- "The public writeup avoids private data and focuses on reproducibility, QC, and methylation-specific design decisions."

That gives hiring managers the signal they need without exposing sensitive study details.
