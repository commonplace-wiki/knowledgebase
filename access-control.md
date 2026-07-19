---
type: Wiki Page
title: Access Control
description: Who can read and edit the wiki, and where those permissions come from.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace has no user database of its own. Identity comes from GitHub sign-in, and permissions are derived from the content repository. Managing wiki access means managing repository access.

## The rules

| Repository | Read the wiki | Edit the wiki |
| --- | --- | --- |
| Public | Everyone, no sign-in required | GitHub users with push access |
| Private | GitHub users with read access | GitHub users with push access |

* Users sign in through the [GitHub App](/Installation/install-github-app.md). Commonplace acts with the signed-in user's permissions, never with elevated rights of its own.
* Every edit is a git commit attributed to the user who made it, so the repository history is a complete audit trail.

## Granting and revoking access

Use GitHub's normal mechanisms: add people or teams as collaborators on the repository, or manage access through your organization. Removing repository access removes wiki access with it; there is nothing to clean up on the Commonplace side.

## AI agents

Agents authenticate to the [MCP endpoint](/mcp.md) with a GitHub token instead of the sign-in flow. Use a fine-grained personal access token restricted to the wiki repository with Contents read/write. The same rule applies: commits are attributed to the token's user.

## Fine-grained permissions

Per-page or per-folder permissions are not supported; the repository is the permission boundary. If one group of pages needs a different audience, put it in its own repository and run a second Commonplace deployment, since each deployment serves exactly one repository.
