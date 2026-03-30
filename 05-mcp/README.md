<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP（模型上下文协议）

此文件夹包含有关 MCP 服务器配置和 Claude Code 用法的综合文档和示例。

## 概述

MCP（模型上下文协议）是 Claude 访问外部工具、API 和实时数据源的标准化方式。与内存不同，MCP 提供对不断变化的数据的实时访问。

主要特点：
- 实时访问外部服务
- 实时数据同步
- 可扩展的架构
- 安全认证
- 基于工具的交互

## MCP 架构
```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["External Service"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#e8f5e9,stroke:#333,color:#333
```
## MCP 生态系统
```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|File I/O| G["Local Files"]
    C -->|API| H["GitHub Repos"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack Workspace"]
    F -->|Docs| K["Google Drive"]

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#f3e5f5,stroke:#333,color:#333
    style G fill:#e8f5e9,stroke:#333,color:#333
    style H fill:#e8f5e9,stroke:#333,color:#333
    style I fill:#e8f5e9,stroke:#333,color:#333
    style J fill:#e8f5e9,stroke:#333,color:#333
    style K fill:#e8f5e9,stroke:#333,color:#333
```
## MCP安装方法

Claude Code 支持 MCP 服务器连接的多种传输协议：

### HTTP 传输（推荐）
```bash
# Basic HTTP connection
claude mcp add --transport http notion https://mcp.notion.com/mcp

# HTTP with authentication header
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"
```
### Stdio 交通（本地）

对于本地运行的 MCP 服务器：
```bash
# Local Node.js server
claude mcp add --transport stdio myserver -- npx @myorg/mcp-server

# With environment variables
claude mcp add --transport stdio myserver --env KEY=value -- npx server
```
### SSE 传输（已弃用）

服务器发送的事件传输已被弃用，取而代之的是 `http` 但仍然受支持：
```bash
claude mcp add --transport sse legacy-server https://example.com/sse
```
### WebSocket 传输

用于持久双向连接的 WebSocket 传输：
```bash
claude mcp add --transport ws realtime-server wss://example.com/mcp
```
### Windows 特定说明

On native Windows (not WSL), use `cmd /c` for npx commands:
```bash
claude mcp add --transport stdio my-server -- cmd /c npx -y @some/package
```
### OAuth 2.0 身份验证

Claude Code supports OAuth 2.0 for MCP servers that require it. When connecting to an OAuth-enabled server, Claude Code handles the entire authentication flow:
```bash
# Connect to an OAuth-enabled MCP server (interactive flow)
claude mcp add --transport http my-service https://my-service.example.com/mcp

# Pre-configure OAuth credentials for non-interactive setup
claude mcp add --transport http my-service https://my-service.example.com/mcp \
  --client-id "your-client-id" \
  --client-secret "your-client-secret" \
  --callback-port 8080
```
|特色|描述 |
|---------|-------------|
| **交互式 OAuth** |使用 `/mcp` 触发基于浏览器的 OAuth 流程 |
| **预配置的 OAuth 客户端** |适用于 Notion、Stripe 等常见服务的内置 OAuth 客户端 (v2.1.30+) |
| **预配置凭据** |用于自动设置的 `--client-id`、`--client-secret`、`--callback-port` 标志 |
| **Token存储** |Token安全地存储在您的系统钥匙串中 |
| **升级授权** |支持特权操作的升级身份验证 |
| **发现缓存** |缓存 OAuth 发现元数据以加快重新连接速度 |
| **元数据覆盖** | `.mcp.json` 中的 `oauth.authServerMetadataUrl` 覆盖默认 OAuth 元数据发现 |

#### 覆盖 OAuth 元数据发现

如果您的 MCP 服务器在标准 OAuth 元数据端点 (`/.well-known/oauth-authorization-server`) 上返回错误，但公开了工作的 OIDC 端点，您可以告诉 Claude Code 从特定 URL 获取 OAuth 元数据。在服务器配置的 `oauth` 对象中设置 `authServerMetadataUrl`：
```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "https://mcp.example.com/mcp",
      "oauth": {
        "authServerMetadataUrl": "https://auth.example.com/.well-known/openid-configuration"
      }
    }
  }
}
```
URL 必须使用 `https://`。此选项需要 Claude Code v2.1.64 或更高版本。

### Claude.ai MCP 连接器

