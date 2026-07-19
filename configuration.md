---
type: Wiki Page
title: Configuration
description: Reference for wiki settings in .commonplace/settings.yaml and deployment environment variables.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace is configured in two places: per-wiki settings live in the content repository, deployment settings are environment variables.

## Wiki settings: `.commonplace/settings.yaml`

Stored in the [Git repository](/Installation/git-repository.md) and editable both via the wiki UI and by hand:

```yaml
name: Commonplace
description: ''
default_type: Wiki Page
update_log: true
```

| Setting | Description |
| --- | --- |
| `name` | Name of the wiki, shown in the header and browser title. |
| `description` | Short description of the wiki. |
| `default_type` | Concept type assigned to new pages when the frontmatter has no `type`. |
| `update_log` | When `true`, every creation, update, move, and deletion is recorded in [log.md](/log.md). |

Changes to this file take effect without restarting the app, since it is read from the repository like any other content.

## Deployment settings: environment variables

Set where the app runs (Docker, Vercel, Azure, Kubernetes):

| Variable | Required | Description |
| --- | --- | --- |
| `GIT_REPO` | yes | URL of the content repository. One deployment serves exactly one repository. |
| `GITHUB_CLIENT_ID` | yes | Client ID of the [GitHub App](/Installation/install-github-app.md). |
| `GITHUB_CLIENT_SECRET` | yes | Client secret of the GitHub App. |
| `SESSION_SECRET` | yes | Random string for session signing; generate with `openssl rand -hex 32`. Changing it signs all users out. |
| `GIT_BRANCH` | no | Branch to serve. Defaults to the repository's default branch. |
| `GIT_ROOT` | no | Subdirectory of the repository that contains the wiki. |

See the [Installation](/Installation/index.md) guides for where to set these on each platform.
