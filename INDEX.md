<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 代码示例 - 完整索引

本文档提供了按功能类型组织的所有示例文件的完整索引。

## 统计摘要

- **文件总数**：100+ 个文件
- **类别**：10 个功能类别
- **Plugins**：3个完整的Plugins
- **skills**：6个完整skills
- **hooks**：8 个hooks示例
- **随时可用**：所有示例

---

## 01. 斜线命令（10 个文件）

用户调用的常见工作流程的快捷方式。

|文件|描述 |使用案例|
|------|-------------|----------|
| `optimize.md` |代码优化分析器|查找性能问题 |
| `pr.md` |拉取请求准备 |公关工作流程自动化 |
| `generate-api-docs.md` | API 文档生成器 |生成 API 文档 |
| `commit.md` |提交消息助手 |标准化提交 |
| `setup-ci-cd.md` | CI/CD 管道设置 | DevOps 自动化 |
| `push-all.md` |推送所有更改 |快推工作流程 |
| `unit-test-expand.md` |扩大单元测试覆盖范围 |测试自动化|
| `doc-refactor.md` |文档重构 |文档改进 |
| `pr-slash-command.png` |截图示例|视觉参考|
| `README.md` |文档 |设置和使用指南 |

**安装路径**：`.claude/commands/`

**用法**：`/optimize`、`/pr`、`/generate-api-docs`、`/commit`、`/setup-ci-cd`、`/push-all`、`/unit-test-expand`、`/doc-refactor`

---

## 02.内存（6个文件）

持久的背景和项目标准。

|文件|描述 |范围 |地点 |
|------|-------------|--------|----------|
| `project-CLAUDE.md` |团队项目标准|项目范围 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 特定规则 |目录 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` |个人喜好|用户 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` |屏幕截图：内存已保存 | - |视觉参考|
| `memory-ask-claude.png` |截图：问claude | - |视觉参考|
| `README.md` |文档 | - |参考|

**安装**：复制到合适的位置

**用法**：由claude自动加载

---

## 03.skills（28档）

通过脚本和模板自动调用功能。

### 代码审查技巧（5个文件）
```
code-review/
├── SKILL.md                          # Skill definition
├── scripts/
│   ├── analyze-metrics.py            # Code metrics analyzer
│   └── compare-complexity.py         # Complexity comparison
└── templates/
    ├── review-checklist.md           # Review checklist
    └── finding-template.md           # Finding documentation
```
**目的**：通过安全性、性能和质量分析进行全面的代码审查

**自动调用**：审查代码时

---

### 品牌声音技巧（4个文件）
```
brand-voice/
├── SKILL.md                          # Skill definition
├── templates/
│   ├── email-template.txt            # Email format
│   └── social-post-template.txt      # Social media format
└── tone-examples.md                  # Example messages
```
**目的**：确保沟通中品牌声音的一致性

**自动调用**：创建营销文案时

---

### 文档生成skills（2 个文件）
```
doc-generator/
├── SKILL.md                          # Skill definition
└── generate-docs.py                  # Python doc extractor
```
**目的**：从源代码生成全面的API文档

**自动调用**：创建/更新API文档时

---

### 重构技巧（5个文件）
```
refactor/
├── SKILL.md                          # Skill definition
├── scripts/
│   ├── analyze-complexity.py         # Complexity analyzer
│   └── detect-smells.py              # Code smell detector
├── references/
│   ├── code-smells.md                # Code smells catalog
│   └── refactoring-catalog.md        # Refactoring patterns
└── templates/
    └── refactoring-plan.md           # Refactoring plan template
```
**目的**：通过复杂性分析进行系统代码重构

**自动调用**：重构代码时

---

###claudeMDskills（1档）
```
claude-md/
└── SKILL.md                          # Skill definition
```
**目的**：管理和优化CLAUDE.md文件

---

###博客草稿技巧（3个文件）
```
blog-draft/
├── SKILL.md                          # Skill definition
└── templates/
    ├── draft-template.md             # Blog draft template
    └── outline-template.md           # Blog outline template
```
**目的**：起草具有一致结构的博客文章

