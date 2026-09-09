# Testing Tool IDE Plugins

Portable **Agent Skills** (and Cursor plugin packaging) so Cursor, Claude, and GitHub Copilot agents manage **AI Testing Tool** TestCases and reports through the **[Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/)**, with confirm-before-write.

**Repository:** https://github.com/ai-testing-tool/testing-tool-ide-plugins

## Prerequisites (MCP)

1. **Connect Atlassian Rovo MCP** in your IDE (Cursor, Claude, or Copilot).  
   - Guide: [Get started with the Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/)  
   - Server URL (v2): `https://mcp.atlassian.com/v2/mcp`
2. Complete Atlassian authentication (OAuth) when the client prompts you.
3. Confirm tools are callable (list tools / try a read). Prefer any **AI Testing Tool**–specific tools when they appear; otherwise use available Atlassian tools carefully (see the skill).
4. AI Testing Tool should be installed on your Jira site for TestCase workflows to make sense.

**Settings → Active is not MCP registration.** Turning Cursor, Claude, or Copilot **Active** in AI Testing Tool Settings is a preference only. It does not install or authenticate [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/).

This pack teaches **agent behavior**. Human MCP client setup is covered by Atlassian’s guide and AI Testing Tool’s IDE user-guide pages.

## Install matrix

| Tool | How to install this pack | MCP |
| ---- | ------------------------ | --- |
| **Cursor** | Local plugin symlink (see below) or copy `skills/testing-tool-mcp` into `.cursor/skills/`. | [Add Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) (Cursor marketplace / MCP settings) |
| **Claude** | See [docs/install/claude.md](docs/install/claude.md) (`.claude/skills/`). | Same Atlassian Rovo MCP URL / Claude MCP setup |
| **GitHub Copilot** | See [docs/install/github-copilot.md](docs/install/github-copilot.md) (`.github/skills/`). | Same Atlassian Rovo MCP (e.g. VS Code Atlassian MCP / Copilot MCP) |

### Manual skill install (all tools)

```bash
# From a clone of this repo — example for Copilot project skills:
cp -R skills/testing-tool-mcp /path/to/your-project/.github/skills/testing-tool-mcp

# Claude project skills:
cp -R skills/testing-tool-mcp /path/to/your-project/.claude/skills/testing-tool-mcp

# Cursor project skills:
cp -R skills/testing-tool-mcp /path/to/your-project/.cursor/skills/testing-tool-mcp
```

Keep a **single** canonical copy from this repo; do not fork divergent instruction bodies.

### Cursor plugin

Install the Cursor plugin so skills load from this pack’s `skills/` folder (no forked copies).

**Local install:**

```bash
# From a clone of this repository:
mkdir -p ~/.cursor/plugins/local
ln -sfn "$(pwd)" ~/.cursor/plugins/local/testing-tool-ide-plugins
```

Reload Cursor. Details: [docs/install/cursor.md](docs/install/cursor.md).

The plugin does **not** register MCP—still add [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/).

Marketplace / `/add-plugin` publish can follow later; local symlink is the supported MVP path.

## Example prompts

After the skill is loaded and Atlassian Rovo MCP is connected:

1. **Create TestCase from an issue:**  
   `Using AI Testing Tool / Atlassian MCP, draft TestCase proposals for issue KEY-123 in this project. Confirm with me before creating anything.`

2. **Summarize reports:**  
   `Using available MCP tools, summarize recent test reports or plan status for this project if AI Testing Tool tools are present.`

3. **Update design (after confirm):**  
   `Load TestCase KEY-456 and propose missing steps. Wait for my confirmation before updating.`

## In Jira vs IDE

- **Inside Jira chat:** use the **AI Testing Tool** Rovo agent (product chat), not this IDE skill alone.
- **In the IDE:** use this skill + [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/).

## Verification

See [docs/verification.md](docs/verification.md) for the smoke checklist (MCP connected and disconnected).

## Docs

- [Product requirements](docs/prd.md)
- [Architecture](docs/architecture.md)

## License

MIT — see [LICENSE](LICENSE).
