---
type: Wiki Page
title: MCP
description: Connect AI agents and assistants to this wiki via the Model Context Protocol.
timestamp: 2026-07-19T09:30:00.000Z
---

Commonplace exposes this wiki as an MCP server, so AI agents and assistants can search, read, and write pages. The endpoint is:

```
https://<your-commonplace-host>/api/mcp
```

It speaks the MCP Streamable HTTP transport and offers three tools:

* **search_pages** — find pages by content, title, path, tag, or concept type
* **get_page** — read a page (frontmatter, markdown body, and the blob sha needed for updates)
* **save_page** — create or update a page as a git commit, with automatic timestamp, an entry in [log.md](/log.md), and conflict detection via the blob sha

## Authentication

Pass a GitHub token in the `Authorization` header:

* A fine-grained personal access token with **Contents read/write** on the wiki repository (recommended), or a classic token with `repo` scope.
* Reading works **without any token** when the wiki repository is public; writing always requires one.
* Commits made through MCP are attributed to the token's GitHub user.

## Claude Code

```bash
claude mcp add --transport http wiki https://<your-commonplace-host>/api/mcp \
  --header "Authorization: Bearer github_pat_..."
```

Then ask things like "search the wiki for onboarding" or "update the MCP page in the wiki".

## Claude.ai and Claude Desktop

Add a custom connector (Settings → Connectors → Add custom connector) with the URL `https://<your-commonplace-host>/api/mcp`. For write access, configure the Authorization header with your token where the client supports custom headers.

## Other MCP clients

Any client that supports the Streamable HTTP transport works. Typical JSON configuration:

```json
{
  "mcpServers": {
    "wiki": {
      "type": "http",
      "url": "https://<your-commonplace-host>/api/mcp",
      "headers": {
        "Authorization": "Bearer github_pat_..."
      }
    }
  }
}
```

## What agents should know

* Pages are markdown files with YAML frontmatter; `type` is required and set automatically when omitted.
* Link between pages with bundle-absolute paths: `[Installation](/Installation/index.md)`.
* When updating a page, first `get_page` and pass the returned `sha` to `save_page`, so concurrent edits are detected instead of overwritten.

To teach these conventions to an agent instead of hoping it discovers them, set up a [Skill](/skill.md).