**加**：`README.md` - skills概述和使用指南

**安装路径**：`~/.claude/skills/` 或 `.claude/skills/`

---

## 04.Subagents（9个文件）

具有自定义功能的专业人工智能助手。

|文件|描述 |工具|使用案例|
|------|-------------|--------|----------|
| `code-reviewer.md` |代码质量分析 |读取、grep、diff、lint_runner |综合评论 |
| `test-engineer.md` |测试覆盖率分析 |读、写、bash、grep |测试自动化|
| `documentation-writer.md` |文档创建 |读、写、grep |文档生成 |
| `secure-reviewer.md` |安全审查（只读）|阅读，grep |安全审计|
| `implementation-agent.md` |全面实施|读、写、bash、grep、编辑、glob |功能开发|
| `debugger.md` |调试专家|阅读、bash、grep |错误调查 |
| `data-scientist.md` |数据分析专家|读、写、bash |数据工作流程 |
| `clean-code-reviewer.md` |整洁的代码标准 |阅读，grep |代码质量 |
| `README.md` |文档 | - |设置和使用指南 |

**安装路径**：`.claude/agents/`

**用法**：由主agents自动委托

---

## 05.MCP协议（5个文件）

外部工具和 API 集成。

|文件|描述 |与 | 集成使用案例|
|------|-------------|-----------------|----------|
| `github-mcp.json` | GitHub 集成 | GitHub API |公关/问题管理 |
| `database-mcp.json` |数据库查询| PostgreSQL/MySQL |实时数据查询|
| `filesystem-mcp.json` |文件操作 |本地文件系统 |文件管理|
| `multi-mcp.json` |多台服务器| GitHub + DB + Slack |完整整合 |
| `README.md` |文档 | - |设置和使用指南 |

**安装路径**：`.mcp.json`（项目范围）或`~/.claude.json`（用户范围）

**用法**：`/mcp__github__list_prs` 等。

---

## 06. hooks（9 个文件）

自动执行的事件驱动的自动化脚本。

|文件|描述 |活动 |使用案例|
|------|-------------|--------|----------|
| `format-code.sh` |自动格式化代码 | PreToolUse：写入 |代码格式化 |
| `pre-commit.sh` |提交前运行测试 |预工具使用：Bash |测试自动化|
| `security-scan.sh` |安全扫描 | PostTool用途：写入|安全检查|
| `log-bash.sh` |记录 bash 命令 | Post工具使用：Bash |命令记录|
| `validate-prompt.sh` |验证提示 |预工具使用 |输入验证 |
| `notify-team.sh` |发送通知 |通知 |团队通知 |
| `context-tracker.py` |跟踪上下文窗口的使用情况 |发布工具使用 |情境监控 |
| `context-tracker-tiktoken.py` |基于Token的上下文跟踪 |发布工具使用 |精确的tokens计数 |
| `README.md` |文档 | - |设置和使用指南 |

**安装路径**：在`~/.claude/settings.json`中配置

**用法**：在设置中配置，自动执行

**hooks类型**（4 种类型，25 个事件）：
- 工具hooks：PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest
- 会话hooks：SessionStart、SessionEnd、Stop、StopFailure、SubagentStart、SubagentStop
- 任务hooks：UserPromptSubmit、TaskCompleted、TaskCreated、TeammateIdle
- 生命周期hooks：ConfigChange、CwdChanged、FileChanged、PreCompact、PostCompact、WorktreeCreate、WorktreeRemove、Notification、InstructionsLoaded、Eliitation、EliitationResult

---

## 07.Plugins（3个完整Plugins，40个文件）

捆绑的功能集合。

### PR 审核Plugins（10 个文件）
```
pr-review/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── review-pr.md                  # Comprehensive review
│   ├── check-security.md             # Security check
│   └── check-tests.md                # Test coverage check
├── agents/
│   ├── security-reviewer.md          # Security specialist
│   ├── test-checker.md               # Test specialist
│   └── performance-analyzer.md       # Performance specialist
├── mcp/
│   └── github-config.json            # GitHub integration
├── hooks/
│   └── pre-review.js                 # Pre-review validation
└── README.md                         # Plugin documentation
```
**功能**：安全分析、测试覆盖率、性能影响

