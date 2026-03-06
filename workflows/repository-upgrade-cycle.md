# Repository Upgrade Cycle

Weekly automated portfolio hygiene — select, audit, upgrade, screenshot, deploy.

## Trigger
Cron: Every Sunday 2 AM
Or manual: `@OpenClaw run repo-upgrade`

## Steps
1. Select 3 lowest-scoring repos from portfolio-manager
2. Run `repo-modernizer` on each:
   - Dead code audit
   - Dependency update
   - README upgrade
   - Screenshot capture
   - Deploy to Vercel
3. Commit changes with conventional commit message
4. Update REPO_UPGRADE_TRACKER.md
5. Report results via Telegram

## Selection Criteria
- README score < 7/10
- Last updated > 60 days ago
- No live deployment