您的 Claude.ai 帐户中配置的 MCP 服务器将自动在 Claude Code 中可用。这意味着您通过 Claude.ai Web 界面设置的任何 MCP 连接都可以访问，无需额外配置。

Claude.ai MCP 连接器还提供 `--print` 模式 (v2.1.83+)，支持非交互式和脚本化使用。

要在 Claude 代码中禁用 Claude.ai MCP 服务器，请将 `ENABLE_CLAUDEAI_MCP_SERVERS` 环境变量设置为 `false`：
```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude
```
> **注意：** 此功能仅适用于使用 Claude.ai 帐户登录的用户。

## MCP 设置过程
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: Type /mcp
    Claude->>Claude: List available MCP servers
    Claude->>User: Show options
    User->>Claude: Select GitHub MCP
    Claude->>Config: Update configuration
    Config->>Claude: Activate connection
    Claude->>Service: Test connection
    Service-->>Claude: Authentication successful
    Claude->>User: ✅ MCP connected!
```
## MCP 工具搜索

当 MCP 工具描述超过上下文窗口的 10% 时，Claude Code 会自动启用工具搜索，以有效地选择正确的工具，而不会压垮模型上下文。

|设置|价值|描述 |
|--------|--------|-------------|
| `ENABLE_TOOL_SEARCH` | `auto`（默认）|当工具描述超过上下文的 10% 时自动启用 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` |在 `N` 工具的自定义阈值下自动启用 |
| `ENABLE_TOOL_SEARCH` | `true` |无论工具数量如何，始终启用 |
| `ENABLE_TOOL_SEARCH` | `false` |残疾人；所有工具说明均已完整发送 |

> **注意：** 工具搜索需要 Sonnet 4 或更高版本，或者 Opus 4 或更高版本。工具搜索不支持 haiku模型。

## 动态工具更新

Claude Code 支持 MCP `list_changed` 通知。当 MCP 服务器动态添加、删除或修改其可用工具时，Claude Code 会收到更新并自动调整其工具列表 - 无需重新连接或重新启动。

## MCP 启发

MCP 服务器可以通过交互式对话框请求用户结构化输入 (v2.1.49+)。这允许 MCP 服务器在工作流程中请求其他信息（例如，提示确认、从选项列表中进行选择或填写必填字段），从而为 MCP 服务器交互添加交互性。

## 工具说明及使用说明

从 v2.1.84 开始，Claude Code 对每个 MCP 服务器的工具描述和指令强制执行 **2 KB 上限**。这可以防止各个服务器通过过于冗长的工具定义消耗过多的上下文，从而减少上下文膨胀并保持交互高效。

## MCP 提示符为斜线命令

MCP 服务器可以公开在 Claude Code 中显示为斜杠命令的提示。可以使用命名约定来访问提示：
```
/mcp__<server>__<prompt>
```
例如，如果名为 `github` 的服务器公开名为 `review` 的提示，您可以将其作为 `/mcp__github__review` 调用。

## 服务器重复数据删除

当在多个范围（本地、项目、用户）定义同一 MCP 服务器时，本地配置优先。这允许您使用本地自定义覆盖项目级或用户级 MCP 设置，而不会发生冲突。

## MCP 资源来自@提及

您可以使用 `@` 提及语法直接在提示中引用 MCP 资源：
```
@server-name:protocol://resource/path
```
例如，要引用特定的数据库资源：
```
@database:postgres://mydb/users
```
这允许 Claude 获取内联 MCP 资源内容并将其包含在对话上下文中。

## MCP 范围

MCP 配置可以存储在具有不同共享级别的不同范围内：

|范围 |地点 |描述 |分享给 |需要批准 |
|--------|----------|-------------|-------------|--------------------|
| **本地**（默认）| `~/.claude.json`（在项目路径下）|对当前用户私有，仅限当前项目（在旧版本中称为 `project`）|只有你|没有 |
| **项目** | `.mcp.json` |签入 git 存储库 |团队成员 |是（首次使用）|
| **用户** | `~/.claude.json` |适用于所有项目（在旧版本中称为 `global`）|只有你|没有 |

### 使用项目范围

将项目特定的 MCP 配置存储在 `.mcp.json` 中：
```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.github.com/mcp"
    }
  }
}
```
首次使用项目 MCP 时，团队成员将看到批准提示。

## MCP 配置管理

