# FitCast Stack

## Summary

FitCast is intentionally a local Python CLI rather than a web app. The stack favors portability, low operating cost, inspectable artifacts, and clear separation between model-assisted extraction and deterministic scoring.

## Application Stack

| Layer | Choice | Role |
|---|---|---|
| Language | Python 3.10+ | CLI workflow, source adapters, scoring, file artifacts |
| Packaging | setuptools via `pyproject.toml` | Editable installs and dependency groups |
| Config | YAML | Human-editable source and filter settings |
| Validation | Pydantic v2 | Strict schema validation for config and model outputs |
| HTTP | `requests` | Public API fetches and webhook POSTs |
| Parsing | Python stdlib HTML parser, regex helpers, structured models | Normalize job descriptions and extract fields |
| Resume parsing | PyPDF2, python-docx | ATS-style PDF/DOCX extraction diagnostics |
| Tests | pytest | Unit coverage for scoring, filters, orchestration, bootstrap, and helpers |
| CI | GitHub Actions | Syntax and test matrix across supported Python versions |

## External APIs

| API | Why It Was Used |
|---|---|
| Greenhouse | High-quality public job descriptions from company ATS boards |
| Lever | Common public job source for modern tech and AI companies |
| Ashby | Common public job source for newer startups |
| The Muse | Public aggregator with category, level, and location filters |
| Anthropic | LLM extraction, pre-rank, audit, tailoring, and keyword support |
| Optional webhook receiver | Notifications to Slack, Discord, or generic JSON endpoints |

## LLM Stack

| Model Role | Model Family | Purpose |
|---|---|---|
| Pre-rank | Claude Haiku | Cheap relevance triage across many candidate jobs |
| Deep extraction | Claude Sonnet | Requirements extraction, evidence matching, structured output |
| Tailoring | Claude Sonnet | Truthful resume and cover letter drafts |
| Audit | Claude Sonnet | Higher-detail re-analysis for one job |

The model layer is used where language understanding matters. Final qualification and ATS scores are derived in Python.

## Data And Artifacts

| Artifact | Format | Purpose |
|---|---|---|
| `config.yaml` | YAML | User-editable search configuration |
| `resume.md` | Markdown | Canonical scoring resume |
| `data/skills.json` | JSON | O*NET plus supplemental skill catalog |
| `results.csv` | CSV | Spreadsheet-friendly ranked results |
| `results.json` | JSON | Full audit trail and score components |
| `applied.json` | JSON | User-managed application tracking |
| `seen.json` | JSON | Cost-saving skip list across runs |
| Tailored outputs | LaTeX or Markdown | Per-job application drafts |

## Optional Dependencies

| Dependency | Why Optional |
|---|---|
| `sentence-transformers` | Useful for domain-fit similarity, but it pulls in a larger ML stack |
| LaTeX tooling | Only needed if compiling tailored LaTeX outputs locally |
| Anthropic API access | Required for paid LLM paths, but not for pure unit tests |

## Why Not A Database Yet

CSV and JSON are sufficient for the current local workflow:

- They are easy to inspect.
- They do not require setup.
- They can be loaded into Google Sheets.
- JSON preserves the full evidence chain.
- They keep the project lightweight while the product loop is still being validated.

If FitCast becomes a hosted service, historical results would likely move to SQLite or Postgres and background runs would move to a scheduled worker.

## Why Not A Web App Yet

A web dashboard would make review nicer, but the core risk was not UI. The first-order questions were:

- Can the pipeline find relevant jobs?
- Can it score them transparently?
- Can it keep API spend reasonable?
- Can it avoid hallucinated resume tailoring?
- Can it run repeatedly without leaking personal data?

The CLI answered those questions faster than a full frontend would have.
