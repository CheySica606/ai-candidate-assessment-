# AI Candidate Assessment Platform

An automated pipeline that administers, scores, and routes dental billing knowledge assessments for job candidates — powered by Make.com, Claude AI, and Airtable.

---

## The Problem

A dental billing company runs a competitive career pathway program (CPP) that requires candidates to demonstrate foundational knowledge before advancing to interviews. The manual process involved:

- Emailing assessment links individually
- Collecting and reviewing free-text responses by hand
- Scoring against a rubric inconsistently across reviewers
- Manually entering results into a tracking spreadsheet
- Delayed turnaround — often 48–72 hours per candidate

With 100+ candidates moving through the pipeline, this consumed roughly **8–10 hours of coordinator time per week** and introduced scoring inconsistency that made pass/fail decisions hard to defend.

---

## The Solution

An end-to-end automated assessment pipeline triggered the moment a candidate submits their form:

1. **Candidate submits** a knowledge assessment via an online form (Typeform/JotForm)
2. **Webhook fires** to Make.com the instant the form is submitted
3. **Make.com** orchestrates the scoring pipeline:
   - Parses the submission payload
   - Sends responses to the **Claude AI API** with a structured scoring prompt
   - Claude returns a JSON score object with domain scores and a pass/fail verdict
   - Result is parsed and written to **Airtable** as a candidate record
   - A summary email is sent to the recruiting coordinator
4. **Coordinator reviews** the Airtable record — no manual scoring required

---

## Tools Used

| Tool | Role |
|---|---|
| Make.com | Automation orchestration (webhook → scoring → storage) |
| Claude API (Anthropic) | AI-powered response scoring with structured JSON output |
| Airtable | Candidate tracking database |
| Typeform / JotForm | Assessment form delivery |
| Gmail | Coordinator notification emails |

---

## Metrics

- **100+ assessments** processed through the pipeline
- **~8 hours/week** saved in manual scoring and data entry
- **Consistent scoring** — every candidate evaluated against the same rubric, eliminating reviewer variance
- **Sub-5-minute turnaround** from submission to Airtable record creation (previously 48–72 hours)
- **Automated pass/fail routing** based on configurable domain score thresholds

---

## Repo Contents

```
ai-candidate-assessment-platform/
├── README.md                          ← This file
├── CHECKLIST.md                       ← Pre-publish safety checklist
├── diagrams/
│   └── workflow.mermaid               ← End-to-end pipeline diagram
├── automation/
│   ├── make-blueprint-sample.json     ← Representative Make.com scenario blueprint
│   └── webhook-payload-sample.json    ← Sample webhook payload from form submission
├── prompts/
│   └── scoring-prompt-template.txt    ← Claude API scoring prompt template
└── data/
    └── sample-records.csv             ← 12 synthetic candidate records (no real data)
```

---

## How It Works — Technical Detail

### Webhook Trigger
The form platform sends a POST request to a Make.com webhook URL when a candidate submits. The payload contains candidate metadata and their free-text question responses.

### Claude Scoring
The scoring module sends a structured prompt to the Claude API (`/v1/messages`) containing:
- The assessment rubric
- The candidate's responses
- An instruction to return a JSON object with per-domain scores and a pass/fail verdict

The response is parsed with `ifempty()` wrapping to handle edge cases where the API returns a null or empty body (e.g., due to content filtering or rate limiting).

### Airtable Write
Parsed scores are mapped to Airtable fields via the Make.com Airtable module. Each run creates or updates a record in the candidate tracking base.

### Error Handling
- Null/empty Claude API responses are caught by `ifempty(...; "{}")` and the run is flagged for manual review rather than crashing
- Gmail OAuth token expiry triggers Make.com auto-deactivation; the scenario is set to process queued webhooks on reactivation

---

## Limitations & Notes

- The scoring prompt is calibrated for dental billing domain knowledge; adapt the rubric for other industries
- Claude API responses are non-deterministic — the same answer may receive slightly different scores across runs; thresholds should be set conservatively
- Make.com queues webhook payloads when a scenario is inactive; payloads are held for up to 30 days
- This repo contains **no real candidate data** — see `data/sample-records.csv` for synthetic examples

---

## License

MIT — feel free to adapt for your own assessment automation use case.
