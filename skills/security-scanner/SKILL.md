---
name: security-scanner
description: "Full-stack security audit for OpenClaw's Node.js ecosystem: npm audit for CVEs, Snyk for supply chain, ESLint security plugin for code patterns, OWASP dependency check, and AI review of the 60-repo portfolio for systemic vulnerabilities. Generates SECURITY_REPORT.md. Adapted from ZeroClaw's comprehensive security-scanner, tuned for Node.js/TypeScript and multi-repo portfolios."
---

# security-scanner

Ecosystem-wide security audit — CVEs, supply chain, code patterns, portfolio scanning.

## Usage
```
@OpenClaw security-scanner --full
@OpenClaw security-scanner --portfolio        # Scan all 60 repos for CVEs
@OpenClaw security-scanner --repo ./target    # Audit a single repository
@OpenClaw security-scanner --fail-on critical # CI gate: fail only on CRITICAL
```

## What It Checks

### 1. Known CVEs — `npm audit` + OWASP
- Direct + transitive dependency scan against npm advisory DB
- OWASP Dependency-Check for non-npm deps (Python scripts, CLI tools)
- Each finding: CVE ID, CVSS score, affected package, fix

### 2. Supply Chain — Snyk
- License policy enforcement (GPL-3.0, AGPL blocklist)
- Typosquatting detection (flags `expressjs` vs `express`)
- Malicious package patterns (dependency confusion)

### 3. Code Security — ESLint + Semgrep
- Injection: SQL, shell, path traversal
- Hardcoded secrets (API keys, tokens, passwords)
- Dangerous functions: `eval`, `exec`, `deserialize`
- Authentication bypass patterns

### 4. Portfolio Scan (OpenClaw-Specific)
- Scans all repositories in the 60-repo matrix
- Identifies shared vulnerable dependencies across repos
- Prioritizes: which repos are in production vs. experimental
- Generates unified report with cross-repo impact analysis

## Files Created
```
SECURITY_REPORT.md          # Full report with severity + remediation
security_audit.json         # Machine-readable for CI artifacts
portfolio_security.md       # Cross-repo vulnerability map
```

## CI Gate Integration
```yaml
# .github/workflows/security-gate.yml
- name: OpenClaw Security Scan
  run: openclaw security-scanner --full --fail-on critical
```

## Philosophy
With 60 repositories to manage, a single vulnerable shared dependency can cascade. Portfolio-level scanning catches these systemic risks before they become incidents.
