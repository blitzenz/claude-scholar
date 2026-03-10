---
name: obsidian-review
description: Read and analyze papers from an existing Obsidian vault folder (or tag query), generate structured notes and a literature review (Zotero-like workflow)
args:
  - name: query
    description: Obsidian folder path relative to vault root, or tag query like "tag:to-read"
    required: true
  - name: depth
    description: Analysis depth (quick/deep)
    required: false
    default: deep
  - name: write_back
    description: Whether to write generated notes back into Obsidian (true/false)
    required: false
    default: "false"
tags: [Research, Obsidian, Literature Review, Paper Analysis]
---

# /obsidian-review - Obsidian Folder/Tag Literature Analysis

Read and analyze paper notes from Obsidian, sourced from "$query", with analysis depth "$depth".

## Usage

### Folder Mode

```bash
/obsidian-review "Research-TransformerInterpretability-2026-03/Core Papers"
```

### Tag Mode

```bash
/obsidian-review "tag:to-read" quick
```

## Workflow

### Step 1: Locate Notes

**Folder mode (recommended):**
1. Call `mcp__obsidian__obsidian_list_notes` with:
   - `dirPath: "$query"`
   - `fileExtensionFilter: "md"`
2. Collect Markdown file paths (notes) from the response.

**Tag mode:**
1. Extract the tag from `tag:xxx` (e.g. `to-read`)
2. Call `mcp__obsidian__obsidian_global_search` with:
   - `query: "#to-read"`
   - Optional: `searchInPath: "Research-..."`
3. Deduplicate results by file path and keep `.md` notes only.

Notes:
- Tag-based discovery is best-effort. For stable automation, prefer folder-based collections or include inline `#tags` in notes.

### Step 2: Read Notes

For each note path:
1. Call `mcp__obsidian__obsidian_read_note` with:
   - `filePath: "{note_path}"`
   - `format: "markdown"`
   - `includeStat: true`
2. If you need stable metadata (title/year/doi), call `mcp__obsidian__obsidian_manage_frontmatter`:
   - `operation: "get"`
   - `key: "title"` / `year` / `doi` / `url` / `authors` / `venue`

### Step 3: Generate Structured Notes (Local Output)

Create structured notes per paper:
- **Research Question**: What problem does this paper address?
- **Core Method**: What method/approach is proposed?
- **Key Findings**: What are the main results?
- **Limitations**: What are the limitations?
- **Relevance to Our Research**: How does it relate to our work?

If depth is "deep": create individual note files in `paper-notes/`, one per paper, and use them as intermediate analysis.

### Step 4: Synthesis

1. Group papers by theme/method
2. Identify common patterns and divergences
3. Generate a comparison matrix (when notes contain datasets/metrics)
4. Update or create `literature-review.md`

### Step 5 (Optional): Write Back to Obsidian

Default is `write_back=false`.

If the user requests write-back:
- Prefer creating new output notes under a dedicated folder in the vault (e.g. `{query}/paper-notes/`) rather than overwriting original notes.
- Use `mcp__obsidian__obsidian_update_note` to create/update output notes:
  - `targetType: "filePath"`
  - `targetIdentifier: "{vault_path_to_output_note}"`
  - `wholeFileMode: "overwrite"` (only for output notes you own)
- Optionally update metadata via `mcp__obsidian__obsidian_manage_frontmatter` (e.g. `status: reviewed`).

## Output Files

```
{project_dir}/
├── literature-review.md      # Structured literature review
└── paper-notes/              # Individual notes per paper (deep mode)
    ├── {paper1-title}.md
    └── {paper2-title}.md
```

## Error Handling

- **`obsidian_list_notes` fails** → Ask user to confirm folder path relative to vault root; retry with a parent folder.
- **`obsidian_read_note` fails** → Try case-insensitive path fix; fallback to `obsidian_global_search` by title to locate the file.
- **Search returns too many notes** → Narrow with `searchInPath` or switch to folder mode.
- **SSL errors** → Prefer `OBSIDIAN_BASE_URL=http://127.0.0.1:27123`, or set `OBSIDIAN_VERIFY_SSL=false` for HTTPS.

## Notes

- This command assumes an MCP server named `obsidian` is configured, so tools are available as `mcp__obsidian__...`.
- v1 does not extract PDF full text. If you keep PDFs in your vault, store a `pdf:` path in frontmatter and extend the workflow later.
