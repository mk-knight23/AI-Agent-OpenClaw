# Heartbeat Tasks — OpenClaw

Continuous background tasks that run on a schedule to keep the OpenClaw ecosystem healthy.

## Architecture
OpenClaw runs heartbeat tasks as Node.js workers. Adapted from PicoClaw's HEARTBEAT.md model.

## Active Heartbeats

### Heartbeat 1: Repo Health Monitor (every 6 hours)
```
INTERVAL: 6h
CHANNEL: telegram/@alerts

- Check all 60 repos for failed CI pipelines
- Flag repos with open CRITICAL issues > 3 days
- Report stale PRs (no activity > 5 days)
```

### Heartbeat 2: Job Campaign Monitor (every 2 hours, weekdays)
```
INTERVAL: 2h (weekdays 8AM-6PM)
CHANNEL: telegram/@job-alerts

- Check Gmail for new replies from job applications
- Track email open rates via Supabase
- Alert on interview requests (priority: URGENT)
```

### Heartbeat 3: Deployment Health (every 30 minutes)
```
INTERVAL: 30m
CHANNEL: slack/#ops

- Ping all deployed services (Railway, Fly.io, Lambda)
- Verify SSL certificates > 30 days remaining
- Check database connection pool status
```

### Heartbeat 4: Daily GitHub Digest (every day 7AM)
```
INTERVAL: 24h
CHANNEL: telegram/@briefing

- Summarize yesterday's GitHub activity across all repos
- List PRs opened/merged
- New issues created
- Stars/forks delta
```

## Configuration
```json
{
  "heartbeats": [
    {"id": "repo-health", "interval": "6h", "enabled": true},
    {"id": "job-monitor", "interval": "2h", "weekdays_only": true},
    {"id": "deployment-health", "interval": "30m", "enabled": true},
    {"id": "github-digest", "cron": "0 7 * * *", "enabled": true}
  ]
}
```

## Implementation
Heartbeats run as Node.js `setInterval` workers inside OpenClaw's gateway process. On gateway restart, all heartbeats resume from their configured intervals. No external scheduler needed.
