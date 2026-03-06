---
name: add-supabase
description: "Adds Supabase as OpenClaw's persistent backend. Creates tables for conversation history, skill invocation logs, and agent memory using the Supabase JS client. Wires memory-read and memory-write into the tool registry so skills can persist state across sessions. Requires a Supabase project URL and anon key."
---

# add-supabase

Give OpenClaw persistent memory. Skills can read and write structured state that survives restarts.

## Usage
```
/add-supabase
```

## Files Created
```
src/db/supabase.ts              # Typed Supabase client with table helpers
src/db/migrations/001_init.sql  # Initial schema: memory, skill_logs, conversations
src/tools/memory.ts             # memory_read and memory_write tools
```

## Files Modified
```
src/tools/index.ts              # Register memory_read, memory_write
src/config.ts                   # Add SUPABASE_URL, SUPABASE_ANON_KEY
package.json                    # Add @supabase/supabase-js
```

## Environment Variables
```
SUPABASE_URL=https://your-project-id.supabase.co
SUPABASE_ANON_KEY=your_anon_key
```

## Tables Created

```sql
-- Run migrations/001_init.sql in the Supabase SQL editor
CREATE TABLE agent_memory (
  key TEXT PRIMARY KEY,
  value JSONB NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE skill_invocations (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  skill_name TEXT NOT NULL,
  input JSONB,
  duration_ms INTEGER,
  ts TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE conversations (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  channel TEXT NOT NULL,
  messages JSONB DEFAULT '[]',
  created_at TIMESTAMPTZ DEFAULT now()
);
```

## Step-by-Step Walkthrough

1. Create a Supabase project at [supabase.com](https://supabase.com)
2. Copy the Project URL and anon key from Settings → API
3. Run `/add-supabase`
4. Open the Supabase SQL editor and run `migrations/001_init.sql`
5. Set env vars in `.env`
6. OpenClaw now remembers state across restarts

## Code Sample
```typescript
// src/db/supabase.ts (generated)
import { createClient } from '@supabase/supabase-js';

export const supabase = createClient(
  process.env.SUPABASE_URL!,
  process.env.SUPABASE_ANON_KEY!
);

export async function memoryGet(key: string): Promise<unknown> {
  const { data } = await supabase.from('agent_memory').select('value').eq('key', key).single();
  return data?.value ?? null;
}

export async function memorySet(key: string, value: unknown): Promise<void> {
  await supabase.from('agent_memory').upsert({ key, value, updated_at: new Date().toISOString() });
}
```

## Philosophy
Supabase is the right default for OpenClaw's persistence layer: it scales from zero, has a generous free tier, and the real-time subscriptions unlock future features like multi-agent shared state.
