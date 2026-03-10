---
name: obsidian-notes
description: Batch read paper notes from an Obsidian vault folder (or tag query) and generate structured reading notes for each paper
args:
  - name: query
    description: Obsidian folder path relative to vault root, or tag query like "tag:to-read"
    required: true
  - name: format
    description: Note format (summary/detailed/comparison)
    required: false
    default: detailed
  - name: write_back
    description: Whether to write generated notes back into Obsidian (true/false)
    required: false
    default: "false"
tags: [Research, Obsidian, Reading Notes, Paper Analysis]
---

# /obsidian-notes - Obsidian Batch Reading Notes Generator

Generate structured reading notes for paper notes in "$query", in "$format" format.

## Usage

```bash
/obsidian-notes "Research-XYZ-2026-03/Core Papers" detailed
```

```bash
/obsidian-notes "tag:to-read" summary
```

## Workflow

### Step 1: Load Notes

Use the same discovery logic as `/obsidian-review`:

- Folder mode: `mcp__obsidian__obsidian_list_notes(dirPath, fileExtensionFilter="md")`
- Tag mode: `mcp__obsidian__obsidian_global_search(query="#tag")` + dedupe by file path

### Step 2: Read and Annotate

For each note:
1. `mcp__obsidian__obsidian_read_note(filePath, format="markdown", includeStat=true)`
2. Optionally fetch structured metadata via `mcp__obsidian__obsidian_manage_frontmatter(operation="get")`
3. Generate notes based on the requested format:

**summary format**
- One-paragraph summary per paper
- One-sentence core contribution

**detailed format (default)**
- One-sentence positioning
- Problem and motivation
- Method (core approach, key modules, differences)
- Experiments (datasets/metrics/overall results/ablation)
- Limitations and questions
- Relationship to other works
- Value for my research

**comparison format**
- Side-by-side comparison tables
- Method comparison matrix
- Performance comparison (if notes contain comparable metrics)

### Step 3: Output

1. Create `reading-notes-{query}.md` containing all notes
2. If format is "comparison", also create `comparison-matrix.md`
3. List notes that are missing required metadata (no title/year/doi) for manual cleanup

### Step 4 (Optional): Write Back to Obsidian

Default is `write_back=false`.

If write-back is requested:
- Create/update a single vault note like `{query}/reading-notes.md`, or per-paper outputs under `{query}/paper-notes/`.
- Use `mcp__obsidian__obsidian_update_note` with:
  - `targetType: "filePath"`
  - `targetIdentifier: "{vault_path_to_output_note}"`
  - `wholeFileMode: "overwrite"` (only for output notes you own)
- Optionally mark processed papers via frontmatter:
  - `status: reviewed`
  - add tag `reviewed`

## Output Files

```
{project_dir}/
├── reading-notes-{query}.md    # Reading notes for all papers
└── comparison-matrix.md        # Comparison matrix (comparison format)
```

## Notes

- This command assumes an MCP server named `obsidian` is configured (tools available as `mcp__obsidian__...`).
- For reliable automation, prefer folder-based collections; tag mode is best-effort and depends on your tagging convention.
