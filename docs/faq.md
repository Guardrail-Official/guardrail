# Frequently Asked Questions

## General

### What is Guardrail?

Guardrail is a CI/CD tool that verifies AI-generated code actually works before you ship it. It catches fake features, broken auth, exposed secrets, and mock data that static analysis misses.

### Why do I need Guardrail?

AI coding assistants create "convincing wrongness" - code that looks correct, passes tests, and seems to work, but doesn't actually do what it should. Guardrail catches these issues before they reach production.

### Does Guardrail send my code anywhere?

No. Guardrail runs 100% locally. Your code never leaves your machine or CI environment. We don't collect telemetry or phone home.

### What languages/frameworks does Guardrail support?

Guardrail works with any JavaScript/TypeScript project:
- React, Next.js, Vue, Angular, Svelte
- Node.js, Express, Fastify, NestJS
- Any npm-based project

---

## Installation

### How do I install Guardrail?

```bash
npm install -D guardrail
# or
pnpm add -D guardrail
# or
yarn add -D guardrail
```

### What Node.js version do I need?

Node.js 20.11 or higher.

### Does Guardrail work in monorepos?

Yes! Guardrail automatically detects monorepo structures (Turborepo, Nx, Lerna) and scans appropriately.

---

## Usage

### How do I run my first scan?

```bash
npx guardrail scan
```

### How do I integrate with CI?

Add this to your GitHub Actions:

```yaml
- run: npx guardrail gate
```

See [CI/CD Integration](./ci-cd.md) for more platforms.

### What's the difference between `scan` and `gate`?

- **`scan`** - Runs checks and produces reports (informational)
- **`gate`** - Runs checks and exits with error code if policy fails (enforcement)

### How do I fix issues Guardrail finds?

```bash
# See fix suggestions
npx guardrail fix --plan

# Apply safe fixes
npx guardrail fix --apply
```

---

## Configuration

### How do I ignore certain files?

In `guardrail.config.json`:

```json
{
  "policy": {
    "ignore": {
      "paths": ["tests/**", "*.test.ts", "mocks/**"]
    }
  }
}
```

### How do I allow certain API domains?

```json
{
  "policy": {
    "allowlist": {
      "domains": ["api.stripe.com", "api.github.com"]
    }
  }
}
```

### How do I change the failure threshold?

```bash
npx guardrail gate --threshold=70
```

Or in config:

```json
{
  "policy": {
    "threshold": 70
  }
}
```

---

## Reality Mode

### What is Reality Mode?

Reality Mode uses Playwright to actually run your app and verify it works at runtime, catching issues that static analysis can't.

### Do I need Playwright installed?

For Runtime Mode, yes:

```bash
npx playwright install chromium
```

### Why did Reality Mode fail?

Check the generated report at `.guardrail/reality/reality-report.html` for details. Common issues:
- App not running at the specified URL
- Network requests hitting mock servers
- Forms not submitting properly

---

## MCP Integration

### What IDEs support MCP?

- Cursor
- Windsurf
- Claude Desktop
- Any MCP-compatible AI assistant

### How do I set up MCP?

See [MCP Integration](./mcp.md) for setup instructions.

---

## Pricing

### Is Guardrail free?

Yes! The core features (`scan`, `gate`) are free forever.

### What's in the paid tiers?

| Feature | Free | Starter | Pro |
|---------|------|---------|-----|
| scan, gate | ✅ | ✅ | ✅ |
| Mock detection | ❌ | ✅ | ✅ |
| Reality Mode | ❌ | ❌ | ✅ |
| AI Agent | ❌ | ❌ | ✅ |
| Fix workflows | ❌ | ❌ | ✅ |

See [getguardrail.io/pricing](https://getguardrail.io/pricing) for details.

---

## Troubleshooting

### Guardrail is slow

Try:
```bash
# Quick profile
npx guardrail scan --profile=quick

# Exclude large directories
# In guardrail.config.json:
{
  "policy": {
    "ignore": {
      "paths": ["node_modules", "dist", ".next"]
    }
  }
}
```

### False positives

Add to allowlist:
```json
{
  "policy": {
    "allowlist": {
      "paths": ["src/testing/**"]
    }
  }
}
```

### Command not found

```bash
# Use npx
npx guardrail scan

# Or install globally
npm install -g guardrail
```

---

## Support

### How do I get help?

- 💬 **Discord:** [discord.gg/guardrail](https://discord.gg/guardrail)
- 📧 **Email:** [support@getguardrail.io](mailto:support@getguardrail.io)
- 🐛 **Issues:** [GitHub Issues](https://github.com/Guardrail-Official/guardrail/issues)

### How do I report a bug?

Open an issue at [github.com/Guardrail-Official/guardrail/issues](https://github.com/Guardrail-Official/guardrail/issues) with:
- Guardrail version (`guardrail --version`)
- Node.js version (`node --version`)
- Steps to reproduce
- Expected vs actual behavior

---

**More questions?** Ask in [Discord](https://discord.gg/guardrail)!
