# Testing Tool IDE Plugins Product Requirements Document (PRD)

## Goals and Background Context

### Goals

- Publish a public GitHub pack that teaches Cursor, Claude, and GitHub Copilot agents to manage AI Testing Tool TestCases and reports via the [Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/)
- Ship a Cursor plugin as the primary install path, with Claude and Copilot adapters that reuse the same skill bodies
- Make agents reliable at create, update, and query of TestCases and at inspecting test reports, with confirm-before-write (and explicit confirm for delete)
- Prevent the failure mode where “Settings → Active” is mistaken for MCP registration
- Link the pack from Testing tool IDE integration docs so users find one clear path

### Background Context

Testing tool already documents IDE integrations for Cursor, GitHub Copilot, and Claude, including MCP-based management of TestCases and reports. Agents still lack shared, portable skill instructions: without them they invent APIs, skip confirmation, or confuse the in-Jira Testing tool Rovo agent with IDE MCP. The industry has converged on the Agent Skills `SKILL.md` format, which Cursor, Claude, and Copilot can all load—so one canonical skill body plus install adapters is the right product shape.

This PRD covers the **Testing Tool IDE Plugins** pack: https://github.com/ai-testing-tool/testing-tool-ide-plugins (repository root = this pack).

### Target personas

1. **IDE QA / test engineer** — Uses Cursor, Claude, or Copilot daily; uses Testing tool in Jira; wants agents to create, update, and query TestCases and inspect reports without leaving the IDE. Pain: agents invent APIs, skip confirm-before-write, or treat Settings → Active as MCP setup.
2. **Open-source adopter / platform engineer** — Discovers the pack on public GitHub; wants a clear install matrix and one skill body that works across tools. Pain: tool-specific forks and missing MCP prerequisite docs.

### Success metrics and MVP timeframe

| Metric | Target (MVP validation) | How measured |
| ------ | ----------------------- | ------------ |
| Skill smoke (MCP connected) | Pass on at least one IDE during Epic 1 verification | Story 1.3: agent uses discovered MCP tools for a TestCase or report prompt |
| Skill smoke (MCP missing) | Pass: agent explains tools are not callable and how to reconnect | Story 1.3 disconnected run |
| Confirm-before-write | Observed on create/update in smoke runs; delete requires explicit confirm | Manual review of agent behavior against skill instructions |
| Install coverage | Cursor local plugin, Claude adapter, and Copilot adapter each documented and smoke-checked once | Stories 2.2, 3.1, 3.2 |
| Doc discoverability | Hub and three IDE user-guide pages link to the pack | Story 3.3 |

**MVP validation timeframe:** Complete Epics 1–3 and smoke checks within **2–3 focused implementation weeks**. After MVP: Cursor marketplace publish, public adoption feedback, optional CI checks for `SKILL.md` frontmatter.

### Public distribution status

- **Pack root:** Repository root for this public pack (skills, plugin metadata, install docs).
- **Public GitHub URL:** https://github.com/ai-testing-tool/testing-tool-ide-plugins  
- **Maintainer:** Owners of Testing tool IDE integrations (same as Testing tool product user-guide IDE pages) until `CODEOWNERS` is set on the public repository.

### Install journeys (summary)

1. **Cursor:** Install the plugin → add the Testing tool MCP server → use TestCase or report prompts that load the skill.
2. **Claude:** Install the canonical skill into `.claude/skills/` (or upload) → connect MCP → same prompts.
3. **Copilot:** Install the canonical skill into `.github/skills/` → connect MCP → same prompts.
4. **Failure path (all):** If MCP tools are absent, the agent follows “not callable” guidance; Settings → Active alone is not enough.

### Change Log

