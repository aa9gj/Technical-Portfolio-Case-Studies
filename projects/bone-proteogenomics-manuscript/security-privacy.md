# Security And Privacy

## Data Sensitivity

The portfolio writeup describes a public manuscript companion repo and public genomics resources. It should still avoid exposing:

- Local absolute paths from analysis workstations or clusters.
- Usernames, project storage names, or scratch paths.
- API keys or service tokens.
- Unpublished intermediate files not already part of the public manuscript data package.
- Private collaborator notes or manuscript-drafting history.

## Public Data Hygiene

The workflow depends on large public datasets. Public documentation should preserve:

- Data source names.
- Accessions and DOIs where appropriate.
- Genome-build information.
- Tool versions where known.
- Expected file roles.
- Whether a file is raw input, generated intermediate, or source-data output.

It should not imply that downloaded public datasets are stored in this portfolio repo.

## Reproducibility Risks

| Risk | Mitigation |
|---|---|
| Genome-build mismatch | Document hg19/hg38 liftover steps and chain-file requirements |
| Changed public resources | Pin dataset releases, accession IDs, and expected input names |
| Local path leakage | Use generic path placeholders in documentation |
| External API drift | Cache or version LD/proxy outputs where possible |
| Heavy intermediate files | Keep them out of Git and reference Zenodo/public downloads |
| GUI-assisted tappAS steps | Document exported inputs and outputs clearly |

## Credential Handling

The inspected workflow does not require secrets for the core public analysis. If future automation adds authenticated downloads or APIs, credentials should be supplied through environment variables or secure local config files excluded from Git.

## Portfolio-Safe Framing

This case study is safe to share publicly because it emphasizes:

- The public problem statement.
- Analysis architecture.
- Pipeline decomposition.
- Public tools and datasets.
- Reproducibility decisions.
- Tradeoffs and future improvements.

It does not need to include private local paths, raw data copies, unpublished files, or private communications.
