---
name: content-creator
description: "Generates blog posts, technical documentation, READMEs, and changelogs in your voice and tone. Publishes to mk-knight23.github.io and dev.to. Use when: (1) writing a technical blog post about something you built, (2) generating changelog from git commits, (3) creating documentation for a new project, (4) writing a README from scratch. NOT for: marketing copy for products you don't own, plagiarizing other content."
---

# content-creator

Write once, publish everywhere — blog posts, docs, READMEs, changelogs in your authentic voice.

## Usage

```
@OpenClaw content-creator blog "How I automated 35 job applications with AI"
@OpenClaw content-creator changelog --repo mk-knight23/AI-Agent-OpenClaw --since v1.0
@OpenClaw content-creator readme --repo mk-knight23/some-project
@OpenClaw content-creator docs --type architecture --project NanoClaw
```

## Content Types

### Blog Post
- Reads your previous posts to learn your voice/tone
- Researches the topic (web search + codebase)
- Writes in first person, technical but accessible
- Includes code snippets from your actual repos
- Generates hero image prompt for DALL-E/Midjourney
- Publishes to GitHub Pages + dev.to

### Changelog
- Reads git log since last version
- Groups commits: features, fixes, breaking changes, performance
- Writes human-readable release notes
- Updates CHANGELOG.md in Keep a Changelog format

### README
- Analyzes repo structure and code
- Detects: language, framework, purpose
- Fills Universal README Template with real content
- Generates architecture Mermaid diagram from code

### Technical Documentation
- Architecture deep-dive
- API reference from code annotations
- Setup guide from package.json/Makefile scripts
- Troubleshooting from common issues in Issues tab

## Voice Configuration

Train the skill on your writing style:
```json
{
  "skill": "content-creator",
  "voice_samples": ["workspace/resume_template_ai_llm.md"],
  "tone": "technical, direct, first-person",
  "publish_to": ["github-pages", "dev.to"],
  "github_pages_repo": "mk-knight23.github.io"
}
```
