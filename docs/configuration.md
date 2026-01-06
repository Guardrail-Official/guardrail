# Configuration

Customize Guardrail behavior with a configuration file.

## Config File

Create `guardrail.config.json` in your project root:

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene"],
  "output": ".guardrail",
  "policy": {
    "failOn": ["critical", "high"],
    "allowlist": {
      "domains": [],
      "packages": [],
      "paths": []
    },
    "ignore": {
      "paths": [],
      "rules": []
    }
  }
}
```

## Configuration Options

### `version`

Config file version. Use `"2.0.0"` for latest.

```json
{
  "version": "2.0.0"
}
```

### `checks`

Which check modules to run.

```json
{
  "checks": ["integrity", "security", "hygiene", "contracts", "mocks"]
}
```

| Module | Description |
|--------|-------------|
| `integrity` | API wiring, routes, auth presence |
| `security` | Secrets, SBOM, vulnerabilities |
| `hygiene` | Duplicates, unused files, lint |
| `contracts` | UI/API matching |
| `mocks` | Mock/test code leakage |

### `output`

Output directory for reports.

```json
{
  "output": ".guardrail"
}
```

### `policy`

Enforcement policy settings.

```json
{
  "policy": {
    "failOn": ["critical", "high"],
    "warnOn": ["medium"],
    "threshold": 80
  }
}
```

### `allowlist`

Exclude from checks.

```json
{
  "policy": {
    "allowlist": {
      "domains": ["api.stripe.com", "api.github.com"],
      "packages": ["@internal/mock-utils"],
      "paths": ["src/testing/**", "**/*.test.*"]
    }
  }
}
```

### `ignore`

Skip specific paths or rules.

```json
{
  "policy": {
    "ignore": {
      "paths": ["node_modules", "__tests__", "dist"],
      "rules": ["no-console", "mock-import"]
    }
  }
}
```

## Profiles

Define reusable check profiles.

```json
{
  "profiles": {
    "quick": {
      "checks": ["integrity"],
      "policy": { "failOn": ["critical"] }
    },
    "full": {
      "checks": ["integrity", "security", "hygiene", "contracts"],
      "policy": { "failOn": ["critical", "high"] }
    },
    "ship": {
      "checks": ["integrity", "security", "hygiene", "contracts", "mocks"],
      "policy": { "failOn": ["critical", "high", "medium"] }
    }
  }
}
```

**Usage:**

```bash
guardrail scan --profile=quick
guardrail scan --profile=ship
```

## Environment-Specific Config

Use different configs per environment:

```
guardrail.config.json        # Default
guardrail.config.ci.json     # CI environment
guardrail.config.local.json  # Local development
```

```bash
guardrail scan --config=guardrail.config.ci.json
```

## Full Example

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene", "contracts"],
  "output": ".guardrail",
  
  "policy": {
    "failOn": ["critical", "high"],
    "warnOn": ["medium"],
    "threshold": 80,
    
    "allowlist": {
      "domains": [
        "api.stripe.com",
        "api.github.com",
        "*.amazonaws.com"
      ],
      "packages": [],
      "paths": [
        "src/testing/**",
        "**/*.test.*",
        "**/*.spec.*",
        "e2e/**"
      ]
    },
    
    "ignore": {
      "paths": [
        "node_modules",
        "dist",
        "build",
        ".next",
        "coverage"
      ],
      "rules": []
    }
  },
  
  "profiles": {
    "quick": {
      "checks": ["integrity"],
      "policy": { "threshold": 70 }
    },
    "ship": {
      "checks": ["integrity", "security", "hygiene", "contracts", "mocks"],
      "policy": { "threshold": 90 }
    }
  },
  
  "reality": {
    "baseUrl": "http://localhost:3000",
    "flows": ["auth", "checkout"],
    "timeout": 30000
  }
}
```

## Validation

Validate your config:

```bash
guardrail doctor
```

---

**Full documentation:** [getguardrail.io/docs/config](https://getguardrail.io/docs/config)
