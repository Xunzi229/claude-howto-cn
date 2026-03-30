<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 代码示例 - 快速参考卡

## 🚀 安装快速命令

### 斜线命令
```bash
# Install all
cp 01-slash-commands/*.md .claude/commands/

# Install specific
cp 01-slash-commands/optimize.md .claude/commands/
```
＃＃＃ 记忆
```bash
# Project memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Personal memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```
### skills
```bash
# Personal skills
cp -r 03-skills/code-review ~/.claude/skills/

# Project skills
cp -r 03-skills/code-review .claude/skills/
```
### Subagents
```bash
# Install all
cp 04-subagents/*.md .claude/agents/

# Install specific
cp 04-subagents/code-reviewer.md .claude/agents/
```
### MCP
```bash
# Set credentials
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# Install config (project scope)
cp 05-mcp/github-mcp.json .mcp.json

# Or user scope: add to ~/.claude.json
```
### hooks
```bash
# Install hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Configure in settings (~/.claude/settings.json)
```
### Plugins
```bash
# Install from examples (if published)
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```
### 检查点
```bash
# Checkpoints are created automatically with every user prompt
# To rewind, press Esc twice or use:
/rewind

# Then choose: Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, or Never mind
```
### 高级功能
```bash
# Configure in settings (.claude/settings.json)
# See 09-advanced-features/config-examples.json

# Planning mode
/plan Task description

# Permission modes (use --permission-mode flag)
# default        - Ask for approval on risky actions
# acceptEdits    - Auto-accept file edits, ask for others
# plan           - Read-only analysis, no modifications
# dontAsk        - Accept all actions except risky ones
# auto           - Background classifier decides permissions automatically
# bypassPermissions - Accept all actions (requires --dangerously-skip-permissions)

# Session management
/resume                # Resume a previous conversation
/rename "name"         # Name the current session
/fork                  # Fork the current session
claude -c              # Continue most recent conversation
claude -r "session"    # Resume session by name/ID
```
---

## 📋 功能备忘单

|特色|安装路径 |用途 |
|--------|-------------|--------|
| **斜线命令 (55+)** | `.claude/commands/*.md` | `/command-name` |
| **内存** | `./CLAUDE.md` |自动加载|
| **skills** | `.claude/skills/*/SKILL.md` |自动调用 |
| **Subagents** | `.claude/agents/*.md` |自动委派|
| **MCP** | `.mcp.json`（项目）或 `~/.claude.json`（用户）| `/mcp__server__action` |
| **hooks（25 个事件）** | `~/.claude/hooks/*.sh` |事件触发（4种）|
| **Plugins** |通过 `/plugin install` |全部捆绑 |
| **检查点** |内置| `Esc+Esc` 或 `/rewind` |
| **规划模式** |内置| `/plan <task>` |
| **权限模式（6）** |内置| `--allowedTools`、`--permission-mode` |
| **会议** |内置| `/session <command>` |
| **后台任务** |内置|后台运行 |
| **远程控制** |内置| WebSocket API |
| **网络会议** |内置| `claude web` |
| **Git 工作树** |内置| `/worktree` |
| **自动记忆** |内置|自动保存到 CLAUDE.md |
| **任务清单** |内置| `/task list` |
| **捆绑skills (5)** |内置| `/simplify`、`/loop`、`/claude-api`、`/voice`、`/browse` |

---

## 🎯 常见用例