**命令**：`/review-pr`、`/check-security`、`/check-tests`

**安装**：`/plugin install pr-review`

---

### DevOps 自动化Plugins（15 个文件）
```
devops-automation/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── deploy.md                     # Deployment
│   ├── rollback.md                   # Rollback
│   ├── status.md                     # System status
│   └── incident.md                   # Incident response
├── agents/
│   ├── deployment-specialist.md      # Deployment expert
│   ├── incident-commander.md         # Incident coordinator
│   └── alert-analyzer.md             # Alert analyzer
├── mcp/
│   └── kubernetes-config.json        # Kubernetes integration
├── hooks/
│   ├── pre-deploy.js                 # Pre-deployment checks
│   └── post-deploy.js                # Post-deployment tasks
├── scripts/
│   ├── deploy.sh                     # Deployment automation
│   ├── rollback.sh                   # Rollback automation
│   └── health-check.sh               # Health checks
└── README.md                         # Plugin documentation
```
**功能**：Kubernetes部署、回滚、监控、事件响应

**命令**：`/deploy`、`/rollback`、`/status`、`/incident`

**安装**：`/plugin install devops-automation`

---

### 文档Plugins（14 个文件）
```
documentation/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── generate-api-docs.md          # API docs generation
│   ├── generate-readme.md            # README creation
│   ├── sync-docs.md                  # Doc synchronization
│   └── validate-docs.md              # Doc validation
├── agents/
│   ├── api-documenter.md             # API doc specialist
│   ├── code-commentator.md           # Code comment specialist
│   └── example-generator.md          # Example creator
├── mcp/
│   └── github-docs-config.json       # GitHub integration
├── templates/
│   ├── api-endpoint.md               # API endpoint template
│   ├── function-docs.md              # Function doc template
│   └── adr-template.md               # ADR template
└── README.md                         # Plugin documentation
```
**功能**：API 文档、自述文件生成、文档同步、验证

**命令**：`/generate-api-docs`、`/generate-readme`、`/sync-docs`、`/validate-docs`

**安装**：`/plugin install documentation`

**加**：`README.md` - Plugins概述和使用指南

---

## 08. 检查点和倒带（2 个文件）

保存对话状态并探索替代方法。

|文件|描述 |内容 |
|------|-------------|---------|
| `README.md` |文档 |全面的检查站指南 |
| `checkpoint-examples.md` |现实世界的例子|数据库迁移、性能优化、UI迭代、调试 |
| | | |

**关键概念**：
- **检查点**：对话状态快照
- **倒带**：返回到上一个检查点
- **分支点**：探索多种方法

**用法**：
```
# Checkpoints are created automatically with every user prompt
# To rewind, press Esc twice or use:
/rewind
# Then choose: Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, or Never mind
```
**用例**：
- 尝试不同的实现
- 从错误中恢复
- 安全实验
- 比较解决方案
- A/B 测试

---

## 09. 高级功能（3 个文件）

适用于复杂工作流程的高级功能。

|文件|描述 |特点|
|------|-------------|----------|
| `README.md` |完整指南 |所有高级功能文档|
| `config-examples.json` |配置示例 | 10 多个特定于用例的配置 |
| `planning-mode-examples.md` |规划实例| REST API、数据库迁移、重构 |
|计划任务 |使用 `/loop` 和 cron 工具执行重复任务 |自动化重复工作流程 |
| Chrome 集成 |通过无头 Chromium 实现浏览器自动化 |网页测试和抓取 |
|远程控制（扩展）|连接方式、安全性对照表|远程会话管理 |
|键盘定制|自定义键绑定、和弦支持、上下文 |个性化快捷键 |
|桌面应用程序（扩展）|连接器、launch.json、企业功能 |桌面集成|
| | | |

**涵盖的高级功能**：

### 规划模式
- 制定详细的实施计划
- 时间估计和风险评估
- 系统化的任务分解

