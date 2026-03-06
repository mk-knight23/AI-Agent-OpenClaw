---
name: deploy-everywhere
description: "One command deploys your project to all major hosting platforms simultaneously: Vercel, Firebase Hosting, Cloudflare Pages, Render, and AWS Amplify. Returns a formatted URL table for README badges. Use when: (1) launching a new project, (2) syncing deployments across platforms, (3) updating the deployment matrix in README. NOT for: backend services requiring persistent storage, Docker-only deployments."
---

# deploy-everywhere

Ship to Vercel + Firebase + Cloudflare + Render + Amplify in one shot.

## Usage

```
@OpenClaw deploy-everywhere --project ./website
@OpenClaw deploy-everywhere --project ./website --platforms "vercel,cloudflare"
@OpenClaw deploy-everywhere status --project AI-Agent-OpenClaw
```

## What Gets Deployed

| Platform | URL Pattern | Best For |
|----------|------------|---------|
| Vercel | `project.vercel.app` | Next.js, primary |
| Firebase | `project.web.app` | Static, backup |
| Cloudflare Pages | `project.pages.dev` | Global CDN |
| Render | `project.onrender.com` | Full-stack |
| AWS Amplify | `branch.appid.amplifyapp.com` | AWS ecosystem |

## Output

After deployment, generates URL table for README:

```markdown
| Platform | URL | Status |
|----------|-----|--------|
| Vercel | https://ai-agent-openclaw.vercel.app | ✅ Live |
| Firebase | https://ai-agent-openclaw.web.app | ✅ Live |
| Cloudflare | https://ai-agent-openclaw.pages.dev | ✅ Live |
```

## Configuration

```json
{
  "skill": "deploy-everywhere",
  "vercel_token": "$VERCEL_TOKEN",
  "firebase_project": "$FIREBASE_PROJECT_ID",
  "cloudflare_account": "$CF_ACCOUNT_ID",
  "render_api_key": "$RENDER_API_KEY",
  "auto_update_readme": true
}
```
