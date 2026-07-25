---
type: Wiki Page
title: Getting Started
description: From zero to a running wiki in about five minutes.
timestamp: '2026-07-21T14:58:55.923Z'
---

This guide takes you from nothing to a running Commonplace wiki. You need a GitHub account and either Docker or a Vercel account.

## 1. Create the content repository

Create a new repository on GitHub, for example `my-org/wiki`. It can stay empty and can be private. Details: [Git Repository](/Installation/git-repository.md).

## 2. Start Commonplace

The quickest local start is Docker:

Replace [https://github.com/commonplace/knowledge](https://github.com/commonplace/knowledge) with your repository.

```bash
docker run -p 3000:3000 \
  -e GIT_REPO=https://github.com/commonplace/knowledge \
  commonplacewiki/commonplace
```

Open [http://localhost:3000](http://localhost:3000). A public repository is browsable right away; sign-in is only needed for editing (and for reading private repositories). Without GitHub App credentials, the setup screen guides you through creating an app in one click; alternatively create it manually and pass `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET` (see [GitHub App](/Installation/github-app.md)), or sign in with a personal access token, no app needed.

For any deployment that outlives a quick test, also set `SESSION_SECRET`: generate it once with `openssl rand -hex 32` and keep it stable, see [Configuration](/configuration.md).

For a hosted instance, follow [Install on Vercel](/Installation/install-on-vercel.md) instead.

## 3. Sign in and write your first page

Sign in with GitHub, open the homepage, and start editing. Every save becomes a git commit in your repository, attributed to you. The Markdown conventions are described in [Open Knowledge Format](/open-knowledge-format.md).

Type `@` while editing to mention a teammate; it links to their GitHub profile. Who is suggested depends on the App's permissions, see [Access Control](/access-control.md).

## 4. Optional next steps

* Adjust the wiki name and settings: [Configuration](/configuration.md)
* Connect AI assistants: [MCP](/mcp.md)
* Understand who can read and edit: [Access Control](/access-control.md)
* If something does not work: [FAQ](/faq.md)
