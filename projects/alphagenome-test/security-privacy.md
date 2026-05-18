# Security And Privacy

This repository primarily uses public genetics resources, but there are still practical security and reproducibility boundaries to document.

## Sensitive Or Controlled Artifacts

Potentially sensitive or non-public artifacts include:

- AlphaGenome API keys.
- Local filesystem paths.
- Downloaded GWAS summary-statistics archives.
- Generated AlphaGenome predictions.
- Large intermediate parquet files.
- Checkpoint files that reveal run progress.
- Any unpublished follow-up interpretations.
- Local cache directories or model/service credentials.

## Public Case Study Boundary

The portfolio writeup describes:

- The GWAS follow-up problem.
- The pipeline architecture.
- The data sources.
- The AlphaGenome prediction/scoring workflow.
- The ranking and collapse outputs.
- The engineering tradeoffs.

It intentionally does not include:

- API keys.
- Local machine paths.
- Private credentials.
- Generated prediction tables.
- Unpublished biological conclusions.
- Large downloaded resources.

## Repo Hygiene Recommendations

The source repository should avoid committing:

- `.env`
- API key files
- downloaded GWAS summary statistics
- VCFs and indexes
- liftover chain files
- parquet outputs
- generated CSV outputs
- checkpoint files
- local model/cache directories
- absolute local paths

The `.gitignore` already covers many of these categories, including secrets, large genomics files, AlphaGenome outputs, support files, and generated intermediates.

## Operational Controls

For real analyses:

1. Store API keys in environment variables or a secret manager.
2. Keep large public resource downloads out of git.
3. Pin source data versions and URLs in documentation.
4. Record AlphaGenome client/package version in outputs.
5. Preserve checkpoint files during a run, but do not publish them by default.
6. Treat model predictions as research artifacts requiring interpretation.
7. Avoid publishing local absolute paths in scripts or reports.

## Interview Positioning

The safest public framing is:

- "I built a regulatory variant prioritization pipeline around AlphaGenome."
- "It starts from GWAS and LD evidence, then uses AlphaGenome to prioritize variants by predicted molecular effect."
- "The public docs focus on workflow design and do not expose API keys, local paths, or generated prediction tables."
