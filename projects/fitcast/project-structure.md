# FitCast Project Structure

## Top-Level Map

```text
fitcast/
  README.md
  pyproject.toml
  requirements.txt
  requirements-full.txt
  requirements.lock
  config.yaml
  resume.example.md
  pipeline.py
  audit.py
  tailor.py
  compare_resumes.py
  track.py
  extract_keywords.py
  check_resume_format.py
  bootstrap_companies.py
  skill_extractor.py
  smoke_test.sh
  data/
  docs/
  scripts/
  tests/
  .github/workflows/
```

## Main Application Files

| File | Responsibility |
|---|---|
| `pipeline.py` | Main scrape -> filter -> pre-rank -> extract -> score -> write loop |
| `audit.py` | Re-analyze one job and print detailed evidence plus score math |
| `tailor.py` | Generate truthful tailored resume and cover letter drafts from scored jobs |
| `compare_resumes.py` | Compare multiple resume versions against the same job set |
| `track.py` | Manage `applied.json` so applied jobs are skipped later |
| `extract_keywords.py` | Suggest profile-aligned search keywords from the resume |
| `check_resume_format.py` | Test whether a PDF/DOCX resume survives ATS-style text extraction |
| `bootstrap_companies.py` | Pull company slugs for Greenhouse, Lever, and Ashby from public lists |
| `skill_extractor.py` | Load and query the O*NET plus supplement skill catalog |

## Config And Packaging

| File | Responsibility |
|---|---|
| `config.yaml` | User-editable source, filter, cost, and notification settings |
| `pyproject.toml` | Package metadata, dependency groups, pytest config |
| `requirements.txt` | Lightweight runtime dependencies |
| `requirements-full.txt` | Runtime plus optional embedding dependencies |
| `requirements.lock` | Pinned full development environment |
| `resume.example.md` | Safe example resume template |
| `LICENSE` | Source-available, noncommercial license |

## Data

| Path | Responsibility |
|---|---|
| `data/skills.json` | Committed O*NET plus curated supplement skill catalog |
| `data/skills_supplement.txt` | Hand-curated terms missing from O*NET |
| `data/.onet_db.zip` | Regenerable O*NET cache, gitignored |

## Documentation

| File | Responsibility |
|---|---|
| `docs/architecture.md` | Pipeline architecture and design decisions |
| `docs/scoring.md` | Qualification, ATS, pre-rank, and domain-fit score explanations |
| `docs/sources.md` | Greenhouse, Lever, Ashby, The Muse, and company slug discovery |
| `docs/customizing.md` | Config options, filters, tracking, cron, tailoring, model switching |
| `docs/cost.md` | API cost estimates and monthly usage scenarios |

## Tests

| Test File | Coverage Area |
|---|---|
| `tests/test_scoring.py` | Qualification and ATS score derivation |
| `tests/test_skill_extractor.py` | O*NET and supplement skill matching |
| `tests/test_orchestration.py` | Source collection, dedupe, filters, config validation, webhooks |
| `tests/test_bootstrap.py` | Company slug extraction and bootstrap behavior |
| `tests/test_helpers.py` | HTML cleaning, date parsing, keyword matching, tokenization |
| `tests/test_filters.py` | Location, time-window, and salary filters |
| `tests/test_pipeline_release_blockers.py` | Source combinations, seen-state behavior, failure handling |

## CI

| Path | Responsibility |
|---|---|
| `.github/workflows/ci.yml` | GitHub Actions workflow for install, syntax check, and pytest matrix |

## Generated And Local-Only Files

| Path | Purpose |
|---|---|
| `resume.md` | User's real resume |
| `results.csv` | Ranked spreadsheet output |
| `results.json` | Full structured output |
| `applied.json` | Application tracking |
| `seen.json` | Cross-run skip state |
| `companies.bootstrap.yaml` | Generated company slug expansion |
| `tailored/` | Generated tailored application files |
| `resume.pdf`, `resume.docx` | Real resume files for parser checks |
| `resume.extracted.*.txt` | Extracted resume text for diagnostics |
