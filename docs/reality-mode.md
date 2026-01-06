# Reality Mode

Runtime verification that catches what static analysis can't.

## Overview

Reality Mode uses Playwright to actually run your application and verify it works as expected. It catches:

- **Fake Success** - UI shows success but no API call was made
- **Mock Backends** - Requests hitting fake/mock servers
- **No-Wire UI** - Forms that don't actually submit
- **Auth Mirage** - Protected routes accessible without login
- **Schema Drift** - API responses that don't match expectations

## Quick Start

```bash
# Install Playwright (first time only)
npx playwright install chromium

# Run Reality Mode
npx guardrail proof reality --url http://localhost:3000
```

## Commands

### Mock Detection

```bash
npx guardrail proof mocks
```

Scans your codebase for:
- Mock imports in production code
- Fixture/demo data files
- Fake API domains (e.g., `jsonplaceholder.typicode.com`)
- Placeholder endpoints

### Runtime Verification

```bash
npx guardrail proof reality --url <your-app-url>
```

Options:
```bash
--url <url>         App URL to test (required)
--flow <name>       Test specific flow (auth, checkout, etc.)
--video             Record video of test run
--trace             Generate Playwright trace
--threshold <n>     Minimum score to pass (0-100)
--timeout <ms>      Test timeout (default: 30000)
```

## What Gets Tested

### 1. Network Traffic Analysis

Reality Mode intercepts all network requests and checks:

| Check | Description |
|-------|-------------|
| Mock Domains | Requests to known mock APIs |
| Localhost Calls | Production hitting localhost |
| Demo Patterns | `demo_`, `test_`, `fake_` in requests |
| Write Operations | Forms that don't trigger POSTs |

### 2. UI Interaction

Reality Mode clicks buttons, fills forms, and verifies:

| Check | Description |
|-------|-------------|
| Form Submissions | Forms actually send data |
| Button Actions | Buttons trigger real operations |
| Navigation | Routes load correctly |
| Error States | Proper error handling |

### 3. Authentication

```bash
npx guardrail proof reality --url http://localhost:3000 --flow auth
```

Tests:
- Login forms actually authenticate
- Protected routes redirect when not logged in
- Logout actually clears session
- Session persists across navigation

## Scoring

Reality Mode produces a score from 0-100:

| Component | Weight | Description |
|-----------|--------|-------------|
| Coverage | 40% | Routes visited, elements tested |
| Functionality | 35% | Actions that worked correctly |
| Stability | 15% | Errors encountered |
| UX | 10% | Response to interactions |

### Score Interpretation

| Score | Status | Meaning |
|-------|--------|---------|
| 80-100 | 🟢 GO | App is real, ship it |
| 60-79 | 🟡 WARN | Some issues, review needed |
| 0-59 | 🔴 NO-GO | Significant problems |

## Flow Packs

Pre-built test flows for common scenarios:

```bash
# Authentication flow
npx guardrail proof reality --url http://localhost:3000 --flow auth

# E-commerce checkout
npx guardrail proof reality --url http://localhost:3000 --flow checkout

# Form validation
npx guardrail proof reality --url http://localhost:3000 --flow forms
```

### Available Flows

| Flow | Tests |
|------|-------|
| `auth` | Login, signup, logout, session |
| `checkout` | Cart, payment, order confirmation |
| `forms` | Validation, submission, errors |
| `navigation` | Routes, links, back/forward |
| `ui` | Modals, dropdowns, dark mode |

## CI Integration

```yaml
# .github/workflows/reality-mode.yml
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
      - run: sleep 5
      
      - name: Install Playwright
        run: npx playwright install chromium
      
      - name: Reality Mode
        run: npx guardrail proof reality --url http://localhost:3000 --threshold 80
      
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: reality-report
          path: .guardrail/reality/
```

## Output Files

Reality Mode generates:

| File | Description |
|------|-------------|
| `reality-report.html` | Visual HTML report |
| `reality-results.json` | Machine-readable results |
| `videos/` | Screen recordings (with `--video`) |
| `trace.zip` | Playwright trace (with `--trace`) |

## Debugging Failed Tests

### View the Video

```bash
npx guardrail proof reality --url http://localhost:3000 --video
# Check .guardrail/reality/videos/
```

### View the Trace

```bash
npx guardrail proof reality --url http://localhost:3000 --trace
npx playwright show-trace .guardrail/reality/trace.zip
```

### Check the Report

Open `.guardrail/reality/reality-report.html` in a browser for detailed findings.

---

**Full documentation:** [getguardrail.io/docs/reality-mode](https://getguardrail.io/docs/reality-mode)
