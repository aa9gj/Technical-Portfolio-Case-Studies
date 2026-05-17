# Bio Claims App Stack

## Summary

The stack is built for local biomedical research workflows: Streamlit for interactive review, FastAPI for programmatic access, ChromaDB for vector retrieval, SQLite for local state, LiteLLM for model access, and a set of adapters for third-party knowledge graphs.

## Application Stack

| Layer | Technology | Role |
|---|---|---|
| UI | Streamlit | Interactive ingestion, chat, claims, review, verification, visualization |
| API | FastAPI, Uvicorn | Programmatic access to extraction, verification, review, and export |
| Core language | Python | Retrieval, graph adapters, scoring, evaluation, orchestration |
| Config | `.env`, python-dotenv | API keys, model settings, KG paths, parser modes |
| Local persistence | SQLite | Evaluation annotations, workspaces, history, ledgers, caches |
| Vector storage | ChromaDB | Persistent semantic search over document chunks |
| Embeddings | sentence-transformers, BGE base | Local dense retrieval |
| PDF parsing | pypdf, pdfplumber, pytesseract, pdf2image, optional Nemotron Parse | Text, table, OCR, and layout-aware extraction |
| Biomedical NER | LLM extraction, OpenBioNER, gazetteer | Entity recognition and recall improvement |
| Fuzzy matching | RapidFuzz | Entity normalization and KG lookup support |
| Data frames | pandas | Tabular review, exports, benchmark reports |
| Observability | Phoenix, Langfuse | LLM/retrieval tracing and usage monitoring |

## Knowledge Graph Stack

| KG | Backend Style | Purpose |
|---|---|---|
| iKraph | DuckDB database | Broad biomedical relationships with probability scores |
| FoodAtlas | Local TSV/CSV tables | Food, chemical, and disease links |
| GENA | Local CSV | Nutrition and mental-health relationships |
| MeNuGUIDE | Preprocessed CSV tables | Food, compound, disease, and pathway relationships |
| Monarch | SQLite via kgw | Cross-species gene, disease, and phenotype relationships |
| HALD | SQLite via kgw | Aging and longevity disease context |
| Custom KG | Uploaded CSV/TSV | User-provided triples for session-specific grounding |
| Study-derived KG | CSV/TSV/JSON/JSONL nodes and edges | GraphRAG retrieval expansion over project-specific structure |

## Model And Evaluation Stack

| Capability | Tools |
|---|---|
| Generation and extraction | OpenAI-compatible models through LiteLLM |
| Biomedical entailment | NLI cross-encoder |
| RAG evaluation | RAGAS |
| Retrieval evaluation | BEIR-style benchmarks and custom retrieval fixtures |
| Claim verification datasets | SciFact-style and PubMedQA-style benchmarks |
| NER evaluation | BC5CDR, NCBI Disease, and domain-specific gold examples |
| PDF parser evaluation | Parser comparison benchmarks |

## Why This Stack Fits

- Streamlit is fast for research workflows with many tabs and review states.
- ChromaDB plus local embeddings keeps retrieval inspectable and low-latency.
- SQLite is enough for local annotation, workspace, and ledger state.
- KG adapters keep graph sources swappable.
- LiteLLM makes model choice configurable without rewriting app logic.
- Benchmarks provide a way to validate each subsystem separately.

## Where The Stack Would Change For Production

For a hosted multi-user system, likely changes would include:

- PostgreSQL instead of SQLite.
- A background job queue for ingestion and benchmark runs.
- Object storage for PDFs and generated artifacts.
- Centralized auth and role-based permissions.
- A graph database for large custom/study-derived KGs.
- Deployment-level TLS, audit logs, backups, and monitoring.
