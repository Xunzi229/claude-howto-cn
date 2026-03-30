<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 高级功能

全面介绍 Claude Code 的高级功能，包括规划模式、扩展思维、自动模式、后台任务、权限模式、打印模式（非交互）、会话管理、交互功能、频道、语音听写、远程控制、Web 会话、桌面应用程序、任务列表、提示建议、git 工作树、沙箱、托管设置和配置。

## 目录

1.[Overview](#overview)
2.[Planning Mode](#planning-mode)
3.[Extended Thinking](#extended-thinking)
4.[Auto Mode](#auto-mode)
5.[Background Tasks](#background-tasks)
6.[Scheduled Tasks](#scheduled-tasks)
7.[Permission Modes](#permission-modes)
8.[Headless Mode](#headless-mode)
9.[Session Management](#session-management)
10.[Interactive Features](#interactive-features)
11.[Voice Dictation](#voice-dictation)
12.[Channels](#channels)
13.[Chrome Integration](#chrome-integration)
14.[Remote Control](#remote-control)
15.[Web Sessions](#web-sessions)
16.[Desktop App](#desktop-app)
17.[Task List](#task-list)
18.[Prompt Suggestions](#prompt-suggestions)
19.[Git Worktrees](#git-worktrees)
20.[Sandboxing](#sandboxing)
21.[Managed Settings (Enterprise)](#managed-settings-enterprise)
22.[Configuration and Settings](#configuration-and-settings)
23.[Best Practices](#best-practices)
24.[Related Concepts](#related-concepts)

---

## 概述

Claude Code 中的高级功能通过规划、推理、自动化和控制机制扩展了核心功能。这些功能为复杂的开发任务、代码审查、自动化和多会话管理提供了复杂的工作流程。

**主要高级功能包括：**
- **规划模式**：在编码之前创建详细的实施计划
- **扩展思维**：复杂问题的深层推理
- **自动模式**：后台安全分类器在执行前检查每个操作（研究预览）
- **后台任务**：运行长时间操作而不阻塞对话
- **权限模式**：控制claude可以做什么（`default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`）
- **打印模式**：以非交互方式运行 Claude 代码以实现自动化和 CI/CD (`claude -p`)
- **会话管理**：管理多个工作会话
- **交互功能**：键盘快捷键、多行输入和命令历史记录
- **语音听写**：即按即说语音输入，支持 20 种语言 STT
- **通道**：MCP 服务器将消息推送到正在运行的会话中（研究预览）
- **远程控制**：从 Claude.ai 或 Claude 应用程序控制 Claude 代码
- **网络会话**：在浏览器中运行 Claude Code（位于 claude.ai/code）
- **桌面应用程序**：用于视觉差异审查和多个会话的独立应用程序
- **任务列表**：跨上下文压缩的持久任务跟踪
- **提示建议**：基于上下文的智能命令建议
- **Git Worktrees**：用于并行工作的独立工作树分支
- **沙箱**：操作系统级文件系统和网络隔离

- **托管设置**：通过 plist、注册表或托管文件进行企业部署
- **配置**：使用 JSON 配置文件自定义行为

---

## 规划模式

计划模式允许claude在实施复杂的任务之前仔细考虑它们，创建一个可供您审查和批准的详细计划。

### 什么是计划模式？

规划模式是一个两阶段的方法：
1. **规划阶段**：claude分析任务并制定详细的实施计划
2. **实施阶段**：批准后，claude执行计划

### 何时使用计划模式

✅ 使用计划模式：
- 复杂的多文件重构
- 新功能的实现
- 架构变化
- 数据库迁移
- 主要 API 重新设计

❌ 不要将计划模式用于：
- 简单的错误修复
- 格式更改
- 单文件编辑
- 快速查询

### 激活计划模式

**斜线命令**：
```bash
/plan Implement user authentication system
```
**CLI 标志**：
```bash
claude --permission-mode plan
```
**设置为默认**：
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```
**键盘快捷键**：
- `Shift + Tab` - 切换权限模式（包括计划）
- `Alt + M` - 切换权限模式 (Windows/Linux)

### 规划模式的好处

- **结构化实施方法**：实施步骤的详细分解
- **审核批准**：执行前批准或调整计划
- **风险识别**：在实施前识别潜在问题
- **明确的阶段**：有组织的实施阶段和里程碑

### 示例：功能实现
```
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.

## Implementation Plan

### Phase 1: Infrastructure Setup
1. Install WebSocket libraries (socket.io)
2. Set up WebSocket server
3. Configure connection management

### Phase 2: Backend Implementation
4. Create notification service
5. Implement notification queue
6. Add database schema for notifications
7. Create API endpoints

### Phase 3: Frontend Integration
8. Set up WebSocket client
9. Create notification UI components
10. Implement notification state management
11. Add notification permissions

### Phase 4: Testing & Deployment
12. Write unit tests
13. Write integration tests
14. Update documentation
15. Deploy to staging

Estimated time: 3-4 hours
Files to modify: 12
New files to create: 8

Ready to proceed? (yes/no/modify plan)

User: yes

Claude: [Begins implementation following the plan]
```
### 计划修改
```
User: Modify the plan - skip the queue for now, we'll add it later

Claude: Updated plan:
[Shows modified plan with queue removed]

User: Looks good, proceed

Claude: [Implements modified plan]
```
### 规划模式配置

计划模式通过 CLI 标志或斜杠命令激活：
```bash
# Activate plan mode via CLI
claude --permission-mode plan

# Or use the /plan slash command inside the REPL
/plan Implement user authentication system
```
**用于规划的模型别名**：使用 `opusplan` 作为模型别名，以使用 Opus 进行规划并使用 Sonnet 进行执行：
```bash
claude --model opusplan "design and implement the new API"
```
**外部编辑计划**：按 `Ctrl+G` 在外部编辑器中打开当前计划以进行详细修改。

---

## 延伸思考

扩展思维使claude能够在提供解决方案之前花更多时间推理复杂问题。

### 什么是扩展思维？

扩展思维是一个深思熟虑的、逐步的推理过程，claude：
- 分解复杂的问题
- 考虑多种方法
- 评估权衡
- 通过边缘案例分析原因

### 激活扩展思维

**键盘快捷键**：
- `Option + T` (macOS) / `Alt + T` (Windows/Linux) - 切换扩展思维

**自动激活**：
- 默认情况下对所有型号启用（Opus 4.6、Sonnet 4.6、Haiku 4.5）
- Opus 4.6：自适应推理与努力水平：`low` (○)、`medium` (◐)、`high` (●)、`max`（仅限 Opus 4.6）
- 其他型号：固定预算最多 31,999 个tokens

**配置方法**：
- 切换：`Alt+T` / `Option+T`，或通过 `/config`
- 查看推理：`Ctrl+O`（详细模式）
- 设置努力：`/effort` 命令或 `--effort` 标志

**定制预算**：
```bash
export MAX_THINKING_TOKENS=1024
```
**努力程度**（仅限 Opus 4.6）：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○), medium (◐), high (●), or max (Opus 4.6 only)
```
**CLI 标志**：
```bash
claude --effort high "complex architectural review"
```
**斜线命令**：
```
/effort high
```
> **注意：** 提示中的关键字“ultrathink”会激活深度推理模式。努力级别 `low`、`medium`、`high` 和 `max`（仅限 Opus 4.6）控制claude执行推理的程度。

### 扩展思维的好处

- **深度推理**：花更多时间思考复杂问题
- **更好的架构决策**：通过综合评估做出明智的选择
- **考虑边缘情况**：思考潜在的问题和场景
- **彻底分析**：解决复杂问题的结构化方法

### 示例：架构决策
```
User: /think Should we use microservices or a monolith for our e-commerce platform?

Claude: Let me think through this architectural decision carefully...

<extended_thinking>
## Context Analysis
- Team size: 5 developers
- Expected traffic: 10k daily users initially
- Need to scale to 100k+ users
- Limited DevOps resources
- 6-month timeline to MVP

## Approach 1: Microservices
Pros:
+ Independent scaling
+ Technology flexibility
+ Team independence
+ Fault isolation

Cons:
- Operational complexity
- Network overhead
- Distributed tracing needed
- Requires DevOps expertise
- Slower initial development

## Approach 2: Monolith
Pros:
+ Simpler deployment
+ Faster initial development
+ Easier debugging
+ Lower operational overhead
+ Single codebase

Cons:
- Scaling challenges later
- All-or-nothing deployments
- Technology lock-in
- Potential for tight coupling

## Decision Factors
1. Team size (5 devs) → Favors monolith
2. DevOps resources (limited) → Favors monolith
3. Timeline (6 months) → Favors monolith
4. Future scale (100k users) → Achievable with monolith + caching
5. Complexity budget → Limited

## Recommendation
Start with a **modular monolith**:
- Single deployment unit
- Clear module boundaries
- Clean interfaces between modules
- Prepare for future extraction to microservices if needed

This gives you:
✓ Fast initial development
✓ Low operational complexity
✓ Option to extract services later
✓ Clear architecture

Migration path: When you reach 50k+ users or specific modules need independent scaling, extract those modules into microservices.
</extended_thinking>

**Recommendation**: Implement a modular monolith with clean module boundaries.

This approach balances your current constraints (team size, timeline, DevOps resources) with future scalability needs. You can migrate specific modules to microservices later when there's a clear business need.
```
### 扩展思维配置

扩展思维是通过环境变量、键盘快捷键和 CLI 标志来控制的：
```bash
# Set thinking token budget
export MAX_THINKING_TOKENS=16000

# Set effort level (Opus 4.6 only): low (○), medium (◐), high (●), or max (Opus 4.6 only)
export CLAUDE_CODE_EFFORT_LEVEL=high
```
在会话期间使用 `Alt+T` / `Option+T` 进行切换，使用 `/effort` 设置工作量，或通过 `/config` 进行配置。

---

## 自动模式

自动模式是一种研究预览权限模式（2026 年 3 月），它使用后台安全分类器在执行前审查每个操作。它让claude能够自主工作，同时阻止危险操作。

### 要求

- **计划**：团队计划（企业和 API 推出）
- **型号**：claude Sonnet 4.6 或 Opus 4.6
- **分类器**：在 Claude Sonnet 4.6 上运行（增加额外的tokens成本）

### 启用自动模式
```bash
# Unlock auto mode with CLI flag
claude --enable-auto-mode

# Then cycle to it with Shift+Tab in the REPL
```
或者将其设置为默认权限模式：
```bash
claude --permission-mode auto
```
通过配置设置：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```
### 分类器如何工作

背景分类器使用以下决策顺序评估每个动作：

1. **允许/拒绝规则** -- 首先检查显式权限规则
2. **只读/编辑自动批准** -- 文件读取和编辑自动通过
3. **分类器** -- 后台分类器审查动作
4. **回退** -- 在连续 3 个或总共 20 个区块后回退到提示

### 默认阻止的操作

自动模式默认阻止以下内容：

|被阻止的行动 |示例|
|----------------|---------|
|管道到外壳安装 | `curl \| bash` |
|外部发送敏感数据 | API 密钥、网络凭据 |
|生产部署 |部署针对生产的命令 |
|批量删除 |大型目录上的 `rm -rf` |
| IAM 变化 |权限和角色修改 |
|强制推送到主 | `git push --force origin main` |

### 默认允许的操作

|允许的操作 |示例|
|----------------|---------|
|本地文件操作 |读取、写入、编辑项目文件 |
|声明的依赖项安装 |清单中的 `npm install`、`pip install` |
|只读 HTTP | `curl` 用于获取文档 |
|推送到当前分支 | `git push origin feature-branch` |

### 配置自动模式

**将默认规则打印为 JSON**：
```bash
claude auto-mode defaults
```
**通过企业部署的 `autoMode.environment` 托管设置配置可信基础设施**。这允许管理员定义可信的 CI/CD 环境、部署目标和基础设施模式。

### 后备行为

当分类器不确定时，自动模式会回退到提示用户：
- **3个连续**分类器块之后
- 在会话中**20 个**分类器块之后

这可以确保当分类器无法自信地批准某个操作时，用户始终保留控制权。

### 预置等效于 auto-mode 的权限（无需 Team 计划）

如果你没有 Team 计划，或者想采用一种更简单、无需后台 classifier 的方式，可以使用脚本向 `~/.claude/settings.json` 预置一组保守的安全权限基线。该脚本默认只包含只读和本地检查类规则，然后按需让你选择是否额外启用编辑、测试、本地 git 写操作、包安装以及 GitHub 写操作。

**文件：** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 预览将要添加的规则（不会写入任何变更）
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 应用保守基线
python3 09-advanced-features/setup-auto-mode-permissions.py

# 只在需要时再增加能力
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

该脚本会按以下类别添加规则：

| 类别 | 示例 |
|----------|---------|
| 核心只读工具 | `Read(*)`、`Glob(*)`、`Grep(*)`、`Agent(*)`、`WebSearch(*)`、`WebFetch(*)` |
| 本地检查 | `Bash(git status:*)`、`Bash(git log:*)`、`Bash(git diff:*)`、`Bash(cat:*)` |
| 可选编辑 | `Edit(*)`、`Write(*)`、`NotebookEdit(*)` |
| 可选测试/构建 | `Bash(pytest:*)`、`Bash(python3 -m pytest:*)`、`Bash(cargo test:*)` |
| 可选 git 写操作 | `Bash(git add:*)`、`Bash(git commit:*)`、`Bash(git stash:*)` |
| Git（本地写入） | `Bash(git add:*)`、`Bash(git commit:*)`、`Bash(git checkout:*)` |
| 包管理器 | `Bash(npm install:*)`、`Bash(pip install:*)`、`Bash(cargo build:*)` |
| 构建与测试 | `Bash(make:*)`、`Bash(pytest:*)`、`Bash(go test:*)` |
| 常用 shell | `Bash(ls:*)`、`Bash(cat:*)`、`Bash(find:*)`、`Bash(cp:*)`、`Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`、`Bash(gh pr create:*)`、`Bash(gh issue list:*)` |

危险操作（`rm -rf`、`sudo`、强制 push、`DROP TABLE`、`terraform destroy` 等）会被刻意排除。该脚本是幂等的，运行两次也不会重复添加规则。

### Seeding Auto-Mode-Equivalent Permissions (No Team Plan Required)

If you don't have a Team plan or want a simpler approach without the background classifier, you can seed your `~/.claude/settings.json` with a conservative baseline of safe permission rules. The script starts with read-only and local-inspection rules, then lets you opt into edits, tests, local git writes, package installs, and GitHub write actions only when you want them.

**File:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# Preview what would be added (no changes written)
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# Apply the conservative baseline
python3 09-advanced-features/setup-auto-mode-permissions.py

# Add more capability only when you need it
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

The script adds rules across these categories:

| Category | Examples |
|----------|---------|
| Core read-only tools | `Read(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| Local inspection | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| Optional edits | `Edit(*)`, `Write(*)`, `NotebookEdit(*)` |
| Optional test/build | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| Optional git writes | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git stash:*)` |
| Git (local write) | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| Package managers | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
| Build & test | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| Common shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

Dangerous operations (`rm -rf`, `sudo`, force push, `DROP TABLE`, `terraform destroy`, etc.) are intentionally excluded. The script is idempotent — running it twice won't duplicate rules.

---

## 后台任务

后台任务允许执行长时间运行的操作，而不会阻塞您的对话。

### 什么是后台任务？

当您继续工作时，后台任务会异步运行：
- 长测试套件
- 构建流程
- 数据库迁移
- 部署脚本
- 分析工具

**基本用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # Show all tasks
/task status bg-1234 # Check progress
/task show bg-1234   # View output
/task cancel bg-1234 # Cancel task
```
### 启动后台任务
```
User: Run the full test suite in the background

Claude: Starting tests in background (task-id: bg-1234)
You can continue working while tests run.

[You can continue the conversation]

User: Meanwhile, let's refactor the auth module

Claude: [Works on auth module while tests run in background]

[Later, when tests complete]

Claude: 📢 Background task bg-1234 completed:
✅ 245 tests passed
❌ 3 tests failed
View results: /task show bg-1234
```
### 管理后台任务

**列出活动任务**：
```
User: /task list

Active background tasks:
1. [bg-1234] Running tests (50% complete, 2min remaining)
2. [bg-1235] Building Docker image (25% complete, 8min remaining)
3. [bg-1236] Deploying to staging (90% complete, 30sec remaining)
```
**检查任务状态**：
```
User: /task status bg-1234

Task bg-1234: Running tests
Status: In progress
Progress: 120/245 tests (49%)
Started: 2025-11-08 10:30:15
Estimated completion: 2025-11-08 10:34:22
```
**查看任务输出**：
```
User: /task show bg-1234

[Shows live output from the test run]
```
**取消任务**：
```
User: /task cancel bg-1234

Cancelled background task bg-1234
```
### 示例：并行开发
```
User: Run the build in the background

Claude: Starting build... (task-id: bg-5001)

User: Also run the linter in background

Claude: Starting linter... (task-id: bg-5002)

User: While those run, let's implement the new API endpoint

Claude: [Implements API endpoint while build and linter run]

[10 minutes later]

Claude: 📢 Build completed successfully (bg-5001)
📢 Linter found 12 issues (bg-5002)

User: Show me the linter issues

Claude: [Shows linter output from bg-5002]
```
＃＃＃ 配置
```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```
---

## 计划任务

计划任务可让您按照定期计划自动运行提示或作为一次性提醒。任务是会话范围的——它们在 Claude Code 处于活动状态时运行，并在会话结束时被清除。自 v2.1.72+ 起可用。

### `/loop` 命令
```bash
# Explicit interval
/loop 5m check if the deployment finished

# Natural language
/loop check build status every 30 minutes
```
还支持标准 5 字段 cron 表达式以进行精确调度。

### 一次性提醒

设置在特定时间触发一次的提醒：
```
remind me at 3pm to push the release branch
in 45 minutes, run the integration tests
```
### 管理计划任务

|工具|描述 |
|------|-------------|
| `CronCreate` |创建新的计划任务 |
| `CronList` |列出所有活动的计划任务 |
| `CronDelete` |删除计划任务 |

**限制和行为**：
- 每个会话最多 **50 个计划任务**
- 会话范围 - 会话结束时清除
- 重复任务在 **3 天**后自动过期
- 任务仅在 Claude Code 运行时触发 - 无法追赶错过的触发

### 行为细节

|方面|详情 |
|--------|--------|
| **反复出现的抖动** |最多 10% 的间隔（最多 15 分钟）|
| **一次性抖动** |在 :00/:30 边界上最长 90 秒 |
| **错过火灾** |没有追赶——如果 Claude Code 没有运行则跳过 |
| **坚持** |重新启动后不保留|

### 云计划任务

使用 `/schedule` 创建在 Anthropic 基础设施上运行的云计划任务：
```
/schedule daily at 9am run the test suite and report failures
```
云计划任务在重新启动后仍然存在，并且不需要 Claude Code 在本地运行。

### 禁用计划任务
```bash
export CLAUDE_CODE_DISABLE_CRON=1
```
### 示例：监控部署
```
/loop 5m check the deployment status of the staging environment.
        If the deploy succeeded, notify me and stop looping.
        If it failed, show the error logs.
```
> **提示**：计划任务是会话范围的。对于在重新启动后仍然存在的持久自动化，请改用 CI/CD 管道、GitHub Actions 或桌面应用程序计划任务。

---

## 权限模式

权限模式控制claude可以在没有明确批准的情况下执行哪些操作。

### 可用的权限模式

|模式|行为 |
|---|---|
| `default` |只读取文件；提示所有其他操作 |
| `acceptEdits` |读取和编辑文件；命令提示 |
| `plan` |仅读取文件（研究模式，无编辑）|
| `auto` |所有操作均带有后台安全分类器检查（研究预览）|
| `bypassPermissions` |所有操作，无权限检查（危险）|
| `dontAsk` |仅执行预先批准的工具；所有其他人均否认|

在 CLI 中使用 `Shift+Tab` 循环切换模式。使用 `--permission-mode` 标志或 `permissions.defaultMode` 设置设置默认值。

### 激活方法

**键盘快捷键**：
```bash
Shift + Tab  # Cycle through all 6 modes
```
**斜线命令**：
```bash
/plan                  # Enter plan mode
```
**CLI 标志**：
```bash
claude --permission-mode plan
claude --permission-mode auto
```
**环境**：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```
### 权限模式示例

#### 默认模式
claude要求确认重大行动：
```
User: Fix the bug in auth.ts

Claude: I need to modify src/auth.ts to fix the bug.
The change will update the password validation logic.

Approve this change? (yes/no/show)
```
#### 计划模式
执行前审查实施计划：
```
User: /plan Implement user authentication system

Claude: I'll create a plan for implementing authentication.

## Implementation Plan
[Detailed plan with phases and steps]

Ready to proceed? (yes/no/modify)
```
#### 接受编辑模式
自动接受文件修改：
```
User: acceptEdits
User: Fix the bug in auth.ts

Claude: [Makes changes without asking]
```
### 用例

**代码审查**：
```
User: claude --permission-mode plan
User: Review this PR and suggest improvements

Claude: [Reads code, provides feedback, but cannot modify]
```
**结对编程**：
```
User: claude --permission-mode default
User: Let's implement the feature together

Claude: [Asks for approval before each change]
```
**自动化任务**：
```
User: claude --permission-mode acceptEdits
User: Fix all linting issues in the codebase

Claude: [Auto-accepts file edits without asking]
```
---

## 无头模式

打印模式 (`claude -p`) 允许 Claude Code 无需交互式输入即可运行，非常适合自动化和 CI/CD。这是非交互模式，取代了旧的 `--headless` 标志。

### 什么是打印模式？

打印模式可以：
- 自动执行脚本
- CI/CD 集成
- 批量处理
- 计划任务

### 在打印模式下运行（非交互式）
```bash
# Run specific task
claude -p "Run all tests"

# Process piped content
cat error.log | claude -p "Analyze these errors"

# CI/CD integration (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR"
```
### 其他打印模式使用示例
```bash
# Run a specific task with output capture
claude -p "Run all tests and generate coverage report"

# With structured output
claude -p --output-format json "Analyze code quality"

# With input from stdin
echo "Analyze code quality" | claude -p "explain this"
```
### 示例：CI/CD 集成

**GitHub 操作**：
```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "Review this PR for:
            - Code quality issues
            - Security vulnerabilities
            - Performance concerns
            - Test coverage
            Output results as JSON" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```
### 打印模式配置

打印模式 (`claude -p`) 支持多个自动化标志：
```bash
# Limit autonomous turns
claude -p --max-turns 5 "refactor this module"

# Structured JSON output
claude -p --output-format json "analyze this codebase"

# With schema validation
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "find bugs in this code"

# Disable session persistence
claude -p --no-session-persistence "one-off analysis"
```
---

## 会话管理

有效管理多个claude代码会话。

### 会话管理命令

|命令 |描述 |
|---------|-------------|
| `/resume` |按 ID 或姓名恢复对话 |
| `/rename` |命名当前会话 |
| `/fork` |将当前会话分叉到一个新分支 |
| `claude -c` |继续最近的对话 |
| `claude -r "session"` |按名称或 ID 恢复会话 |

### 恢复会话

**继续上次谈话**：
```bash
claude -c
```
**恢复指定会话**：
```bash
claude -r "auth-refactor" "finish this PR"
```
**重命名当前会话**（在 REPL 内）：
```
/rename auth-refactor
```
### 分叉会话

分叉一个会话来尝试替代方法而不丢失原始方法：
```
/fork
```
或者从 CLI：
```bash
claude --resume auth-refactor --fork-session "try OAuth instead"
```
### 会话持续性

会话会自动保存并可以恢复：
```bash
# Continue last conversation
claude -c

# Resume specific session by name or ID
claude -r "auth-refactor"

# Resume and fork for experimentation
claude --resume auth-refactor --fork-session "alternative approach"
```
---

## 互动功能

### 键盘快捷键

Claude Code 支持键盘快捷键以提高效率。这是官方文档的完整参考：

|快捷方式|描述 |
|----------|-------------|
| `Ctrl+C` |取消当前输入/生成|
| `Ctrl+D` |退出claude代码 |
| `Ctrl+G` |在外部编辑器中编辑计划 |
| `Ctrl+L` |清除终端屏幕|
| `Ctrl+O` |切换详细输出（查看推理）|
| `Ctrl+R` |反向搜索历史记录 |
| `Ctrl+T` |切换任务列表视图 |
| `Ctrl+B` |后台运行任务|
| `Esc+Esc` |倒回代码/对话 |
| `Shift+Tab` / `Alt+M` |切换权限模式 |
| `Option+P` / `Alt+P` |开关型号|
| `Option+T` / `Alt+T` |切换扩展思维 |

**行编辑（标准阅读行快捷键）：**

|快捷方式|行动|
|----------|--------|
| `Ctrl + A` |移至行首 |
| `Ctrl + E` |移至行尾 |
| `Ctrl + K` |切到行尾 |
| `Ctrl + U` |切至行首 |
| `Ctrl + W` |向后删除单词 |
| `Ctrl + Y` |粘贴（猛拉）|
| `Tab` |自动完成 |
| `↑ / ↓` |命令历史 |

### 自定义键绑定

通过运行 `/keybindings` 创建自定义键盘快捷键，这将打开 `~/.claude/keybindings.json` 进行编辑 (v2.1.18+)。

**配置格式**：
```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```
将绑定设置为 `null` 以取消绑定默认快捷方式。

### 可用上下文

键绑定的范围仅限于特定的 UI 上下文：

|背景 |关键行动|
|---------|-------------|
| **聊天** | `submit`、`cancel`、`cycleMode`、`modelPicker`、`thinkingToggle`、`undo`、`externalEditor`、`stash`、`imagePaste` |
| **确认** | `yes`、`no`、`previous`、`next`、`nextField`、`cycleMode`、`toggleExplanation` |
| **全球** | `interrupt`、`exit`、`toggleTodos`、`toggleTranscript` |
| **自动完成** | `accept`、`dismiss`、`next`、`previous` |
| **历史搜索** | `search`、`previous`、`next` |
| **设置** |特定于上下文的设置导航 |
| **标签** |选项卡切换与管理 |
| **帮助** |帮助面板导航 |

总共有 18 个上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 和 `Select`。

### 和弦支持

键绑定支持和弦序列（多键组合）：
```
"ctrl+k ctrl+s"   → Two-key sequence: press ctrl+k, then ctrl+s
"ctrl+shift+p"    → Simultaneous modifier keys
```
**击键语法**：
- **修饰符**：`ctrl`、`alt`（或 `opt`）、`shift`、`meta`（或 `cmd`）
- **大写意味着 Shift**：`K` 相当于 `shift+k`
- **特殊键**：`escape`、`enter`、`return`、`tab`、`space`、`backspace`、`delete`、方向键

### 保留键和冲突键

|关键|状态 |笔记|
|-----|--------|--------|
| `Ctrl+C` |保留 |不可反弹（中断）|
| `Ctrl+D` |保留 |无法反弹（退出）|
| `Ctrl+B` |终端冲突 | tmux 前缀键 |
| `Ctrl+A` |终端冲突 | GNU Screen 前缀键 |
| `Ctrl+Z` |终端冲突 |进程挂起 |

> **提示**：如果快捷方式不起作用，请检查与终端仿真器或多路复用器是否存在冲突。

### 制表符补全

Claude Code 提供智能制表符补全：
```
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```
### 命令历史

访问之前的命令：
```
User: <↑>  # Previous command
User: <↓>  # Next command
User: Ctrl+R  # Search history

(reverse-i-search)`test': run all tests
```
### 多行输入

对于复杂的查询，使用多行模式：
```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```
**例子：**
```
User: \
> Implement a user authentication system
> with the following requirements:
> - JWT tokens
> - Email verification
> - Password reset
> - 2FA support
> \end

Claude: [Processes the multi-line request]
```
### 内联编辑

发送前编辑命令：
```
User: Deploy to prodcution<Backspace><Backspace>uction

[Edit in-place before sending]
```
### Vim 模式

启用 Vi/Vim 键绑定进行文本编辑：

**激活**：
- 使用 `/vim` 命令或 `/config` 启用
- 模式切换为 `Esc` 为 NORMAL，`i/a/o` 为 INSERT

**导航键**：
- `h` / `l` - 左/右移动
- `j` / `k` - 向下/向上移动
- `w` / `b` / `e` - 按字移动
- `0` / `$` - 移至行开头/结尾
- `gg` / `G` - 跳转到文本的开头/结尾

**文本对象**：
- `iw` / `aw` - 内部/周围字
- `i"` / `a"` - 内部/周围带引号的字符串
- `i(` / `a(` - 内/括号内

### 重击模式

直接使用 `!` 前缀执行 shell 命令：
```bash
! npm test
! git status
! cat src/index.js
```
使用它可以快速执行命令，而无需切换上下文。

---

## 语音听写

语音听写为 Claude Code 提供即按即说语音输入，使您可以说出提示而不是键入提示。

### 激活语音听写
```
/voice
```
### 特点

|特色|描述 |
|---------|-------------|
| **一键通** |按住按键录音，松开发送|
| **20 种语言** |语音转文本支持 20 种语言 |
| **自定义按键绑定** |通过 `/keybindings` 配置一键通键 |
| **帐户要求** |需要 Claude.ai 帐户才能进行 STT 处理 |

### 配置

在键绑定文件 (`/keybindings`) 中自定义一键通键绑定。语音听写使用您的 Claude.ai 帐户进行语音到文本的处理。

---

## 频道

通道（研究预览）允许 MCP 服务器将消息推送到正在运行的 Claude Code 会话中，从而实现与外部服务的实时集成。

### 订阅频道
```bash
# Subscribe to channel plugins at startup
claude --channels discord,telegram
```
### 支持的集成

|整合 |描述 |
|-------------|-------------|
| **不和谐** |在您的会话中接收和回复 Discord 消息 |
| **电报** |在您的会话中接收并回复 Telegram 消息 |

### 配置

**企业部署的托管设置**：
```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```
`allowedChannelPlugins` 托管设置控制整个组织允许哪些通道Plugins。

### 它是如何运作的

1. MCP服务器充当连接外部服务的通道Plugins
2. 传入消息被推送到活动的 Claude Code 会话中
3. Claude 可以在会话上下文中阅读和回复消息
4. 频道Plugins必须通过 `allowedChannelPlugins` 托管设置批准

---

## Chrome 集成

Chrome 集成将 Claude Code 连接到您的 Chrome 或 Microsoft Edge 浏览器，以实现实时 Web 自动化和调试。这是自 v2.0.73+ 起提供的测试版功能（v1.0.36+ 中添加了 Edge 支持）。

### 启用 Chrome 集成

**启动时**：
```bash
claude --chrome      # Enable Chrome connection
claude --no-chrome   # Disable Chrome connection
```
**在一个会话中**：
```
/chrome
```
选择“默认启用”可为所有未来会话激活 Chrome 集成。 Claude Code 共享您浏览器的登录状态，因此它可以与经过身份验证的 Web 应用程序进行交互。

### 能力

|能力|描述 |
|------------|-------------|
| **实时调试** |实时读取控制台日志、检查 DOM 元素、调试 JavaScript |
| **设计验证** |将渲染页面与设计模型进行比较 |
| **表单验证** |测试表单提交、输入验证和错误处理 |
| **网络应用程序测试** |与经过身份验证的应用程序交互（Gmail、Google Docs、Notion 等）|
| **数据提取** |从网页中抓取和处理内容 |
| **会议录音** |将浏览器交互记录为 GIF 文件 |

### 站点级权限

Chrome 扩展程序管理每个站点的访问。通过扩展弹出窗口随时授予或撤销对特定站点的访问权限。claude代码仅与您明确允许的网站进行交互。

### 它是如何工作的

Claude Code 在可见窗口中控制浏览器 - 您可以实时观看操作发生。当浏览器遇到登录页面或验证码时，Claude 会暂停并等待您手动处理后再继续。

### 已知限制

- **浏览器支持**：仅限 Chrome 和 Edge — 不支持 Brave、Arc 和其他 Chromium 浏览器
- **WSL**：在适用于 Linux 的 Windows 子系统中不可用
- **第三方提供商**：Bedrock、Vertex 或 Foundry API 提供商不支持
- **Service Worker 空闲**：Chrome 扩展 Service Worker 可能会在长时间会话期间处于空闲状态

> **提示**：Chrome 集成是测试版功能。浏览器支持可能会在未来版本中扩展。

---

## 远程控制

远程控制可让您从手机、平板电脑或任何浏览器继续本地运行的 Claude Code 会话。您的本地会话继续在您的计算机上运行 - 没有任何内容移动到云端。适用于 Pro、Max、Team 和 Enterprise 计划 (v2.1.51+)。

### 启动远程控制

**从 CLI**：
```bash
# Start with default session name
claude remote-control

# Start with a custom name
claude remote-control --name "Auth Refactor"
```
**在会话中**：
```
/remote-control
/remote-control "Auth Refactor"
```
**可用标志**：

|旗帜|描述 |
|------|-------------|
| `--name "title"` |自定义会话标题，易于识别 |
| `--verbose` |显示详细的连接日志 |
| `--sandbox` |启用文件系统和网络隔离 |
| `--no-sandbox` |禁用沙箱（默认）|

### 连接到会话

从其他设备连接的三种方法：

1. **会话 URL** — 会话开始时打印到终端；在任何浏览器中打开
2. **二维码** — 开始显示可扫描的二维码后按`spacebar`
3. **按名称查找** — 在 claude.ai/code 或 Claude 移动应用程序 (iOS/Android) 中浏览您的会话

### 安全

- **您的计算机上没有打开入站端口**
- **仅限出站 HTTPS** 通过 TLS
- **范围凭证** — 多个短期、范围狭窄的Token
- **会话隔离** — 每个远程会话都是独立的

### 远程控制与网络上的claude代码

|方面|远程控制|网络上的claude·代码 |
|--------|-------------|--------------------|
| **执行** |在您的机器上运行 |在 Anthropic 云上运行 |
| **本地工具** |对本地 MCP 服务器、文件和 CLI 的完全访问权限没有本地依赖|
| **用例** |从另一台设备继续本地工作 |从任何浏览器开始 |

### 限制

- 每个 Claude Code 实例一个远程会话
- 终端必须在主机上保持打开状态
- 如果网络无法访问，会话将在大约 10 分钟后超时

### 用例

- 离开办公桌时通过移动设备或平板电脑控制 Claude Code
- 使用更丰富的 claude.ai UI，同时保持本地工具执行
- 在完整的本地开发环境中进行快速代码审查

---

## 网络会话

Web 会话允许您直接在浏览器（claude.ai/code）中运行 Claude Code，或从 CLI 创建 Web 会话。

### 创建网络会话
```bash
# Create a new web session from the CLI
claude --remote "implement the new API endpoints"
```
这将在 claude.ai 上启动一个 Claude Code 会话，您可以从任何浏览器访问该会话。

### 在本地恢复 Web 会话

如果您在网络上启动了会话并希望在本地继续该会话：
```bash
# Resume a web session in the local terminal
claude --teleport
```
或者从交互式 REPL 中：
```
/teleport
```
### 用例

- 在一台机器上开始工作并在另一台机器上继续
- 与团队成员共享会话 URL
- 使用Web UI进行视觉差异审查，然后切换到终端执行

---

## 桌面应用程序

Claude Code 桌面应用程序提供了一个独立的应用程序，具有视觉差异审查、并行会话和集成连接器。适用于 macOS 和 Windows（Pro、Max、Team 和 Enterprise 计划）。

### 安装

从 [claude.ai](https://claude.ai) 下载适合您平台的：
- **macOS**：通用构建（Apple Silicon 和 Intel）
- **Windows**：提供 x64 和 ARM64 安装程序

请参阅 [Desktop Quickstart](https://code.claude.com/docs/en/desktop-quickstart) 了解设置说明。

### 从 CLI 移交

将当前的 CLI 会话转移到桌面应用程序：
```
/desktop
```
### 核心功能

|特色|描述 |
|---------|-------------|
| **差异视图** |逐个文件的视觉审查以及内嵌注释；claude阅读评论并修改|
| **应用程序预览** |自动启动带有嵌入式浏览器的开发服务器以进行实时验证 |
| **公关监控** | GitHub CLI 集成自动修复 CI 故障并在检查通过时自动合并 |
| **平行会议** |侧边栏中的多个会话具有自动 Git 工作树隔离 |
| **计划任务** |应用程序打开时运行的重复任务（每小时、每天、工作日、每周）|
| **丰富的渲染** |带有语法高亮的代码、Markdown 和图表渲染 |

### 应用程序预览配置

在 `.claude/launch.json` 中配置开发服务器行为：
```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```
### 连接器

连接外部服务以获得更丰富的上下文：

|连接器|能力|
|------------|------------|
| **GitHub** | PR 监控、问题跟踪、代码审查 |
| **Slack** |通知、渠道上下文 |
| **线性** |问题跟踪、冲刺管理 |
| **概念** |文档、知识库访问 |
| **Asana** |任务管理、项目跟踪 |
| **日历** |日程安排意识、会议背景 |

> **注意**：连接器不可用于远程（云）会话。

### 远程和 SSH 会话

- **远程会话**：在 Anthropic 云基础设施上运行；即使应用程序关闭也可以继续。可从 claude.ai/code 或 Claude 移动应用程序访问
- **SSH 会话**：通过 SSH 连接到远程计算机，并具有对远程文件系统和工具的完全访问权限。claude代码必须安装在远程机器上

### 桌面中的权限模式

桌面应用程序支持与 CLI 相同的 4 种权限模式：

|模式|行为 |
|------|----------|
| **请求权限**（默认）|审查并批准每个编辑和命令 |
| **自动接受编辑** |文件编辑自动批准；命令需要手动批准 |
| **计划模式** |在进行任何更改之前审查方法 |
| **绕过权限** |自动执行（仅限沙箱，管理员控制）|

### 企业特色

- **管理控制台**：组织的控制代码选项卡访问和权限设置
- **MDM 部署**：通过 macOS 上的 MDM 或 Windows 上的 MSIX 进行部署
- **SSO 集成**：要求组织成员单点登录
- **托管设置**：集中管理团队配置和模型可用性

---

## 任务列表

任务列表功能提供了持久的任务跟踪，可以在上下文压缩中幸存下来（当对话历史记录被修剪以适合上下文窗口时）。

### 切换任务列表

在会话期间按 `Ctrl+T` 可打开或关闭任务列表视图。

### 持久任务

任务在上下文压缩中持续存在，确保在修剪对话上下文时不会丢失长时间运行的工作项。这对于复杂的多步骤实施特别有用。

### 命名任务目录

使用 `CLAUDE_CODE_TASK_LIST_ID` 环境变量创建跨会话共享的命名任务目录：
```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```
这允许多个会话共享相同的任务列表，这对于团队工作流程或多会话项目非常有用。

---

## 提示建议

提示建议根据您的 git 历史记录和当前对话上下文显示灰显的示例命令。

### 它是如何运作的

- 建议在输入提示下方显示为灰色文本
- 按 `Tab` 接受建议
- 按 `Enter` 接受并立即提交
- 建议是上下文感知的，来自 git 历史记录和对话状态

### 禁用提示建议
```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```
---

## Git 工作树

Git Worktrees 允许您在隔离的工作树中启动 Claude Code，从而可以在不同分支上并行工作，而无需隐藏或切换。

### 从工作树开始
```bash
# Start Claude Code in an isolated worktree
claude --worktree
# or
claude -w
```
### 工作树位置

工作树创建于：
```
<repo>/.claude/worktrees/<name>
```
### Monorepos 的稀疏结账

使用 `worktree.sparsePaths` 设置在 monorepos 中执行稀疏签出，减少磁盘使用和克隆时间：
```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```
### Worktree 工具和hooks

|项目 |描述 |
|------|-------------|
| `ExitWorktree` |退出并清理当前工作树的工具 |
| `WorktreeCreate` |创建工作树时触发 Hook 事件 |
| `WorktreeRemove` |删除工作树时触发 Hook 事件 |

### 自动清理

如果工作树中未进行任何更改，则会在会话结束时自动清除。

### 用例

- 在功能分支上工作，同时保持主分支不变
- 独立运行测试而不影响工作目录
- 在一次性环境中尝试实验性改变
- 在 monorepos 中稀疏签出特定包以加快启动速度

---

## 沙箱

沙盒为 Claude Code 执行的 Bash 命令提供操作系统级文件系统和网络隔离。这是对权限规则的补充，并提供了额外的安全层。

### 启用沙箱

**斜线命令**：
```
/sandbox
```
**CLI 标志**：
```bash
claude --sandbox       # Enable sandboxing
claude --no-sandbox    # Disable sandboxing
```
### 配置设置

|设置|描述 |
|---------|-------------|
| `sandbox.enabled` |启用或禁用沙箱 |
| `sandbox.failIfUnavailable` |如果沙箱无法激活则失败 |
| `sandbox.filesystem.allowWrite` |允许写访问的路径 |
| `sandbox.filesystem.allowRead` |允许读取访问的路径 |
| `sandbox.filesystem.denyRead` |读取访问被拒绝的路径 |
| `sandbox.enableWeakerNetworkIsolation` |在 macOS 上启用较弱的网络隔离 |

### 配置示例
```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```
### 它是如何运作的

- Bash 命令在文件系统访问受限的沙盒环境中运行
- 可以隔离网络访问以防止意外的外部连接
- 与深度防御的权限规则一起工作
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 进行网络限制（macOS 上不提供完整的网络隔离）

### 用例

- 安全地运行不受信任或生成的代码
- 防止意外修改项目外的文件
- 在自动化任务期间限制网络访问

---

## 托管设置（企业）

托管设置使企业管理员能够使用平台本机管理工具在整个组织中部署 Claude Code 配置。

### 部署方法

|平台|方法|自从 |
|----------|--------|--------|
| macOS |托管 plist 文件 (MDM) | v2.1.51+ |
|窗户| Windows 注册表 | v2.1.51+ |
|跨平台|托管配置文件| v2.1.51+ |
|跨平台|托管Plugins（`managed-settings.d/` 目录）| v2.1.83+ |

### 托管Plugins

从 v2.1.83 开始，管理员可以将多个托管设置文件部署到 `managed-settings.d/` 目录中。文件按字母顺序合并，允许跨团队进行模块化配置：
```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```
### 可用的托管设置

|设置|描述 |
|---------|-------------|
| `disableBypassPermissionsMode` |阻止用户启用绕过权限|
| `availableModels` |限制用户可以选择哪些机型 |
| `allowedChannelPlugins` |控制允许使用哪些频道Plugins |
| `autoMode.environment` |为自动模式配置可信基础设施 |
|定制政策|组织特定的权限和工具策略 |

### 示例：macOS Plist
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```
---

## 配置和设置

### 配置文件位置

1. **全局配置**：`~/.claude/config.json`
2. **项目配置**：`./.claude/config.json`
3. **用户配置**：`~/.config/claude-code/settings.json`

### 完整配置示例

**核心高级功能配置：**
```json
{
  "permissions": {
    "mode": "default"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```
**扩展配置示例：**
```json
{
  "permissions": {
    "mode": "default",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```
### 环境变量

使用环境变量覆盖配置：
```bash
# Model selection
export ANTHROPIC_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API configuration
export ANTHROPIC_API_KEY=sk-ant-...

# Thinking configuration
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high

# Feature toggles
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # Set by --bare flag

# MCP configuration
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# Task management
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# Agent teams (experimental)
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true

# Subagent and plugin configuration
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=true

# Subprocess and streaming
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000
```
### 配置管理命令
```
User: /config
[Opens interactive configuration menu]
```
`/config` 命令提供了一个交互式菜单来切换设置，例如：
- 扩展思考开/关
- 详细输出
- 权限模式
- 型号选择

### 每个项目配置

在您的项目中创建 `.claude/config.json`：
```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```
---

## 最佳实践

### 规划模式
- ✅ 用于复杂的多步骤任务
- ✅ 在批准之前审查计划
- ✅ 需要时修改计划
- ❌ 不要用于简单任务

### 延伸思考
- ✅ 用于架构决策
- ✅ 用于解决复杂的问题
- ✅ 回顾思考过程
- ❌不要用于简单查询

### 后台任务
- ✅ 用于长时间运行的操作
- ✅ 监控任务进度
- ✅ 优雅地处理任务失败
- ❌不要启动太多并发任务

### 权限
- ✅ 使用 `plan` 进行代码审查（只读）
- ✅ 使用 `default` 进行交互开发
- ✅ 使用 `acceptEdits` 进行自动化工作流程
- ✅ 使用 `auto` 进行带有安全护栏的自主工作
- ❌ 除非绝对必要，否则不要使用 `bypassPermissions`

### 会议
- ✅ 对不同的任务使用单独的会话
- ✅ 保存重要的会话状态
- ✅ 清理旧会话
- ❌ 不要在一个会话中混合不相关的工作

---

## 其他资源

有关claude代码和相关功能的更多信息：

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode)
- [Official Headless Mode Documentation](https://code.claude.com/docs/en/headless)
- [CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [Checkpoints Guide](../08-checkpoints/) - 会话管理和倒带
- [Slash Commands](../01-slash-commands/) - 命令参考
- [Memory Guide](../02-memory/) - 持久上下文
- [Skills Guide](../03-skills/) - 自主能力
- [Subagents Guide](../04-subagents/) - 委派任务执行
- [MCP Guide](../05-mcp/) - 外部数据访问
- [Hooks Guide](../06-hooks/) - 事件驱动的自动化
- [Plugins Guide](../07-plugins/) - 捆绑扩展
- [Official Scheduled Tasks Documentation](https://code.claude.com/docs/en/scheduled-tasks)
- [Official Chrome Integration Documentation](https://code.claude.com/docs/en/chrome)
- [Official Remote Control Documentation](https://code.claude.com/docs/en/remote-control)
- [Official Keybindings Documentation](https://code.claude.com/docs/en/keybindings)
- [Official Desktop App Documentation](https://code.claude.com/docs/en/desktop)
