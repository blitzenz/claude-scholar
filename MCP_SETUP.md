# MCP Server Setup Guide

Claude Scholar relies on MCP (Model Context Protocol) servers for extended capabilities. MCP servers are **not included** in this repository — users must install and configure them separately.

## Required MCP Servers

### 1. Zotero MCP (Research Workflow)

**Used by**: `literature-reviewer` agent, `/research-init`, `/zotero-review`, `/zotero-notes` commands

**Package**: [Galaxy-Dawn/zotero-mcp](https://github.com/Galaxy-Dawn/zotero-mcp) — Web API mode, supports remote access without Zotero desktop app.

#### Features

| Category | Tools |
|----------|-------|
| **Import** | `zotero_add_items_by_doi`, `zotero_add_items_by_arxiv`, `zotero_add_item_by_url` |
| **Read** | `zotero_get_collections`, `zotero_get_collection_items`, `zotero_search_items`, `zotero_semantic_search` |
| **Update** | `zotero_update_item`, `zotero_update_note`, `zotero_create_collection`, `zotero_move_items_to_collection` |
| **Delete** | `zotero_delete_items` (to trash), `zotero_delete_collection` |
| **PDF** | `zotero_find_and_attach_pdfs` (via Unpaywall), `zotero_add_linked_url_attachment` |

#### Prerequisites

1. Install [Zotero](https://www.zotero.org/) (optional, for local mode)
2. Get Zotero API key and library ID from [zotero.org/settings/keys](https://www.zotero.org/settings/keys)

#### Installation

```bash
# Install via uv (recommended)
uv tool install git+https://github.com/Galaxy-Dawn/zotero-mcp.git
```

#### Configuration

Choose your platform below:

##### Claude Code

For Claude Code v2.1.5+, add to your `~/.claude.json` under `mcpServers`.

For earlier versions, add to your `~/.claude/settings.json` under `mcpServers`:

```json
{
  "mcpServers": {
    "zotero": {
      "command": "zotero-mcp",
      "args": ["serve"],
      "env": {
        "ZOTERO_API_KEY": "your-api-key",
        "ZOTERO_LIBRARY_ID": "your-library-id",
        "ZOTERO_LIBRARY_TYPE": "user",
        "UNPAYWALL_EMAIL": "your-email@example.com",
        "UNSAFE_OPERATIONS": "all"
      }
    }
  }
}
```

##### Codex CLI

Add to your `~/.codex/config.toml`:

```toml
[mcp_servers.zotero]
command = "zotero-mcp"
args = ["serve"]
enabled = true

[mcp_servers.zotero.env]
ZOTERO_API_KEY = "your-api-key"
ZOTERO_LIBRARY_ID = "your-library-id"
ZOTERO_LIBRARY_TYPE = "user"
UNPAYWALL_EMAIL = "your-email@example.com"
UNSAFE_OPERATIONS = "all"
NO_PROXY = "localhost,127.0.0.1"
```

##### OpenCode

Add to your `~/.opencode/opencode.jsonc`:

```jsonc
{
  "mcp": {
    "zotero": {
      "type": "local",
      "command": ["zotero-mcp", "serve"],
      "enabled": true
    }
  }
}
```

Then set environment variables in `~/.zshrc`:

```bash
# Zotero MCP
export ZOTERO_API_KEY="your-api-key"
export ZOTERO_LIBRARY_ID="your-library-id"
export ZOTERO_LIBRARY_TYPE="user"
export UNPAYWALL_EMAIL="your-email@example.com"
export UNSAFE_OPERATIONS="all"
```

#### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ZOTERO_API_KEY` | Yes | Your Zotero API key |
| `ZOTERO_LIBRARY_ID` | Yes | Your library ID (numeric) |
| `ZOTERO_LIBRARY_TYPE` | Yes | `user` or `group` |
| `UNPAYWALL_EMAIL` | No | Email for Unpaywall PDF search |
| `UNSAFE_OPERATIONS` | No | `items` (delete_items), `all` (delete_collection) |
| `NO_PROXY` | No | Bypass proxy for localhost |

#### Available Tools

| Tool | Purpose |
|------|---------|
| `zotero_get_collections` | List all collections |
| `zotero_get_collection_items` | Get items in a collection |
| `zotero_search_items` | Search library by keyword |
| `zotero_search_by_tag` | Search by tag |
| `zotero_get_item_metadata` | Get item metadata and abstract |
| `zotero_get_item_fulltext` | Read PDF full text |
| `zotero_get_annotations` | Get PDF annotations |
| `zotero_get_notes` | Get notes |
| `zotero_semantic_search` | Semantic search (uses embeddings) |
| `zotero_advanced_search` | Advanced search |
| `zotero_add_items_by_doi` | Import papers by DOI |
| `zotero_add_items_by_arxiv` | Import preprints by arXiv ID |
| `zotero_add_item_by_url` | Save webpage as item |
| `zotero_update_item` | Update item fields |
| `zotero_update_note` | Update note content |
| `zotero_create_collection` | Create collection |
| `zotero_move_items_to_collection` | Move items between collections |
| `zotero_update_collection` | Rename collection |
| `zotero_delete_collection` | Delete collection |
| `zotero_delete_items` | Move items to trash |
| `zotero_find_and_attach_pdfs` | Find and attach OA PDFs |
| `zotero_add_linked_url_attachment` | Add linked URL attachment |

### 2. Obsidian MCP (Obsidian Vault Workflow)

**Used by**: `literature-reviewer-obsidian` agent, `/obsidian-review`, `/obsidian-notes` commands

**Package**: [cyanheads/obsidian-mcp-server](https://github.com/cyanheads/obsidian-mcp-server) — integrates with the [Obsidian Local REST API plugin](https://github.com/coddingtonbear/obsidian-local-rest-api).

#### Features

| Category | Tools |
|----------|-------|
| **Read/Write** | `obsidian_read_note`, `obsidian_update_note`, `obsidian_delete_note` |
| **Search** | `obsidian_global_search`, `obsidian_search_replace` |
| **Organize** | `obsidian_list_notes`, `obsidian_manage_frontmatter`, `obsidian_manage_tags` |

#### Prerequisites

1. Install [Obsidian](https://obsidian.md/)
2. Install and enable the community plugin **Local REST API** (Obsidian → Settings → Community plugins)
3. In the plugin settings, generate/copy your API key
4. Recommended: use non-encrypted HTTP `http://127.0.0.1:27123` (no SSL issues). If you must use HTTPS (`https://127.0.0.1:27124`), you may need to disable SSL verification.

#### Installation

No installation required (recommended): run via `npx`.

#### Configuration

Choose your platform below:

##### Claude Code

For Claude Code v2.1.5+, add to your `~/.claude.json` under `mcpServers`.

For earlier versions, add to your `~/.claude/settings.json` under `mcpServers`:

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["obsidian-mcp-server"],
      "env": {
        "OBSIDIAN_API_KEY": "your-api-key",
        "OBSIDIAN_BASE_URL": "http://127.0.0.1:27123"
      }
    }
  }
}
```

If you must use HTTPS:

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["obsidian-mcp-server"],
      "env": {
        "OBSIDIAN_API_KEY": "your-api-key",
        "OBSIDIAN_BASE_URL": "https://127.0.0.1:27124",
        "OBSIDIAN_VERIFY_SSL": "false"
      }
    }
  }
}
```

##### Codex CLI

Add to your `~/.codex/config.toml`:

```toml
[mcp_servers.obsidian]
command = "npx"
args = ["obsidian-mcp-server"]
enabled = true

[mcp_servers.obsidian.env]
OBSIDIAN_API_KEY = "your-api-key"
OBSIDIAN_BASE_URL = "http://127.0.0.1:27123"
# OBSIDIAN_VERIFY_SSL = "false" # only when using https://127.0.0.1:27124
```

##### OpenCode

Add to your `~/.opencode/opencode.jsonc`:

```jsonc
{
  "mcp": {
    "obsidian": {
      "type": "local",
      "command": ["npx", "obsidian-mcp-server"],
      "enabled": true
    }
  }
}
```

Then set environment variables in `~/.zshrc`:

```bash
# Obsidian MCP
export OBSIDIAN_API_KEY="your-api-key"
export OBSIDIAN_BASE_URL="http://127.0.0.1:27123"
```

#### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `OBSIDIAN_API_KEY` | Yes | API key from the Obsidian Local REST API plugin |
| `OBSIDIAN_BASE_URL` | Yes | Base URL for the plugin (recommended: `http://127.0.0.1:27123`) |
| `OBSIDIAN_VERIFY_SSL` | No | Set to `false` if using HTTPS with self-signed cert |

#### Available Tools

| Tool | Purpose |
|------|---------|
| `obsidian_list_notes` | List notes and folders in a vault directory |
| `obsidian_read_note` | Read note content and metadata |
| `obsidian_update_note` | Append/prepend/overwrite note content (can create files) |
| `obsidian_global_search` | Search across the vault |
| `obsidian_search_replace` | Search-and-replace within a note |
| `obsidian_manage_frontmatter` | Get/set/delete YAML frontmatter keys |
| `obsidian_manage_tags` | Add/remove/list tags for a note |
| `obsidian_delete_note` | Permanently delete a note |

#### Obsidian URI (Optional)

For quick navigation, Obsidian supports the `obsidian://` URI scheme (open/search/new, etc.).

Examples:

- `obsidian://open?vault=my%20vault&file=Research%2FMyPaper`
- `obsidian://search?vault=my%20vault&query=%23to-read`

See: Obsidian Help → "Obsidian URI" for full details and URL encoding rules.

#### Alternative MCP Server (filesystem fallback)

If you don't want to rely on the REST plugin (or want folder operations), you can try [newtype-01/obsidian-mcp](https://github.com/newtype-01/obsidian-mcp) (`@huangyihe/obsidian-mcp`).

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["@huangyihe/obsidian-mcp"],
      "env": {
        "OBSIDIAN_VAULT_PATH": "/path/to/your/vault",
        "OBSIDIAN_API_TOKEN": "your_api_token",
        "OBSIDIAN_API_PORT": "27123"
      }
    }
  }
}
```

Note: tool names differ (e.g. `list_notes`, `read_note`, `update_note`), so you may need to adapt prompts/commands.

### 3. Browser Automation MCP (Optional)

Used for: Chrome browser control, web page interaction.

#### Configuration

```json
{
  "mcpServers": {
    "streamable-mcp-server": {
      "type": "streamable-http",
      "url": "http://127.0.0.1:12306/mcp"
    }
  }
}
```

## Verification

After configuration, restart your CLI and verify MCP servers are connected:

```
# Zotero example:
> List my Zotero collections

# Obsidian example:
> List notes under "Research-XYZ-2026-03/Core Papers"
```

If the tool responds with your data (collections/notes), the setup is complete.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Tools return error | Check API key and library ID are correct |
| PDF attach fails | Ensure `UNPAYWALL_EMAIL` is set |
| Delete operations blocked | Set `UNSAFE_OPERATIONS=items` or `all` |
| HTTP errors | Check `NO_PROXY` includes localhost |
| Obsidian 401/403 | Check `OBSIDIAN_API_KEY` (plugin key) and restart Obsidian |
| Obsidian connection refused | Check Obsidian is running and `OBSIDIAN_BASE_URL` is correct |
| Obsidian SSL errors | Prefer `http://127.0.0.1:27123` or set `OBSIDIAN_VERIFY_SSL=false` for HTTPS |
| API rate limit (429) | Batch ≤10 papers at a time, add delays between batches |
