# Bio Claims App Project Structure

## Top-Level Map

```text
bio-claims-app/
  README.md
  USER_GUIDE.md
  ARCHITECTURE.md
  METHODS.md
  SECURITY_REVIEW.md
  requirements.txt
  .env.template
  app.py
  app_chat.py
  api.py
  preseed.py
  run_all_benchmarks.sh
  core/
  tabs/
  tests/
  benchmarks/
  scripts/
```

## Application Entrypoints

| File | Responsibility |
|---|---|
| `app.py` | Full Streamlit application |
| `app_chat.py` | Chat-focused Streamlit entrypoint |
| `api.py` | FastAPI wrapper for programmatic workflows |
| `preseed.py` | Pre-ingestion helper |
| `run_all_benchmarks.sh` | Benchmark orchestration script |

## Core Modules

| Module | Responsibility |
|---|---|
| `core/rag.py` | Hybrid retrieval, vector search, graph fusion, context formatting |
| `core/query_router.py` | Domain and intent classification for KG weighting |
| `core/claims.py` | Claim extraction, validation, deduplication, certainty scoring |
| `core/nli_eval.py` | Natural Language Inference verification |
| `core/graph.py` | iKraph adapter |
| `core/foodatlas.py` | FoodAtlas adapter |
| `core/gena.py` | GENA adapter |
| `core/menuguide.py` | MeNuGUIDE adapter |
| `core/monarch.py` | Monarch adapter |
| `core/hald.py` | HALD adapter |
| `core/custom_kg.py` | Uploaded custom KG parsing and querying |
| `core/internal_study_kg.py` | Study-derived KG loading, traversal, and retrieval-query expansion |
| `core/fsr_graph.py` | Query-time source-derived graph construction and third-party KG comparison |
| `core/fsr_ontology_graph.py` | Persistent ontology graph extraction/export from ingested study chunks |
| `core/evidence_provenance.py` | Evidence modality inference and provenance summaries |
| `core/source_citations.py` | Canonical source citation metadata |
| `core/citation_verify.py` | Citation checking against retrieved evidence |
| `core/evaluation.py` | SQLite persistence for human review annotations |
| `core/ragas_eval.py` | RAGAS evaluation wrapper |
| `core/agent.py` | Streaming agentic loop with tool use |
| `core/agent_tools.py` | Tool registry for paper search, KG queries, verification, literature, and hypotheses |
| `core/calibration.py` | Calibration reporting |
| `core/calibration_feedback.py` | Learned scoring-weight feedback |
| `core/export_formats.py` | CSV/TSV/Nanopub/RDF/BEL/JSON-LD exports |
| `core/workspace.py` | Saved project workspaces |
| `core/cache_db.py` | Local cache persistence |

## Streamlit Tabs

| Tab File | UI Area |
|---|---|
| `tabs/ingest.py` | PDF ingestion |
| `tabs/chat.py` | Conversational Q&A |
| `tabs/claims.py` | Claim extraction and review preparation |
| `tabs/verify.py` | Standalone claim verification |
| `tabs/evaluate.py` | Human review and calibration |
| `tabs/visualization.py` | Entity relationship visualizations |
| `tabs/evolution_view.py` | Claim evolution tracking |
| `tabs/literature.py` | Literature discovery |
| `tabs/chat_feedback.py` | Chat answer feedback |
| `tabs/gold_annotation.py` | Gold annotation workflows |
| `tabs/log.py` | Usage and tracing logs |

## Benchmarks

| Path | Purpose |
|---|---|
| `benchmarks/benchmark_embeddings.py` | Compare embedding retrieval quality |
| `benchmarks/benchmark_embeddings_beir.py` | BEIR-style retrieval evaluation |
| `benchmarks/benchmark_chunking.py` | Chunking strategy evaluation |
| `benchmarks/benchmark_kg_grounding.py` | KG grounding evaluation |
| `benchmarks/benchmark_pdf_parsing.py` | PDF parser comparison |
| `benchmarks/benchmark_ner.py` | Domain NER benchmark |
| `benchmarks/benchmark_ner_bc5cdr.py` | BC5CDR NER benchmark |
| `benchmarks/benchmark_ner_ncbi_disease.py` | NCBI Disease NER benchmark |
| `benchmarks/benchmark_scifact.py` | Claim verification benchmark |
| `benchmarks/benchmark_pubmedqa.py` | Biomedical QA benchmark |
| `benchmarks/benchmark_mednli.py` | Clinical NLI benchmark |
| `benchmarks/benchmark_ragas.py` | RAGAS benchmark |
| `benchmarks/generate_report.py` | Consolidated benchmark report generation |

## Tests

The test suite covers normalizer behavior, literature search helpers, gold annotation, NLI evaluation, KG adapters, verification, retrieval performance, usage tracking, evidence provenance, evaluation persistence, multihop search, ingestion, chat audit, RAGAS wrappers, claims, and RAG helpers.

## Scripts

| Script | Purpose |
|---|---|
| `scripts/download_kgw_kgs.py` | Download KGW-backed graphs |
| `scripts/preprocess_menuguide.py` | Convert MeNuGUIDE raw data into local CSV tables |
| `scripts/download_models.sh` | Download local models |
| `scripts/check_models.sh` | Check model availability |
| `scripts/build_fsr_ontology_graph.py` | Build ontology graph export from ingested chunks |
| `scripts/rebuild_fsr_poc.py` | Rebuild a study-report corpus and derived caches |
| `scripts/build_fsr_chat_caches.py` | Build chat-facing caches from ingested records |
| `scripts/backfill_source_citations.py` | Backfill canonical citation metadata |
| `scripts/backup.sh` | Local backup helper |
| `scripts/slurm_*.sh` | HPC benchmark job wrappers |

## Local-Only Data

Common local-only assets include uploaded PDFs, ChromaDB collections, SQLite stores, KG datasets, model caches, generated claims, exports, logs, and benchmark outputs. These should not be treated as public portfolio material unless explicitly redacted.
