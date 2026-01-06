<div align="center">

# 🛡️ Guardrail

### CI Truth for AI-Generated Code

**Prove your app is real — before you ship.**

[![npm version](https://img.shields.io/npm/v/guardrail?style=flat-square&color=cb3837&logo=npm)](https://npmjs.com/package/guardrail)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Node](https://img.shields.io/badge/node-%3E%3D20.11-brightgreen?style=flat-square&logo=node.js)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Discord](https://img.shields.io/badge/Discord-Join%20Us-7289da?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/guardrail)

[🌐 Website](https://getguardrail.io) · [📖 Documentation](https://getguardrail.io/docs) · [💬 Discord](https://discord.gg/guardrail) · [𝕏 Twitter](https://twitter.com/getguardrail)

</div>

---

## 🎯 The Problem

AI-assisted coding creates **convincing wrongness**: dead routes, fake data, stub APIs, missing auth, leaked secrets, UI/API drift. Your tests pass. Your CI is green. But your app isn't real.

**Guardrail catches that and blocks it in CI.**

<div align="center">

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   🔴 BEFORE: "It looks like it works!"                          │
│   ───────────────────────────────────                           │
│   ✓ Tests pass                                                  │
│   ✓ CI green                                                    │
│   ✗ Routes return 404                                           │
│   ✗ Auth bypassed                                               │
│   ✗ API keys in code                                            │
│   ✗ Mock data in production                                     │
│                                                                 │
│   🟢 AFTER: "It actually works!"                                │
│   ───────────────────────────────                               │
│   ✓ Tests pass                                                  │
│   ✓ CI green                                                    │
│   ✓ Guardrail SHIP                                              │
│   ✓ Routes verified                                             │
│   ✓ Auth enforced                                               │
│   ✓ Secrets safe                                                │
│   ✓ Real data flows                                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

</div>

---

## ✨ What Guardrail Catches

| Check | Description |
|-------|-------------|
| 🚫 **Dead Routes** | Placeholder handlers and 404s caught before merge |
| 🔐 **Missing Auth** | Sensitive endpoints require authentication + RBAC |
| 🔑 **Leaked Secrets** | API keys, tokens, passwords detected and blocked |
| 🎭 **Mock Data** | Fake domains, fixtures, demo data caught in prod builds |
| 🔄 **Contract Drift** | UI ↔ API endpoint mismatch detection |
| 🚦 **CI Gating** | Policy-driven merge blocking |

---

## 🚀 Quick Start

```bash
# Install
npm install -D guardrail

# Scan your codebase
npx guardrail scan

# Block merges on real issues
npx guardrail gate

# Auto-fix common problems
npx guardrail fix --plan
```

**That's it!** Guardrail starts protecting your code immediately.

---

## 🔬 Reality Mode

Runtime verification that catches what static analysis can't.

```bash
# Detect mock data at runtime
npx guardrail proof mocks

# Full runtime verification
npx guardrail proof reality --url http://localhost:3000
```

**Catches:**
- "Looks real" UI using fake data at runtime
- Network requests hitting mock services
- Runtime UI/API mismatches

---

## 🤖 AI IDE Integration (MCP)

Expose Guardrail as tools in AI agent workflows (Cursor, Windsurf, etc.)

```bash
npx guardrail mcp
```

**Available Tools:**
- `guardrail.scan` — Run checks
- `guardrail.gate` — CI enforcement
- `guardrail.fix` — Apply patches
- `guardrail.proof` — Runtime verification

---

## ⚙️ CI/CD Integration

### GitHub Actions

```yaml
name: Guardrail Check

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
      - run: npx guardrail gate --sarif
      - uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: .guardrail/results.sarif
```

---

## 🔒 Privacy & Security

| Feature | Status |
|---------|--------|
| **Local-First** | ✅ All scans run locally |
| **No Telemetry** | ✅ No code sent to external servers |
| **SARIF Export** | ✅ Opt-in only |
| **SOC2/HIPAA Ready** | ✅ Compliance tier available |

---

## 💰 Pricing

| Tier | Price | Includes |
|------|-------|----------|
| **Free** | $0 | `scan`, `gate`, basic reports |
| **Starter** | $29/mo | + contract checks, mock detection |
| **Pro** | $99/mo | + `fix` workflows, proof modes, advanced reports |
| **Compliance** | $199/mo | + SOC2, HIPAA, GDPR, PCI, NIST, ISO27001 |
| **Enterprise** | Custom | + SSO, on-prem, custom policies, dedicated support |

[View Full Pricing →](https://getguardrail.io/pricing)

---

## 📚 Resources

| Resource | Link |
|----------|------|
| 🌐 Website | [getguardrail.io](https://getguardrail.io) |
| 📖 Documentation | [getguardrail.io/docs](https://getguardrail.io/docs) |
| 💬 Discord | [discord.gg/guardrail](https://discord.gg/guardrail) |
| 𝕏 Twitter | [@getguardrail](https://twitter.com/getguardrail) |
| 📧 Support | [support@getguardrail.io](mailto:support@getguardrail.io) |

---

## 📄 License

Guardrail CLI is available under the [MIT License](LICENSE).

---

<div align="center">

**🛡️ Guardrail — CI Truth for AI-Generated Code**

[Website](https://getguardrail.io) · [Docs](https://getguardrail.io/docs) · [Discord](https://discord.gg/guardrail) · [Twitter](https://twitter.com/getguardrail)

Made with ❤️ for developers who want AI that actually works.

</div>
