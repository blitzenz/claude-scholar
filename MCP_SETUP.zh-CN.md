# MCP 服务配置指南

Claude Scholar 依赖 MCP（Model Context Protocol）服务器提供扩展能力。MCP 服务器**不包含**在本仓库中，用户需自行安装和配置。

## 所需 MCP 服务

### 1. Zotero MCP（研究工作流）

**使用场景**: `literature-reviewer` agent、`/research-init`、`/zotero-review`、`/zotero-notes` 命令

**安装包**: [Galaxy-Dawn/zotero-mcp](https://github.com/Galaxy-Dawn/zotero-mcp) — Web API 模式，支持无需 Zotero 桌面客户端的远程访问。

#### 功能

| 类别 | 工具 |
|------|------|
| **导入** | `zotero_add_items_by_doi`, `zotero_add_items_by_arxiv`, `zotero_add_item_by_url` |
| **读取** | `zotero_get_collections`, `zotero_get_collection_items`, `zotero_search_items`, `zotero_semantic_search` |
| **更新** | `zotero_update_item`, `zotero_update_note`, `zotero_create_collection`, `zotero_move_items_to_collection` |
| **删除** | `zotero_delete_items`（移至回收站）, `zotero_delete_collection` |
| **PDF** | `zotero_find_and_attach_pdfs`（通过 Unpaywall）, `zotero_add_linked_url_attachment` |

#### 前置条件

1. 安装 [Zotero](https://www.zotero.org/)（可选，用于本地模式）
2. 从 [zotero.org/settings/keys](https://www.zotero.org/settings/keys) 获取 API 密钥和库 ID

#### 安装

```bash
# 通过 uv 安装（推荐）
uv tool install git+https://github.com/Galaxy-Dawn/zotero-mcp.git
```

#### 配置

选择您的平台：

##### Claude Code

Claude Code v2.1.5+：在 `~/.claude.json` 的 `mcpServers` 中添加。

更早版本：在 `~/.claude/settings.json` 的 `mcpServers` 中添加：

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

在 `~/.codex/config.toml` 中添加：

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

在 `~/.opencode/opencode.jsonc` 中添加：

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

然后在 `~/.zshrc` 中设置环境变量：

```bash
# Zotero MCP
export ZOTERO_API_KEY="your-api-key"
export ZOTERO_LIBRARY_ID="your-library-id"
export ZOTERO_LIBRARY_TYPE="user"
export UNPAYWALL_EMAIL="your-email@example.com"
export UNSAFE_OPERATIONS="all"
```

#### 环境变量

| 变量 | 必需 | 说明 |
|------|------|------|
| `ZOTERO_API_KEY` | 是 | 您的 Zotero API 密钥 |
| `ZOTERO_LIBRARY_ID` | 是 | 您的库 ID（数字） |
| `ZOTERO_LIBRARY_TYPE` | 是 | `user` 或 `group` |
| `UNPAYWALL_EMAIL` | 否 | 用于 Unpaywall PDF 搜索的邮箱 |
| `UNSAFE_OPERATIONS` | 否 | `items`（delete_items）, `all`（delete_collection） |
| `NO_PROXY` | 否 | 绕过本地代理 |

#### 可用工具

| 工具 | 功能 |
|------|------|
| `zotero_get_collections` | 列出所有集合 |
| `zotero_get_collection_items` | 获取集合中的条目 |
| `zotero_search_items` | 按关键词搜索库 |
| `zotero_search_by_tag` | 按标签搜索 |
| `zotero_get_item_metadata` | 获取条目元数据和摘要 |
| `zotero_get_item_fulltext` | 读取 PDF 全文 |
| `zotero_get_annotations` | 获取 PDF 标注 |
| `zotero_get_notes` | 获取笔记 |
| `zotero_semantic_search` | 语义搜索（使用嵌入向量） |
| `zotero_advanced_search` | 高级搜索 |
| `zotero_add_items_by_doi` | 通过 DOI 导入论文 |
| `zotero_add_items_by_arxiv` | 通过 arXiv ID 导入预印本 |
| `zotero_add_item_by_url` | 将网页保存为条目 |
| `zotero_update_item` | 更新条目字段 |
| `zotero_update_note` | 更新笔记内容 |
| `zotero_create_collection` | 创建集合 |
| `zotero_move_items_to_collection` | 在集合间移动条目 |
| `zotero_update_collection` | 重命名集合 |
| `zotero_delete_collection` | 删除集合 |
| `zotero_delete_items` | 将条目移至回收站 |
| `zotero_find_and_attach_pdfs` | 查找并附加开放获取 PDF |
| `zotero_add_linked_url_attachment` | 添加链接 URL 附件 |

### 2. Obsidian MCP（Obsidian Vault 工作流）

**使用场景**: `literature-reviewer-obsidian` agent、`/obsidian-review`、`/obsidian-notes` 命令

**安装包**: [cyanheads/obsidian-mcp-server](https://github.com/cyanheads/obsidian-mcp-server) — 通过 [Obsidian Local REST API 插件](https://github.com/coddingtonbear/obsidian-local-rest-api) 接入 Obsidian vault。

#### 功能

| 类别 | 工具 |
|------|------|
| **读写** | `obsidian_read_note`, `obsidian_update_note`, `obsidian_delete_note` |
| **搜索** | `obsidian_global_search`, `obsidian_search_replace` |
| **组织/元数据** | `obsidian_list_notes`, `obsidian_manage_frontmatter`, `obsidian_manage_tags` |

#### 前置条件

1. 安装 [Obsidian](https://obsidian.md/)
2. 安装并启用社区插件 **Local REST API**（Obsidian → Settings → Community plugins）
3. 在插件设置中生成/复制 API key
4. 推荐使用非加密 HTTP `http://127.0.0.1:27123`（避免 SSL 问题）。如必须使用 HTTPS（`https://127.0.0.1:27124`），可能需要关闭 SSL 校验。

#### 安装

无需安装（推荐）：使用 `npx` 直接运行。

#### 配置

选择您的平台：

##### Claude Code

Claude Code v2.1.5+：在 `~/.claude.json` 的 `mcpServers` 中添加。

更早版本：在 `~/.claude/settings.json` 的 `mcpServers` 中添加：

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

如必须使用 HTTPS：

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

在 `~/.codex/config.toml` 中添加：

```toml
[mcp_servers.obsidian]
command = "npx"
args = ["obsidian-mcp-server"]
enabled = true

[mcp_servers.obsidian.env]
OBSIDIAN_API_KEY = "your-api-key"
OBSIDIAN_BASE_URL = "http://127.0.0.1:27123"
# OBSIDIAN_VERIFY_SSL = "false" # 仅在使用 https://127.0.0.1:27124 时需要
```

##### OpenCode

在 `~/.opencode/opencode.jsonc` 中添加：

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

然后在 `~/.zshrc` 中设置环境变量：

```bash
# Obsidian MCP
export OBSIDIAN_API_KEY="your-api-key"
export OBSIDIAN_BASE_URL="http://127.0.0.1:27123"
```

#### 环境变量

| 变量 | 必需 | 说明 |
|------|------|------|
| `OBSIDIAN_API_KEY` | 是 | Obsidian Local REST API 插件生成的 API key |
| `OBSIDIAN_BASE_URL` | 是 | 插件 Base URL（推荐：`http://127.0.0.1:27123`） |
| `OBSIDIAN_VERIFY_SSL` | 否 | 使用 HTTPS 且为自签证书时可设置为 `false` |

#### 可用工具

| 工具 | 功能 |
|------|------|
| `obsidian_list_notes` | 列出目录下的笔记与子目录 |
| `obsidian_read_note` | 读取笔记内容与元信息 |
| `obsidian_update_note` | 追加/前置/覆盖写入（可创建文件） |
| `obsidian_global_search` | 全库搜索 |
| `obsidian_search_replace` | 笔记内搜索替换 |
| `obsidian_manage_frontmatter` | 读取/设置/删除 YAML frontmatter 键 |
| `obsidian_manage_tags` | 添加/移除/列出笔记标签 |
| `obsidian_delete_note` | 永久删除笔记 |

#### Obsidian URI（可选）

Obsidian 支持 `obsidian://` URI scheme（open/search/new 等），便于一键跳转。

示例：

- `obsidian://open?vault=my%20vault&file=Research%2FMyPaper`
- `obsidian://search?vault=my%20vault&query=%23to-read`

详见：Obsidian Help → "Obsidian URI"（包含 URL 编码规则）。

#### 备选 MCP Server（filesystem fallback）

如果你不想依赖 REST 插件（或需要目录操作），可以尝试 [newtype-01/obsidian-mcp](https://github.com/newtype-01/obsidian-mcp)（`@huangyihe/obsidian-mcp`）。

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

注意：工具名不同（例如 `list_notes`, `read_note`, `update_note`），需要相应调整 prompts/commands。

### 3. 浏览器自动化 MCP（可选）

用途: Chrome 浏览器控制、网页交互。

#### 配置

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

## 验证

配置完成后，重启您的 CLI 并验证 MCP 服务是否连接：

```
# Zotero 示例：
> 列出我的 Zotero 集合

# Obsidian 示例：
> 列出 "Research-XYZ-2026-03/Core Papers" 目录下的笔记
```

如果工具返回了数据（集合/笔记列表），说明配置成功。

## 常见问题

| 问题 | 解决方案 |
|------|---------|
| 工具返回错误 | 检查 API 密钥和库 ID 是否正确 |
| PDF 附加失败 | 确保已设置 `UNPAYWALL_EMAIL` |
| 删除操作被阻止 | 设置 `UNSAFE_OPERATIONS=items` 或 `all` |
| HTTP 错误 | 检查 `NO_PROXY` 是否包含 localhost |
| Obsidian 401/403 | 检查 `OBSIDIAN_API_KEY`（插件 key），并重启 Obsidian |
| Obsidian 连接被拒绝 | 确认 Obsidian 正在运行，`OBSIDIAN_BASE_URL` 是否正确 |
| Obsidian SSL 报错 | 优先使用 `http://127.0.0.1:27123`，或在 HTTPS 下设置 `OBSIDIAN_VERIFY_SSL=false` |
| API 速率限制 (429) | 每批 ≤10 篇论文，批次间添加延迟 |
