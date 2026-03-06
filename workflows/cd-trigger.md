# Continuous Deployment Trigger Workflow

## Description
Autonomous multi-platform deployment pipeline that ensures your repositories are always live across the entire cloud ecosystem.

## Platforms Supported
1. Vercel
2. Netlify
3. Firebase Hosting
4. Cloudflare Pages
5. AWS Amplify
6. Railway
7. Render
8. Surge
9. GitHub Pages
10. Heroku

## Steps

### 1. Change Detection
- Monitors all 60 repos for new pushes to the `main` branch.
- Validates CI status before triggering deployment.

### 2. Multi-Platform Build
- Triggers parallel builds across all configured platforms.
- Handles platform-specific environment variables and build commands.

### 3. Verification
- Pings the unique deployment URL for each platform.
- Screenshots the homepage using Playwright to verify visual integrity.

### 4. Status Update
- Updates the `DEPLOYMENT_MATRIX.md` with live URLs and build times.
- Alerts the user on Discord/Telegram if any platform fails to deploy.

## Usage
Activate the workflow in `config.json`.
`"workflows": ["cd-trigger"]`
