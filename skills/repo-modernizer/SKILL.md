---
name: repo-modernizer
description: "Scans any GitHub repository and upgrades it to elite standards. Audits dead code, unused imports, legacy configs, outdated README. Upgrades README to professional format with badges, architecture diagram, quick start. Captures screenshot via Playwright. Deploys to Vercel/Firebase/Cloudflare. Use when: (1) modernizing old repos in your portfolio, (2) upgrading a specific repo before sharing, (3) running weekly portfolio hygiene. NOT for: active development repos with in-progress work, private repos with secrets."
---

# repo-modernizer

Transform any stale repo into a professional showcase — audit, upgrade, screenshot, deploy.

## Usage

```
@OpenClaw repo-modernizer --repo mk-knight23/my-old-project
@OpenClaw repo-modernizer --all          # Run across all 60 repos (weekly mode)
@OpenClaw repo-modernizer --preview      # Show what would change without applying
```

## What It Does

### 1. Code Audit
- Detects unused imports (ESLint/pylint/cargo check)
- Finds dead code (functions never called)
- Identifies outdated dependency versions
- Checks for hardcoded secrets (gitleaks patterns)
- Output: `AUDIT_REPORT.md`

### 2. README Upgrade
Applies the Universal README Template:
- Hero banner placeholder
- Badge row: stars, license, last commit, deploy status
- Architecture Mermaid diagram (auto-generated from code structure)
- Quick start (5 steps max)
- Tech stack table
- Project structure tree
- Roadmap

### 3. Playwright Screenshot
- Spins up the app locally (or uses deployed URL)
- Captures full-page screenshot at 1280×800
- Saves to `/assets/screenshot.png`
- Resizes for README embedding

### 4. Deploy
- Detects framework (Next.js, React, Vue, static)
- Deploys to Vercel via `vercel --prod`
- Returns live URL for README badge
- Also deploys to Firebase + Cloudflare if configured

## Configuration

```json
{
  "skill": "repo-modernizer",
  "targets": ["vercel", "firebase"],
  "screenshot": true,
  "badge_style": "for-the-badge",
  "readme_template": "universal"
}
```

## Output

```
REPO_UPGRADE_TRACKER.md    # Progress across all repos
assets/screenshot.png      # Captured screenshot
AUDIT_REPORT.md           # Code quality findings
```

## Integration with Portfolio Manager

This skill feeds into `portfolio-manager` — every modernized repo updates the deployment matrix and portfolio dashboard.
