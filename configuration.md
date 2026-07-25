---
type: Wiki Page
title: Configuration
description: Deployment settings for the knowledge base and security, and wiki settings for look and feel.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace is configured in two places. Deployment settings are environment variables that configure the knowledge base and security. Wiki settings live in the content repository and configure look and feel.

## Deployment settings: environment variables

Set where the app runs (Docker, Vercel, Azure, Kubernetes):

| Variable | Required | Description |
| --- | --- | --- |
| `GIT_REPO` | yes | URL of the content repository, or an absolute directory path for a [local repository](/Git-Repositories/local-git.md). One deployment serves exactly one repository. |
| `SESSION_SECRET` | production | Random string for encrypting session cookies. Generate once with `openssl rand -hex 32` and keep it stable; changing it signs all users out. Without it, a public fallback is used and sessions can be forged. |
| `GITHUB_CLIENT_ID` | no | Client ID of the [GitHub App](/Installation/github-app.md) used for sign-in. Without it, users sign in with a personal access token instead. |
| `GITHUB_CLIENT_SECRET` | no | Client secret of the GitHub App. |
| `GITLAB_CLIENT_ID` | no | Application ID of the [GitLab OAuth application](/Git-Repositories/gitlab.md) used for sign-in. |
| `GITLAB_CLIENT_SECRET` | no | Secret of the GitLab OAuth application. |
| `GIT_PROVIDER` | no | Set to `gitlab` when the repository lives on a self-hosted GitLab (for gitlab.com it is detected from the URL). |
| `GIT_BRANCH` | no | Branch to serve. Defaults to `main`. |
| `GIT_ROOT` | no | Subdirectory of the repository that contains the wiki. |
| `PUBLIC_ORIGIN` | no | External origin as users reach the wiki, e.g. `https://wiki.example.com`. Set it behind a reverse proxy so OAuth redirects and absolute links do not depend on forwarded headers. |
| `GOOGLE_SITE_VERIFICATION` | no | Google Search Console verification token (the `content` value of the "HTML tag" method); emitted as a meta tag on every page. |

See the [Installation](/Installation/index.md) guides for where to set these on each platform, and [Access Control](/access-control.md) for how permissions are derived from the repository.

## Search engines

A wiki on a public repository is search-engine friendly out of the box: it serves a `robots.txt` that allows indexing of pages (but not the editor or API), a `sitemap.xml` with all visible pages, and per-page browser titles in the form `Page - Wiki Name - Commonplace`. A wiki on a private repository serves a `robots.txt` that disallows all crawling and no sitemap. There is nothing to configure; the behavior follows the repository's visibility.

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
