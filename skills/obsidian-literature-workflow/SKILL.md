---
name: Obsidian Literature Workflow
description: Use this skill when the user manages papers/reading notes in an Obsidian vault and wants a Zotero-like workflow (folder/tag selection → batch reading → structured notes → synthesis) via an Obsidian MCP server.
version: 0.1.0
---

# Obsidian Literature Workflow

This skill enables a Zotero-like literature review workflow when a user's paper library lives in an Obsidian vault (Markdown notes + YAML frontmatter), using MCP tools to list/read/search/update notes.

## When to Use

Use this skill when the user says:

- "My papers/notes are in Obsidian"
- "Please review this folder/tag in my vault"
- "Generate structured paper notes and a literature review from my vault"

## Requirements (Recommended Setup)

1. **Obsidian Desktop** installed and your vault is accessible
2. **Obsidian Community Plugin: Local REST API** enabled
3. **MCP server: `obsidian-mcp-server`** configured in your MCP client

Minimum env vars:

- `OBSIDIAN_API_KEY`
- `OBSIDIAN_BASE_URL` (recommended: `http://127.0.0.1:27123`)

## Concept Mapping (Zotero → Obsidian)

| Zotero concept | Obsidian concept |
|---|---|
| Collection | Folder (or a tag-based "virtual collection") |
| Item (paper) | A Markdown note (one note per paper) |
| Metadata fields | YAML frontmatter (`title`, `authors`, `doi`, ...) |
| Full text | Note content (Phase 2: optionally link a `pdf:` path and extract text) |

## Recommended Vault Layout

Mirror the existing Claude Scholar Zotero structure in your vault:

```
Research-{Topic}-{YYYY-MM}/
  Core Papers/
  Methods/
  Applications/
  Baselines/
  To-Read/
```

This lets you reuse the same mental model: "folder = collection".

## Recommended Frontmatter Schema (Minimal, Stable)

Keep metadata stable so notes are machine-readable:

```yaml
---
type: paper
title: "Paper Title"
authors: ["A. Author", "B. Author"]
year: 2026
venue: "NeurIPS"
doi: "10.xxxx/xxxxx"
url: "https://doi.org/10.xxxx/xxxxx"
citekey: "author2026paper"
status: "to-read" # to-read|reading|reviewed
tags: ["research/topic-x", "method/transformer"]
---
```

Notes:
- Prefer `type: paper` for easy filtering.
- Store tags in `tags:` (frontmatter) and optionally inline `#tags` for human workflows.

## Core MCP Tools (obsidian-mcp-server)

These tool names assume your MCP server id is `obsidian`, so tool calls look like `mcp__obsidian__obsidian_read_note`.

- `obsidian_list_notes(dirPath, ...)` — list notes under a folder (best for folder-based collections)
- `obsidian_read_note(filePath, format?, includeStat?)` — read a paper note
- `obsidian_global_search(query, searchInPath?, ...)` — tag-based selection and keyword search
- `obsidian_manage_frontmatter(filePath, operation, key, value?)` — read/update metadata safely
- `obsidian_manage_tags(filePath, operation, tags)` — add/remove/list tags
- `obsidian_update_note(targetType, content, targetIdentifier?, wholeFileMode)` — write back (append/prepend/overwrite; can create new files)
- `obsidian_delete_note(filePath)` — delete notes (avoid unless explicitly requested)

## Workflow: Folder/Tag → Notes → Synthesis

### Step 1: Select the Corpus

**Folder mode (recommended):**
- Ask the user for a folder path relative to vault root, e.g. `Research-XYZ-2026-03/Core Papers`
- Call `obsidian_list_notes(dirPath=...)` and collect Markdown note paths

**Tag mode:**
- Ask the user for a tag, e.g. `to-read` or `research/topic-x`
- Use `obsidian_global_search(query=\"#to-read\")` (and optionally `searchInPath=\"Research-XYZ-2026-03\"`)
- Extract unique file paths from results and filter to `.md`

### Step 2: Read Notes + Parse Metadata

For each file:
1. `obsidian_read_note(filePath, format=\"markdown\", includeStat=true)`
2. If needed, read metadata keys via `obsidian_manage_frontmatter(operation=\"get\")`

### Step 3: Generate Structured Reading Notes (Markdown Output)

For each paper, produce:
- Research question & motivation
- Core method (key idea + modules)
- Experiments (datasets/metrics/main results/ablation)
- Limitations & open questions
- Relevance to the user's research

### Step 4: Synthesis Output

Across the set:
- Group by theme/method
- Identify trends + gaps
- Produce a comparison matrix (method × dataset × metric, when available)
- Write `literature-review.md` and optionally `paper-notes/*.md` in the current project directory

### Step 5 (Optional): Write Back to Obsidian

Default is **no write-back**.

If the user requests writing results into the vault:
- Prefer creating new notes under a dedicated folder (e.g. `Research-.../paper-notes/`) to avoid damaging originals.
- Use `obsidian_update_note(wholeFileMode=\"overwrite\")` only on output notes you own.
- Update metadata safely via `obsidian_manage_frontmatter`:
  - `status: reviewed`
  - `tags: ["reviewed"]`

## Obsidian URI Shortcuts (Optional)

Obsidian supports `obsidian://` links for quick navigation:

- Open a note: `obsidian://open?vault=my%20vault&file=Research%2FMyPaper`
- Search: `obsidian://search?vault=my%20vault&query=%23to-read`

Remember to URI-encode reserved characters (`/` → `%2F`, space → `%20`).

## Troubleshooting

- **Connection refused / timeout**: ensure Obsidian is running and the Local REST API plugin server is enabled; confirm `OBSIDIAN_BASE_URL`.
- **401 Unauthorized**: API key is wrong (regenerate/copy again).
- **SSL errors**: use HTTP `http://127.0.0.1:27123` or set `OBSIDIAN_VERIFY_SSL=false` when using HTTPS.
- **Search too slow on large vaults**: narrow `searchInPath` to a research folder; prefer folder mode over global search.
