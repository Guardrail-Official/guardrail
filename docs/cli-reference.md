# CLI Reference

Complete reference for all Guardrail CLI commands.

## Global Options

```bash
guardrail [command] [options]

Options:
  --version, -v     Show version number
  --help, -h        Show help
  --verbose         Detailed output
  --quiet, -q       Minimal output
  --config <path>   Custom config file path
  --format <type>   Output format (json, html, sarif, md)
```

---

## Commands

### `guardrail scan`

Run checks and produce artifacts.

```bash
guardrail scan [options]

Options:
  --profile <name>    Check profile (quick, full, ship)
  --only <checks>     Run specific checks only
  --format <types>    Output formats (comma-separated)
  --output <dir>      Output directory (default: .guardrail)
  --fix               Show fix suggestions
```

**Examples:**

```bash
# Quick scan
guardrail scan

# Full scan with HTML report
guardrail scan --profile=full --format=html

# Security checks only
guardrail scan --only=security

# Multiple output formats
guardrail scan --format=json,html,sarif
```

**Check Profiles:**

| Profile | Checks | Use Case |
|---------|--------|----------|
| `quick` | integrity | Fast CI checks |
| `full` | integrity, security, hygiene | Thorough analysis |
| `ship` | all checks + mocks | Pre-deployment |

---

### `guardrail gate`

Enforce policy in CI - exit with appropriate code.

```bash
guardrail gate [options]

Options:
  --policy <name>     Policy to apply (default, strict, lenient)
  --sarif             Generate SARIF for GitHub Code Scanning
  --threshold <n>     Minimum score to pass (0-100)
  --allow-warnings    Don't fail on warnings
```

**Exit Codes:**

| Code | Status | Meaning |
|------|--------|---------|
| 0 | ✅ SHIP | All checks passed |
| 1 | 🚫 NO-SHIP | Policy violations |
| 2 | ⚠️ ERROR | Configuration error |

**Examples:**

```bash
# Default policy
guardrail gate

# Strict policy with SARIF
guardrail gate --policy=strict --sarif

# Custom threshold
guardrail gate --threshold=80
```

---

### `guardrail fix`

Preview and apply safe patches.

```bash
guardrail fix [options]

Options:
  --plan              Preview recommended fixes (dry run)
  --apply             Apply safe fixes
  --scope <type>      Fix specific issue types
  --risk <level>      Risk tolerance (safe, moderate, aggressive)
  --interactive, -i   Interactive mode
```

**Examples:**

```bash
# Preview fixes
guardrail fix --plan

# Apply safe fixes
guardrail fix --apply

# Fix only secrets
guardrail fix --apply --scope=secrets

# Interactive mode
guardrail fix -i
```

---

### `guardrail proof`

Runtime verification (Reality Mode).

```bash
guardrail proof <subcommand> [options]

Subcommands:
  mocks               Detect mock data in production
  reality             Full runtime verification
```

**Examples:**

```bash
# Detect mocks
guardrail proof mocks

# Runtime verification
guardrail proof reality --url http://localhost:3000

# With video recording
guardrail proof reality --url http://localhost:3000 --video

# Test specific flow
guardrail proof reality --url http://localhost:3000 --flow checkout
```

---

### `guardrail mcp`

Start MCP server for AI IDEs.

```bash
guardrail mcp [options]

Options:
  --port <n>          Server port (default: auto)
  --stdio             Use stdio transport
```

**IDE Configuration:**

```json
{
  "mcpServers": {
    "guardrail": {
      "command": "npx",
      "args": ["guardrail", "mcp"]
    }
  }
}
```

---

### `guardrail init`

Initialize Guardrail in a project.

```bash
guardrail init [options]

Options:
  --template <name>   Project template
  --ci <platform>     Generate CI config (github, gitlab, circleci)
```

**Examples:**

```bash
# Interactive setup
guardrail init

# With GitHub Actions
guardrail init --ci=github
```

---

### `guardrail doctor`

Diagnose installation and configuration issues.

```bash
guardrail doctor
```

**Output:**

```
🩺 Guardrail Doctor
━━━━━━━━━━━━━━━━━━━

✓ Node.js: v20.11.0
✓ Guardrail: v2.0.0
✓ Config: guardrail.config.json found
✓ Git: repository detected
⚠ Playwright: not installed (needed for Reality Mode)

Recommendations:
  npm install -D playwright
```

---

## Natural Language

Guardrail understands plain English:

```bash
guardrail "what's my status"
guardrail "run ai agent"
guardrail "block demo patterns"
guardrail "generate ship badge"
guardrail "launch checklist"
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `GUARDRAIL_API_KEY` | API key for premium features |
| `GUARDRAIL_CONFIG` | Custom config file path |
| `GUARDRAIL_OUTPUT` | Output directory |
| `CI` | Detected automatically in CI |

---

**Full documentation:** [getguardrail.io/docs/cli](https://getguardrail.io/docs/cli)