### 代码审查
```bash
# Method 1: Slash command
cp 01-slash-commands/optimize.md .claude/commands/
# Use: /optimize

# Method 2: Subagent
cp 04-subagents/code-reviewer.md .claude/agents/
# Use: Auto-delegated

# Method 3: Skill
cp -r 03-skills/code-review ~/.claude/skills/
# Use: Auto-invoked

# Method 4: Plugin (best)
/plugin install pr-review
# Use: /review-pr
```
### 文档
```bash
# Slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# Subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# Skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# Plugin (complete solution)
/plugin install documentation
```
### 开发运营
```bash
# Complete plugin
/plugin install devops-automation

# Commands: /deploy, /rollback, /status, /incident
```
### 团队标准
```bash
# Project memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Edit for your team
vim CLAUDE.md
```
### 自动化和hooks
```bash
# Install hooks (25 events, 4 types: command, http, prompt, agent)
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Examples:
# - Pre-commit tests: pre-commit.sh
# - Auto-format code: format-code.sh
# - Security scanning: security-scan.sh

# Auto Mode for fully autonomous workflows
claude --enable-auto-mode -p "Refactor and test the auth module"
# Or cycle modes interactively with Shift+Tab
```
### 安全重构
```bash
# Checkpoints are created automatically before each prompt
# Try refactoring
# If it works: continue
# If it fails: press Esc+Esc or use /rewind to go back
```
### 复杂的实现
```bash
# Use planning mode
/plan Implement user authentication system

# Claude creates detailed plan
# Review and approve
# Claude implements systematically
```
### CI/CD 集成
```bash
# Run in headless mode (non-interactive)
claude -p "Run all tests and generate report"

# With permission mode for CI
claude -p "Run tests" --permission-mode dontAsk

# With Auto Mode for fully autonomous CI tasks
claude --enable-auto-mode -p "Run tests and fix failures"

# With hooks for automation
# See 09-advanced-features/README.md
```
### 学习与实验
```bash
# Use plan mode for safe analysis
claude --permission-mode plan

# Experiment safely - checkpoints are created automatically
# If you need to rewind: press Esc+Esc or use /rewind
```
### 特工团队
```bash
# Enable agent teams
export CLAUDE_AGENT_TEAMS=1

# Or in settings.json
{ "agentTeams": { "enabled": true } }

# Start with: "Implement feature X using a team approach"
```
### 计划任务
```bash
# Run a command every 5 minutes
/loop 5m /check-status

# One-time reminder
/loop 30m "remind me to check the deploy"
```
---

## 📁 文件位置参考
```
Your Project/
├── .claude/
│   ├── commands/              # Slash commands go here
│   ├── agents/                # Subagents go here
│   ├── skills/                # Project skills go here
│   └── settings.json          # Project settings (hooks, etc.)
├── .mcp.json                  # MCP configuration (project scope)
├── CLAUDE.md                  # Project memory
└── src/
    └── api/
        └── CLAUDE.md          # Directory-specific memory

User Home/
├── .claude/
│   ├── commands/              # Personal commands
│   ├── agents/                # Personal agents
│   ├── skills/                # Personal skills
│   ├── hooks/                 # Hook scripts
│   ├── settings.json          # User settings
│   ├── managed-settings.d/    # Managed settings (enterprise/org)
│   └── CLAUDE.md              # Personal memory
└── .claude.json               # Personal MCP config (user scope)
```
---

## 🔍 寻找例子

### 按类别
- **斜线命令**：`01-slash-commands/`
- **内存**：`02-memory/`
- **skills**：`03-skills/`
- **Subagents**：`04-subagents/`
- **MCP**：`05-mcp/`
- **hooks**：`06-hooks/`
- **Plugins**：`07-plugins/`
- **检查点**：`08-checkpoints/`
- **高级功能**：`09-advanced-features/`
- **CLI**：`10-cli/`

### 按用例
- **性能**：`01-slash-commands/optimize.md`
- **安全**：`04-subagents/secure-reviewer.md`
- **测试**：`04-subagents/test-engineer.md`
- **文档**：`03-skills/doc-generator/`
- **DevOps**：`07-plugins/devops-automation/`

### 按复杂性
- **简单**：斜线命令
- **中**：Subagents、内存
- **高级**：skills、技巧
- **完整**：Plugins

---

## 🎓 学习路径

### 第一天
```bash
# Read overview
cat README.md

# Install a command
cp 01-slash-commands/optimize.md .claude/commands/

# Try it
/optimize
```
### 第 2-3 天
```bash
# Set up memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# Install subagent
cp 04-subagents/code-reviewer.md .claude/agents/
```
### 第 4-5 天
```bash
# Set up MCP
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# Try MCP commands
/mcp__github__list_prs
```
### 第 2 周
```bash
# Install skill
cp -r 03-skills/code-review ~/.claude/skills/

# Let it auto-invoke
# Just say: "Review this code for issues"
```
### 第 3 周以上
```bash
# Install complete plugin
/plugin install pr-review

# Use bundled features
/review-pr
/check-security
/check-tests
```
---

## 新功能（2026 年 3 月）

