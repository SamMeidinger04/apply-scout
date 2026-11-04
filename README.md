# Apply Scout

**Autonomous job-search & application agent** for early-career cybersecurity, IT support, and junior software roles — with calendar-driven scanning, multi-board sourcing, ATS form automation, resume/cover-letter tailoring, and an audit-grade application tracker.

Built for a real Calgary-based candidate pipeline (Canadian citizen, SAIT BTech Cyber Security, FT availability May 2027). This repo documents the **system design** and public architecture; secrets, session cookies, and personal documents stay private.

## Problem

Entry-level tech hiring is high-volume and fragmented: LinkedIn Easy Apply, Indeed, Greenhouse, Lever, Workday, Dayforce, Ashby, BambooHR, Breezy, and one-off career sites. Manual apply loops burn hours and lose state. Apply Scout turns that into a **repeatable agent workflow** with standing rules, human-in-the-loop only for captchas/SSO/payment, and a durable spreadsheet ledger.

## What it does

| Capability | Detail |
|------------|--------|
| Target filter | Entry cyber · IT/helpdesk/app support · junior SWE · internships/co-ops · **Calgary or remote Canada** |
| Multi-source hunt | LinkedIn first, then Indeed, company career pages, other boards |
| Apply automation | Browser automation + ATS guest/account flows; resume upload; screening Qs; email OTP via Gmail |
| Tailoring | Standard one-pager for high-volume Easy Apply; **role-specific resume + cover letter** for strong fits (cyber, Calgary onsite IT, high pay, priority companies) |
| Standing policy | Apply matching roles without per-job confirmation; skip ~$20/hr or less; honest answers (never invent experience) |
| Human escalation | Puzzle captcha / SSO / payment → mark `Needs human`, continue the queue |
| Tracker | Excel ledger: Date Applied, Company, Role, Location, Source, URL, Status, Cover Letter, Notes, Last Updated |
| Status sync | Gmail-driven rejection / interview detection folded into the tracker |
| Schedule | Weekday morning job scan routine (~08:30 local) |

## Pipeline architecture

```
weekday scan ──► board search (LinkedIn / Indeed / careers)
                      │
                      ▼
              match filter (role · location · pay · anti-dup)
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
   Easy Apply / guest ATS    account ATS / SSO
          │                       │
          ▼                       ▼
   tailored resume/CL?      Needs human OR forgot-password flow
          │                       │
          └───────────┬───────────┘
                      ▼
              submit + confirm
                      │
                      ▼
         tracker row + optional cover-letter archive
                      │
                      ▼
              Gmail status watch (reject / interview)
```

## Outcomes (real pipeline snapshot — Nov 2026)

Public-facing metrics from the live tracker (personal PII redacted from this repo):

| Metric | Count |
|--------|-------|
| Rows in ledger | 476 |
| **Applied** | 89 |
| **Interview-stage** | 3 (incl. junior software + IT analyst tracks) |
| Rejected (logged) | 11 |
| Needs human (queued blockers) | 23 |
| Skipped (out of target / pay / requirements) | 333 |

Interview-stage examples the pipeline has produced: **PRI Consultants — Junior Software Developer** (video screen), **Suncor — student / analyst track**, and **Kaco Systems** (meeting booked).

> Framing for recruiters: this is an **ops + automation systems** project — agent orchestration, browser/ATS integration, document generation, and CRM-like tracking — not a claim that every application converts.

## Tech surface

- Agent runtime with scheduled routines and background browser workers
- Document pipeline: DOCX/PDF resume generation, role-specific cover letters
- Spreadsheet CRM (`openpyxl`) as source of truth
- Email connector for OTP + status classification
- Policy engine: location, pay floor, honesty constraints, anti-fabrication rules

## Repo layout

```
apply-scout/
  README.md                 # this file
  docs/architecture.md      # deeper design notes
  docs/policy.md            # standing apply / skip / escalate rules (sanitized)
  schemas/tracker.schema.json
```

## Privacy

This repository **does not** include resumes, transcripts, Gmail contents, passwords, session cookies, or the live tracker spreadsheet. Those stay in a private agent data store.

## Author

[Samuel Meidinger](https://github.com/SamMeidinger04) · samuelmeidinger7@gmail.com · Calgary, AB

## License

MIT — documentation and schemas only; no warranty.
