# Install: Cursor

Install **Testing Tool IDE Plugins** so Cursor can load the canonical `testing-tool-mcp` skill. The plugin does **not** register MCP—connect [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) separately.

## Local plugin install (recommended for development)

From a clone of https://github.com/ai-testing-tool/testing-tool-ide-plugins:

```bash
mkdir -p ~/.cursor/plugins/local
ln -sfn "$(pwd)" ~/.cursor/plugins/local/testing-tool-ide-plugins
```

Then reload Cursor (or restart) so local plugins are picked up.

Confirm the skill is available (e.g. invoke `/testing-tool-mcp` or ask about TestCases / Testing tool MCP).

## Alternative: project skill only

Without the plugin, copy or symlink the skill folder:

```bash
mkdir -p .cursor/skills
ln -sfn /absolute/path/to/testing-tool-ide-plugins/skills/testing-tool-mcp .cursor/skills/testing-tool-mcp
```

## MCP (required for TestCase tools)

1. Add Atlassian Rovo MCP in Cursor — see [get started](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) (`https://mcp.atlassian.com/v2/mcp`).
2. Authenticate when prompted.
3. Remember: AI Testing Tool **Settings → Active** does not install MCP.

## Smoke check

| Step | Expected |
| ---- | -------- |
| Plugin or project skill installed | `testing-tool-mcp` discoverable |
| Prompt about TestCases / Testing tool MCP | Skill guidance applies |
| Atlassian MCP connected | Tools listable; plugin alone is not enough for writes |

Do not duplicate `SKILL.md` under the plugin—`.cursor-plugin/plugin.json` points `skills` at `./skills/`.
