---
name: add-slack
description: "Adds Slack integration to OpenClaw using the @slack/bolt framework. Creates a SlackChannel adapter with Socket Mode (no public URL required), handles app_mention events, routes messages through OpenClaw's skill dispatcher, and responds in-thread. Use when you want OpenClaw accessible from Slack workspaces. Requires a Slack app with Socket Mode enabled."
---

# add-slack

OpenClaw listens for @mentions in any Slack channel and responds in-thread via Socket Mode.

## Usage
```
/add-slack
```

## Files Created
```
src/channels/slack.ts           # SlackChannel using @slack/bolt Socket Mode
```

## Files Modified
```
src/channels/index.ts           # Register SlackChannel
src/config.ts                   # Add SLACK_BOT_TOKEN, SLACK_APP_TOKEN
package.json                    # Add @slack/bolt
```

## Environment Variables
```
SLACK_BOT_TOKEN=xoxb-your-bot-token
SLACK_APP_TOKEN=xapp-your-app-level-token    # For Socket Mode
SLACK_SIGNING_SECRET=your_signing_secret
```

## Setup: Creating the Slack App

1. Go to [api.slack.com/apps](https://api.slack.com/apps) → Create App → From Scratch
2. Socket Mode → Enable → Create App-Level Token with `connections:write` scope → copy as `SLACK_APP_TOKEN`
3. OAuth & Permissions → Bot Token Scopes: `app_mentions:read`, `chat:write`, `channels:history`
4. Install to Workspace → copy Bot Token as `SLACK_BOT_TOKEN`
5. Event Subscriptions → Enable → Subscribe to `app_mention`

## Step-by-Step Walkthrough

1. Create Slack app (above)
2. Run `/add-slack`
3. Set env vars in `.env`
4. Start OpenClaw — connects via WebSocket (no public URL needed)
5. Mention `@OpenClaw` in any channel → OpenClaw responds in thread

## Code Sample
```typescript
// src/channels/slack.ts (generated)
import { App } from '@slack/bolt';
import { Channel, Message } from '../types';

export class SlackChannel implements Channel {
  private app: App;

  constructor(botToken: string, appToken: string, signingSecret: string) {
    this.app = new App({
      token: botToken,
      appToken,
      signingSecret,
      socketMode: true,
    });
  }

  async listen(handler: (msg: Message) => Promise<string>): Promise<void> {
    this.app.event('app_mention', async ({ event, say }) => {
      const reply = await handler({ text: event.text, source: 'slack', threadTs: event.ts });
      await say({ text: reply, thread_ts: event.ts });
    });
    await this.app.start();
  }
}
```

## Philosophy
Socket Mode is the correct default — it works behind firewalls, in local development, and on private networks. Switch to HTTP mode only when running on a public server with a stable URL.
