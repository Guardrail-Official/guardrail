# Changelog

All notable changes to Guardrail will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Coming Soon
- Enhanced AI agent testing capabilities
- Expanded compliance framework support
- Performance optimizations for large codebases

---

## [2.0.0] - 2026-01-05

### Added
- **Reality Mode** - Runtime verification with Playwright
- **MockProof Gate** - Block mock/demo data in production
- **AI Agent Testing** - Autonomous app exploration
- **Natural Language CLI** - Plain English commands
- **MCP Integration** - AI IDE tools (Cursor, Windsurf)
- **Compliance Center** - SOC2, HIPAA, GDPR, PCI, NIST, ISO27001
- **Intelligence Suite** - AI-powered code analysis
- **SARIF Output** - GitHub Code Scanning integration

### Changed
- Complete CLI redesign with intuitive commands
- Improved scoring algorithm (0-100 scale)
- Enhanced HTML report generation
- Better error messages and fix suggestions

### Fixed
- False positives in secret detection
- Performance issues with monorepos
- Auth verification edge cases

---

## [1.5.0] - 2025-11-15

### Added
- Contract drift detection (UI ↔ API)
- Dependency vulnerability scanning
- License compliance checking
- Team-based configuration

### Changed
- Faster scan performance (3x improvement)
- Reduced memory usage

---

## [1.0.0] - 2025-08-01

### Added
- Initial release
- `guardrail scan` - Code analysis
- `guardrail gate` - CI enforcement
- Secret detection (25+ patterns)
- Dead route detection
- Auth verification
- Mock data detection
- HTML and JSON reports

---

## Version History

| Version | Release Date | Highlights |
|---------|--------------|------------|
| 2.0.0 | 2026-01-05 | Reality Mode, AI Agent, MCP |
| 1.5.0 | 2025-11-15 | Contract drift, faster scans |
| 1.0.0 | 2025-08-01 | Initial release |

---

## Upgrade Guide

### From 1.x to 2.x

```bash
# Update to latest
npm install -D guardrail@latest

# New command structure
guardrail scan          # Was: guardrail check
guardrail gate          # Was: guardrail ci
guardrail fix --plan    # New!
```

### Configuration Changes

```json
{
  "version": "2.0.0",
  "checks": ["integrity", "security", "hygiene"],
  "policy": {
    "failOn": ["critical", "high"]
  }
}
```

---

[View Full Documentation →](https://getguardrail.io/docs/changelog)
