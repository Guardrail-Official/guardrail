# Guardrail Examples

Example configurations and integrations for Guardrail.

## Quick Start Examples

| Example | Description |
|---------|-------------|
| [Basic Config](./basic-config/) | Simple guardrail.config.json |
| [GitHub Actions](./github-actions/) | CI/CD workflow examples |
| [Next.js](./nextjs/) | Next.js project setup |
| [Express](./express/) | Express API setup |

## Basic Configuration

### Minimal Config

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security"]
}
```

### Full Config

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene", "contracts", "mocks"],
  "output": ".guardrail",
  "policy": {
    "failOn": ["critical", "high"],
    "threshold": 80,
    "allowlist": {
      "domains": ["api.stripe.com"],
      "paths": ["tests/**", "*.test.*"]
    },
    "ignore": {
      "paths": ["node_modules", "dist"]
    }
  }
}
```

## GitHub Actions Examples

### Basic CI

```yaml
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

### With SARIF

```yaml
name: Guardrail + Security
on: [push, pull_request]
jobs:
  guardrail:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
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

### With Reality Mode

```yaml
name: Reality Mode
on: [push]
jobs:
  reality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
      - run: npm start &
      - run: sleep 10
      - run: npx playwright install chromium
      - run: npx guardrail proof reality --url http://localhost:3000
```

## Framework Examples

### Next.js

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene"],
  "policy": {
    "ignore": {
      "paths": [".next", "node_modules", "out"]
    }
  }
}
```

### Express API

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security"],
  "policy": {
    "failOn": ["critical", "high"],
    "allowlist": {
      "paths": ["tests/**", "__mocks__/**"]
    }
  }
}
```

### Monorepo (Turborepo)

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene"],
  "policy": {
    "ignore": {
      "paths": [
        "node_modules",
        "**/dist",
        "**/.turbo",
        "**/coverage"
      ]
    }
  }
}
```

## MCP Config Examples

### Cursor

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

### With API Key

```json
{
  "mcpServers": {
    "guardrail": {
      "command": "npx",
      "args": ["guardrail", "mcp"],
      "env": {
        "GUARDRAIL_API_KEY": "your-key"
      }
    }
  }
}
```

---

**Full documentation:** [getguardrail.io/docs](https://getguardrail.io/docs)
