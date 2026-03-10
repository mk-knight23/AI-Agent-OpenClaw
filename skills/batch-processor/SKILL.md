---
name: batch-processor
description: "High-throughput parallel processing of large datasets using Node.js streams and Claude AI. Accepts CSV, JSON, JSONL, or plain text. Processes in configurable chunk sizes with backpressure-aware Node.js Readable/Transform/Writable streams. Tracks progress, retries failures with exponential backoff, estimates cost before run. Adapted from ZeroClaw's Rayon-based batch processor for Node.js streaming architecture."
---

# batch-processor

Process massive datasets with Claude via Node.js streams. Backpressure-aware. Cost-efficient.

## Usage
```
@OpenClaw batch-processor --input data.csv --task "extract all company names"
@OpenClaw batch-processor --input records.jsonl --task "classify intent" --output results.jsonl
@OpenClaw batch-processor --input large.csv --chunk-size 100 --concurrency 8 --estimate-cost
@OpenClaw batch-processor --resume job_20240310_093045
```

## Architecture

```typescript
// Node.js streaming pipeline — no entire file in memory
createReadStream('data.csv')
  .pipe(csvParser())
  .pipe(chunkTransform({ size: 50 }))
  .pipe(claudeTransform({ task, concurrency: 8 }))  // Haiku workers
  .pipe(createWriteStream('results.jsonl'));
```

## Supported Formats

| Format | Input | Output |
|--------|-------|--------|
| CSV | ✓ | ✓ |
| JSONL | ✓ | ✓ |
| JSON array | ✓ | ✓ |
| Plain text (line-by-line) | ✓ | ✓ |

## Cost Estimation
```
@OpenClaw batch-processor --input contacts.csv --task "personalize email" --estimate-cost

Records: 5,000
Avg tokens/record: 200 input + 300 output
Model: claude-haiku-4-5
Estimated cost: $0.38
Estimated time: ~6 minutes

Proceed? [y/N]
```

## Progress & Resumability
- `rich`-style progress bar in terminal
- Checkpoint every 200 records (configurable)
- `--resume <job_id>` skips already-processed records
- Failed records → `failed_records.jsonl` with error context

## Output
```
output/
├── results.jsonl               # All processed records
├── batch_summary.md            # Stats: total, success, failed, cost, time
├── failed_records.jsonl        # Retry-able failures
└── jobs/job_20240310_093045/   # Checkpoints
```

## Philosophy
OpenClaw orchestrates. Haiku does the heavy lifting at ~15x lower cost. This skill processes the 60-repo matrix, HR campaign data, and content pipelines without breaking the budget.
