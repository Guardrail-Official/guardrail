# Contributing to Guardrail

Thank you for your interest in contributing to Guardrail! We welcome contributions from the community.

## Ways to Contribute

### 🐛 Report Bugs

Found a bug? Please [open an issue](https://github.com/Guardrail-Official/guardrail/issues/new) with:
- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Your environment (OS, Node version, Guardrail version)

### 💡 Suggest Features

Have an idea? We'd love to hear it! [Open a feature request](https://github.com/Guardrail-Official/guardrail/issues/new) with:
- Clear description of the feature
- Use case / problem it solves
- Any implementation ideas

### 📝 Improve Documentation

Documentation improvements are always welcome:
- Fix typos or unclear explanations
- Add examples
- Improve guides

### 🔧 Submit Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit with clear messages (`git commit -m 'Add amazing feature'`)
5. Push to your branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

## Development Setup

```bash
# Install Guardrail CLI
npm install -g guardrail

# Verify installation
guardrail --version

# Run your first scan
guardrail scan
```

## Code Style

- Use TypeScript for type safety
- Follow existing code patterns
- Add tests for new features
- Update documentation as needed

## Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add new mock detection rule
fix: resolve false positive in auth check
docs: update CLI reference
test: add integration tests for gate command
```

## Pull Request Guidelines

- Keep PRs focused and small
- Include tests for new features
- Update documentation if needed
- Link related issues
- Respond to review feedback promptly

## Community

- 💬 [Discord](https://discord.gg/guardrail) - Chat with the team
- 🐦 [Twitter](https://twitter.com/getguardrail) - Updates and tips
- 📧 [Email](mailto:support@getguardrail.io) - Direct support

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for helping make Guardrail better! 🛡️