### 添加 MCP 服务器
```bash
# Add HTTP-based server
claude mcp add --transport http github https://api.github.com/mcp

# Add local stdio server
claude mcp add --transport stdio database -- npx @company/db-server

# List all MCP servers
claude mcp list

# Get details on specific server
claude mcp get github

# Remove an MCP server
claude mcp remove github

# Reset project-specific approval choices
claude mcp reset-project-choices

# Import from Claude Desktop
claude mcp add-from-claude-desktop
```
## 可用 MCP 服务器表

| MCP 服务器 |目的|常用工具|授权 |实时|
|------------|---------|--------------|-----|------------|
| **文件系统** |文件操作 |读、写、删除|操作系统权限 | ✅ 是的 |
| **GitHub** |存储库管理| list_prs、create_issue、推送 | OAuth | ✅ 是的 |
| **Slack** |团队沟通|发送消息、列表频道 |tokens| ✅ 是的 |
| **数据库** | SQL 查询 |查询、插入、更新 |证书 | ✅ 是的 |
| **Google Docs** |文档访问 |阅读、写作、分享 | OAuth | ✅ 是的 |
| **Asana** |项目管理|创建任务、更新状态 | API 密钥 | ✅ 是的 |
| **Stripe** |付款数据|列表费用，创建发票 | API 密钥 | ✅ 是的 |
| **内存** |持久记忆|存储、检索、删除 |本地| ❌ 否 |

## 实际例子

### 示例 1：GitHub MCP 配置

**文件：** `.mcp.json`（项目根目录）
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```
**可用的 GitHub MCP 工具：**

#### 拉取请求管理
- `list_prs` - 列出存储库中的所有 PR
- `get_pr` - 获取 PR 详细信息，包括差异
- `create_pr` - 创建新 PR
- `update_pr` - 更新公关描述/标题
- `merge_pr` - 将 PR 合并到主分支
- `review_pr` - 添加评论意见

**请求示例：**
```
/mcp__github__get_pr 456

# Returns:
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```
#### 问题管理
- `list_issues` - 列出所有问题
- `get_issue` - 获取问题详细信息
- `create_issue` - 创建新问题
- `close_issue` - 关闭问题
- `add_comment` - 添加评论到问题

#### 存储库信息
- `get_repo_info` - 存储库详细信息
- `list_files` - 文件树结构
- `get_file_content` - 读取文件内容
- `search_code` - 跨代码库搜索

#### 提交操作
- `list_commits` - 提交历史记录
- `get_commit` - 具体提交详细信息
- `create_commit` - 创建新提交

**设置**：
```bash
export GITHUB_TOKEN="your_github_token"
# Or use the CLI to add directly:
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```
### 配置中的环境变量扩展

MCP 配置支持具有后备默认值的环境变量扩展。 `${VAR}` 和 `${VAR:-default}` 语法适用于以下字段：`command`、`args`、`env`、`url` 和 `headers`。
```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}",
        "X-Custom-Header": "${CUSTOM_HEADER:-default-value}"
      }
    },
    "local-server": {
      "command": "${MCP_BIN_PATH:-npx}",
      "args": ["${MCP_PACKAGE:-@company/mcp-server}"],
      "env": {
        "DB_URL": "${DATABASE_URL:-postgresql://localhost/dev}"
      }
    }
  }
}
```
变量在运行时扩展：
- `${VAR}` - 使用环境变量，如果未设置则出错
- `${VAR:-default}` - 使用环境变量，如果未设置则回退到默认值

### 示例 2：数据库 MCP 设置

**配置：**
```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```
**用法示例：**
```markdown
User: Fetch all users with more than 10 orders

Claude: I'll query your database to find that information.

# Using MCP database tool:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# Results:
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```
**设置**：
```bash
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
# Or use the CLI to add directly:
claude mcp add --transport stdio database -- npx @modelcontextprotocol/server-database
```
### 示例 3：多 MCP 工作流程

**场景：生成日报**
```markdown
# Daily Report Workflow using Multiple MCPs

## Setup
1. GitHub MCP - fetch PR metrics
2. Database MCP - query sales data
3. Slack MCP - post report
4. Filesystem MCP - save report

## Workflow

### Step 1: Fetch GitHub Data
/mcp__github__list_prs completed:true last:7days

Output:
- Total PRs: 42
- Average merge time: 2.3 hours
- Review turnaround: 1.1 hours

### Step 2: Query Database
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

Output:
- Sales: 247
- Revenue: $12,450

### Step 3: Generate Report
Combine data into HTML report

