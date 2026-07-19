---
type: Wiki Page
title: Skill
description: Give AI agents a skill so they use the wiki correctly, not just have access to it.
timestamp: '2026-07-19T10:00:54.000Z'
---

The [MCP server](/mcp.md) gives AI agents the tools to search, read, and write this wiki. A skill adds the second half: instructions that tell the agent when to consult the wiki and how to follow its conventions (frontmatter, links, conflict-safe updates). With only the tools, an agent has access; with a skill, it behaves like a careful wiki editor.

## Claude Code

First connect the MCP server:

```bash
claude mcp add --transport http wiki https://<your-commonplace-host>/api/mcp \
  --header "Authorization: Bearer github_pat_..."
```

Then create a skill in your project (or in `~/.claude/skills/` to use it everywhere):

```
.claude/skills/wiki/SKILL.md
```

```markdown
---
name: wiki
description: >
  Search, read, and update the team wiki (Commonplace). Use when the user asks
  about team knowledge, documentation, runbooks, or decisions, or asks to
  document something "in the wiki".
---

# Working with the wiki

The wiki is available through the `wiki` MCP server (tools: search_pages,
get_page, save_page).

## Reading

- Search before answering questions about team knowledge; prefer wiki content
  over assumptions.
- Start with broad search terms, then refine.

## Writing

- Before creating a page, search for an existing one and update it instead of
  creating a duplicate.
- Pages are Markdown with YAML frontmatter; `type` is set automatically when
  omitted. Give every page a `title` and a one-line `description`.
- Link related pages with bundle-absolute paths: `[Installation](/Installation/index.md)`.
- To update a page: call get_page first and pass the returned `sha` to
  save_page, so concurrent edits are detected instead of overwritten.
- Keep edits minimal and preserve the existing structure of a page.
```

Claude Code loads the skill automatically when a task matches its description. Ask something like "document today's deployment steps in the wiki" and the agent will search, reuse an existing page if one fits, and save with conflict detection.

## Claude.ai and Claude Desktop

Connect the wiki as a custom connector as described on the [MCP](/mcp.md) page. Skills can be added in the settings; upload the same skill content, or add it to a project's instructions. The skill text works unchanged, since it only references the MCP tool names.

## Other agents

Any agent framework with MCP support can use the same pattern:

1. Connect the MCP endpoint `https://<your-commonplace-host>/api/mcp` with a GitHub token (see [Access Control](/access-control.md) for token scopes).
2. Put the instructions from the skill above into whatever the framework offers: a system prompt, custom instructions, or an agent definition.

The important rules to carry over are the same three everywhere: search before creating, link with bundle-absolute paths, and pass the `sha` from get_page when saving to avoid overwriting concurrent edits.

## Scoping write access

Agents inherit the permissions of their token. For a read-only assistant, use a token with Contents read-only (or none at all if the repository is public). Give write tokens only to agents that are supposed to edit, and remember that every agent edit lands as an attributed commit in the [update log](/log.md) and the git history, so changes are easy to review and revert.
