# Architecture notes

## Control plane
A persistent agent (“Apply Scout”) owns the job search domain: filters, memory of prior applies, resume strategy, and weekday automation. Subagents handle browser automation; spreadsheet + Gmail connectors handle ledger and OTP/status.

## Data plane
- **Tracker workbook** — append-only style updates; status transitions Applied → Interview / Rejected / Needs human
- **Cover-letter archive** — per-role PDF/TXT generated from resume facts
- **Tailored resumes** — only for strong-fit roles; default remains the approved one-pager

## Failure modes
| Failure | Handling |
|---------|----------|
| Cloudflare / board block | Skip source for that run; continue others |
| Puzzle captcha | `Needs human`; do not block the batch |
| ATS password unknown | Forgot-password flow when policy allows; document in Notes |
| Role outside targets | Skip + reason |

## Honesty constraints
Never invent employers, dates, skills, metrics, or certifications. Screening questions answered factually (e.g. years of infosec experience). Availability stated as May 2027 FT unless the candidate updates policy.