### Step 4: Save to Filesystem
Write report.html to /reports/

### Step 5: Post to Slack
Send summary to #daily-reports channel

Final Output:
✅ Report generated and posted
📊 47 PRs merged this week
💰 $12,450 in daily sales
```
**设置**：
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="your_slack_token"
# Add each MCP server via the CLI or configure them in .mcp.json
```
### 示例 4：文件系统 MCP 操作

**配置：**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```
**可用操作：**

|运营|命令 |目的|
|------------|---------|---------|
|列出文件 | `ls ~/projects` |显示目录内容 |
|读取文件 | `cat src/main.ts` |读取文件内容 |
|写入文件| `create docs/api.md` |创建新文件 |
|编辑文件 | `edit src/app.ts` |修改文件|
|搜索 | `grep "async function"` |在文件中搜索 |
|删除 | `rm old-file.js` |删除文件 |

**设置**：
```bash
# Use the CLI to add directly:
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /home/user/projects
```
## MCP 与内存：决策矩阵
```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style A fill:#fff3e0,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#fff3e0,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#e8f5e9,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
```
## 请求/响应模式
```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: Request: "SELECT * FROM users WHERE id=1"
    MCP->>DB: Execute query
    DB-->>MCP: Result set
    MCP-->>App: Return parsed data
    App->>App: Process result
    App->>App: Continue task

    Note over MCP,DB: Real-time access<br/>No caching
```
## 环境变量

将敏感凭据存储在环境变量中：
```bash
# ~/.bashrc or ~/.zshrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="xoxb-xxxxxxxxxxxxx"
```
然后在 MCP 配置中引用它们：
```json
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```
## Claude 作为 MCP 服务器 (`claude mcp serve`)

Claude Code 本身可以充当其他应用程序的 MCP 服务器。这使得外部工具、编辑器和自动化系统能够通过标准 MCP 协议利用 Claude 的功能。
```bash
# Start Claude Code as an MCP server on stdio
claude mcp serve
```
然后，其他应用程序可以像连接任何基于 stdio 的 MCP 服务器一样连接到该服务器。例如，要将 Claude Code 添加为另一个 Claude Code 实例中的 MCP 服务器：
```bash
claude mcp add --transport stdio claude-agent -- claude mcp serve
```
这对于构建多agents工作流程非常有用，其中一个 Claude 实例协调另一个实例。

## 托管 MCP 配置（企业）

对于企业部署，IT 管理员可以通过 `managed-mcp.json` 配置文件强制执行 MCP 服务器策略。此文件提供对组织范围内允许或阻止哪些 MCP 服务器的独占控制。

**地点：**
- macOS：`/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux：`~/.config/ClaudeCode/managed-mcp.json`
- Windows：`%APPDATA%\ClaudeCode\managed-mcp.json`

**特点：**
- `allowedMcpServers` -- 允许的服务器白名单
- `deniedMcpServers` -- 禁止服务器的黑名单
- 支持按服务器名称、命令和 URL 模式进行匹配
- 在用户配置之前实施组织范围的 MCP 策略
- 防止未经授权的服务器连接

**配置示例：**
```json
{
  "allowedMcpServers": [
    {
      "serverName": "github",
      "serverUrl": "https://api.github.com/mcp"
    },
    {
      "serverName": "company-internal",
      "serverCommand": "company-mcp-server"
    }
  ],
  "deniedMcpServers": [
    {
      "serverName": "untrusted-*"
    },
    {
      "serverUrl": "http://*"
    }
  ]
}
```
> **注意：** 当 `allowedMcpServers` 和 `deniedMcpServers` 都匹配服务器时，拒绝规则优先。

## Plugins提供的 MCP 服务器

Plugins可以捆绑自己的 MCP 服务器，使其在安装Plugins时自动可用。Plugins提供的 MCP 服务器可以通过两种方式定义：

1. **独立`.mcp.json`** -- 在Plugins根目录下放置一个`.mcp.json`文件
2. **Inline in `plugin.json`** -- 直接在Plugins清单中定义 MCP 服务器

使用 `${CLAUDE_PLUGIN_ROOT}` 变量来引用相对于Plugins安装目录的路径：
```json
{
  "mcpServers": {
    "plugin-tools": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/dist/mcp-server.js"],
      "env": {
        "CONFIG_PATH": "${CLAUDE_PLUGIN_ROOT}/config.json"
      }
    }
  }
}
```
## Subagents范围的 MCP

