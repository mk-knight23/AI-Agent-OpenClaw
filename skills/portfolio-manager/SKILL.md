---
name: portfolio-manager
description: "Orchestrates your entire GitHub portfolio of 60+ repositories. Runs batch status checks, dependency audits, README consistency enforcement, and deployment matrix management. Generates weekly portfolio health reports. Use when: (1) doing weekly portfolio review, (2) checking which repos need attention, (3) enforcing README consistency across all repos. NOT for: individual repo deep-dives (use repo-modernizer), active development repos."
---

# portfolio-manager

Your 60-repo portfolio command center — batch status, audits, consistency enforcement, deployment tracking.

## Usage

```
@OpenClaw portfolio-manager status           # Full portfolio health check
@OpenClaw portfolio-manager audit            # Dependency audit across all repos
@OpenClaw portfolio-manager readme-check     # README quality scores for all repos
@OpenClaw portfolio-manager deploy-matrix    # Show deployment status table
@OpenClaw portfolio-manager weekly-report    # Generate comprehensive weekly digest
```

## What It Manages

### Portfolio Inventory
Tracks all repos from: `workspace/REPO_INVENTORY_20260305.md`

Categories:
- Active projects (updated last 30 days)
- Showcase repos (high stars/engagement)
- Learning projects (experiments)
- Archived (no updates 90+ days)

### Deployment Matrix

| Repo | Vercel | Firebase | Cloudflare | Render | Status |
|------|--------|----------|------------|--------|--------|
| AI-Agent-OpenClaw | ✅ | ✅ | ✅ | ✅ | Live |
| AI-Agent-Nanobot | ✅ | ✅ | ✅ | ✅ | Live |
| AI-Agent-ZeroClaw | ✅ | ✅ | ✅ | ❌ | Live |
| AI-Agent-PicoClaw | ✅ | ❌ | ❌ | ❌ | Live |
| AI-Agent-NanoClaw | ✅ | ❌ | ❌ | ❌ | Live |

### README Quality Scoring

Scores each README on 10 criteria:
- Has hero banner (2pts)
- Has badge row (1pt)
- Has architecture diagram (2pts)
- Has quick start (2pts)
- Has tech stack table (1pt)
- Has roadmap (1pt)
- Last updated within 30 days (1pt)

### Weekly Report Format

```
PORTFOLIO WEEKLY DIGEST — Week of 2026-03-05
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Active repos: 12/60
Repos needing README update: 8
Outdated dependencies: 15 repos
Deployment failures: 0
New stars this week: +847
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Action items: [3 repos need README upgrade]
```

## Configuration

```json
{
  "skill": "portfolio-manager",
  "github_username": "mk-knight23",
  "total_repos": 60,
  "report_channel": "telegram",
  "report_schedule": "weekly",
  "readme_min_score": 7
}
```
