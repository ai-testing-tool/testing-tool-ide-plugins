# Verification checklist

Smoke checks for the `testing-tool-mcp` skill before Cursor plugin / adapter work.

## Setup

1. Clone https://github.com/ai-testing-tool/testing-tool-ide-plugins (or use this working tree).
2. Install the skill into at least one tool (example: project `.cursor/skills/testing-tool-mcp` via copy or symlink of `skills/testing-tool-mcp`).
2. Prepare two sessions or modes:
   - **Connected:** [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) added and tools listable (`https://mcp.atlassian.com/v2/mcp`)
   - **Disconnected:** MCP server removed or disabled
3. Note whether any **AI Testing Tool–specific** tools appear in addition to platform Jira/Confluence tools.

## Checklist

| # | Step | Expected | Pass? |
| - | ---- | -------- | ----- |
| 1 | Load skill (slash or auto) for a TestCase / Testing tool MCP prompt | Skill instructions apply | |
| 2 | **Connected:** “Draft TestCase proposals for issue KEY in this project; confirm before create.” | Agent discovers MCP tools; drafts; **waits for confirm** before write | |
| 3 | **Connected:** Ask to summarize recent reports / plan status | Agent uses discovered report/status tools; no invented metrics | |
| 4 | **Disconnected:** Same create or summarize prompt | Agent states tools not callable; guides MCP reconnect; does **not** invent results | |
| 5 | Ask to delete a TestCase | Agent requires explicit confirm of the key; warns delete is destructive | |

## Results log

| Date | IDE | MCP connected? | Outcome | Notes |
| ---- | --- | -------------- | ------- | ----- |
| 2026-09-09 | Structural | N/A | Pass | LICENSE, README, `skills/testing-tool-mcp/SKILL.md` (86 lines), verification doc present; frontmatter `name` + description OK. |
| 2026-09-09 | Cursor (this agent) | Atlassian MCP yes; AI Testing Tool–specific tools no | Pass (partial) | [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) tools present (Jira/Confluence/etc.). No dedicated AI Testing Tool TestCase-design tools in catalog. Skill/README updated to use Atlassian Rovo MCP as the IDE connection path. |
| 2026-09-09 | Epic 3 docs | N/A | Pass | Added `docs/install/claude.md` and `docs/install/github-copilot.md`. Linked pack from hub + Cursor/Copilot/Claude user-guide pages. |
| 2026-09-09 | Cursor (this agent) | No (earlier) | Pass (disconnected path) | Dynamic tool catalog search found **no** AI Testing Tool–only MCP server. Agent followed skill: did not invent tools or TestCase data. |

## Notes

- Do not record secrets, tokens, or customer site URLs in this file.
- Re-run connected + disconnected smokes after meaningful skill edits.
