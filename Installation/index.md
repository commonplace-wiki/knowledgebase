---
type: Wiki Page
title: Installation
description: Overview of the ways to run Commonplace and what you need before you start.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace is a stateless web application. All content lives in a Git repository; the app itself holds no data, so you can run it anywhere a container or Node.js app can run.

## Prerequisites

Every installation needs three things:

1. A [Git repository](/Installation/git-repository.md) that stores the wiki content.
2. A [GitHub App](/Installation/install-github-app.md) for sign-in and repository access.
3. A place to run the app. Pick one of the guides below.

## Configuration

The app is configured entirely through environment variables:

| Variable | Required | Description |
| --- | --- | --- |
| `GIT_REPO` | yes | URL of the repository that stores the wiki, e.g. `https://github.com/owner/repo`. Each deployment serves exactly one repository. |
| `GITHUB_CLIENT_ID` | yes | Client ID of your GitHub App. |
| `GITHUB_CLIENT_SECRET` | yes | Client secret of your GitHub App. |
| `SESSION_SECRET` | yes | Random string used to sign sessions. Generate with `openssl rand -hex 32`. |
| `GIT_BRANCH` | no | Branch to read and write. Defaults to the repository's default branch. |
| `GIT_ROOT` | no | Subdirectory within the repository that contains the wiki. |

## Installation guides

* [Install on Vercel](/Installation/install-on-vercel.md) — the fastest way to get a hosted instance.
* [Install on Azure](/Installation/install-on-azure.md) — run the container on Azure App Service.
* [Install on Kubernetes](/Installation/install-on-kubernetes.md) — plain manifests for any cluster.

You can also run Commonplace locally with Docker:

```bash
docker run -p 3000:3000 \
  -e SESSION_SECRET=$(openssl rand -hex 32) \
  -e GIT_REPO=https://github.com/owner/repo \
  -e GITHUB_CLIENT_ID=... \
  -e GITHUB_CLIENT_SECRET=... \
  commonplacewiki/commonplace
```

Then open [http://localhost:3000](http://localhost:3000).