|特色|描述 |用途 |
|--------|-------------|--------|
| **自动模式** |具有背景分类器的完全自主操作| `--enable-auto-mode` 标志，`Shift+Tab` 循环模式 |
| **频道** | Discord 和 Telegram 集成 | `--channels` 标志，Discord/Telegram 机器人 |
| **语音听写** |向claude说出命令和上下文 | `/voice` 命令 |
| **hooks（25 个事件）** | 4 种扩展hooks系统 |命令、http、提示、agentshooks类型 |
| **MCP 诱导** | MCP 服务器可以在运行时请求用户输入 |服务器需要澄清时自动提示 |
| **WebSocket MCP** |用于 MCP 连接的 WebSocket 传输 |在 `.mcp.json` 中配置 `ws://` URL |
| **Plugins LSP** |Plugins的语言服务器协议支持`userConfig`、`${CLAUDE_PLUGIN_DATA}` 变量 |
| **远程控制** |通过 WebSocket API 控制 Claude 代码 | `claude --remote` 用于外部集成 |
| **网络会议** |基于浏览器的claude代码界面 | `claude web` 启动 |
| **桌面应用程序** |本机桌面应用程序 |从 claude.ai/download 下载 |
| **任务清单** |管理后台任务 | `/task list`、`/task status <id>` |
| **自动记忆** |对话自动节省内存 | Claude 自动将关键上下文保存到 CLAUDE.md |
| **Git 工作树** |用于并行开发的独立工作区| `/worktree` 创建隔离工作区 |
| **型号选择** |在 Sonnet 4.6 和 Opus 4.6 之间切换 | `/model` 或 `--model` 标志 |
| **agents团队** |协调多个agents执行任务 |使用 `CLAUDE_AGENT_TEAMS=1` 环境变量启用 |
| **计划任务** | `/loop` 的重复任务 | `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** |浏览器自动化 | `--chrome` 标志或 `/chrome` 命令 |
| **键盘定制** |自定义键绑定 | `/keybindings` 命令 |

---

## 提示与技巧

### 定制
- 从原样的示例开始
- 修改以满足您的需求
- 在与团队共享之前进行测试
- 版本控制您的配置

### 最佳实践
- 使用记忆作为团队标准
- 使用Plugins实现完整的工作流程
- 使用Subagents来执行复杂的任务
- 使用斜杠命令执行快速任务

### 故障排除
```bash
# Check file locations
ls -la .claude/commands/
ls -la .claude/agents/

# Verify YAML syntax
head -20 .claude/agents/code-reviewer.md

# Test MCP connection
echo $GITHUB_TOKEN
```
---

## 📊 特征矩阵

|需要|使用这个 |示例|
|------|----------|---------|
|快捷捷径|斜线命令 (55+) | `01-slash-commands/optimize.md` |
|团队标准|内存| `02-memory/project-CLAUDE.md` |
|自动工作流程 |skills| `03-skills/code-review/` |
|专门任务 |Subagents | `04-subagents/code-reviewer.md` |
|外部数据| MCP（+ 启发、WebSocket）| `05-mcp/github-mcp.json` |
|事件自动化| Hook（25 个事件，4 种类型）| `06-hooks/pre-commit.sh` |
|完整的解决方案|Plugins（+ LSP 支持）| `07-plugins/pr-review/` |
|安全实验|检查站| `08-checkpoints/checkpoint-examples.md` |
|完全自主 |自动模式| `--enable-auto-mode` 或 `Shift+Tab` |
|聊天集成 |频道 | `--channels`（Discord、电报）|
| CI/CD 管道 |命令行 | `10-cli/README.md` |

---

## 🔗 快速链接

- **主要指南**：`README.md`
- **完整索引**：`INDEX.md`
- **摘要**：`EXAMPLES_SUMMARY.md`
- **原始指南**：`claude_concepts_guide.md`

---

## 📞 常见问题

**问：我应该使用哪个？**
A：从斜线命令开始，根据需要添加功能。

**问：我可以混合使用功能吗？**
答：是的！他们一起工作。内存 + 命令 + MCP = 强大。

**问：我如何与团队分享？**
A：将 `.claude/` 目录提交到 git。

**问：秘密呢？**
答：使用环境变量，不要硬编码。

**问：我可以修改示例吗？**
答：当然！它们是可以定制的模板。

---

## ✅ 清单

入门清单：

- [ ] 阅读 `README.md`
- [ ] 安装 1 个斜线命令
- [ ] 尝试命令
- [ ] 创建项目 `CLAUDE.md`
- [ ] 安装 1 个Subagents
- [ ] 设置 1 MCP 集成
- [ ] 安装 1 个skills
- [ ] 尝试完整的Plugins
- [ ] 根据您的需求定制
- [ ] 与团队分享

---

**快速入门**：`cat README.md`

**完整索引**：`cat INDEX.md`

**此卡**：请将其放在手边以供快速参考！
