# RALPH Methodology

**RALPH** (Research, Analyze, Learn, Plan, Hack) is the core autonomous evolution loop used by OpenClaw to maintain and modernize codebases 24/7.

## The Phases

### 1. Research (R)
- **Action**: Scans the target repository for latest commits, issues, and PRs.
- **Goal**: Identify the most impactful area for improvement (e.g., a buggy component, outdated dependency, or missing tests).

### 2. Analyze (A)
- **Action**: Deep-dives into the codebase. Runs static analysis, reads related files, and maps dependencies.
- **Goal**: Understand the *why* behind existing code and the implications of potential changes.

### 3. Learn (L)
- **Action**: Searches documentation, checks best practices, and reviews similar patterns in other repositories.
- **Goal**: Acquire the specific knowledge needed to implement the improvement correctly.

### 4. Plan (P)
- **Action**: Drafts a step-by-step implementation plan and identifies potential risks.
- **Goal**: Minimize breaking changes and ensure a smooth migration path.

### 5. Hack (H)
- **Action**: Executes the plan. Writes code, refactors, updates documentation, and runs tests.
- **Goal**: Deliver a verified improvement that is ready for deployment.

## Automation
In OpenClaw, the `ralph-evolution` workflow triggers this loop periodically for every repository in the workspace, ensuring the entire portfolio evolves autonomously.
