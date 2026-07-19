---
type: Wiki Page
title: Local Git
description: Serving a wiki from a local repository without a Git host is not yet supported.
timestamp: '2026-07-19T10:00:54.000Z'
---

Running Commonplace against a plain local Git repository (a path on disk, without a Git host) is not supported yet. Authentication and permissions are built on the Git host, so a hosted [GitHub](/Git-Repositories/github.md) or [GitLab](/Git-Repositories/gitlab.md) repository is currently required.

For local work you have two options today:

* **Run the app locally against your Git host.** Start Commonplace on `http://localhost:3000` with `GIT_REPO` pointing at your GitHub or GitLab repository. You get the full editing experience locally while the content stays hosted.
* **Edit the files directly.** Since pages are plain Markdown, you can clone the repository, edit with any editor or IDE, and push. Commonplace picks up the changes immediately.

Native local-repository support is on the roadmap for offline and air-gapped use cases.
