# Getting Started with Guardrail

Get up and running with Guardrail in under 5 minutes.

## Prerequisites

- **Node.js** >= 20.11
- **npm**, **pnpm**, or **yarn**

## Installation

```bash
# npm
npm install -D guardrail

# pnpm (recommended)
pnpm add -D guardrail

# yarn
yarn add -D guardrail

# Or run directly with npx
npx guardrail --version
```

## Your First Scan

```bash
# Run a quick scan
npx guardrail scan

# See detailed output
npx guardrail scan --verbose
```

### What Gets Checked

| Check | Description |
|-------|-------------|
| 🔐 **Auth** | Endpoints have proper authentication |
| 🔑 **Secrets** | No API keys or tokens in code |
| 🎭 **Mocks** | No mock/demo data in production paths |
| 🚫 **Dead Routes** | No placeholder or 404 handlers |
| 🔄 **Contracts** | UI matches API endpoints |

## Understanding Results

```
🛡️ Guardrail Scan Results
━━━━━━━━━━━━━━━━━━━━━━━━━━

Score: 85/100 🟢

✓ Auth Check: PASSED
✓ Secrets: PASSED  
⚠ Mocks: 2 warnings
✗ Dead Routes: 1 issue

Issues Found:
─────────────
[HIGH] src/api/users.ts:45
  └─ Route handler returns placeholder response

[WARN] src/config.ts:12
  └─ Demo API endpoint detected
```

### Score Breakdown

| Score | Status | Meaning |
|-------|--------|---------|
| 80-100 | 🟢 SHIP | Ready to deploy |
| 50-79 | 🟡 WARN | Review recommended |
| 0-49 | 🔴 BLOCK | Critical issues |

## CI Integration

Add to your workflow:

```yaml
# .github/workflows/guardrail.yml
name: Guardrail

on: [push, pull_request]

jobs:
  guardrail:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx guardrail gate
```

## Configuration

Create `guardrail.config.json`:

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene"],
  "policy": {
    "failOn": ["critical", "high"],
    "allowlist": {
      "paths": ["tests/**", "*.test.*"]
    }
  }
}
```

## Next Steps

- 📖 [CLI Reference](./cli-reference.md) - All commands
- ⚙️ [Configuration](./configuration.md) - Customize behavior
- 🔬 [Reality Mode](./reality-mode.md) - Runtime verification
- 🤖 [MCP Integration](./mcp.md) - AI IDE setup

---

**Need help?** [Discord](https://discord.gg/guardrail) · [Support](mailto:support@getguardrail.io)
