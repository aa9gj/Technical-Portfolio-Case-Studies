# Technical Portfolio Case Studies

This repository is a lightweight companion to my public and private GitHub projects. It is meant to help hiring managers quickly understand the problems I chose to solve, how I decomposed them, what stacks I used, and what engineering tradeoffs shaped each implementation.

For private repositories, the case studies stay public while the source code can be shared separately with interviewers when appropriate.

## Project Index

| Project | Problem Area | Stack | What It Shows | Status |
|---|---|---|---|---|
| [FitCast](projects/fitcast/) | Job search automation, fit scoring, resume tailoring | Python, Anthropic Claude, Pydantic, public ATS APIs, pytest, GitHub Actions | Product decomposition, LLM orchestration, deterministic scoring, cost-aware pipelines, CI/smoke testing | Draft v1 |

## Reading Guide

Each project writeup is intentionally short and decision-focused:

- Problem statement: what pain point the project addresses.
- System design: how the work is decomposed into components and pipelines.
- Stack: languages, frameworks, APIs, data stores, testing, and deployment tools.
- Tradeoffs: what I optimized for, what I avoided, and why.
- Learning: what the project taught me technically.

The goal is not to duplicate every repo README. The goal is to make the engineering thinking easy to inspect.

## Folder Convention

Each project gets its own folder under `projects/`:

```text
projects/
  project-name/
    README.md
    architecture.md
    pipelines.md
    stack.md
    security-privacy.md
    project-structure.md
    screenshots/
```

Small projects can keep everything in the project `README.md`. Larger projects can use the supporting files when there is enough architecture, workflow, or security context to justify the extra detail.