### 延伸思考
- 复杂问题的深度推理
- 架构决策分析
- 权衡评估

### 后台任务
- 长时间运行的操作不会阻塞
- 并行开发工作流程
- 任务管理和监控

### 权限模式
- **默认**：要求批准风险行为
- **acceptEdits**：自动接受文件编辑，询问其他人
- **计划**：只读分析，无修改
- **自动**：自动批准安全操作，提示有风险的操作
- **dontAsk**：接受除有风险的操作之外的所有操作
- **bypassPermissions**：接受全部（需要 `--dangerously-skip-permissions`）

### 无头模式 (`claude -p`)
- CI/CD 集成
- 自动执行任务
- 批量处理

### 会话管理
- 多次工作会议
- 会话切换和保存
- 会话持续性

### 互动功能
- 键盘快捷键
- 命令历史
- 制表符补全
- 多行输入

### 配置
- 全面的设置管理
- 特定于环境的配置
- 按项目定制

### 计划任务
- 使用 `/loop` 命令重复执行任务
- Cron工具：CronCreate、CronList、CronDelete
- 自动重复工作流程

### Chrome 集成
- 通过无头 Chromium 实现浏览器自动化
- 网络测试和抓取功能
- 页面交互和数据提取

### 远程控制（扩展）
- 连接方法和协议
- 安全考虑和最佳实践
- 远程访问选项比较表

### 键盘定制
- 自定义键绑定配置
- 多键快捷键的和弦支持
- 上下文感知键绑定激活

### 桌面应用程序（扩展）
- 用于 IDE 集成的连接器
- launch.json 配置
- 企业功能和部署

---

## 10. CLI 用法（1 个文件）

命令行界面使用模式和参考。

|文件|描述 |内容 |
|------|-------------|---------|
| `README.md` | CLI 文档 |标志、选项和使用模式 |

**主要 CLI 功能**：
- `claude` - 开始互动会话
- `claude -p "prompt"` - 无头/非交互模式
- `claude web` - 启动网络会话
- `claude --model` - 选择型号（Sonnet 4.6、Opus 4.6）
- `claude --permission-mode` - 设置权限模式
- `claude --remote` - 通过 WebSocket 启用远程控制

---

## 文档文件（13 个文件）

|文件|地点 |描述 |
|------|----------|-------------|
| `README.md` | `/` |主要示例概述 |
| `INDEX.md` | `/` |这个完整的索引|
| `QUICK_REFERENCE.md` | `/` |快速参考卡|
| `README.md` | `/01-slash-commands/` |斜线命令指南 |
| `README.md` | `/02-memory/` |记忆指南|
| `README.md` | `/03-skills/` |skills指导|
| `README.md` | `/04-subagents/` |分agents指南 |
| `README.md` | `/05-mcp/` | MCP 指南 |
| `README.md` | `/06-hooks/` |hooks指南 |
| `README.md` | `/07-plugins/` |Plugins指南 |
| `README.md` | `/08-checkpoints/` |检查站指南|
| `README.md` | `/09-advanced-features/` |高级功能指南 |
| `README.md` | `/10-cli/` | CLI 指南 |

---

