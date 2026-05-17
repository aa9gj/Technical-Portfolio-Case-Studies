# low-pass-WGS-pipeline Security And Privacy Notes

## Sensitive Data

Whole-genome sequencing workflows can expose highly sensitive information:

- Raw FASTQ reads.
- Aligned BAMs.
- Variant calls.
- Imputed genotypes.
- Sample IDs.
- Cohort composition.
- Local filesystem paths.
- Reference panels and study-specific resources.
- Cluster account and storage details.

This public case study avoids sample-specific and environment-specific details.

## Public Documentation Strategy

Safe public material:

- Workflow architecture.
- Module responsibilities.
- Tool choices.
- QC concepts.
- Reproducibility and validation strategy.
- Sanitized examples.

Unsafe public material:

- Real sample manifests.
- Raw or aligned sequencing files.
- VCFs or imputed genotype files.
- QC plots tied to identifiable cohorts.
- Cluster paths or account names.
- Private reference panels.

## Config Hygiene

The repository includes `pipeline.config.example` with placeholder paths. Real `pipeline.config` files should remain local because they can reveal:

- Storage layouts.
- Cohort directory names.
- Reference-panel locations.
- Temporary directory locations.
- Cluster-specific module naming.

## Data Governance

For real runs:

- Store raw and derived genomic data in controlled storage.
- Restrict access to BAM, VCF, and imputed genotype files.
- Archive run configs and logs with appropriate access controls.
- Track reference genome, known-sites, and imputation panel versions.
- Delete temporary data according to project policy.

## Operational Risk

Long-running SLURM jobs can fail after consuming substantial compute. The project reduces this risk through:

- Setup validation.
- Strict-mode shell utilities.
- Array bounds checks.
- Early file/command validation.
- Stage-level checkpoints.

## Future Hardening

- Add shellcheck in CI.
- Add a `.gitignore` pattern for real config files if not already present.
- Add checks that refuse placeholder paths before submission.
- Add manifest logging for reference and panel versions.
- Add a dry-run mode that validates inputs without running compute-heavy tools.
