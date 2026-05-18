# Project Structure

The source repo is organized by manuscript analysis stage.

```text
Bone_proteogenomics_manuscript/
  README.md
  CITATION.cff
  LICENSE
  setup_r_env.R
  sQTL_colocalization_analysis/
    Step0_Colocalization/
      README.md
      coloc_per_tissue_analysis.R
      snp_retrieval_v2.py
      SLURM and automation helper scripts
  Reference_transcriptome_generation/
    Isoseq_analysis/
      Isoseq_analysis.md
    Step1_Long-read_RNAseq_filtering_in_hFOBs/
      README.md
      Step1_code.R
  sQTL_to_isoform_mapping/
    Step2_sQTL_coloc_res_hFOBs_isoforms/
      README.md
      Step2_code.R
    Step3_Add_effect_size/
      README.md
      Step3_code.R
    Step4_event_annotaion_and_enrichment/
      README.md
      Step4_code.R
    Step5_tappAS_analysis/
      README.md
    Step6_Generation_of_source_data/
      README.md
      Step6_code.R
    Step7_generation_of_ORFs/
      README.md
      Step7_code.R
    create_ucsc_hfob_sqtl_track/
      README.md
```

## Reading The Repo

Start with the root `README.md` for manuscript context and data sources. Then read the stage READMEs in order:

1. `Step0_Colocalization` for GWAS/sQTL coloc setup.
2. `Isoseq_analysis` for raw long-read RNA-seq processing.
3. `Step1_Long-read_RNAseq_filtering_in_hFOBs` for expression filtering.
4. `Step2_sQTL_coloc_res_hFOBs_isoforms` for exact event-to-isoform mapping.
5. `Step3_Add_effect_size` and `Step4_event_annotaion_and_enrichment` for effect direction, LD, and enrichment.
6. `Step5_tappAS_analysis` for tappAS preparation and exported result integration.
7. `Step6_Generation_of_source_data` for prioritization-ready tables.
8. `Step7_generation_of_ORFs` for predicted protein consequences.

## Organization Pattern

The project uses a pragmatic manuscript-reproducibility pattern:

- Each stage has its own folder.
- Stage READMEs describe required public inputs.
- R scripts encode the actual analysis transformations.
- Heavy public data and generated intermediates are referenced rather than stored directly in Git.
- Some stages allow users to download precomputed intermediates from Zenodo instead of regenerating expensive files.

This is appropriate for a published analysis, but the next maintainability step would be wrapping the same stages in a workflow engine with pinned environments and small test fixtures.
