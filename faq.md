---
type: Wiki Page
title: FAQ
description: Common questions and troubleshooting for setup and daily use.
timestamp: '2026-07-19T10:00:54.000Z'
---

## Setup

**Sign-in redirects to GitHub but fails or loops.**
The GitHub App's callback URL must exactly match your instance URL, including scheme and host: `https://<your-host>/api/auth/callback`. A mismatch after adding a custom domain is the most common cause. See [Install GitHub App](/Installation/install-github-app.md).

**I signed in but get no access.**
Your GitHub user needs read access to the repository in `GIT_REPO` (for private repositories), and the GitHub App must be installed on that repository. Check both under the repository's settings on GitHub.

**Edits fail with a permission error.**
Editing requires push access to the repository. See [Access Control](/access-control.md).

**Can one deployment serve several wikis?**
No. One deployment serves exactly one repository (`GIT_REPO`). Run one deployment per wiki; they are small and stateless.

## Content

**Can I edit pages outside the wiki?**
Yes. Pages are Markdown files in a [Git repository](/Installation/git-repository.md); clone, edit, and push with any tools you like. The wiki reflects pushed changes immediately.

**How do I restore a deleted or broken page?**
Use git: the full history is in the repository, so `git revert` or restoring a file from an earlier commit brings it back. The [update log](/log.md) helps find when a change happened.

**Why did my links break after moving a page?**
Links use bundle-absolute file paths, so moving or renaming a file changes its address. Update the links that point to it; see [Open Knowledge Format](/open-knowledge-format.md).

**Where do images live?**
In the `assets/` folder of the repository. The editor uploads them there automatically.

## Migration and integrations

**Can I migrate from Confluence?**
Yes, Commonplace ships a Confluence migration; see [Migrate from Confluence](/migrate-from-confluence.md).

**How do AI assistants connect?**
Through the built-in MCP server; see [MCP](/mcp.md).

**Is GitLab supported?**
Yes — gitlab.com and self-hosted instances; see [GitLab](/Git-Repositories/gitlab.md).
