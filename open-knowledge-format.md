---
type: Wiki Page
title: Open Knowledge Format
description: How Commonplace maps to the OKF specification, and the Markdown conventions for writing pages.
timestamp: '2026-07-19T15:36:44.000Z'
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

## Writing pages

Every page is a Markdown file with YAML frontmatter, stored in the [Git repository](/Installation/git-repository.md). You can write pages in the wiki editor, in any text editor, or through the [MCP](/mcp.md) tools; the result is the same file.

### Frontmatter

```yaml
---
type: Wiki Page
title: Writing Pages
description: Markdown conventions, frontmatter fields, links, and assets.
timestamp: '2026-07-19T10:00:54.000Z'
---
```

| Field | Required | Description |
| --- | --- | --- |
| `type` | yes | Concept type of the page. Defaults to the wiki's `default_type` (see [Configuration](/configuration.md)) and is set automatically when omitted. |
| `title` | yes | Display title. Independent of the filename. |
| `description` | no | One-line summary, shown in search results and page listings. |
| `timestamp` | yes | Last modification time; maintained automatically on every save through the wiki. |

### Links

Link between pages with bundle-absolute paths, starting at the repository root and including the `.md` extension:

```markdown
[Installation](/Installation/index.md)
[MCP](/mcp.md)
```

External links use regular Markdown syntax with full URLs.

### Folders and filenames

* Folders are sections; a folder's `index.md` is its landing page.
* Filenames are kebab-case (`install-on-vercel.md`). The displayed name comes from the `title` field, so renaming a title does not break links.
* Moving or renaming a file changes its path, so update links that point to it. Moves are recorded in the [update log](/log.md).

### Images and attachments

Paste or upload images in the editor; they are committed to the `assets/` folder and referenced with a bundle-absolute path:

```markdown
![](/assets/Screenshot-2026-07-19-at-10.28.56.png)
```

### Markdown

Standard GitHub-flavored Markdown works: headings, tables, task lists, fenced code blocks with syntax highlighting. Keep one `#`-level structure per page and start body headings at `##`; the page title is rendered from the frontmatter.

## Conventions on top of OKF

Commonplace adds a few conventions of its own: `.commonplace/settings.yaml` for [wiki settings](/configuration.md), the automatic [update log](/log.md), and `index.md` as the landing page of the bundle and of each folder.