## 完整的文件树
```
claude-howto/
├── README.md                                    # Main overview
├── INDEX.md                                     # This file
├── QUICK_REFERENCE.md                           # Quick reference card
├── claude_concepts_guide.md                     # Original guide
│
├── 01-slash-commands/                           # Slash Commands
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   ├── commit.md
│   ├── setup-ci-cd.md
│   ├── push-all.md
│   ├── unit-test-expand.md
│   ├── doc-refactor.md
│   ├── pr-slash-command.png
│   └── README.md
│
├── 02-memory/                                   # Memory
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                   # Skills
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-metrics.py
│   │   │   └── compare-complexity.py
│   │   └── templates/
│   │       ├── review-checklist.md
│   │       └── finding-template.md
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   │   ├── email-template.txt
│   │   │   └── social-post-template.txt
│   │   └── tone-examples.md
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   ├── refactor/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-complexity.py
│   │   │   └── detect-smells.py
│   │   ├── references/
│   │   │   ├── code-smells.md
│   │   │   └── refactoring-catalog.md
│   │   └── templates/
│   │       └── refactoring-plan.md
│   ├── claude-md/
│   │   └── SKILL.md
│   ├── blog-draft/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── draft-template.md
│   │       └── outline-template.md
│   └── README.md
│
├── 04-subagents/                                # Subagents
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md
│
├── 05-mcp/                                      # MCP Protocol
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
├── 06-hooks/                                    # Hooks
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md
│
├── 07-plugins/                                  # Plugins
│   ├── pr-review/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── review-pr.md
│   │   │   ├── check-security.md
│   │   │   └── check-tests.md
│   │   ├── agents/
│   │   │   ├── security-reviewer.md
│   │   │   ├── test-checker.md
│   │   │   └── performance-analyzer.md
│   │   ├── mcp/
│   │   │   └── github-config.json
│   │   ├── hooks/
│   │   │   └── pre-review.js
│   │   └── README.md
│   ├── devops-automation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── deploy.md
│   │   │   ├── rollback.md
│   │   │   ├── status.md
│   │   │   └── incident.md
│   │   ├── agents/
│   │   │   ├── deployment-specialist.md
│   │   │   ├── incident-commander.md
│   │   │   └── alert-analyzer.md
│   │   ├── mcp/
│   │   │   └── kubernetes-config.json
│   │   ├── hooks/
│   │   │   ├── pre-deploy.js
│   │   │   └── post-deploy.js
│   │   ├── scripts/
│   │   │   ├── deploy.sh
│   │   │   ├── rollback.sh
│   │   │   └── health-check.sh
│   │   └── README.md
│   ├── documentation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── generate-api-docs.md
│   │   │   ├── generate-readme.md
│   │   │   ├── sync-docs.md
│   │   │   └── validate-docs.md
│   │   ├── agents/
│   │   │   ├── api-documenter.md
│   │   │   ├── code-commentator.md
│   │   │   └── example-generator.md
│   │   ├── mcp/
│   │   │   └── github-docs-config.json
│   │   ├── templates/
│   │   │   ├── api-endpoint.md
│   │   │   ├── function-docs.md
│   │   │   └── adr-template.md
│   │   └── README.md
│   └── README.md
│
├── 08-checkpoints/                              # Checkpoints
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                        # Advanced Features
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                      # CLI Usage
    └── README.md
```
---

## 按用例快速入门

### 代码质量和评论
```bash
# Install slash command
cp 01-slash-commands/optimize.md .claude/commands/

# Install subagent
cp 04-subagents/code-reviewer.md .claude/agents/

# Install skill
cp -r 03-skills/code-review ~/.claude/skills/

# Or install complete plugin
/plugin install pr-review
```
### DevOps 和部署
```bash
# Install plugin (includes everything)
/plugin install devops-automation
```
### 文档
```bash
# Install slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# Install subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# Install skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# Or install complete plugin
/plugin install documentation
```
### 团队标准
```bash
# Set up project memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Edit to match your team's standards
```
### 外部集成
```bash
# Set environment variables
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# Install MCP config (project scope)
cp 05-mcp/multi-mcp.json .mcp.json
```
### 自动化和验证
```bash
# Install hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Configure hooks in settings (~/.claude/settings.json)
# See 06-hooks/README.md
```
### 安全实验
```bash
# Checkpoints are created automatically with every user prompt
# To rewind: press Esc+Esc or use /rewind
# Then choose what to restore from the rewind menu

# See 08-checkpoints/README.md for examples
```
### 高级工作流程
```bash
# Configure advanced features
# See 09-advanced-features/config-examples.json

# Use planning mode
/plan Implement feature X

# Use permission modes
claude --permission-mode plan          # For code review (read-only)
claude --permission-mode acceptEdits   # Auto-accept edits
claude --permission-mode auto          # Auto-approve safe actions

# Run in headless mode for CI/CD
claude -p "Run tests and report results"

# Run background tasks
Run tests in background

# See 09-advanced-features/README.md for complete guide
```
---