| Date | Version | Description | Author |
| ---- | ------- | ----------- | ------ |
| 2026-09-09 | 0.1 | Initial PRD: MCP skill, Cursor plugin, Claude/Copilot adapters | Product |
| 2026-09-09 | 0.2 | Epics 1–3 with stories and acceptance criteria | Product |
| 2026-09-09 | 0.3 | Personas, success metrics, distribution status, install journeys | Product |
| 2026-09-09 | 0.4–0.6 | Public-document wording cleanup | Product |
| 2026-09-09 | 0.7 | Set public GitHub URL to ai-testing-tool/testing-tool-ide-plugins | Product |

## Requirements

### Functional

- **FR1:** Pack ships one or more portable Agent Skills (`SKILL.md` with `name` and `description`) that instruct agents how to manage AI Testing Tool TestCases and reports through Atlassian Rovo MCP (and any AI Testing Tool tools exposed there).
- **FR2:** Skills cover create, update, and query of TestCases, plus inspect or summarize test reports and run insights, using only discovered MCP tools (no invented APIs).
- **FR3:** Skills require confirm-before-write for create and update, and explicit user confirmation before delete; they state that delete is destructive.
- **FR4:** Skills distinguish IDE MCP clients from the in-Jira Testing tool Rovo agent and direct users to Rovo for in-Jira chat workflows.
- **FR5:** Skills document that Settings → Active is preference-only and does not register MCP; agents must check that tools are callable and guide reconnect if not.
- **FR6:** Pack ships a Cursor plugin (`.cursor-plugin`) that exposes the canonical skills for Cursor install.
- **FR7:** Pack ships Claude and Copilot install adapters (paths, docs, or copy/symlink instructions) that install the **same** skill bodies into `.claude/skills/` and `.github/skills/` (or documented equivalents).
- **FR8:** Public README includes an install matrix (Cursor / Claude / Copilot), MCP prerequisites, and example prompts that exercise the skill.
- **FR9:** Testing tool user-guide pages for Cursor, Copilot, and Claude link to this public pack as the agent-skills path.
- **FR10:** Skill descriptions include what the skill does and when to use it, so agents can discover it for TestCases, Testing tool MCP, or IDE test management.

### Non-Functional

- **NFR1:** Canonical skill content is tool-agnostic; tool-specific differences live only in install adapters and the README (no forked instruction bodies for v1).
- **NFR2:** Each `SKILL.md` stays concise (target under 500 lines), with progressive disclosure for longer reference material.
- **NFR3:** Pack uses an OSI-approved license suitable for public GitHub (MIT unless a different license is required); no secrets, brittle site-specific URLs, or private credentials in the repo.
- **NFR4:** Skills remain useful when MCP tools are not yet live: clear “not callable yet” guidance, not silent failure.
- **NFR5:** Naming and frontmatter follow Agent Skills conventions so Cursor, Claude, and Copilot can load them without conversion.

## User Interface Design Goals

Not applicable—this pack has no product UI screens. Install experience is README and install docs (FR6–FR8). Cursor marketplace listing is optional after MVP.

## Technical Assumptions

### Repository structure: public pack root

- Public repository root is this pack.
- Canonical skills live under `skills/`; Cursor metadata under `.cursor-plugin/`; adapter docs under `docs/install/` (or README sections).

### Service architecture

- Not an application service—content and plugin packaging only.
- Runtime dependency: the user’s IDE connected to the **[Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/)** (`https://mcp.atlassian.com/v2/mcp`), with AI Testing Tool installed on the site; product-specific tools appear when exposed to that MCP session.
- This pack owns no backend and stores no secrets.

### Testing requirements

- Manual verification: install the skill in each target tool; run example prompts with MCP connected and with MCP disconnected.
- Optional: validate `SKILL.md` frontmatter (name and description rules) in CI when inexpensive.
- No application testing pyramid for v1 (no app runtime in this pack).

### Additional technical assumptions

- Single canonical skill body; the Cursor plugin points at that `skills/` folder; Claude and Copilot adapters copy, symlink, or document the same content (prefer one source of truth).
- Cursor plugin shape follows common `.cursor-plugin/plugin.json` conventions, including a `skills` path.
- Testing tool product user-guide IDE pages own human MCP setup; this pack’s skill owns agent behavior (avoid duplicating long setup guides).
- License: MIT unless a different OSS license is required.
- Out of scope for this pack: implementing Forge MCP tools, Rovo agent prompts, and CI test reporters.

