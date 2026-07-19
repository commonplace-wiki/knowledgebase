---
type: Wiki Page
title: GitLab
description: Run Commonplace on a GitLab repository — gitlab.com or self-hosted.
timestamp: '2026-07-19T13:30:00.000Z'
---

Commonplace fully supports GitLab as the wiki's git repository, both on gitlab.com and on self-hosted instances. Reading, editing, uploads, page moves, the [knowledge graph](/graph), and the MCP server all work the same as with [GitHub](/Git-Repositories/github.md).

## Configuration

Point the deployment at the GitLab project:

```
GIT_REPO=https://gitlab.com/group/subgroup/wiki
GIT_BRANCH=main
```

The provider is detected from the host. Nested groups are supported. For a **self-hosted GitLab** instance, additionally set:

```
GIT_PROVIDER=gitlab
```

## Sign-in

Two options:

* **OAuth application** (recommended for teams): create one under User or Group Settings → Applications with redirect URI `https://<your-commonplace-host>/api/auth/callback`, the `api` scope, and **Confidential** checked. Put the credentials into `GITLAB_CLIENT_ID` and `GITLAB_CLIENT_SECRET`. Opening `/setup` on your deployment shows these steps with the exact URLs filled in.
* **Personal access token**: a `glpat-…` token with the `api` scope, entered on the sign-in page. No application needed.

Access tokens are refreshed automatically; sessions live in an encrypted cookie, and nothing is stored server-side.

## Permissions

* GitLab's permission model applies: **public projects are readable without signing in**; private projects require sign-in.
* Editing requires at least the **Developer** role on the project. Commonplace shows a warning after sign-in when the token cannot write.
* Commits are attributed to the signed-in GitLab user.

## Behavior details

* Concurrent edits are detected via the file's last commit id and reported instead of silently overwritten.
* Moving or renaming pages happens as a single commit using GitLab's commits API, including rewriting links in other pages.
