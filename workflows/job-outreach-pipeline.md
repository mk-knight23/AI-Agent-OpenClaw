# Job Outreach Pipeline

Automated HR email campaign — from company list to tracked replies.

## Trigger

Manual: `@OpenClaw run job-outreach-pipeline`
Scheduled: Every Monday 10 AM

## Pipeline Steps

```
1. Load target companies
2. HR Email Discovery (Hunter.io + Apollo)
3. SMTP Verification
4. Claude Personalization (one unique email per company)
5. Gmail Rotation (80/hr × 4 accounts)
6. Track sends in Supabase
7. Monitor replies via IMAP (hourly)
8. Weekly digest → Telegram
```

## Requirements

- `job-hunter` skill installed
- Hunter.io API key + Apollo API key
- 4 Gmail accounts with OAuth tokens
- Supabase project for tracking

## Target Companies

Cohere · Replit · Perplexity · OpenAI · Anthropic · HuggingFace · Scale AI · Together AI · xAI · Character.ai · Jina AI · Adept AI · Modular (+ 22 more)

## Output Files

```
workspace/
├── campaign_summary.md      # Stats: sent, opened, replied
├── job_tracker.md           # Per-company status
└── hr_contacts_collected.md # All verified contacts
```