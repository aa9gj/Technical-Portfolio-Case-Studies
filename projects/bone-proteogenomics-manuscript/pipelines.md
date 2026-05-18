# Pipelines

## Stage 0: GWAS/sQTL Colocalization

Purpose: identify splice events whose sQTL signal colocalizes with a bone mineral density GWAS signal.

Main steps:

1. Download GTEx v8 sQTL association files and GWAS summary statistics.
2. Harmonize coordinates, including liftover where needed.
3. Build per-tissue and per-chromosome working files.
4. For each GWAS lead association, find nearby genes and junctions in a 400 kb window.
5. For each candidate junction, retrieve local sQTL SNPs.
6. Run `coloc.abf` with GWAS and sQTL p-values, MAF, and sample-size metadata.
7. Write coloc summaries by tissue.

Outputs: coloc summary tables with posterior-probability evidence for shared signals.

## Stage 1: Long-Read Transcriptome Generation And Filtering

Purpose: build a disease-relevant full-length transcriptome and remove weak or likely-artifactual isoforms.

Main steps:

1. Process raw PacBio Iso-Seq reads from GEO.
2. Refine reads to trim polyA sequence and remove concatemers.
3. Cluster reads into consensus isoforms.
4. Align isoforms with pbmm2/minimap2.
5. Collapse redundant transcript structures.
6. Classify isoforms with SQANTI3.
7. Join cDNA_Cupcake isoform counts to SQANTI3 and GENCODE annotations.
8. Keep protein-coding genes and remove unannotated likely artifacts.
9. Calculate TPM and isoform-percentage support.
10. Keep isoforms expressed in enough samples/time points and above isoform-percentage threshold.
11. Add DESeq2-normalized counts for retained isoforms.

Outputs: filtered long-read isoforms, normalized counts, and classification metadata.

## Stage 2: sQTL-To-Isoform Mapping

Purpose: determine which colocalized splice junctions are present in full-length long-read isoforms.

Main steps:

1. Load significant colocalized splice junctions across tissues.
2. Restrict to protein-coding genes.
3. Convert junction event IDs into genomic ranges.
4. Construct introns from SQANTI3 GTF transcript models.
5. Keep filtered isoforms belonging to genes with colocalized events.
6. Use exact overlap between event coordinates and isoform introns.
7. Count retained genes, events, and isoforms.
8. Label events as known-only, novel-only, or represented by both known and novel isoforms.

Outputs: event-to-isoform maps and known/novel status summaries.

## Stage 3: Effect Size Annotation

Purpose: add directional variant evidence to mapped events.

Main steps:

1. Load chromosome-specific GWAS summary statistics.
2. Load chromosome-specific GTEx lookup and event SNP files.
3. Rank sQTL SNPs by p-value.
4. Join top sQTL variants back to GWAS effect sizes.
5. Record p-value separation between top and secondary sQTLs where available.
6. Merge chromosome outputs into an event-level effect-size table.

Outputs: mapped events with GWAS beta, sQTL slope, p-value, and variant context.

## Stage 4: Event Annotation And Enrichment

Purpose: move from mapped events to biologically interpretable variant and splicing context.

Main steps:

1. Add GENCODE gene names and event IDs.
2. Retrieve or load LD proxy variants.
3. Keep high-LD proxies using an `r2 >= 0.80` threshold.
4. Identify lead and proxy variants overlapping affected introns.
5. Construct donor and acceptor splice-site intervals from SQANTI3 introns.
6. Test lead/proxy SNP overlap with splice donor and acceptor windows.
7. Lift variants where needed for enrichment resources.
8. Compare variant sets against RNA-binding-protein and eCLIP-style annotations.

Outputs: annotated events, lead/proxy overlap summaries, splice-site hits, and enrichment inputs/results.

## Stage 5: tappAS Analysis

Purpose: integrate transcript expression and isoform-usage evidence.

Main steps:

1. Convert SQANTI3 outputs into tappAS-compatible GFF3 using IsoAnnotLite.
2. Prepare tappAS design and expression matrix files.
3. Run tappAS time-series analyses with MaSigPro.
4. Export DGE, DTE, DIU, and related transcript/gene result tables.
5. Feed those outputs into source-data integration.

Outputs: differential gene expression, transcript expression, and isoform usage calls.

## Stage 6: Source-Data Generation

Purpose: create auditable tables that combine molecular findings with external disease and functional evidence.

Main steps:

1. Load GENCODE protein-coding gene annotations.
2. Load tappAS gene, transcript, and isoform-usage outputs.
3. Load curated disease and phenotype resources.
4. Add flags for differential expression, differential transcript expression, DIU, and major isoform switching.
5. Add disease evidence from phenotype databases, curated bone resources, eQTL evidence, and monogenic disease genes.
6. Add tissue-count and event-count evidence.
7. Write source-data tables for manuscript interpretation and prioritization.

Outputs: prioritization-ready tables at gene, event, transcript, and evidence-summary levels.

## Stage 7: ORF/CDS And Protein Consequence Analysis

Purpose: predict coding consequences for long-read isoforms linked to colocalized sQTL events.

Main steps:

1. Run CPAT-style ORF prediction on corrected long-read FASTA.
2. Call ORFs using long-read proteogenomics scripts and GENCODE annotations.
3. Refine ORF database outputs.
4. Generate CDS GTF records with transcript identifiers.
5. Join predicted ORFs/CDS back to filtered isoforms and exact-overlap events.
6. Prepare downstream interpretation of truncation, NMD, or protein-isoform consequences.

Outputs: ORF/CDS annotations linked to affected long-read transcripts.

## Stage 8: Genome Browser Tracks

Purpose: support visual inspection and communication.

Main steps:

1. Prepare GENCODE track context.
2. Prepare hFOB long-read transcript/protein isoform track context.
3. Prepare colocalized sQTL junction and event tracks.
4. Use UCSC custom track views for inspection.

Outputs: track assets and links for reviewing loci in genomic context.
