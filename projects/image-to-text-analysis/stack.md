# Stack

`image-to-text-analysis` is a Python Applied AI pipeline combining local OCR models, hosted language models, embeddings, and deterministic scoring.

## Core Stack

| Area | Tools |
|---|---|
| CLI/runtime | Python, argparse, pathlib |
| Environment | python-dotenv, `.env`, configurable paths |
| OCR/VLM | NVIDIA Nemotron-Parse, RolmOCR, Nanonets OCR |
| Model loading | Hugging Face Transformers, Accelerate |
| GPU runtime | PyTorch, CUDA, bfloat16, device maps |
| Image loading | Pillow |
| Image preprocessing | OpenCV, NumPy |
| PDF rendering | PyMuPDF |
| Structured extraction | OpenAI chat completions, JSON response format |
| Embeddings | OpenAI `text-embedding-3-small` |
| Vector math | NumPy |
| Persistence | SQLite |
| Fuzzy matching | RapidFuzz |
| Output formats | JSON, CSV, terminal text |

## OCR Model Options

| Model | Role |
|---|---|
| Nemotron-Parse | Default document-style OCR backend |
| RolmOCR | General OCR option based on a vision-language model |
| Nanonets OCR | OCR option with structured/markdown-style output |

## Storage Model

SQLite stores:

- product identity fields
- extracted measurement fields
- ordered ingredients
- source image names
- raw OCR text
- extraction JSON
- embedding vectors
- embedding model name
- embedding dimension
- embedding text hash

The schema separates product rows from ingredient rows so ordered ingredient lists can be compared positionally.

## Scoring Stack

| Signal | Implementation |
|---|---|
| Ingredient overlap | Rank-biased overlap |
| OCR/name tolerance | RapidFuzz WRatio fuzzy matching |
| Calorie comparison | Relative numeric difference when units match |
| Numeric macro-style fields | Per-field tolerance and average similarity |
| Overall score | Weighted average over available subscores |
| Review diagnostics | Side-by-side field deltas and alternate ingredient metrics |

## Why This Stack

| Choice | Reason |
|---|---|
| Local OCR models | Keeps image text extraction controllable and model-swappable |
| Hosted structuring model | Handles messy label text better than brittle parsing alone |
| SQLite | Lightweight baseline store with no service dependency |
| Embeddings | Makes reference lookup tolerant to wording variation |
| Deterministic scorer | Gives reviewers an explainable result after AI extraction |
| JSON plus CSV | Supports both machine processing and spreadsheet review |
| Pretty report script | Turns dense JSON into something a reviewer can quickly scan |

## Engineering Signal

The stack demonstrates:

- Multimodal AI integration.
- GPU-aware model loading.
- Prompt/schema design.
- Failure-aware extraction.
- Retrieval and matching with embeddings.
- Deterministic scoring layered on top of AI outputs.
- Practical reporting for non-engineering reviewers.