MCP 服务器可以使用 `mcpServers:` 键在agents frontmatter 内内联定义，将它们的范围限定到特定的Subagents而不是整个项目。当agents需要访问工作流中的其他agents不需要的特定 MCP 服务器时，这非常有用。
```yaml
---
mcpServers:
  my-tool:
    type: http
    url: https://my-tool.example.com/mcp
---

You are an agent with access to my-tool for specialized operations.
```
Subagents范围的 MCP 服务器仅在该agents的执行上下文中可用，并且不与父agents或同级agents共享。

## MCP 输出限制

Claude Code 对 MCP 工具输出实施限制，以防止上下文溢出：

|限制|门槛|行为 |
|--------|------------|----------|
| **警告** | 10,000 个tokens |显示输出过大的警告 |
| **默认最大值** | 25,000 个tokens |超出此限制后输出将被截断 |
| **磁盘持久性** | 50,000 个字符 |超过 50K 字符的工具结果将保存到磁盘 |

最大输出限制可通过 `MAX_MCP_OUTPUT_TOKENS` 环境变量进行配置：
```bash
# Increase the max output to 50,000 tokens
export MAX_MCP_OUTPUT_TOKENS=50000
```
## 通过代码执行解决上下文膨胀

随着 MCP 采用规模的扩大，使用数百或数千种工具连接到数十台服务器会带来重大挑战：**上下文膨胀**。这可以说是大规模 MCP 的最大问题，Anthropic 的工程团队提出了一个优雅的解决方案——使用代码执行而不是直接工具调用。

> **来源**：[Code Execution with MCP: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp) — 人类工程博客

### 问题：tokens浪费的两个来源

**1.工具定义使上下文窗口过载**

大多数 MCP 客户端会预先加载所有工具定义。当连接到数千个工具时，模型必须处理数十万个Token，然后才能读取用户的请求。

**2.中间结果消耗额外的Token**

每个中间工具结果都会通过模型的上下文。考虑将会议记录从 Google Drive 传输到 Salesforce — 完整的记录在上下文中流动**两次**：一次是在读取时，另一次是在将其写入目的地时。 2 小时的会议记录可能意味着 50,000 多个额外tokens。
```mermaid
graph LR
    A["Model"] -->|"Tool Call: getDocument"| B["MCP Server"]
    B -->|"Full transcript (50K tokens)"| A
    A -->|"Tool Call: updateRecord<br/>(re-sends full transcript)"| B
    B -->|"Confirmation"| A

    style A fill:#ffcdd2,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
```
### 解决方案：MCP 工具作为代码 API

agents并不通过上下文窗口传递工具定义和结果，而是**编写代码**来调用 MCP 工具作为 API。代码在沙盒执行环境中运行，只有最终结果返回给模型。
```mermaid
graph LR
    A["Model"] -->|"Writes code"| B["Code Execution<br/>Environment"]
    B -->|"Calls tools directly"| C["MCP Servers"]
    C -->|"Data stays in<br/>execution env"| B
    B -->|"Only final result<br/>(minimal tokens)"| A

    style A fill:#c8e6c9,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
```
#### 它是如何工作的

MCP 工具以类型函数的文件树形式呈现：
```
servers/
├── google-drive/
│   ├── getDocument.ts
│   └── index.ts
├── salesforce/
│   ├── updateRecord.ts
│   └── index.ts
└── ...
```
每个工具文件都包含一个类型化包装器：
```typescript
// ./servers/google-drive/getDocument.ts
import { callMCPTool } from "../../../client.js";

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

export async function getDocument(
  input: GetDocumentInput
): Promise<GetDocumentResponse> {
  return callMCPTool<GetDocumentResponse>(
    'google_drive__get_document', input
  );
}
```
然后，agents编写代码来编排工具：
```typescript
import * as gdrive from './servers/google-drive';
import * as salesforce from './servers/salesforce';

// Data flows directly between tools — never through the model
const transcript = (
  await gdrive.getDocument({ documentId: 'abc123' })
).content;

await salesforce.updateRecord({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  data: { Notes: transcript }
});
```
**结果：tokens使用量从约 150,000 下降至约 2,000 — 减少了 98.7%。**

### 主要优点