## Epic List

- **Epic 1: Foundation and canonical MCP skill** — Public pack skeleton (LICENSE, README install matrix, repo layout) and portable Testing tool MCP `SKILL.md`.
- **Epic 2: Cursor plugin distribution** — Package the same skills as a Cursor plugin so Cursor users can install in one path.
- **Epic 3: Claude/Copilot adapters and product doc links** — Claude and Copilot install adapters, plus links from Testing tool user-guide IDE pages.

## Epic 1 Foundation and canonical MCP skill

Establish the public pack skeleton and deliver the portable Testing tool MCP skill so any Agent Skills–compatible client can load it and manage TestCases and reports correctly—even before the Cursor plugin or adapters exist.

### Story 1.1 Pack skeleton and public README

As a developer discovering the pack on GitHub,
I want a clear repo layout, LICENSE, and README with an install matrix and MCP prerequisites,
so that I know what this is and how to try it in Cursor, Claude, or Copilot.

**Acceptance Criteria**

1. Directory includes `LICENSE` (MIT unless changed), root `README.md`, and `skills/` (may be empty until Story 1.2).
2. README states purpose, that Active is not MCP registration, and that the IDE must use [Atlassian Rovo MCP](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) (`https://mcp.atlassian.com/v2/mcp`).
3. README includes an install matrix for Cursor, Claude, and Copilot (Cursor plugin may note “see Epic 2”; manual skill path is documented).
4. README includes at least two example prompts (for example, create a TestCase from an issue; summarize recent reports).
5. No secrets or site-specific credentials in the repo.

### Story 1.2 Canonical `testing-tool-mcp` skill

As an IDE coding agent,
I want a portable `skills/testing-tool-mcp/SKILL.md` with clear what/when guidance and workflows,
so that I use Testing tool MCP tools correctly instead of inventing APIs.

**Acceptance Criteria**

1. Skill has valid Agent Skills frontmatter (`name`, third-person `description` with trigger terms).
2. Instructions cover: discover MCP tools → query, create, and update TestCases → inspect reports; no invented tool names.
3. Confirm-before-write for create and update; explicit confirm before delete; delete called out as destructive.
4. Distinguishes IDE MCP from the in-Jira Rovo agent; points users to Rovo for in-Jira chat.
5. Documents that Active is not MCP; if tools are not callable, the agent explains and guides reconnect (no silent failure).
6. `SKILL.md` stays under about 500 lines; longer detail, if any, lives in one-level `references/`.
7. Manual check: the description alone makes what and when obvious for TestCases, Testing tool MCP, and IDE test management.

### Story 1.3 Smoke-verify skill load path

As a maintainer,
I want a short verification checklist executed once against the skill,
so that we know the pack is usable before plugin work.

**Acceptance Criteria**

1. A doc section or `docs/verification.md` lists steps: load the skill in at least one tool, run one example prompt with MCP connected and one without.
2. Checklist records expected outcomes (tools used vs “not callable” guidance).
3. Checklist is run once; results are recorded in the verification doc (pass/fail and date).

## Epic 2 Cursor plugin distribution

Package the Epic 1 canonical skills as a Cursor plugin so Cursor users get a first-class install path without manually copying folders.

### Story 2.1 Cursor plugin manifest and metadata

As a Cursor user,
I want a valid `.cursor-plugin` package that points at the pack’s skills,
so that I can install Testing Tool IDE Plugins like other Cursor plugins.

**Acceptance Criteria**

1. `.cursor-plugin/plugin.json` exists with name, displayName, version, description, license, and `skills` path to canonical `skills/`.
2. Metadata follows Cursor plugin conventions; required fields are present and valid.
3. Plugin description mentions Testing tool MCP and TestCases so it is discoverable by intent.
4. README Cursor section includes concrete install steps (local install and/or documented publish path).

