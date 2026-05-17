# Bio Claims App Security And Privacy Notes

## Sensitive Data

The app can process sensitive scientific documents, extracted claims, reviewer decisions, API keys, local graph paths, tracing metadata, and generated evidence ledgers.

This case study does not include private corpora, generated answers, reviewer names, configured paths, secrets, or domain-specific business context.

## Security Review Highlights

The source repo includes a security review that identified and remediated critical, high, medium, and low findings. Major mitigations include:

| Area | Mitigation |
|---|---|
| API authentication | Optional bearer token through `API_TOKEN` |
| Rate limiting | In-memory per-IP limiter for API endpoints |
| Upload safety | Configurable max upload size with HTTP 413 behavior |
| CORS | Configurable allowed origins |
| Session IDs | Cryptographically secure token generation |
| SQL migrations | Whitelisted migration columns and escaped identifiers |
| DuckDB paths | Escaped path handling for local KG file reads |
| Secrets | `.env` ignored and `.env.template` kept free of hardcoded credentials |
| Tracing | Exception-safe tracing initialization to avoid key leakage |
| Prompt injection | Instructions and user-provided evidence separated into distinct messages |
| Model supply chain | Pinned revision for remote-code model loading where required |

## Deployment Notes

The local development configuration is not the same as a production deployment. A production setup should provide:

- HTTPS through a reverse proxy or Uvicorn TLS settings.
- Strong API token or full authentication provider.
- Explicit CORS origins.
- Tight rate limits.
- Storage backups.
- Access controls around uploaded documents and review records.
- Model and KG version tracking.
- Audit logging for review and export actions.

## Data Handling

Local stores include:

- ChromaDB vector collections.
- SQLite databases for reviews, workspaces, chat history, ledgers, and caches.
- Uploaded PDFs and parsed outputs.
- Generated claims and exports.

These should be treated as sensitive working data. Public documentation should use redacted examples only.

## Knowledge Graph Paths

KG paths are environment-specific and should not be committed. The project removed hardcoded user-specific path defaults and expects KG locations to be provided through environment variables.

## Prompt And Output Safety

Prompt-injection risk is reduced by separating instructions from evidence and extracted claims. The app also uses citation verification, NLI checks, KG contradiction checks, and human review to reduce unsupported output.

## Public Case Study Strategy

The public portfolio writeup focuses on reusable architecture:

- RAG and GraphRAG design.
- KG adapters and multi-hop traversal.
- Claim scoring and verification.
- Evaluation and human review.
- Security posture.

It avoids exposing private documents, generated outputs, proprietary context, secrets, or configured infrastructure.
