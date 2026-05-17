# Pipeline And Workflow

The project exposes four workflows through `pipeline.py`.

## Mode 1: `legacy`

Purpose:

- Run OCR and flatten structured extraction into CSV rows.

Steps:

1. Resolve image/PDF inputs from CLI arguments or `input/`.
2. Group related files by filename prefix.
3. Load the selected OCR model.
4. Run OCR on every file or PDF page.
5. Save raw OCR text and readable OCR text.
6. Optionally skip OpenAI in `--dry-run` mode.
7. Send readable text for structured extraction.
8. Flatten product JSON into CSV.
9. Validate CSV shape.
10. Merge rows into `output/merged_<model>.csv`.

Useful for:

- Quick one-off extraction.
- Inspecting OCR quality before full structured workflows.
- Comparing OCR models in `--model all` mode.

## Mode 2: `ingest-original`

Purpose:

- Build a baseline database of known reference products.

Steps:

1. Resolve and group input images.
2. Load selected OCR model.
3. Run OCR per product group.
4. Save raw/readable OCR.
5. Extract structured JSON.
6. Validate that a product description exists.
7. Upsert the product into SQLite.
8. Store ordered ingredients in a child table.
9. Store source images, raw text, and extraction JSON.
10. Build enriched embedding text.
11. Compute or reuse an embedding.
12. Persist embedding vector, model, dimension, and text hash.
13. Record failures in `failed_ingests.csv`.

Outputs:

- `data/originals.db`
- raw/readable OCR text files
- `output/failed_ingests.csv` when needed

## Mode 3: `compare`

Purpose:

- Compare new product images against the stored baseline.

Steps:

1. Resolve and group new images.
2. Load selected OCR model.
3. Run OCR and structured extraction.
4. Build embedding text from extracted identity fields.
5. Retrieve candidate references from SQLite.
6. Filter by extracted category-style fields when available.
7. Rank candidates by cosine similarity.
8. Apply secondary brand compatibility check.
9. If no candidate passes threshold, write a `no_match` report.
10. If a candidate matches, run deterministic comparison.
11. Write one JSON report per product.
12. Append one row to a timestamped summary CSV.

Outputs:

- `output/comparisons/<product>_<timestamp>.json`
- `output/comparisons/comparisons_<timestamp>.csv`

## Mode 4: `compare-paired`

Purpose:

- Compare a supplied reference package directly with a supplied new package.

Steps:

1. Validate `--original` and `--new` image paths.
2. OCR and structure the original image group.
3. OCR and structure the new image group.
4. Skip database lookup and embedding retrieval.
5. Run deterministic comparison directly.
6. Write paired JSON report and one-row CSV summary.

Why it matters:

- Avoids false nearest-neighbor matches.
- Avoids stale baseline problems.
- Makes the human-provided reference pairing explicit.

## OCR Workflow

Supported backends:

- Nemotron-Parse.
- RolmOCR.
- Nanonets OCR.

The parser abstraction returns:

- `raw`: model output with minimal post-processing.
- `readable`: cleaned text for humans and structured extraction.

## Structured Extraction Workflow

The extraction layer:

1. Sends readable OCR text to the OpenAI model.
2. Requests a JSON object response.
3. Requires a top-level `products` array.
4. Retries malformed JSON, missing products, or transient API errors.
5. Uses exponential backoff.
6. Raises an exception with the final raw output if all attempts fail.

## Similarity Workflow

Reference matching:

- Embedding input combines product identity fields.
- Vectors are normalized.
- Similarity is cosine similarity.
- Threshold defaults to a conservative setting.
- Secondary filters reduce sibling-product collisions.

Product scoring:

- Ingredient score uses rank-biased overlap plus fuzzy matching.
- Calorie score compares values only when units match.
- Numeric macro-style score averages comparable fields with tolerance.
- Missing signals are excluded from the weighted sum.
- Alternative ingredient metrics are still reported for visibility.

## Report Workflow

The pipeline writes:

- Raw OCR text.
- Readable OCR text.
- Structured extraction JSON.
- Comparison JSON.
- Timestamped summary CSV.
- Pretty terminal reports through `report.py`.

This keeps the workflow inspectable from raw model output through final reviewer-facing summary.
