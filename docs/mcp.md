# MCP Integration

Integrate Guardrail with AI IDEs like Cursor and Windsurf using the Model Context Protocol (MCP).

## What is MCP?

MCP (Model Context Protocol) allows AI assistants in your IDE to use Guardrail tools directly. Ask your AI to "scan the code" or "check for security issues" and it will use Guardrail automatically.

## Setup

### Cursor

Add to your Cursor settings (`.cursor/mcp.json`):

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

### Windsurf

Add to your Windsurf MCP config:

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

### Claude Desktop

Add to Claude's MCP configuration:

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

## Available Tools

Once configured, your AI assistant has access to these tools:

| Tool | Description |
|------|-------------|
| `guardrail.scan` | Run code analysis |
| `guardrail.gate` | Check if code passes policy |
| `guardrail.fix` | Get fix suggestions |
| `guardrail.proof` | Runtime verification |
| `guardrail.status` | Check project health |
| `guardrail.context` | Get codebase context |

## Usage Examples

Just ask your AI naturally:

```
"Scan this project for security issues"
"Does this code pass the guardrail gate?"
"Find any mock data that might leak to production"
"Check if all auth endpoints are protected"
"Generate a fix for this security issue"
```

## How It Works

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   AI Assistant  │────▶│  MCP Protocol   │────▶│   Guardrail     │
│  (Cursor, etc)  │◀────│                 │◀────│   CLI Tools     │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

1. You ask the AI to check your code
2. AI recognizes it needs Guardrail
3. AI calls Guardrail via MCP
4. Guardrail runs the check locally
5. Results returned to AI
6. AI explains findings and suggests fixes

## Best Practices

### 1. Let AI Run Checks Before Commits

Ask: "Before I commit, are there any issues Guardrail would catch?"

### 2. Get Contextual Fixes

Ask: "Guardrail found a security issue on line 45. How do I fix it?"

### 3. Understand Scan Results

Ask: "Explain what this Guardrail warning means and why it matters"

### 4. Automate in Your Workflow

Ask: "Set up Guardrail to run on every save" (IDE-dependent)

## Troubleshooting

### MCP Server Not Starting

```bash
# Test manually
npx guardrail mcp

# Check version
npx guardrail --version
```

### Tools Not Available

1. Restart your IDE
2. Check MCP config syntax
3. Ensure Guardrail is installed: `npm install -D guardrail`

### Permission Errors

```bash
# Clear npm cache
npm cache clean --force

# Reinstall
npm install -D guardrail
```

## Advanced Configuration

### Custom Port

```json
{
  "mcpServers": {
    "guardrail": {
      "command": "npx",
      "args": ["guardrail", "mcp", "--port", "3847"]
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
        "GUARDRAIL_API_KEY": "your-api-key"
      }
    }
  }
}
```

---

**Full documentation:** [getguardrail.io/docs/mcp](https://getguardrail.io/docs/mcp)