|效益 |描述 |
|---------|-------------|
| **逐步披露** |agents浏览文件系统以仅加载所需的工具定义，而不是预先加载所有工具 |
| **上下文有效的结果** |数据在返回模型之前在执行环境中进行过滤/转换 |
| **强大的控制流程** |循环、条件和错误处理在代码中运行，无需往返模型 |
| **隐私保护** |中间数据（PII、敏感记录）保留在执行环境中；永远不会进入模型上下文 |
| **状态持久性** |agents可以将中间结果保存到文件并构建可重用的skills函数 |

#### 示例：过滤大型数据集
```typescript
// Without code execution — all 10,000 rows flow through context
// TOOL CALL: gdrive.getSheet(sheetId: 'abc123')
//   -> returns 10,000 rows in context

// With code execution — filter in the execution environment
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(
  row => row["Status"] === 'pending'
);
console.log(`Found ${pendingOrders.length} pending orders`);
console.log(pendingOrders.slice(0, 5)); // Only 5 rows reach the model
```
#### 示例：不带往返的循环
```typescript
// Poll for a deployment notification — runs entirely in code
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({
    channel: 'C123456'
  });
  found = messages.some(
    m => m.text.includes('deployment complete')
  );
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('Deployment notification received');
```
### 需要考虑的权衡

代码执行引入了其自身的复杂性。运行agents生成的代码需要：

- 具有适当资源限制的**安全沙盒执行环境**
- **监视和记录**执行的代码
- 与直接工具调用相比，额外的**基础设施开销**

其好处——降低tokens成本、降低延迟、改进工具组合——应该与这些实施成本进行权衡。对于只有几个 MCP 服务器的agents，直接工具调用可能更简单。对于大规模agents（数十台服务器、数百种工具）来说，代码执行是一项重大改进。

### MCPorter：MCP 工具组合的运行时

