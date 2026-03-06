---
name: add-telegram
description: "Adds a Telegram bot channel to OpenClaw. Installs node-telegram-bot-api, creates src/channels/telegram.ts with a TelegramChannel class, registers it in the channel registry, and wires trigger-word filtering. Use when you want OpenClaw to receive and respond to messages sent to a Telegram bot. Requires a bot token from @BotFather."
---

# add-telegram

Connect OpenClaw to Telegram. Users can message your bot; OpenClaw routes, processes, and replies.

## Usage
```
/add-telegram
```

## Files Created
```
src/channels/telegram.ts        # TelegramChannel class with polling and webhook support
```

## Files Modified
```
src/channels/index.ts           # Register TelegramChannel
src/config.ts                   # Add TELEGRAM_BOT_TOKEN to config schema
package.json                    # Add node-telegram-bot-api dependency
```

## Environment Variables
```
TELEGRAM_BOT_TOKEN=your_bot_token_from_botfather
OPENCLAW_TRIGGER=@OpenClaw          # Optional: trigger word, default @OpenClaw
TELEGRAM_WEBHOOK_URL=               # Optional: set for webhook mode; empty = polling
```

## Architecture: Polling vs Webhook
- **Polling** (default): Bot polls Telegram every second. Works locally, no public URL needed.
- **Webhook**: Telegram pushes updates to your server. Faster, better for production. Set `TELEGRAM_WEBHOOK_URL`.

## Step-by-Step Walkthrough

1. Get bot token: Message `@BotFather` on Telegram → `/newbot` → copy token
2. Run `/add-telegram`
3. OpenClaw installs `node-telegram-bot-api` and generates the channel adapter
4. Set `TELEGRAM_BOT_TOKEN` in your `.env`
5. Start OpenClaw — bot is now active in polling mode

## Code Sample
```typescript
// src/channels/telegram.ts (generated)
import TelegramBot from 'node-telegram-bot-api';
import { Channel, Message } from '../types';

export class TelegramChannel implements Channel {
  private bot: TelegramBot;
  private trigger: string;

  constructor(token: string, trigger = '@OpenClaw') {
    this.bot = new TelegramBot(token, { polling: true });
    this.trigger = trigger;
  }

  listen(handler: (msg: Message) => Promise<string>): void {
    this.bot.on('message', async (msg) => {
      const text = msg.text ?? '';
      if (!text.includes(this.trigger)) return;
      const reply = await handler({ text, source: 'telegram', chatId: msg.chat.id });
      await this.bot.sendMessage(msg.chat.id, reply);
    });
  }
}
```

## Philosophy
OpenClaw treats each channel as a stateless adapter — the channel transforms platform-specific message formats into OpenClaw's internal `Message` type, then back to the platform's native reply format.
