# Security And Privacy

GWAS target-prioritization projects can expose sensitive study and phenotype information even when the code itself is public.

## Data Sensitivity

Potentially sensitive artifacts include:

- Raw GWAS summary statistics.
- Phenotype names and trait labels.
- Cohort metadata.
- Sample identifiers.
- Study-specific covariates.
- Internal GTF/reference paths.
- Candidate-gene and target-prioritization outputs.
- Locus plots.
- API query logs or cached evidence files.
- Local OpenTargets/IMPC downloads if mixed with private analysis outputs.

## Public Case Study Boundary

The public portfolio case study describes:

- The package architecture.
- The workflow decomposition.
- The evidence sources.
- The R/Bioconductor stack.
- The online/offline design.
- The validation strategy.

It intentionally does not include:

- Private GWAS results.
- Private phenotypes.
- Study-specific target rankings.
- Cohort metadata.
- Internal file paths.
- Private plots.
- Unpublished biological interpretation.

## Repo Hygiene Recommendations

The source repository can safely include:

- Source code.
- Documentation.
- Synthetic or small public example data.
- Orthology mapping examples that are already public.
- Tests using mocked evidence tables.

The source repository should avoid committing:

- Full private GWAS summary statistics.
- Large local OpenTargets/IMPC download directories.
- Study-specific result tables.
- API caches from private exploratory sessions.
- Private `ResultsPath` outputs.
- R session artifacts.
- Internal absolute paths.

## Operational Controls

For real analyses:

1. Keep GWAS inputs and outputs in approved storage.
2. Use deidentified sample and cohort labels.
3. Pin public evidence-source versions for reproducibility.
4. Separate package source code from private result directories.
5. Review generated plots before sharing.
6. Avoid caching private query/result combinations in the repo.
7. Document whether API mode or manual mode was used.
8. Treat target-prioritization output as biological interpretation, not generic software output.

## Interview Positioning

The safest public framing is:

- "I built an R package that turns GWAS loci into evidence-enriched candidate gene tables."
- "It integrates OpenTargets, IMPC, and Zoonomia orthology for cross-species interpretation."
- "The public repo demonstrates the software architecture and test strategy without exposing private GWAS results."
