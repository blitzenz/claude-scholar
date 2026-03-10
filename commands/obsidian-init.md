---
name: obsidian-init
description: Initialize an Obsidian research folder structure (Zotero-like) for a topic and create an index note in your vault
args:
  - name: topic
    description: Research topic or keywords
    required: true
  - name: parent
    description: Optional parent folder path in the vault (default: vault root)
    required: false
    default: ""
tags: [Research, Obsidian, Workflow, Project Setup]
---

# /obsidian-init - Obsidian Research Folder Bootstrap

Create a Zotero-like research folder structure in Obsidian for "$topic", and write an index note.

## Usage

```bash
/obsidian-init "transformer interpretability"
```

```bash
/obsidian-init "speech decoding" "Research"
```

## Workflow

### Step 1: Derive Folder Name

1. Convert "$topic" into a short keyword (PascalCase or kebab-case)
2. Use current year and month: `YYYY-MM`
3. Construct the folder name: `Research-{Topic}-{YYYY-MM}`

Example:

`Research-TransformerInterpretability-2026-03`

If `parent` is provided, the full base path is:

`{parent}/Research-{Topic}-{YYYY-MM}`

### Step 2: Create Index Note

Use `mcp__obsidian__obsidian_update_note` to create:

- `{base}/README.md`

Include:
- A short project description
- Links to subfolders (Core Papers, Methods, Applications, Baselines, To-Read)
- A minimal frontmatter schema reminder (`type: paper`, `doi`, `year`, etc.)

### Step 3: Ensure Subfolders Exist (Best-Effort)

Create placeholder index notes (so folders exist even if the API doesn't create directories automatically):

- `{base}/Core Papers/_index.md`
- `{base}/Methods/_index.md`
- `{base}/Applications/_index.md`
- `{base}/Baselines/_index.md`
- `{base}/To-Read/_index.md`

If any write fails due to missing directories:
- Ask the user to create the folder(s) in Obsidian UI first, then rerun.

## Notes

- This command assumes an MCP server named `obsidian` is configured.
- Folder creation behavior depends on the Obsidian REST plugin/version; always handle missing-folder errors gracefully.
