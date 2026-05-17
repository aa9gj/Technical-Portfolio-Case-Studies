# Project Structure

The source repo is a compact Python CLI project.

```text
image-to-text-analysis/
  README.md
  requirements.txt
  .env.example
  config.py
  pipeline.py
  llm_structurer.py
  embeddings.py
  originals_db.py
  comparator.py
  report.py
  nemotron_parser.py
  rolmocr_parser.py
  nanonets_parser.py
  utils.py
  input/
    .gitkeep
```

## Core Files

| File | Purpose |
|---|---|
| `pipeline.py` | Main CLI, mode dispatch, image grouping, OCR orchestration, ingestion, comparison, paired comparison, and summary CSV writing |
| `config.py` | Environment loading, model names, retry settings, embedding settings, paths, and supported extensions |
| `llm_structurer.py` | Structured extraction prompt, OpenAI JSON calls, retry/backoff, extraction errors, and legacy CSV flattening |
| `embeddings.py` | Enriched embedding text, OpenAI embeddings, L2 normalization, cache logic, and best-match retrieval |
| `originals_db.py` | SQLite schema, product upserts, ingredient rows, embedding storage, and product iteration |
| `comparator.py` | Ingredient similarity, fuzzy matching, calorie comparison, numeric-field comparison, weighting, and diff payloads |
| `report.py` | Pretty-printed comparison reports and summary CSV display |
| `utils.py` | Image/PDF discovery, preprocessing, PDF rendering, CSV validation, and GPU memory helpers |

## OCR Backend Files

| File | Purpose |
|---|---|
| `nemotron_parser.py` | Loads and runs Nemotron-Parse, then cleans model tokens into readable text |
| `rolmocr_parser.py` | Loads and runs RolmOCR and returns raw/readable text |
| `nanonets_parser.py` | Loads and runs Nanonets OCR and converts structured markup into readable text |

## Runtime Directories

| Directory | Purpose |
|---|---|
| `input/` | Drop zone for images and PDFs |
| `output/` | Generated raw OCR, readable OCR, CSVs, JSON reports, summaries, and failure logs |
| `data/` | SQLite reference database |

`output/` and `data/` are runtime artifacts and should not be treated as source.

## Data Flow By File

1. `pipeline.py` resolves inputs and groups files.
2. OCR parser module returns raw/readable text.
3. `llm_structurer.py` turns readable text into structured JSON.
4. `originals_db.py` persists reference products.
5. `embeddings.py` computes/reuses reference embeddings.
6. `comparator.py` scores new versus reference products.
7. `pipeline.py` writes JSON and CSV outputs.
8. `report.py` formats results for review.

## Portfolio Interpretation

The structure shows a useful applied-AI pattern:

- Model-specific wrappers stay isolated.
- Prompt/schema logic is isolated.
- Persistence is isolated.
- Retrieval is isolated.
- Scoring is deterministic and testable.
- Reporting is separated from core comparison logic.