[MCPorter](https://github.com/steipete/mcporter) 是一个 TypeScript 运行时和 CLI 工具包，可以在没有样板的情况下调用 MCP 服务器，并通过选择性工具公开和类型化包装器帮助减少上下文膨胀。

**它解决的问题：** MCPorter 无需预先从所有 MCP 服务器加载所有工具定义，而是让您按需发现、检查和调用特定工具 - 保持上下文精简。

**主要特点：**

|特色|描述 |
|---------|-------------|
| **零配置发现** |从 Cursor、Claude、Codex 或本地配置自动发现 MCP 服务器 |
| **类型化工具客户端** | `mcporter emit-ts` 生成 `.d.ts` 接口和准备运行的包装器 |
| **可组合 API** | `createServerProxy()` 使用 `.text()`、`.json()`、`.markdown()` 帮助程序将工具公开为驼峰命名法方法 |
| **CLI 生成** | `mcporter generate-cli` 将任何 MCP 服务器转换为具有 `--include-tools` / `--exclude-tools` 过滤功能的独立 CLI |
| **参数隐藏** |可选参数默认隐藏，减少模式冗长|

**安装：**
```bash
npx mcporter list          # No install required — discover servers instantly
pnpm add mcporter          # Add to a project
brew install steipete/tap/mcporter  # macOS via Homebrew
```
**示例 - 使用 TypeScript 编写工具：**
```typescript
import { createRuntime, createServerProxy } from "mcporter";

const runtime = await createRuntime();
const gdrive = createServerProxy(runtime, "google-drive");
const salesforce = createServerProxy(runtime, "salesforce");

// Data flows between tools without passing through the model context
const doc = await gdrive.getDocument({ documentId: "abc123" });
await salesforce.updateRecord({
  objectType: "SalesMeeting",
  recordId: "00Q5f000001abcXYZ",
  data: { Notes: doc.text() }
});
```
**示例 — CLI 工具调用：**
```bash
# Call a specific tool directly
npx mcporter call linear.create_comment issueId:ENG-123 body:'Looks good!'

# List available servers and tools
npx mcporter list
```
MCPorter 通过提供用于以类型化 API 形式调用 MCP 工具的运行时基础设施来补充上述代码执行方法，从而可以直接将中间数据保留在模型上下文之外。

## 最佳实践

### 安全考虑

#### 要做的事情 ✅
- 对所有凭据使用环境变量
- 定期轮换Token和 API 密钥（建议每月一次）
- 尽可能使用只读Token
- 将 MCP 服务器访问范围限制为所需的最小范围
- 监控MCP服务器使用情况和访问日志
- 如果可用，请使用 OAuth 进行外部服务
- 对 MCP 请求实施速率限制
- 在生产使用之前测试 MCP 连接
- 记录所有活动的 MCP 连接
- 保持MCP服务器包更新

#### 不该做的事 ❌
- 不要在配置文件中硬编码凭据
- 不要向 git 提交Token或秘密
- 不要在团队聊天或电子邮件中共享tokens
- 不要将个人tokens用于团队项目
- 不要授予不必要的权限
- 不要忽略身份验证错误
- 不要公开暴露 MCP 端点
- 不要以 root/admin 权限运行 MCP 服务器
- 不要在日志中缓存敏感数据
- 不要禁用身份验证机制

### 配置最佳实践

1. **版本控制**：在git中保留`.mcp.json`，但使用环境变量作为机密
2. **最低权限**：授予每个MCP服务器所需的最低权限
3. **隔离**：尽可能在单独的进程中运行不同的MCP服务器
4. **监控**：记录所有 MCP 请求和错误以进行审计跟踪
5. **测试**：在部署到生产环境之前测试所有 MCP 配置

### 性能提示

- 在应用程序级别缓存经常访问的数据
- 使用特定的 MCP 查询来减少数据传输
- 监控 MCP 操作的响应时间
- 考虑外部 API 的速率限制
- 执行多个操作时使用批处理

## 安装说明

### 先决条件
- 安装了 Node.js 和 npm
- 安装了claude代码 CLI
- 外部服务的 API Token/凭证

### 分步设置

1. **使用 CLI 添加您的第一个 MCP 服务器**（示例：GitHub）：
```bash
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```
或者在项目根目录中创建一个 `.mcp.json` 文件：
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```
2. **设置环境变量：**
```bash
export GITHUB_TOKEN="your_github_personal_access_token"
```
3. **测试连接：**
```bash
claude /mcp
```
4. **使用MCP工具：**
```bash
/mcp__github__list_prs
/mcp__github__create_issue "Title" "Description"
```
### 特定服务的安装

**GitHub MCP：**
```bash
npm install -g @modelcontextprotocol/server-github
```
**数据库MCP：**
```bash
npm install -g @modelcontextprotocol/server-database
```
**文件系统MCP：**
```bash
npm install -g @modelcontextprotocol/server-filesystem
```
**Slack MCP：**
```bash
npm install -g @modelcontextprotocol/server-slack
```
## 故障排除

### 未找到 MCP 服务器
```bash
# Verify MCP server is installed
npm list -g @modelcontextprotocol/server-github

# Install if missing
npm install -g @modelcontextprotocol/server-github
```
### 身份验证失败
```bash
# Verify environment variable is set
echo $GITHUB_TOKEN

# Re-export if needed
export GITHUB_TOKEN="your_token"

# Verify token has correct permissions
# Check GitHub token scopes at: https://github.com/settings/tokens
```
### 连接超时
- 检查网络连接：`ping api.github.com`
- 验证API端点是否可访问
- 检查 API 的速率限制
- 尝试增加配置中的超时
- 检查防火墙或agents问题

### MCP 服务器崩溃
- 检查 MCP 服务器日志：`~/.claude/logs/`
- 验证所有环境变量是否已设置
- 确保适当的文件权限
- 尝试重新安装MCP服务器包
- 检查同一端口上的冲突进程

## 相关概念

### 内存与 MCP
- **内存**：存储持久的、不变的数据（首选项、上下文、历史记录）
- **MCP**：访问实时变化的数据（API、数据库、实时服务）

### 何时使用每个
- **使用内存**用于：用户偏好、对话历史记录、学习的上下文
- **使用 MCP** 用于：当前 GitHub 问题、实时数据库查询、实时数据

### 与其他 Claude 功能集成
- 将 MCP 与 Memory 结合起来以获得丰富的上下文
- 在提示中使用MCP工具以获得更好的推理
- 利用多个 MCP 来完成复杂的工作流程

## 其他资源

- [Official MCP Documentation](https://code.claude.com/docs/en/mcp)
- [MCP Protocol Specification](https://modelcontextprotocol.io/specification)
- [MCP GitHub Repository](https://github.com/modelcontextprotocol/servers)
- [Available MCP Servers](https://github.com/modelcontextprotocol/servers)
- [MCPorter](https://github.com/steipete/mcporter) — TypeScript 运行时和 CLI，无需样板即可调用 MCP 服务器
- [Code Execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic 关于解决上下文膨胀的工程博客
- [Claude Code CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [Claude API Documentation](https://docs.anthropic.com)
