# Install: Claude

Install the canonical `testing-tool-mcp` skill for Claude. Skills teach **agent behavior**; MCP is separate.

## Project skill (recommended)

From a clone of https://github.com/ai-testing-tool/testing-tool-ide-plugins:

```bash
mkdir -p /path/to/your-project/.claude/skills
ln -sfn /absolute/path/to/testing-tool-ide-plugins/skills/testing-tool-mcp \
  /path/to/your-project/.claude/skills/testing-tool-mcp
# Or: cp -R skills/testing-tool-mcp /path/to/your-project/.claude/skills/
```

Use the **same** `skills/testing-tool-mcp` content as Cursor and Copilot—do not maintain a divergent fork.

## Personal / upload path

If your Claude client supports uploading skills, zip or select the `skills/testing-tool-mcp` folder (must include `SKILL.md`) and enable it in Claude’s skills settings.

## MCP (required)

1. Connect [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) (`https://mcp.atlassian.com/v2/mcp`). Example for Claude Code:  
   `claude mcp add --transport http atlassian https://mcp.atlassian.com/v2/mcp`  
   then authenticate with `/mcp`.
2. AI Testing Tool **Settings → Active** for Claude does **not** register MCP.

## Example prompt

`Using AI Testing Tool / Atlassian MCP, draft TestCase proposals for issue KEY-123. Confirm with me before creating anything.`
