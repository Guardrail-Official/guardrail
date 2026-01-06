# Security Policy

## Supported Versions

We release patches for security vulnerabilities in the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 2.x.x   | ✅ Yes             |
| 1.x.x   | ⚠️ Critical only   |
| < 1.0   | ❌ No              |

## Reporting a Vulnerability

We take security seriously at Guardrail. If you discover a security vulnerability, please report it responsibly.

### How to Report

**DO NOT** create a public GitHub issue for security vulnerabilities.

Instead, please report security issues via:

📧 **security@getguardrail.io**

Or use GitHub's private vulnerability reporting:
https://github.com/Guardrail-Official/guardrail/security/advisories/new

### What to Include

Please include the following information:

- **Description** of the vulnerability
- **Steps to reproduce** the issue
- **Potential impact** of the vulnerability
- **Suggested fix** (if you have one)
- **Your contact information** for follow-up

### What to Expect

1. **Acknowledgment:** We'll acknowledge your report within 48 hours
2. **Assessment:** We'll assess the vulnerability and determine severity
3. **Updates:** We'll keep you informed of our progress
4. **Fix:** We'll develop and test a fix
5. **Disclosure:** We'll coordinate disclosure with you

### Disclosure Policy

- We follow responsible disclosure practices
- We aim to fix critical vulnerabilities within 7 days
- We'll credit reporters (unless they prefer anonymity)
- We'll publish security advisories for significant issues

## Security Best Practices

When using Guardrail:

### API Keys & Secrets

```bash
# ✅ Use environment variables
export GUARDRAIL_API_KEY="your-key"

# ❌ Don't hardcode keys
guardrail scan --api-key "your-key"  # Bad!
```

### Configuration

```json
{
  "version": "2.0.0",
  "policy": {
    "failOn": ["critical", "high"],
    "allowlist": {
      "paths": ["tests/**", "*.test.*"]
    }
  }
}
```

### CI/CD Integration

```yaml
# Use secrets for API keys
- name: Guardrail Gate
  run: npx guardrail gate
  env:
    GUARDRAIL_API_KEY: ${{ secrets.GUARDRAIL_API_KEY }}
```

## Security Features

Guardrail itself helps you find security issues:

| Feature | Description |
|---------|-------------|
| **Secret Detection** | Finds API keys, tokens, passwords in code |
| **Dependency Scanning** | CVE detection in dependencies |
| **License Compliance** | Identifies problematic licenses |
| **Auth Verification** | Ensures endpoints have proper auth |

## Bug Bounty

We don't currently have a formal bug bounty program, but we do recognize and thank security researchers who help us improve.

## Contact

- 🔒 Security issues: security@getguardrail.io
- 💬 General questions: support@getguardrail.io
- 🌐 Website: https://getguardrail.io

---

Thank you for helping keep Guardrail and our users safe! 🛡️
