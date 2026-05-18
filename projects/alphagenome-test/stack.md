# Stack

`alphagenome-test` combines Python data engineering, population-genetics preprocessing, coordinate liftover, and AlphaGenome model APIs.

## Core Stack

| Area | Tools |
|---|---|
| Language | Python 3.11+ |
| Dataframes | pandas |
| Numerical work | NumPy |
| VCF parsing | cyvcf2 |
| VCF region slicing | bcftools |
| Coordinate conversion | pyliftover, UCSC chain files |
| Variant effect model | AlphaGenome Python client |
| Model API objects | `alphagenome.data.genome`, `alphagenome.models.dna_client` |
| Variant scoring | `alphagenome.models.variant_scorers` |
| Output storage | CSV, parquet |
| Parquet engine | pyarrow |
| Reference resources | 1000 Genomes Phase 3, GTEx metadata, GWAS summary statistics |

## External Data

| Data | Role |
|---|---|
| GWAS summary statistics | Source of locus association signal |
| 1000 Genomes Phase 3 panel | Population labels for selecting European samples |
| 1000 Genomes regional VCF | LD estimation and proxy selection |
| UCSC hg19-to-hg38 chain file | Coordinate conversion before AlphaGenome calls |
| AlphaGenome metadata | GTEx tissue and ontology enumeration |

## Runtime Assumptions

The scripts assume:

- Python dependencies are installed.
- `bcftools` is available on `PATH`.
- External support files are downloaded or reachable.
- `ALPHA_GENOME_API_KEY` is set for AlphaGenome calls.
- The working directory contains the output of prior stages.

## Why This Stack

| Choice | Reason |
|---|---|
| pandas/NumPy | Fits tabular GWAS, LD, and ranking operations |
| cyvcf2 | Efficient VCF iteration and genotype access |
| bcftools | Reliable remote region slicing from indexed VCFs |
| pyliftover | Lightweight coordinate conversion for SNP tables |
| AlphaGenome client | Official interface for regulatory variant prediction and recommended scorers |
| parquet | Handles large long-format track output better than CSV |
| checkpoint CSVs | Simple restart mechanism without database infrastructure |

## Engineering Signal

The stack demonstrates:

- Genetic association preprocessing.
- Variant and coordinate-system handling.
- API-based model inference at scale.
- Long-format scientific data management.
- Ranking and summarization for biological decision-making.
