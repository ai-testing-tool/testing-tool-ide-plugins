# Install: GitHub Copilot

Install the canonical `testing-tool-mcp` skill for GitHub Copilot (VS Code / Copilot agents). Skills teach **agent behavior**; MCP is separate.

## Project skill (recommended)

From a clone of https://github.com/ai-testing-tool/testing-tool-ide-plugins:

```bash
mkdir -p /path/to/your-project/.github/skills
ln -sfn /absolute/path/to/testing-tool-ide-plugins/skills/testing-tool-mcp \
  /path/to/your-project/.github/skills/testing-tool-mcp
# Or: cp -R skills/testing-tool-mcp /path/to/your-project/.github/skills/
```

Copilot may also discover skills under `.claude/skills/` or `.agents/skills/` depending on the client—prefer `.github/skills/` for Copilot-first repos. Always use the **same** canonical skill body (no divergent fork).

## MCP (required)

1. Connect [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) in VS Code / Copilot (e.g. Atlassian MCP extension or `https://mcp.atlassian.com/v2/mcp`).
2. Authenticate when prompted.
3. AI Testing Tool **Settings → Active** for GitHub Copilot does **not** register MCP.

## Example prompt

`Using AI Testing Tool / Atlassian MCP, summarize recent test reports for this project if tools are available. Do not invent metrics.`
