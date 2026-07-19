---
type: Wiki Page
title: GitHub
description: GitHub is the primary supported backend for Commonplace wikis.
timestamp: '2026-07-19T10:00:54.000Z'
---

GitHub is currently the primary supported Git provider. Point `GIT_REPO` at a GitHub repository URL and Commonplace detects the provider from the host:

```
GIT_REPO=https://github.com/owner/wiki
```

## Authentication

* **Users** sign in via the [GitHub App](/Installation/install-github-app.md). Commonplace acts on their behalf, so every edit becomes a commit attributed to the signed-in user.
* **Permissions** follow the repository: read access to the repository means read access to the wiki, push access means edit access. See [Access Control](/access-control.md).
* **AI agents** authenticate with a fine-grained personal access token (Contents read/write on the wiki repository) via the [MCP endpoint](/mcp.md).

## Public and private repositories

Both work. With a public repository, the wiki can be read without signing in; editing always requires authentication. With a private repository, both reading and editing require sign-in and repository access.

## Branches and subdirectories

* `GIT_BRANCH` serves the wiki from a branch other than the default one, for example to review content changes via pull requests before they land.
* `GIT_ROOT` serves the wiki from a subdirectory, so a wiki can live inside an existing project repository (e.g. `docs/`).
