---
type: Wiki Page
title: Getting Started
description: From zero to a running wiki in about five minutes.
timestamp: '2026-07-19T10:00:54.000Z'
---

This guide takes you from nothing to a running Commonplace wiki. You need a GitHub account and either Docker or a Vercel account.

## 1. Create the content repository

Create a new repository on GitHub, for example `my-org/wiki`. It can stay empty and can be private. Details: [Git Repository](/Installation/git-repository.md).

## 2. Start Commonplace

The quickest local start is Docker:

```bash
docker run -p 3000:3000 \
  -e SESSION_SECRET=$(openssl rand -hex 32) \
  -e GIT_REPO=https://github.com/my-org/wiki \
  commonplacewiki/commonplace
```

Open [http://localhost:3000](http://localhost:3000). Without GitHub App credentials, the setup screen guides you through creating one; alternatively create it manually and pass `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET`. See [Install GitHub App](/Installation/install-github-app.md).

For a hosted instance, follow [Install on Vercel](/Installation/install-on-vercel.md) instead.

## 3. Sign in and write your first page

Sign in with GitHub, open the homepage, and start editing. Every save becomes a git commit in your repository, attributed to you. The Markdown conventions are described in [Open Knowledge Format](/open-knowledge-format.md).

## 4. Optional next steps

* Adjust the wiki name and settings: [Configuration](/configuration.md)
* Connect AI assistants: [MCP](/mcp.md)
* Understand who can read and edit: [Access Control](/access-control.md)
* If something does not work: [FAQ](/faq.md)
