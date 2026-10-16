# Pre-Publish Safety Checklist

Run through every item before pushing this repo to GitHub. Check each box when confirmed.

---

## PHI / PII

- [ ] No real candidate names appear anywhere in the repo
- [ ] No real email addresses appear anywhere in the repo
- [ ] No real phone numbers appear anywhere in the repo
- [ ] data/sample-records.csv contains only synthetic/fictional names and @example.com addresses
- [ ] No screenshots or attached files contain real candidate information
- [ ] No internal Slack messages, emails, or notes are embedded in any file

---

## Credentials & Secrets

- [ ] No Anthropic API keys — all replaced with YOUR_ANTHROPIC_API_KEY
- [ ] No Make.com webhook URLs — replaced with YOUR_WEBHOOK_ID
- [ ] No Airtable base or table IDs — replaced with YOUR_AIRTABLE_BASE_ID / YOUR_AIRTABLE_TABLE_ID
- [ ] No Gmail OAuth tokens or connection IDs — replaced with YOUR_GMAIL_CONNECTION_ID
- [ ] No form platform API keys or form IDs — replaced with YOUR_FORM_ID
- [ ] Run a global search for common secret patterns before publishing:
  - sk-ant- (Anthropic key prefix)
  - key or token followed by a long alphanumeric string
  - Bearer  in any file
  - app followed by letters and numbers (Airtable base ID format)

---

## Company / Brand

- [ ] No company name, logo, or branding in any file
- [ ] No internal job titles, team names, or org chart references
- [ ] No internal program names that could identify the organization
- [ ] No internal Airtable base names or view names that identify the company

---

## Internal Infrastructure

- [ ] No internal domain names or email domains (only @example.com in sample data)
- [ ] No internal Make.com scenario IDs or team IDs
- [ ] No internal Airtable workspace IDs
- [ ] No references to internal tools, vendor contracts, or pricing

---

## Code Quality

- [ ] Blueprint JSON is valid (validate at jsonlint.com or python -m json.tool make-blueprint-sample.json)
- [ ] Webhook payload JSON is valid
- [ ] Mermaid diagram renders correctly (paste into mermaid.live to verify)
- [ ] README is accurate and complete
- [ ] All placeholder values are clearly marked (e.g., YOUR_... convention)

---

## Final GitHub Steps

- [ ] Repo is set to Private initially — switch to Public only after all checks pass
- [ ] Add a .gitignore if any local config or env files exist
- [ ] Confirm the repo description matches your intent (no internal branding)
- [ ] Review the "Files changed" diff one final time before the first push

---

*Last reviewed: [DATE]*
