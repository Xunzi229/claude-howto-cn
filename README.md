<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

<p align="center">
  <a href="https://github.com/trending">
    <img src="https://img.shields.io/badge/GitHub-🔥%20%231%20Trending-purple?style=for-the-badge&logo=github"/>
  </a>
</p>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)]（许可证）
[![Version](https://img.shields.io/badge/version-2.2.0-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

# 周末掌握 Claude 代码

从键入 `claude` 到agents、hooks、skills和 MCP 服务器 - 提供可视化教程、复制粘贴模板和指导学习路径。

**[Get Started in 15 Minutes](#-get-started-in-15-minutes)** | **[Find Your Level](#-not-sure-where-to-start)** | **[Browse the Feature Catalog](CATALOG.md)**

---

## 目录

- [The Problem](#the-problem)
- [How Claude How To Fixes This](#how-claude-how-to-fixes-this)
- [How It Works](#how-it-works)
- [Not Sure Where to Start?](#-not-sure-where-to-start)
- [Get Started in 15 Minutes](#-get-started-in-15-minutes)
- [What Can You Build With This?](#what-can-you-build-with-this)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## 问题

您安装了claude代码。您运行了一些提示。现在怎么办？

- **官方文档描述了功能 - 但没有向您展示如何组合它们。**  您知道斜线命令的存在，但不知道如何将它们与Hook、内存和Subagents链接到实际上节省时间的工作流程中。
- **没有明确的学习路径。** 你应该在 hooks 之前学习 MCP 吗？skills先于 subagents？你最终会浏览所有内容，却一无所获。
- **示例太基础了。** “hello world”斜杠命令无法帮助您构建使用内存、委托给专门agents并自动运行安全扫描的生产代码审查管道。

你把Claude Code's 90% 的权力都留在了桌面上——而你不知道自己不知道什么。

---

## claude如何解决这个问题

这不是另一个功能参考。这是一本**结构化、可视化、示例驱动的指南**，教您使用每个 Claude Code 功能以及可立即复制到项目中的真实模板。

| |官方文档 |本指南 |
|--|----------------|------------|
| **格式** |参考文档|带有Mermaid图的视觉教程 |
| **深度** |功能描述|它的幕后工作原理是怎样的？
| **示例** |基本片段|您可以立即使用的生产就绪模板 |
| **结构** |特色组织|渐进式学习路径（初级到高级）|
| **入职** |自导|带有时间估计的指导路线图 |
| **自我评估** |无 |互动测验，找出你的差距并建立个性化的道路 |

### 你会得到什么：

- **10 个教程模块** 涵盖每个 Claude Code 功能 — 从斜杠命令到自定义agents团队
- **复制粘贴配置** — 斜杠命令、CLAUDE.md 模板、hooks脚本、MCP 配置、Subagents定义和完整Plugins包
- **Mermaid图**显示每个功能的内部工作原理，让您了解*为什么*，而不仅仅是*如何*
- **指导学习路径**，让您在 11-13 小时内从初学者变成高级用户
- **内置自我评估** — 直接在 Claude 代码中运行 `/self-assessment` 或 `/lesson-quiz hooks` 来识别差距

**[Start the Learning Path  ->](LEARNING-ROADMAP.md)**

---

## 它是如何工作的

### 1. 找到你的水平

在claude代码中获取 [self-assessment quiz](LEARNING-ROADMAP.md#-find-your-level) 或运行 `/self-assessment` 。根据您已知的信息获取个性化的路线图。

### 2. 遵循引导路径

按顺序完成 10 个模块——每个模块都建立在最后一个模块的基础上。当您学习时，将模板直接复制到您的项目中。

### 3. 将功能组合到工作流程中

真正的力量在于组合功能。学习将斜线命令+内存+Subagents+hooks连接到处理代码审查、部署和文档生成的自动化管道中。

### 4.测试你的理解力

在每个模块之后运行 `/lesson-quiz [topic]`。该测验会指出您错过的内容，以便您可以快速填补空白。

**[Get Started in 15 Minutes](#-get-started-in-15-minutes)**

---

## 受到 5,900 多名开发人员的信赖

- **5,900+ GitHub star**，来自每天使用 Claude Code 的开发人员
- **690+ 分支** — 团队根据自己的工作流程调整本指南
- **积极维护** — 与每个 Claude Code 版本同步（最新：v2.2.0，2026 年 3 月）
- **社区驱动** — 来自分享真实世界配置的开发人员的贡献

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不确定从哪里开始？

进行自我评估或选择您的级别：

|水平|你可以... |从这里开始 |时间 |
|--------|---------|------------|-----|
| **初学者** |启动 Claude Code 并聊天 | [Slash Commands](01-slash-commands/) |约 2.5 小时 |
| **中级** |使用 CLAUDE.md 和自定义命令 | [Skills](03-skills/) |约 3.5 小时 |
| **高级** |配置 MCP 服务器和hooks | [Advanced Features](09-advanced-features/) |约 5 小时 |

**包含所有 10 个模块的完整学习路径：**

|订单|模块|水平|时间 |
|--------|--------|--------|------|
| 1 | [Slash Commands](01-slash-commands/) |初学者 | 30 分钟 |
| 2 | [Memory](02-memory/) |初学者+ | 45 分钟 |
| 3 | [Checkpoints](08-checkpoints/) |中级| 45 分钟 |
| 4 | [CLI Basics](10-cli/) |初学者+ | 30 分钟 |
| 5 | [Skills](03-skills/) |中级| 1小时|
| 6 | [Hooks](06-hooks/) |中级| 1小时|
| 7 | [MCP](05-mcp/) |中级+ | 1小时|
| 8 | [Subagents](04-subagents/) |中级+ | 1.5 小时 |
| 9 | [Advanced Features](09-advanced-features/) |高级| 2-3小时|
| 10 | 10 [Plugins](07-plugins/) |高级| 2小时|

**[Complete Learning Roadmap ->](LEARNING-ROADMAP.md)**

---

## 15 分钟内开始
```bash
# 1. Clone the guide
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. Copy your first slash command
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. Try it — in Claude Code, type:
# /optimize

# 4. Ready for more? Set up project memory:
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. Install a skill:
cp -r 03-skills/code-review ~/.claude/skills/
```
想要完整的设置吗？这是 **1 小时的基本设置**：
```bash
# Slash commands (15 min)
cp 01-slash-commands/*.md .claude/commands/

# Project memory (15 min)
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Install a skill (15 min)
cp -r 03-skills/code-review ~/.claude/skills/

# Weekend goal: add hooks, subagents, MCP, and plugins
# Follow the learning path for guided setup
```
**[View the Full Installation Reference](#installation-quick-reference)**

---

## 你可以用它构建什么？

|使用案例|您将组合的功能 |
|----------|------------------------|
| **自动代码审查** | Slash 命令 + Subagents + 内存 + MCP |
| **团队入职** |内存 + 斜杠命令 + Plugins |
| **CI/CD 自动化** | CLI 参考 + hooks + 后台任务 |
| **文档生成** |skills+Subagents+Plugins|
| **安全审核** |Subagents+skills+Hook（只读模式）|
| **DevOps 管道** |Plugins+MCP+hooks+后台任务|
| **复杂重构** |检查点+计划模式+Hook|

---

## 常见问题解答

**这是免费的吗？**
是的。麻省理工学院授权，永久免费。在个人项目、工作中、团队中使用它——除了许可声明之外没有任何限制。

**这个维护了吗？**
积极主动。该指南与每个 Claude Code 版本同步。当前版本：v2.2.0（2026 年 3 月），兼容 Claude Code 2.1+。

**这与官方文档有何不同？**
官方文档是功能参考。本指南是一个包含图表、可用于生产的模板和渐进式学习路径的教程。它们相辅相成——从这里开始学习，当您需要具体信息时参考文档。

**完成所有事情需要多长时间？**
完整路径需要 11-13 小时。但您将在 15 分钟内立即获得价值 - 只需复制斜线命令模板并尝试即可。

**我可以将其与claude Sonnet/ haiku/作品一起使用吗？**
是的。所有模板均适用于 Claude Sonnet 4.6、Claude Opus 4.6 和 Claude Haiku 4.5。

**我可以贡献吗？**
绝对的。请参阅 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南。我们欢迎新的示例、错误修复、文档改进和社区模板。

**我可以离线阅读此内容吗？**
是的。运行 `uv run scripts/build_epub.py` 以生成包含所有内容和渲染图表的 EPUB 电子书。

---

## 从今天开始掌握 Claude 代码

您已经安装了claude代码。您与 10 倍生产力之间的唯一障碍就是了解如何使用它。本指南为您提供了结构化路径、可视化解释以及实现目标的复制粘贴模板。

麻省理工学院许可。永远免费。克隆它，分叉它，让它成为你的。

**[Start the Learning Path ->](LEARNING-ROADMAP.md)** | **[Browse the Feature Catalog](CATALOG.md)** | **[Get Started in 15 Minutes](#-get-started-in-15-minutes)**

---

<details>
<summary>快速导航 — 所有功能</summary>

|特色|描述 |文件夹|
|--------|-------------|--------|
| **功能目录** |完整参考安装命令 | [CATALOG.md](CATALOG.md) |
| **斜线命令** |用户调用的快捷方式 | [01-slash-commands/](01-slash-commands/) |
| **内存** |持久上下文 | [02-memory/](02-memory/) |
| **skills** |可重复使用的能力| [03-skills/](03-skills/) |
| **Subagents** |专业人工智能助手| [04-subagents/](04-subagents/) |
| **MCP 协议** |外部工具访问 | [05-mcp/](05-mcp/) |
| **hooks** |事件驱动的自动化 | [06-hooks/](06-hooks/) |
| **Plugins** |捆绑功能 | [07-plugins/](07-plugins/) |
| **检查点** |会议快照和倒带 | [08-checkpoints/](08-checkpoints/) |
| **高级功能** |规划、思考、后台任务| [09-advanced-features/](09-advanced-features/) |
| **CLI 参考** |命令、标志和选项 | [10-cli/](10-cli/) |
| **博客文章** |真实世界的使用示例 | [Blog Posts](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能比较</summary>

|特色|调用|坚持|最适合 |
|--------|---------|------------|---------|
| **斜线命令** |手册 (`/cmd`) |仅限会议 |快捷方式 |
| **内存** |自动加载|跨会议 |长期学习|
| **skills** |自动调用 |文件系统 |自动化工作流程 |
| **Subagents** |自动委派|孤立的背景|任务分配|
| **MCP 协议** |自动查询|实时|实时数据访问 |
| **hooks** |事件触发|配置|自动化与验证 |
| **Plugins** |一个命令 |所有功能 |完整的解决方案|
| **检查点** |手动/自动|基于会话的 |安全实验|
| **规划模式** |手动/自动|计划阶段|复杂的实施 |
| **后台任务** |手册|任务持续时间|长时间运行的操作 |
| **CLI 参考** |终端命令 |会话/脚本 |自动化和脚本编写 |

</details>

<details>
<summary>安装快速参考</summary>
```bash
# Slash Commands
cp 01-slash-commands/*.md .claude/commands/

# Memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Skills
cp -r 03-skills/code-review ~/.claude/skills/

# Subagents
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Plugins
/plugin install pr-review

# Checkpoints (auto-enabled, configure in settings)
# See 08-checkpoints/README.md

# Advanced Features (configure in settings)
# See 09-advanced-features/config-examples.json

# CLI Reference (no installation needed)
# See 10-cli/README.md for usage examples
```
</details>

<details>
<summary>01。斜杠命令</summary>

**地点**：[01-slash-commands/](01-slash-commands/)

**什么**：用户调用的快捷方式存储为 Markdown 文件

**示例**：
- `optimize.md` - 代码优化分析
- `pr.md` - 拉取请求准备
- `generate-api-docs.md` - API 文档生成器

**安装**：
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```
**用法**：
```
/optimize
/pr
/generate-api-docs
```
**了解更多**：[Discovering Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02。内存</summary>

**地点**：[02-memory/](02-memory/)

**什么**：跨会话的持久上下文

**示例**：
- `project-CLAUDE.md` - 团队范围的项目标准
- `directory-api-CLAUDE.md` - 特定于目录的规则
- `personal-CLAUDE.md` - 个人喜好

**安装**：
```bash
# Project memory
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# Directory memory
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# Personal memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```
**用法**：由claude自动加载

</details>

<details>
<summary>03。skills</summary>

**地点**：[03-skills/](03-skills/)

**什么**：可重用、自动调用的功能以及指令和脚本

**示例**：
- `code-review/` - 使用脚本进行全面的代码审查
- `brand-voice/` - 品牌声音一致性检查器
- `doc-generator/` - API 文档生成器

**安装**：
```bash
# Personal skills
cp -r 03-skills/code-review ~/.claude/skills/

# Project skills
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```
**用法**：相关时自动调用

</details>

<details>
<summary>04。Subagents</summary>

**地点**：[04-subagents/](04-subagents/)

**什么**：具有独立上下文和自定义提示的专业人工智能助手

**示例**：
- `code-reviewer.md` - 全面的代码质量分析
- `test-engineer.md` - 测试策略和覆盖范围
- `documentation-writer.md` - 技术文档
- `secure-reviewer.md` - 以安全为重点的审查（只读）
- `implementation-agent.md` - 完整功能实现

**安装**：
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```
**用法**：由主agents自动委托

</details>

<details>
<summary>05。 MCP 协议</summary>

**地点**：[05-mcp/](05-mcp/)

**什么**：用于访问外部工具和 API 的模型上下文协议

**示例**：
- `github-mcp.json` - GitHub 集成
- `database-mcp.json` - 数据库查询
- `filesystem-mcp.json` - 文件操作
- `multi-mcp.json` - 多个 MCP 服务器

**安装**：
```bash
# Set environment variables
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# Add MCP server via CLI
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Or add to project .mcp.json manually (see 05-mcp/ for examples)
```
**用法**：配置后，Claude 会自动使用 MCP 工具

</details>

<details>
<summary>06。hooks</summary>

**地点**：[06-hooks/](06-hooks/)

**什么**：事件驱动的 shell 命令，自动执行以响应 Claude Code 事件

**示例**：
- `format-code.sh` - 写入前自动格式化代码
- `pre-commit.sh` - 在提交之前运行测试
- `security-scan.sh` - 扫描安全问题
- `log-bash.sh` - 记录所有 bash 命令
- `validate-prompt.sh` - 验证用户提示
- `notify-team.sh` - 发送事件通知

**安装**：
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```
在 `~/.claude/settings.json` 中配置Hook：
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```
**用法**：hooks在事件上自动执行

**hooks类型**（4 种类型，25 个事件）：
- **工具hooks**：`PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`
- **会话hooks**：`SessionStart`、`SessionEnd`、`Stop`、`StopFailure`、`SubagentStart`、`SubagentStop`
- **任务hooks**：`UserPromptSubmit`、`TaskCompleted`、`TaskCreated`、`TeammateIdle`
- **生命周期hooks**：`ConfigChange`、`CwdChanged`、`FileChanged`、`PreCompact`、`PostCompact`、`WorktreeCreate`、`WorktreeRemove`、`Notification`、`InstructionsLoaded`、`Elicitation`、`ElicitationResult`

</details>

<details>
<summary>07。Plugins</summary>

**地点**：[07-plugins/](07-plugins/)

**什么**：命令、agents、MCP 和hooks的捆绑集合

**示例**：
- `pr-review/` - 完整的公关审核工作流程
- `devops-automation/` - 部署和监控
- `documentation/` - 文档生成

**安装**：
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```
**用法**：使用捆绑的斜杠命令和功能

</details>

<details>
<summary>08。检查点和倒带</summary>

**地点**：[08-checkpoints/](08-checkpoints/)

**内容**：保存对话状态并倒回到之前的点以探索不同的方法

**关键概念**：
- **检查点**：对话状态快照
- **倒带**：返回到上一个检查点
- **分支点**：从同一检查点探索多种方法

**用法**：
```
# Checkpoints are created automatically with every user prompt
# To rewind, press Esc twice or use:
/rewind

# Then choose from five options:
# 1. Restore code and conversation
# 2. Restore conversation
# 3. Restore code
# 4. Summarize from here
# 5. Never mind
```
**用例**：
- 尝试不同的实施方法
- 从错误中恢复
- 安全实验
- 比较替代解决方案
- A/B测试不同的设计

</details>

<details>
<summary>09。高级功能</summary>

**地点**：[09-advanced-features/](09-advanced-features/)

**内容**：复杂工作流程和自动化的高级功能

**包括**：
- **规划模式** — 在编码之前创建详细的实施计划
- **扩展思维** — 对复杂问题的深层推理（用 `Alt+T` / `Option+T` 切换）
- **后台任务** — 运行长时间操作而不阻塞
- **权限模式** — `default`、`acceptEdits`、`plan`、`dontAsk`、`bypassPermissions`
- **无头模式** — 在 CI/CD 中运行 Claude 代码：`claude -p "Run tests and generate report"`
- **会话管理** — `/resume`、`/rename`、`/fork`、`claude -c`、`claude -r`
- **配置** — 自定义 `~/.claude/settings.json` 中的行为

请参阅 [config-examples.json](09-advanced-features/config-examples.json) 了解完整配置。

</details>

<details>
<summary>10。 CLI 参考</summary>

**地点**：[10-cli/](10-cli/)

**内容**：Claude Code 的完整命令行界面参考

**简单示例**：
```bash
# Interactive mode
claude "explain this project"

# Print mode (non-interactive)
claude -p "review this code"

# Process file content
cat error.log | claude -p "explain this error"

# JSON output for scripts
claude -p --output-format json "list functions"

# Resume session
claude -r "feature-auth" "continue implementation"
```
**用例**：CI/CD 管道集成、脚本自动化、批处理、多会话工作流程、自定义agents配置

</details>

<details>
<summary>示例工作流程</summary>

### 完整的代码审查工作流程
```markdown
# Uses: Slash Commands + Subagents + Memory + MCP

User: /review-pr

Claude:
1. Loads project memory (coding standards)
2. Fetches PR via GitHub MCP
3. Delegates to code-reviewer subagent
4. Delegates to test-engineer subagent
5. Synthesizes findings
6. Provides comprehensive review
```
### 自动化文档
```markdown
# Uses: Skills + Subagents + Memory

User: "Generate API documentation for the auth module"

Claude:
1. Loads project memory (doc standards)
2. Detects doc generation request
3. Auto-invokes doc-generator skill
4. Delegates to api-documenter subagent
5. Creates comprehensive docs with examples
```
### DevOps 部署
```markdown
# Uses: Plugins + MCP + Hooks

User: /deploy production

Claude:
1. Runs pre-deploy hook (validates environment)
2. Delegates to deployment-specialist subagent
3. Executes deployment via Kubernetes MCP
4. Monitors progress
5. Runs post-deploy hook (health checks)
6. Reports status
```
</details>

<details>
<summary>目录结构</summary>
```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md (this file)
```
</details>

<details>
<summary>最佳实践</summary>

### 要做的事
- 从简单的斜线命令开始
- 逐步添加功能
- 使用记忆作为团队标准
- 首先在本地测试配置
- 记录自定义实现
- 版本控制项目配置
- 与团队共享Plugins

### 不该做的事
- 不要创建多余的功能
- 不要对凭据进行硬编码
- 不要跳过文档
- 不要让简单的任务变得过于复杂
- 不要忽视安全最佳实践
- 不要提交敏感数据

</details>

<details>
<summary>疑难解答</summary>

### 功能未加载
1.检查文件位置和命名
2.验证YAML frontmatter语法
3.检查文件权限
4.检查Claude代码版本兼容性

### MCP 连接失败
1. 验证环境变量
2.检查MCP服务器安装
3. 测试凭证
4. 检查网络连接

### Subagents不委托
1.检查工具权限
2. 验证agents描述的清晰度
3.审查任务复杂性
4. 独立测试agents

</details>

<details>
<summary>测试</summary>

该项目包括全面的自动化测试：

- **单元测试**：使用 pytest 进行 Python 测试（Python 3.10、3.11、3.12）
- **代码质量**：使用 Ruff 进行检查和格式化
- **安全**：使用 Bandit 进行漏洞扫描
- **类型检查**：使用 mypy 进行静态类型分析
- **构建验证**：EPUB 生成测试
- **覆盖范围跟踪**：Codecov 集成
```bash
# Install development dependencies
uv pip install -r requirements-dev.txt

# Run all unit tests
pytest scripts/tests/ -v

# Run tests with coverage report
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# Run code quality checks
ruff check scripts/
ruff format --check scripts/

# Run security scan
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# Run type checking
mypy scripts/ --ignore-missing-imports
```
每次推送到 `main`/`develop` 以及每次 PR 到 `main` 时，测试都会自动运行。有关详细信息，请参阅 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 生成</summary>

想离线阅读本指南吗？生成 EPUB 电子书：
```bash
uv run scripts/build_epub.py
```
这将创建包含所有内容的 `claude-howto-guide.epub` ，包括渲染的Mermaid图。

有关更多选项，请参阅 [scripts/README.md](scripts/README.md)。

</details>

<details>
<summary>贡献</summary>

发现问题或想贡献示例？我们希望得到您的帮助！

**请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 了解有关以下内容的详细指南：**
- 贡献类型（示例、文档、功能、错误、反馈）
- 如何设置您的开发环境
- 目录结构以及如何添加内容
- 编写指南和最佳实践
- 提交和公关流程

**我们的社区标准：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我们如何对待彼此
- [SECURITY.md](SECURITY.md) - 安全策略和漏洞报告

### 报告安全问题

如果您发现安全漏洞，请负责任地报告：

1. **使用 GitHub 私有漏洞报告**：https://github.com/luongnv89/claude-howto/security/advisories
2. **或阅读** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 了解详细说明
3. **不要**针对安全漏洞提出公开问题

快速启动：
1. 分叉并克隆存储库
2. 创建描述性分支（`add/feature-name`、`fix/bug`、`docs/improvement`）
3. 按照指南进行更改
4. 提交带有清晰描述的拉取请求

**需要帮助？**提出问题或讨论，我们将指导您完成整个过程。

</details>

<details>
<summary>其他资源</summary>

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Skills Repository](https://github.com/luongnv89/skills) - 即用型skills的集合
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) - Claude Code 的创建者分享了他的系统化工作流程：并行agents、共享 CLAUDE.md、计划模式、斜杠命令、Subagents和用于自主长时间运行会话的验证hooks。

</details>

---

## 贡献

我们欢迎贡献！请参阅我们的 [Contributing Guide](CONTRIBUTING.md) 了解如何开始的详细信息。

## 贡献者

感谢所有为这个项目做出贡献的人！

|贡献者 |公关 |
|-------------|-----|
| [wjhrdy](https://github.com/wjhrdy) | [#1 - add a tool to create an epub](https://github.com/luongnv89/claude-howto/pull/1) |
| [VikalpP](https://github.com/VikalpP) | [#7 - fix(docs): Use tilde fences for nested code blocks in concepts guide](https://github.com/luongnv89/claude-howto/pull/7) |

---

## 许可证

麻省理工学院许可证 - 请参阅 [LICENSE](LICENSE)。免费使用、修改和分发。唯一的要求是包含许可通知。

---

**最后更新**：2026 年 3 月
**claude代码版本**：2.1+
**兼容型号**：claude Sonnet 4.6、claude作品 4.6、claude haiku 4.5
