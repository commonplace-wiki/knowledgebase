---
type: Wiki Page
title: Git Repository
description: Create and structure the repository that stores your wiki content.
timestamp: '2026-07-19T10:00:54.000Z'
---

All wiki content is stored in a single Git repository, following the [Open Knowledge Format](/open-knowledge-format.md). Commonplace reads from and commits to this repository; it keeps no data of its own.

## Create the repository

Create a new repository on GitHub or GitLab (private or public, both work). An empty repository is fine; Commonplace creates the files it needs on first use.

If you want to start from a template, this knowledge base itself is a working example: [commonplace-wiki/knowledgebase](https://github.com/commonplace-wiki/knowledgebase).

## Repository layout

```
├── index.md                  # the homepage of the wiki
├── log.md                    # update log, maintained automatically
├── .commonplace/
│   └── settings.yaml         # wiki settings, see Configuration
├── assets/                   # uploaded images and attachments
├── Installation/             # a folder becomes a section
│   ├── index.md              # optional section homepage
│   └── install-on-vercel.md  # a page
└── ...
```

* Every page is a Markdown file with YAML frontmatter. See [Writing Pages](/writing-pages.md).
* Folders group pages into sections. A folder's `index.md` acts as its landing page.
* Images and attachments go into `assets/` and are uploaded there automatically when you use the editor.

## Access and permissions

Read and write permissions follow the repository:

* Users sign in with the Git provider (GitHub or GitLab). Whoever can read the repository can read the wiki, and whoever can push can edit. See [Access Control](/access-control.md).
* Commits made through the wiki are attributed to the signed-in user, so `git blame` and the history stay meaningful.

## Branch and subdirectory

By default Commonplace uses the repository's default branch and the repository root. Set `GIT_BRANCH` to serve a different branch, or `GIT_ROOT` to keep the wiki in a subdirectory (useful when the wiki lives inside an existing project repository).

Next step: [Install GitHub App](/Installation/install-github-app.md) — or, for GitLab, see [GitLab](/Git-Repositories/gitlab.md).
