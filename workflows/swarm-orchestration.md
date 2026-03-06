# Swarm Orchestration — OpenClaw

OpenClaw coordinates multiple Claude instances in parallel — one for each open source project, target company, or content piece — and synthesizes their outputs.

## Trigger
- **Manual**: `@OpenClaw swarm "<goal>"`

## OpenClaw-Specific Swarm Use Cases

### Multi-Repo Modernization
```
@OpenClaw swarm "modernize all 8 repos in my GitHub profile"
```
Spawns 8 workers (one per repo), each running `repo-modernizer`. Orchestrator prioritizes and sequences deployments to avoid conflicts.

### Job Application Campaign
```
@OpenClaw swarm "apply to the 10 companies in my target list using my best portfolio pieces"
```
10 parallel workers, each: researching the company, selecting the best portfolio project, drafting a tailored cover letter, submitting. Orchestrator tracks status.

### Content Calendar Execution
```
@OpenClaw swarm "draft 4 posts from my content ideas list"
```
4 workers draft in parallel (one per post). Orchestrator reviews for consistency of voice and schedules publication.

### Portfolio Audit
```
@OpenClaw swarm "audit all my repos and update READMEs to match current code"
```
Maps across repos, reduces to a completion report.

## Configuration

```typescript
// config/swarm.ts
export const swarmConfig = {
  maxAgents: 8,
  agentModel: 'claude-haiku-4-5',
  orchestratorModel: 'claude-sonnet-4-5',
  timeoutSeconds: 600,     // Content creation takes longer
};
```
