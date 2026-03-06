---
name: job-hunter
description: "Automated HR email campaign pipeline. Finds HR contacts at target companies via Hunter.io/Apollo, verifies emails with SMTP ping, generates Claude-personalized cold emails, sends via Gmail rotation across 4 inboxes. Full pipeline from sourcing to tracking. Use when: (1) starting a new job search campaign, (2) adding new target companies, (3) following up on sent applications. NOT for: bulk spam, non-tech companies, or when you haven't prepared your resume/portfolio first."
---

# job-hunter

Full HR email automation pipeline — from finding contacts to tracking replies.

## Pipeline Overview

```
Target Companies → HR Email Discovery → SMTP Verification → Claude Personalization → Gmail Rotation → Open Tracking
```

## Usage

```
@OpenClaw run job-hunter --companies "Anthropic, OpenAI, Cohere, Replit"
@OpenClaw job-hunter status          # Check campaign progress
@OpenClaw job-hunter followup        # Generate follow-up for non-replies after 5 days
```

## Configuration

```json
{
  "skill": "job-hunter",
  "hunter_api_key": "$HUNTER_API_KEY",
  "apollo_api_key": "$APOLLO_API_KEY",
  "gmail_accounts": ["primary@gmail.com", "backup1@gmail.com"],
  "rate_limit": 80,
  "model": "claude-opus-4-6",
  "role": "AI/ML Engineer",
  "portfolio": "https://github.com/mk-knight23"
}
```

## What It Does

### 1. HR Email Discovery
- Queries Hunter.io for `hr@company.com`, `hiring@company.com`, `talent@company.com`
- Cross-references Apollo.io for direct recruiter names
- Scrapes LinkedIn job postings for hiring manager names
- Output: `hr_contacts.json` with name, email, confidence score

### 2. SMTP Verification
- Pings each email's MX records
- RCPT TO verification without sending
- Filters out invalid/catch-all domains
- Output: verified contact list with deliverability score

### 3. Claude Email Personalization
- Reads company's recent GitHub activity, blog posts, job postings
- Generates unique email per company referencing specific projects/values
- Uses your resume + portfolio as context
- A/B tests subject lines

### 4. Gmail Rotation
- Distributes sends across 4 Gmail accounts
- Max 80 emails/hour per account
- Random delays (2-5 min between sends) to avoid spam detection
- Full MIME headers for inbox placement

### 5. Tracking
- Open pixel tracking via Supabase
- Reply detection via IMAP monitoring
- Weekly digest: opens, replies, interview requests

## Real Results (my campaign)

Targeted companies from my workspace:
- Cohere, Replit, Perplexity, OpenAI, Anthropic, HuggingFace, Scale AI, Together AI, xAI, Character.ai, Jina AI, Adept AI, Modular + 22 more

Results stored in: `workspace/CAMPAIGN_COMPLETE_35_EMAILS.md`

## Dependencies

- `hunter.io` API key (free tier: 25 searches/month)
- `apollo.io` API key
- Gmail OAuth tokens (4 accounts)
- Supabase for tracking database
- OpenClaw Gateway running locally

## Files Generated

```
workspace/
├── hr_contacts_collected.md    # All found contacts
├── emails_to_send.md          # Personalized email drafts
├── campaign_summary.md        # Stats and results
└── job_tracker.md             # Application status tracker
```
