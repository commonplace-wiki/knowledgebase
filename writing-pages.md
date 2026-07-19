---
type: Wiki Page
title: Writing Pages
description: Markdown conventions, frontmatter fields, links, and assets.
timestamp: '2026-07-19T10:00:54.000Z'
---

Every page is a Markdown file with YAML frontmatter, stored in the [Git repository](/Installation/git-repository.md). You can write pages in the wiki editor, in any text editor, or through the [MCP](/mcp.md) tools; the result is the same file.

## Frontmatter

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

## Links

Link between pages with bundle-absolute paths, starting at the repository root and including the `.md` extension:

```markdown
[Installation](/Installation/index.md)
[MCP](/mcp.md)
```

External links use regular Markdown syntax with full URLs.

## Folders and filenames

* Folders are sections; a folder's `index.md` is its landing page.
* Filenames are kebab-case (`install-on-vercel.md`). The displayed name comes from the `title` field, so renaming a title does not break links.
* Moving or renaming a file changes its path, so update links that point to it. Moves are recorded in the [update log](/log.md).

## Images and attachments

Paste or upload images in the editor; they are committed to the `assets/` folder and referenced with a bundle-absolute path:

```markdown
![](/assets/Screenshot-2026-07-19-at-10.28.56.png)
```

## Markdown

Standard GitHub-flavored Markdown works: headings, tables, task lists, fenced code blocks with syntax highlighting. Keep one `#`-level structure per page and start body headings at `##`; the page title is rendered from the frontmatter.
