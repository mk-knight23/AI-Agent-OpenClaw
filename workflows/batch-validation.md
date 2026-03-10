# Batch Validation — OpenClaw

Run large-scale data validation across the 60-repo portfolio using batch-processor + data-validator patterns from ZeroClaw.

## Trigger
- **Manual**: `@OpenClaw batch-validation --target <dataset>`
- **Scheduled**: Sunday 3:00 AM (post-repo-upgrade-cycle)

## Use Cases

### 1. Portfolio Metadata Validation
Validates all 60 repos have required fields in their JSON metadata:
```
@OpenClaw batch-processor --input repo_metadata.json \
  --task "validate: has README, has LICENSE, has CI, has description"
```

### 2. HR Campaign Email Validation
Before sending, validates all 80+ personalized emails:
- Required sections present (intro, value prop, CTA)
- No placeholder text remaining (`[COMPANY_NAME]`, `{{role}}`)
- Subject line length < 60 chars
- No spam trigger words

### 3. Content Quality Gate
Validates all generated blog posts / docs before publishing:
- Factual accuracy check (AI review vs. source material)
- Broken link detection
- Readability score > 60 (Flesch-Kincaid)
- SEO: title < 60 chars, meta description 150-160 chars

## Pipeline
```javascript
// Batch validation orchestration
const results = await batchProcessor.run({
  input: 'data/repo_metadata.jsonl',
  task: 'validate each record against schema and return: {valid, errors}',
  concurrency: 10,
  model: 'claude-haiku-4-5'
});

const failures = results.filter(r => !r.valid);
await generateReport(failures);
```

## Output
```
VALIDATION_REPORT.md        # Summary: X/Y records valid, top failure categories
failed_records.jsonl         # Records that failed validation + specific errors
validation_stats.json        # Machine-readable for CI artifact
```
