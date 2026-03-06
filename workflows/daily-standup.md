# Daily Standup Automation

Every morning at 9 AM: scan GitHub, summarize PRs, check deployments, deliver briefing.

## Trigger
Cron: `0 9 * * *` (9 AM daily)

## Steps
1. Fetch GitHub notifications (unread)
2. List open PRs needing review across all repos
3. Check deployment health (Vercel/Firebase status APIs)
4. Scan for failing CI on recent commits
5. Check Supabase for pending tasks
6. Generate briefing with Claude (Sonnet 4.6, concise mode)
7. Send to Telegram via `wacli` or Telegram channel

## Output Format
```
📋 Daily Standup — March 5, 2026
━━━━━━━━━━━━━━━━━
🔔 Notifications: 3 unread
📝 PRs to review: 1 (AI-Agent-OpenClaw #12)
✅ Deployments: All 5 live
⚠️  CI failures: 0
📌 Tasks: 2 pending
━━━━━━━━━━━━━━━━━
Action: Review PR #12 before noon
```
