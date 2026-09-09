---
name: testing-tool-mcp
description: >-
  Guides IDE agents to manage AI Testing Tool TestCases and test reports through
  the Atlassian Rovo MCP Server (and any AI Testing Tool tools exposed there).
  Use when the user asks about TestCases, Testing tool MCP, Atlassian MCP,
  IDE test management, creating or updating test design, querying cases,
  inspecting reports, or confirm-before-write workflows with AI Testing Tool.
---

# AI Testing Tool via Atlassian Rovo MCP

Teach the agent to work with **AI Testing Tool** from the IDE using **Atlassian Rovo MCP** tools already connected in Cursor, Claude, or GitHub Copilot. Do not invent APIs or tool names. Discover what is available in this session.

**MCP client (how IDEs connect):** [Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) — typically `https://mcp.atlassian.com/v2/mcp`.

## When this skill applies

- User wants to create, update, query, or delete TestCases through the IDE agent
- User wants report / run / plan insights related to AI Testing Tool
- User mentions Testing tool MCP, Atlassian Rovo MCP, `rovo:mcp`, or IDE test management with AI Testing Tool

## Hard rules

1. **Discover tools first.** List MCP tools from the Atlassian Rovo MCP connection in this session. Prefer tools that clearly belong to AI Testing Tool / test management when present. Otherwise match intent to the closest available Atlassian tool (e.g. Jira issue read/search). Never invent tool names or REST endpoints.
2. **Confirm-before-write.** For create and update, show the draft (summary, preconditions, steps, etc.) and wait for explicit user confirmation before calling a write tool.
3. **Delete is destructive.** Hard-delete of a TestCase issue requires **explicit** user confirmation naming the TestCase key. State that delete cannot be undone via this skill.
4. **Active ≠ MCP.** AI Testing Tool Settings → Active for Cursor/Claude/Copilot is preference-only. It does not register Atlassian Rovo MCP in the IDE.
5. **IDE vs Jira.** For chat **inside Jira**, tell the user to use the AI Testing Tool Rovo agent. This skill is for IDE MCP clients.

## If the right tools are not callable

Say clearly what is missing (Atlassian MCP not connected, or no AI Testing Tool–specific tools yet). Then:

1. Confirm Atlassian Rovo MCP is set up in this IDE ([get started guide](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/)) and the user has authenticated (OAuth).
2. Confirm AI Testing Tool is installed on the site and any product MCP / Forge exposure required for TestCase design tools is live.
3. Suggest reconnect / reload MCP and retry tool discovery.
4. Do **not** invent results, issue keys, or step text. Do **not** pretend TestCase design fields were updated if only generic Jira fields were writable.

## Workflow

### 1. Orient

- Identify project / issue keys the user cares about (ask if missing).
- Discover MCP tools; note Atlassian platform tools vs any AI Testing Tool–specific tools; note read vs write.

### 2. Query / read

- Prefer read tools before writes.
- Prefer work-context or issue-linked reads when the user names an issue.
- Cite only keys, fields, and values returned by tools—never invent TestCase content.
- If only generic Jira tools exist, you may read the TestCase **issue**, but say when TestCase **design** (steps/preconditions) is not available via those tools.

### 3. Create TestCase

1. Gather requirement context (issue key, pasted requirements, or tool-provided work context).
2. Draft proposal(s): summary, preconditions/postconditions if needed, steps (action / data / expected).
3. Present draft to the user.
4. **Only after confirmation**, call the matching create tool with the confirmed draft (prefer AI Testing Tool–specific create when available).

### 4. Update TestCase

1. Load current design via a read/query tool when available.
2. Propose a concrete diff (which steps/fields change).
3. **Only after confirmation**, call the matching update tool. If only generic Jira edit exists, limit updates to fields that tool can set and say what cannot be updated (e.g. structured steps).

### 5. Delete TestCase

1. Identify the TestCase key and why it should be removed.
2. Warn that delete is a **hard delete** of the TestCase issue.
3. Require the user to explicitly confirm the key (e.g. “delete KEY-123”).
4. Call the matching delete tool only after that confirmation.

### 6. Reports / insights

- Use discovered report or status tools when available.
- Otherwise say insights are unavailable without inventing metrics.

## Response style

- Be concise. Lead with what you will do and what needs confirmation.
- After tool calls, report outcomes with real keys/URLs from tool data.
- If a tool returns an error, explain the message; do not retry blindly with invented inputs.

## Out of scope

- Replacing the in-Jira Rovo agent
- Implementing Atlassian or Forge MCP platform modules
- CI reporters, ingest tokens, or storing secrets in the repo
