# Security And Privacy

Image-to-text pipelines can expose sensitive commercial and operational information. This case study is written to describe the architecture without exposing private source images, client context, or proprietary result values.

## Data Sensitivity

Potentially sensitive artifacts include:

- Package images and PDFs.
- Raw OCR text.
- Readable OCR text.
- Structured JSON extraction.
- Product names and descriptions.
- Manufacturer or distributor text.
- Internal reference databases.
- Embedding vectors derived from product identity text.
- Comparison reports.
- Reviewer CSV summaries.
- Failed extraction logs containing raw model output.
- API keys and model cache paths.

## Public Case Study Boundary

The portfolio writeup describes:

- The image-to-text architecture.
- The OCR and structuring workflow.
- The storage and matching design.
- The scoring model.
- The reporting outputs.
- The failure-handling approach.

It intentionally does not include:

- Private images.
- Private OCR output.
- Proprietary product result tables.
- Client or brand context.
- Internal operating procedures.
- API keys.
- Local cache paths tied to a real environment.

## Repo Hygiene Recommendations

The source repository should avoid committing:

- `.env`
- API keys
- raw input images
- private PDFs
- `output/`
- `data/originals.db`
- comparison JSON/CSV reports
- failed extraction CSVs
- raw/readable OCR text dumps
- Hugging Face model caches
- generated embeddings from private product data

Safe public artifacts include:

- Source code.
- Generic documentation.
- Synthetic fixtures.
- Empty `input/` placeholders.
- Example `.env` templates without secrets.

## Operational Controls

For real usage:

1. Keep private images and reports outside git.
2. Rotate API keys if they are ever exposed in logs.
3. Treat raw OCR text as sensitive because it can include full package copy.
4. Review failed extraction logs before sharing.
5. Keep local model caches and output folders out of the public repo.
6. Use synthetic test fixtures for public demos.
7. Store baseline databases in controlled storage when they reflect private catalogs.
8. Separate architecture documentation from client-specific workflows.

## Interview Positioning

The safest framing is:

- "I built an applied AI pipeline that extracts structured data from product images."
- "The system combines OCR/VLM models, structured LLM extraction, embeddings, SQLite references, and deterministic comparison scoring."
- "The public writeup focuses on engineering design and avoids private product or client context."
