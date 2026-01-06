# Troubleshooting

Common issues and solutions for Guardrail.

## Installation Issues

### `command not found: guardrail`

**Solution:** Use npx or install globally:

```bash
# Use npx (recommended)
npx guardrail scan

# Or install globally
npm install -g guardrail
guardrail scan
```

### `EACCES: permission denied`

**Solution:** Fix npm permissions:

```bash
# Option 1: Use a node version manager (recommended)
# Install nvm, then:
nvm install 20
nvm use 20

# Option 2: Change npm's default directory
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
# Add to ~/.bashrc or ~/.zshrc:
export PATH=~/.npm-global/bin:$PATH
```

### `Unsupported Node.js version`

**Solution:** Update Node.js to v20.11+:

```bash
# Check version
node --version

# Update with nvm
nvm install 20
nvm use 20
```

---

## Scan Issues

### Scan is slow

**Causes:**
- Large codebase
- Scanning node_modules
- Full profile when quick would suffice

**Solutions:**

```bash
# Use quick profile
npx guardrail scan --profile=quick

# Ignore large directories
# In guardrail.config.json:
{
  "policy": {
    "ignore": {
      "paths": ["node_modules", "dist", ".next", "build"]
    }
  }
}
```

### Too many false positives

**Solution:** Add legitimate files to allowlist:

```json
{
  "policy": {
    "allowlist": {
      "paths": ["src/testing/**", "**/*.test.*", "e2e/**"],
      "domains": ["api.stripe.com"]
    }
  }
}
```

### Scan finds nothing

**Possible causes:**
- Not in project root
- No supported files found
- Everything is ignored

**Solutions:**

```bash
# Verify you're in the right directory
pwd
ls package.json

# Check config
npx guardrail doctor

# Run verbose to see what's happening
npx guardrail scan --verbose
```

---

## Gate Issues

### Gate fails unexpectedly

**Debug steps:**

```bash
# Run scan first to see details
npx guardrail scan --format=json

# Check the report
cat .guardrail/summary.json

# Lower threshold temporarily
npx guardrail gate --threshold=50
```

### Exit code 2 (configuration error)

**Solution:** Validate your config:

```bash
npx guardrail doctor
```

Common config issues:
- Invalid JSON syntax
- Unknown check names
- Invalid paths

---

## Reality Mode Issues

### `Playwright not found`

**Solution:** Install Playwright:

```bash
npx playwright install chromium
```

### Reality Mode times out

**Solutions:**

```bash
# Increase timeout
npx guardrail proof reality --url http://localhost:3000 --timeout 60000

# Make sure app is running
curl http://localhost:3000
```

### App not accessible

**Checklist:**
1. Is the app running? `curl http://localhost:3000`
2. Correct port?
3. Firewall blocking?
4. In CI, wait for app to start:

```yaml
- run: npm start &
- run: sleep 10  # Wait for app
- run: npx guardrail proof reality --url http://localhost:3000
```

---

## MCP Issues

### MCP server won't start

**Debug:**

```bash
# Test manually
npx guardrail mcp

# Check for port conflicts
lsof -i :3847
```

### AI doesn't see Guardrail tools

**Solutions:**
1. Restart your IDE
2. Verify config file location and syntax
3. Check IDE's MCP logs

---

## CI Issues

### Works locally, fails in CI

**Common causes:**
- Different Node.js versions
- Missing dependencies
- Different file paths

**Solutions:**

```yaml
# Pin Node.js version
- uses: actions/setup-node@v4
  with:
    node-version: '20.11.0'

# Clean install
- run: npm ci  # Not npm install
```

### SARIF upload fails

**Solution:** Check permissions:

```yaml
permissions:
  contents: read
  security-events: write
```

---

## Output Issues

### Can't find report files

**Default location:** `.guardrail/`

```bash
ls -la .guardrail/
```

**Custom output:**

```bash
npx guardrail scan --output=./my-reports
```

### Report is empty

**Possible causes:**
- No issues found (good!)
- Wrong working directory
- Files ignored by config

**Debug:**

```bash
npx guardrail scan --verbose
```

---

## Getting Help

### Diagnostic Command

```bash
npx guardrail doctor
```

This checks:
- Node.js version
- Guardrail version
- Config file validity
- Git repository
- Required dependencies

### Debug Mode

```bash
DEBUG=guardrail* npx guardrail scan
```

### Support Channels

- 💬 **Discord:** [discord.gg/guardrail](https://discord.gg/guardrail) (fastest)
- 📧 **Email:** [support@getguardrail.io](mailto:support@getguardrail.io)
- 🐛 **GitHub:** [Report an issue](https://github.com/Guardrail-Official/guardrail/issues)

### Bug Report Template

When reporting issues, include:

```
**Guardrail version:** (guardrail --version)
**Node.js version:** (node --version)
**OS:** (macOS/Linux/Windows)
**Package manager:** (npm/pnpm/yarn)

**Steps to reproduce:**
1. 
2. 
3. 

**Expected behavior:**

**Actual behavior:**

**Error output:**
```

---

**Still stuck?** Join [Discord](https://discord.gg/guardrail) for real-time help!