### Story 2.2 Local install path documented and verified

As a Cursor user or maintainer,
I want documented local install steps plus a smoke check,
so that the plugin loads the `testing-tool-mcp` skill in Cursor.

**Acceptance Criteria**

1. README or `docs/install/cursor.md` documents local install steps that match Cursor’s documented local plugin mechanism.
2. After install, the skill is invokable or discoverable for a TestCase MCP–related prompt (manual verification noted).
3. Verification notes that MCP must still be configured separately; the plugin does not register MCP.
4. No duplicate forked `SKILL.md` inside the plugin—the plugin references the canonical skill folder.

## Epic 3 Claude/Copilot adapters and product doc links

Complete cross-tool parity and discoverability: same skill bodies for Claude and Copilot, and Testing tool user-guide IDE pages that point users at this pack.

### Story 3.1 Claude install adapter

As a Claude user,
I want documented steps to install the canonical skill into Claude’s skills location,
so that Claude loads the same Testing tool MCP behavior as Cursor.

**Acceptance Criteria**

1. README or `docs/install/claude.md` documents install into `.claude/skills/` (project) and/or the personal Claude skills upload path.
2. Instructions use the same canonical `skills/testing-tool-mcp` content (copy, symlink, or release artifact—no divergent fork).
3. Notes that MCP connection is separate and that Active is not MCP (consistent with product FAQ).
4. At least one example prompt is listed for Claude.

### Story 3.2 GitHub Copilot install adapter

As a GitHub Copilot user,
I want documented steps to install the canonical skill into Copilot’s skills location,
so that Copilot agents load the same Testing tool MCP behavior.

**Acceptance Criteria**

1. README or `docs/install/github-copilot.md` documents install into `.github/skills/` (and notes other supported paths if relevant).
2. Same canonical skill content as Claude and Cursor (no divergent fork).
3. Notes that MCP connection is separate and that Active is not MCP.
4. At least one example prompt is listed for Copilot.

### Story 3.3 Link pack from Testing tool user guide

As a Testing tool user reading IDE integration docs,
I want links from Cursor, Copilot, Claude, and hub pages to this public pack,
so that I find agent skills without hunting GitHub.

**Acceptance Criteria**

1. Testing tool user-guide pages for Cursor, GitHub Copilot, Claude, and the IDE integrations hub each add a clear “Agent skills / IDE plugins” link to https://github.com/ai-testing-tool/testing-tool-ide-plugins (README).
2. Copy states that skills teach agent behavior; pages still own human MCP setup steps (no large duplication).
3. The hub page lists the pack alongside the three IDE setup pages.

## Validation summary

| Dimension | Assessment |
| --------- | ---------- |
| Completeness | Sufficient for architecture and implementation |
| MVP scope | Appropriate for a skills and plugin pack |
| Architecture readiness | Ready |
| Public repository | https://github.com/ai-testing-tool/testing-tool-ide-plugins |

**In scope:** portable MCP skill, Cursor plugin, Claude and Copilot adapters, links from Testing tool IDE docs.  
**Out of scope:** Forge MCP implementation, in-Jira Rovo agent behavior, generic QA method packs, TDD generators, Cursor marketplace publish (after MVP).

Optional later: CI lint for `SKILL.md` frontmatter; public-repo `CODEOWNERS`; landscape note.

## Follow-on

### Install and docs (optional)

Draft README and `docs/install/*` information architecture for Cursor, Claude, and Copilot using this PRD. Skip if packaging design already owns those docs.

### Architecture

Using this PRD (`docs/prd.md`), define packaging and publish layout for https://github.com/ai-testing-tool/testing-tool-ide-plugins: canonical `skills/` plus Cursor `.cursor-plugin`, Claude and Copilot adapters without forking skill bodies, and links from Testing tool product user-guide IDE pages. Document MCP as an external runtime dependency (not owned by this pack).
