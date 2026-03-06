# Daily GitHub Digest — OpenClaw

Every morning, OpenClaw delivers a digest of GitHub activity relevant to your open source projects, job search, and content creation pipeline.

## Trigger
- **Schedule**: Daily at 8:00 AM
- **Manual**: `@OpenClaw run daily-github-digest`

## Prerequisites
- `GITHUB_TOKEN` in environment
- Repos configured in `config.ts`

## What OpenClaw Tracks

### Your Repos
- New stars, forks, issues, and PRs on repos you own
- Mentions of your username in issues/PR comments
- New releases from repos you watch

### Job Search Signals (if job-hunter skill installed)
- GitHub job postings matching your criteria
- Companies that starred your portfolio repos
- Engineers from target companies who engaged with your work

### Portfolio Activity (if portfolio-manager skill installed)
- Traffic spikes on your portfolio repos
- New followers that correlate with content you published

## Example Output

```
📋 GitHub Digest — 2026-03-05

your-repos/awesome-project
  • ⭐ 47 new stars (best day this month)
  • PR #12 opened by external contributor
  • Issue #89: "Windows compatibility broken"

Job signals:
  • 3 engineers from Stripe engaged with your work today
  • New job posting: "Senior TypeScript Engineer" at Linear (matches your criteria)
```

## Configuration

```typescript
// config.ts
export const digestConfig = {
  time: '08:00',
  ownedRepos: true,
  watchedRepos: ['anthropics/anthropic-sdk-typescript'],
  jobSignals: true,
  skipBots: true,
};
```
