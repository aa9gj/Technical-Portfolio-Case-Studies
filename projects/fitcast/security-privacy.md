# FitCast Security And Privacy Notes

## Sensitive Data

FitCast can touch sensitive user data:

- Real resume content.
- Job search history.
- Application status.
- Generated tailored resumes and cover letters.
- API keys.
- Webhook URLs.
- Company/job preferences.

The project treats these as local user data rather than public repository content.

## Gitignore Policy

The source repo gitignores personal and generated artifacts:

| Path | Reason |
|---|---|
| `resume.md` | Real resume content |
| `results.csv` | Job search results and fit scores |
| `results.json` | Full evidence chain and posting details |
| `applied.json` | Application history |
| `seen.json` | Job-search state |
| `tailored/` | Generated personal application materials |
| `resume.pdf`, `resume.docx` | Real resume files |
| `resume.extracted.*.txt` | Extracted resume text |
| `.env`, `.env.*` | Secrets and local environment config |
| `companies.bootstrap.yaml` | Regenerable local source expansion |
| `data/.onet_db.zip` | Large regenerable ontology cache |

The committed sample resume is `resume.example.md`, which gives users a safe starting shape without exposing personal data.

## API Keys

Anthropic API access is expected through `ANTHROPIC_API_KEY` in the environment. The quickstart uses an exported environment variable rather than committing secrets to config files.

The smoke test warns when the key is missing and only runs paid checks when explicitly requested.

## Source Access Choices

FitCast intentionally uses public job-board APIs:

- Greenhouse public boards.
- Lever public postings.
- Ashby public job boards.
- The Muse public API.

It does not attempt to scrape LinkedIn or Indeed because those platforms require authentication, aggressively block automation, and raise Terms-of-Service concerns.

## No Auto-Apply

FitCast ranks opportunities and can generate tailored drafts, but it does not auto-apply. That boundary is intentional:

- It avoids spammy recruiter behavior.
- It keeps the applicant responsible for final judgment.
- It reduces the risk of submitting inaccurate or unreviewed materials.

## Truth Constraint In Tailoring

Resume and cover letter tailoring is designed around a truth constraint:

- Do not invent skills.
- Do not inflate years of experience.
- Do not fabricate metrics.
- Do not claim company facts unless they were verified.
- Produce a changes audit so edits can be reviewed.

This lets the tool improve fit and wording without crossing into dishonest generation.

## Webhook Considerations

Webhook notifications are optional and should be treated as potentially sensitive. A payload can include job titles, companies, URLs, scores, and verdicts.

Good defaults:

- Use private Slack or Discord channels.
- Avoid sending full resume text.
- Keep webhook timeout short so notification failure does not block the run.
- Store webhook URLs outside committed files when possible.

## Public Case Study Strategy

Because the FitCast source repo is private, this public case study avoids exposing:

- Personal resume content.
- Generated job results.
- API keys or webhook URLs.
- Private application history.
- Any sensitive implementation output.

It still exposes the important engineering thinking: the problem, stack, architecture, pipelines, quality gates, and tradeoffs.

## License Note

The source repository uses a source-available, noncommercial license during development. That is separate from this public case study, which exists to explain the engineering work for review and interviews.
