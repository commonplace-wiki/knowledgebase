---
type: Wiki Page
title: Migrate from Confluence
description: Move a Confluence Cloud space into a Commonplace wiki, agent-driven or by script.
timestamp: '2026-07-19T12:30:00.000Z'
---

Commonplace ships a migration for Confluence Cloud. It exports a space, converts it to an [Open Knowledge Format](/open-knowledge-format.md) bundle (Markdown files with YAML frontmatter), and pushes the result to your wiki's [Git repository](/Installation/git-repository.md). It is packaged as a Claude Code skill named `confluence-to-commonplace` in the [Commonplace repository](https://github.com/commonplace-wiki/commonplace), so a coding agent runs the export, conversion, review, and push, and asks before overwriting or pushing anything.

## What you need

* The **Confluence site URL** (`https://acme.atlassian.net`) and the **space key**, visible in the space URL: `/wiki/spaces/<KEY>/`.
* **Confluence credentials**: an [Atlassian API token](https://id.atlassian.com/manage-profile/security/api-tokens) for your account. On macOS, an authenticated Atlassian CLI (`acli`) works too; the migration reuses its stored token automatically.
* The **target GitHub repository** for the wiki content and a branch; optionally a subdirectory as bundle root (see `GIT_ROOT` in [Configuration](/configuration.md)).
* A checkout of the Commonplace repository, Claude Code, and Python 3.

## Run the migration

Clone the Commonplace repository and start Claude Code in it, then ask:

```
Migrate the Confluence space ED from https://acme.atlassian.net
to the repository my-org/wiki.
```

The skill collects any missing inputs, clones the target repository, runs the conversion, and shows you a summary and sample pages before committing and pushing.

If you prefer to run it without an agent, call the underlying script directly (`python3 -m pip install --user requests markdownify` first):

```bash
python3 .claude/skills/confluence-to-commonplace/scripts/migrate.py \
  --site https://acme.atlassian.net \
  --space ED \
  --out /path/to/wiki-clone \
  --email user@acme.com \
  --token-file ~/.confluence_token \
  --type "Wiki Page"
```

## What gets migrated

| Confluence | In the wiki |
| --- | --- |
| Page hierarchy | Folders: a page becomes `<slug>.md`, its children live in `<slug>/`. The space homepage becomes `overview.md`, and a root `index.md` listing the top-level pages is generated. |
| Page content | Markdown. Code macros become fenced blocks with language, info/note/warning/tip panels become blockquotes, task lists become checkboxes; tables, expand/status/toc macros, emoticons, and user mentions are handled. |
| Page-to-page links | Bundle-absolute links (`/a/b.md`), as described in [Open Knowledge Format](/open-knowledge-format.md). Links that cannot be resolved fall back to the original Confluence URL and are reported. |
| Attachments | Downloaded into an `assets/` folder next to the page, with image and attachment references rewritten. |
| Metadata | Frontmatter per page: `title`, `description` (first paragraph), `tags` (Confluence labels), `timestamp` (last version date), and `confluence_id` for traceability. |

A `log.md` recording the migration is written as well.

## What is not migrated

* Comments, page restrictions, and version history; git history starts fresh with the migration commit.
* Archived pages and drafts; only current pages migrate.

## Review and go live

1. Check the script's summary: page and attachment counts, unresolved links, failed downloads, skipped macros.
2. Spot-check a few converted pages against the originals, including one with a table or image.
3. Push, then point a Commonplace deployment at the repository (`GIT_REPO`, see [Installation](/Installation/index.md)) and browse the result.

Re-running the migration is safe: unchanged pages are overwritten in place, and files the script did not generate are never deleted. So you can keep Confluence writable during a trial run and migrate again before the final switch.
