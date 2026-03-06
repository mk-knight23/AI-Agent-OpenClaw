---
name: add-obsidian
description: "Mounts an Obsidian vault as a read-only knowledge source for OpenClaw. Registers a vault_search tool that performs full-text search across all .md files, returning file names and relevant excerpts. Use when you want OpenClaw to answer questions using your personal notes or team knowledge base. Requires the vault to be accessible as a local directory."
---

# add-obsidian

Give OpenClaw access to your Obsidian vault. Ask questions; it searches your notes to answer.

## Usage
```
/add-obsidian
```

## Files Created
```
src/tools/vault-search.ts       # VaultSearch tool with fuzzy full-text search
```

## Files Modified
```
src/tools/index.ts              # Register vault_search in tool registry
src/config.ts                   # Add OBSIDIAN_VAULT_PATH
```

## Environment Variables
```
OBSIDIAN_VAULT_PATH=/Users/yourname/Documents/MyVault
VAULT_MAX_RESULTS=10            # Max results returned per search, default 10
VAULT_EXCERPT_LENGTH=400        # Characters per excerpt, default 400
```

## How It Works

1. OpenClaw's tool dispatcher exposes `vault_search(query: string)` to the LLM
2. When you ask a question, the LLM calls `vault_search` with relevant keywords
3. The tool walks the vault directory, reads each `.md` file, and scores against the query
4. Top results returned as `[{file, excerpt, path}]` — the LLM synthesizes a response

## Step-by-Step Walkthrough

1. Run `/add-obsidian`
2. Set `OBSIDIAN_VAULT_PATH` in `.env` to your absolute vault path
3. Start OpenClaw
4. Ask: *"What did I write about the Pomodoro technique?"* → OpenClaw searches vault and summarizes

## Code Sample
```typescript
// src/tools/vault-search.ts (generated)
import fs from 'fs';
import path from 'path';
import { Tool } from '../types';

export const vaultSearch: Tool = {
  name: 'vault_search',
  description: 'Search the Obsidian knowledge vault for notes matching a query',
  parameters: {
    query: { type: 'string', description: 'Search terms' },
  },
  async execute({ query }: { query: string }) {
    const vaultPath = process.env.OBSIDIAN_VAULT_PATH!;
    const results: Array<{ file: string; excerpt: string }> = [];
    walkMarkdown(vaultPath, (filePath, content) => {
      if (content.toLowerCase().includes(query.toLowerCase())) {
        const idx = content.toLowerCase().indexOf(query.toLowerCase());
        const excerpt = content.slice(Math.max(0, idx - 100), idx + 300);
        results.push({ file: path.basename(filePath), excerpt });
      }
    });
    return results.slice(0, Number(process.env.VAULT_MAX_RESULTS ?? 10));
  },
};
```

## Philosophy
The vault is mounted read-only by convention — OpenClaw reads, never writes. Your notes stay exactly as you left them.