## 功能覆盖矩阵

|类别 |命令 |agents| MCP|hooks|脚本 |模板|文档 |图片 |总计 |
|----------|----------|--------|-----|--------|---------|------------|------|--------|--------|
| **01 斜线命令** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 记忆** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 skills** | - | - | - | - | 5 | 9 | 1 | - | **28** |
| **04 Subagents** | - | 8 | - | - | - | - | 1 | - | **9** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 hooks** | - | - | - | 8 | - | - | 1 | - | **9** |
| **07 Plugins** | 11 | 11 9 | 3 | 3 | 3 | 3 | 4 | - | **40** |
| **08 检查站** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 高级** | - | - | - | - | - | - | 1 | 2 | **3** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 学习路径

### 初学者（第一周）
1. ✅ 阅读 `README.md`
2. ✅ 安装1-2个斜杠命令
3. ✅ 创建项目内存文件
4. ✅ 尝试基本命令

### 中级（第 2-3 周）
1. ✅ 设置 GitHub MCP
2. ✅ 安装Subagents
3. ✅ 尝试委派任务
4. ✅ 安装skills

### 高级（第 4 周以上）
1. ✅ 安装完整的Plugins
2. ✅ 创建自定义斜线命令
3. ✅ 创建自定义Subagents
4. ✅ 创建自定义skills
5. ✅ 构建您自己的Plugins

### 专家（第 5 周以上）
1. ✅ 设置自动化hooks
2. ✅ 使用检查点进行实验
3. ✅ 配置计划模式
4. ✅ 有效使用权限模式
5. ✅为CI/CD设置headless模式
6. ✅ 主会话管理

---

## 按关键字搜索

### 性能
- `01-slash-commands/optimize.md` - 性能分析
- `04-subagents/code-reviewer.md` - 绩效评估
- `03-skills/code-review/` - 性能指标
- `07-plugins/pr-review/agents/performance-analyzer.md` - 性能专家

### 安全
- `04-subagents/secure-reviewer.md` - 安全审查
- `03-skills/code-review/` - 安全分析
- `07-plugins/pr-review/` - 安全检查

### 测试
- `04-subagents/test-engineer.md` - 测试工程师
- `07-plugins/pr-review/commands/check-tests.md` - 测试覆盖率

### 文档
- `01-slash-commands/generate-api-docs.md` - API 文档命令
- `04-subagents/documentation-writer.md` - 文档编写agents
- `03-skills/doc-generator/` - 文档生成器skills
- `07-plugins/documentation/` - 完整的文档Plugins

### 部署
- `07-plugins/devops-automation/` - 完整的 DevOps 解决方案

### 自动化
- `06-hooks/` - 事件驱动的自动化
- `06-hooks/pre-commit.sh` - 预提交自动化
- `06-hooks/format-code.sh` - 自动格式化
- `09-advanced-features/` - CI/CD 的无头模式

### 验证
- `06-hooks/security-scan.sh` - 安全验证
- `06-hooks/validate-prompt.sh` - 提示验证

### 实验
- `08-checkpoints/` - 安全倒带实验
- `08-checkpoints/checkpoint-examples.md` - 现实世界的例子

### 规划
- `09-advanced-features/planning-mode-examples.md` - 规划模式示例
- `09-advanced-features/README.md` - 扩展思维

### 配置
- `09-advanced-features/config-examples.json` - 配置示例

---

## 注释

- 所有示例都可以使用
- 修改以满足您的特定需求
- 示例遵循claude代码最佳实践
- 每个类别都有自己的自述文件和详细说明
- 脚本包括正确的错误处理
- 模板可定制

---

## 贡献

想要添加更多示例吗？遵循以下结构：
1. 创建适当的子目录
2. 包含 README.md 和用法
3.遵循命名约定
4. 彻底测试
5.更新该索引

---

**最后更新**：2026 年 3 月
**示例总数**：100 多个文件
**类别**：10 个功能
**hooks**：8 个自动化脚本
**配置示例**：10+场景
**随时可用**：所有示例
