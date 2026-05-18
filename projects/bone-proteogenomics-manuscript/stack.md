# Stack

## Languages

| Tool | Use |
|---|---|
| R | Core statistical analysis, genomic ranges, table joins, expression filtering, source-data generation |
| Python | sQTL SNP retrieval and support scripts |
| Bash | File orchestration, loops, and SLURM wrappers |

## R And Bioconductor

| Package | Role |
|---|---|
| `data.table` | Fast loading and manipulation of large tabular genomics files |
| `dplyr`, `tidyr`, `tidyverse` | Joins, reshaping, filtering, and summary tables |
| `coloc` | Bayesian colocalization of GWAS and sQTL signals |
| `GenomicRanges` | Interval representation and overlap analysis |
| `plyranges` | Tidy-style genomic interval manipulation |
| `rtracklayer` | GTF/GFF import and liftover chain handling |
| `DESeq2` | Size-factor normalization for isoform count matrices |
| `biomaRt` | Annotation retrieval when needed |
| `pheatmap`, `RColorBrewer` | Heatmaps and visual summaries |
| `ggtranscript` | Transcript-structure visualization support |

## Genomics And Transcriptomics Tools

| Tool | Role |
|---|---|
| PacBio Iso-Seq | Long-read transcript sequencing workflow |
| pbmm2/minimap2 | Long-read alignment |
| cDNA_Cupcake | Isoform collapsing, demultiplexing, and count support |
| SQANTI3 | Isoform structural classification and corrected GTF outputs |
| GENCODE | Reference gene and transcript annotation |
| tappAS | Differential gene expression, transcript expression, and isoform-usage analyses |
| IsoAnnotLite | SQANTI-to-tappAS annotation conversion |
| CPAT-style ORF calling | Coding potential and ORF prediction |
| Long-read proteogenomics scripts | ORF refinement and CDS GTF creation |
| UCSC Genome Browser | Visual inspection through custom tracks |

## Public Data Sources

| Source | Use |
|---|---|
| Zenodo | Manuscript input files and regenerable intermediate outputs |
| GEO | Raw long-read sequencing accessions |
| GTEx v8 | sQTL association data across tissues |
| GWAS summary statistics | Bone mineral density association signals |
| GENCODE v26/v38 | Gene/transcript annotations for GTEx and hg38 long-read contexts |
| UCSC chain files | hg19/hg38 coordinate conversion |
| Ensembl REST API | LD proxy lookup for variant annotation |
| IMPC-style phenotype resources | Functional phenotype evidence for prioritization |
| Curated bone/disease gene resources | Additional prioritization evidence |

## Execution Environment

The project expects a research-compute environment rather than a single laptop-only workflow:

- SLURM for chromosome/tissue jobs and long-running summary-statistic loops.
- Large local or shared storage for GWAS, GTEx, long-read, and intermediate genomics files.
- R/Bioconductor environment with compiled genomics packages.
- Python environment for helper scripts and REST/API-supported retrieval.
- External command-line tools installed and available on `PATH`.

## Outputs

| Output Type | Examples |
|---|---|
| Colocalization results | Per-tissue coloc summaries and significant event tables |
| Transcriptome products | SQANTI3 classification files, corrected GTFs, cDNA_Cupcake count tables |
| Filtered isoform tables | Protein-coding expressed isoforms with normalized counts |
| Event maps | Colocalized junctions linked to full-length isoforms |
| Variant annotations | Effect-size tables, LD proxy files, splice-site overlap outputs |
| tappAS outputs | DGE, DTE, DIU, and major isoform-switching results |
| Protein annotations | ORF/CDS predictions and transcript-linked protein consequence tables |
| Source data | Integrated manuscript tables for candidate prioritization |
| Browser assets | UCSC-compatible track inputs |
