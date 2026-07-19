---
type: Wiki Page
title: Open Knowledge Format
description: How Commonplace maps to the OKF specification.
timestamp: '2026-07-19T10:00:54.000Z'
---

Commonplace stores content following the [Open Knowledge Format (OKF)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md), an open specification for keeping knowledge in plain files inside a Git repository. This has a practical consequence: your wiki is not locked into Commonplace. Any OKF-aware tool, any text editor, and any AI agent can work with the same repository.

## The mapping

An OKF knowledge bundle is a Git repository (or a subdirectory of one, see `GIT_ROOT` in [Configuration](/configuration.md)). Commonplace maps its concepts like this:

| OKF concept | In Commonplace |
| --- | --- |
| Bundle | The [Git repository](/Installation/git-repository.md) served by one deployment. |
| Concept | A Markdown file with YAML frontmatter, i.e. a page. |
| Concept type | The `type` frontmatter field (`Wiki Page` by default, configurable via `default_type`). |
| Bundle-absolute links | `[Page](/folder/page.md)` links between pages. |
| Assets | Files in the `assets/` folder. |

## What this buys you

* **Longevity.** The content is readable without any running software: it is Markdown in Git.
* **Interoperability.** AI agents work with the same files through the [MCP server](/mcp.md), and search or catalog tools can index the repository directly.
* **Real version control.** History, blame, branches, and pull requests work on wiki content exactly as they do on code.

## Conventions on top of OKF

Commonplace adds a few conventions of its own: `.commonplace/settings.yaml` for [wiki settings](/configuration.md), the automatic [update log](/log.md), and `index.md` as the landing page of the bundle and of each folder.
