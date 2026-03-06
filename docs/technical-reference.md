# OpenClaw Technical Documentation

OpenClaw is a highly extensible, multi-agent AI framework built for professional-grade automation.

## Core Concepts

### 1. The Gateway
The central control plane that manages messaging sessions, channel adapters, and skill execution. It maintains persistent WebSocket connections to local and remote channels.

### 2. Skills Engine
A plugin architecture where each skill is a standalone package. Skills can expose:
- **Commands**: Natural language triggers.
- **Hooks**: Listeners for system events.
- **Background Tasks**: Long-running cron jobs.

### 3. Multi-Agent Orchestrator
Uses specialized system prompts and memory scoping to define agent identities. Handover between agents is managed via the `delegate_task` tool.

## API Reference

### Message Object
```typescript
interface Message {
  id: string;
  sender: string;
  channel: 'whatsapp' | 'telegram' | 'slack' | 'discord';
  content: string;
  timestamp: Date;
  metadata: Record<string, any>;
}
```

### Skill Definition
```typescript
export interface Skill {
  name: string;
  description: string;
  onMessage: (msg: Message) => Promise<boolean>;
  onEvent?: (event: SystemEvent) => Promise<void>;
}
```

## Advanced Configuration

### Memory Persistence
OpenClaw uses Supabase by default for long-term memory. Configure your credentials in `.env`:
```bash
SUPABASE_URL=...
SUPABASE_SERVICE_ROLE_KEY=...
```

### Rate Limiting
Prevent API exhaustion by setting limits per agent:
```json
{
  "limits": {
    "daily_tokens": 1000000,
    "max_concurrent_tasks": 5
  }
}
```
