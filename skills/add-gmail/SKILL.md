---
name: add-gmail
description: "Adds Gmail monitoring to OpenClaw using the googleapis library. Creates OAuth2 credentials, polls INBOX for unread messages matching a trigger pattern, drafts AI replies, and sends after approval. Use when you want OpenClaw to monitor and respond to emails. Requires a Google Cloud project with Gmail API enabled and OAuth2 credentials."
---

# add-gmail

OpenClaw reads your inbox, understands emails, drafts responses, and sends after you approve.

## Usage
```
/add-gmail
```

## Files Created
```
src/channels/gmail.ts           # GmailChannel using googleapis OAuth2
src/auth/gmail-oauth.ts         # OAuth2 token flow and refresh logic
```

## Files Modified
```
src/channels/index.ts           # Register GmailChannel
src/config.ts                   # Add Gmail config fields
package.json                    # Add googleapis
```

## Environment Variables
```
GMAIL_CLIENT_ID=your_oauth_client_id
GMAIL_CLIENT_SECRET=your_oauth_client_secret
GMAIL_REFRESH_TOKEN=your_refresh_token
GMAIL_ADDRESS=yourname@gmail.com
GMAIL_POLL_INTERVAL_MS=60000        # Default: 1 minute
GMAIL_TRIGGER_LABEL=openclaw        # Only process emails with this label
```

## Setup: Getting OAuth2 Credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create project → Enable Gmail API
3. OAuth2 credentials → Desktop App → Download JSON
4. Run the auth helper: `npx openclaw auth gmail`
5. Complete browser flow → refresh token saved automatically

## Step-by-Step Walkthrough

1. Run `/add-gmail`
2. OpenClaw scaffolds the channel and auth helper
3. Run `npx openclaw auth gmail` to get your refresh token
4. Set env vars in `.env`
5. Start OpenClaw — polls Gmail every minute
6. Label emails `openclaw` in Gmail to trigger processing

## Code Sample
```typescript
// src/channels/gmail.ts (generated)
import { google } from 'googleapis';
import { Channel, Message } from '../types';

export class GmailChannel implements Channel {
  private gmail;

  constructor(auth: OAuth2Client) {
    this.gmail = google.gmail({ version: 'v1', auth });
  }

  async *poll(): AsyncGenerator<Message> {
    const res = await this.gmail.users.messages.list({
      userId: 'me',
      q: 'label:openclaw is:unread',
    });
    for (const m of res.data.messages ?? []) {
      const full = await this.gmail.users.messages.get({ userId: 'me', id: m.id! });
      yield parseGmailMessage(full.data);
    }
  }
}
```

## Philosophy
The approval step before sending is intentional — email carries more weight than chat. OpenClaw drafts; you decide when to send.
